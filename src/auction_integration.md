# Auction integration in LTV protocol rebalancing

The LTV (Leveraged Token Vault) protocol uses an auction system to manage rebalancing operations. This section explains how these auctions work and how they help keep leverage ratios stable while ensuring fair results for users.

### Key Concepts

- **Collateral**: The main asset you put into the vault (like ETH or LST tokens). This acts as a security to borrow other assets and create leverage.
- **Borrowed asset:** The additional asset borrowed against your collateral (like USDC or DAI). This allows you to have more exposure than your initial deposit.
- **Loan-to-Value (LTV)** - The ratio of borrowed value to collateral value:

```solidity
LTV = (Borrowed Asset Value) / (Collateral Asset Value)
```

**Example**: If you deposit $1000 worth of ETH as collateral and borrow $800 worth of USDC, your LTV is 80%.

- **Target LTV:** The preferred ratio the protocol aims to keep, like 75%, representing the best balance of yield and risk.

```solidity
(Borrow + ΔBorrow) / (Collateral + ΔCollateral) = targetLTV
```

where:

- **Borrow** and **Collateral** are the vault’s current state.

The vault’s design ensures that after applying these deltas (or after an auction), the resulting ratio matches the **target LTV**.

- **Auction:**  A process where participants compete to assist in rebalancing vaults by providing needed assets for rebalance, receiving rewards for participation, and earning fees based on auction timing and amount.

### What is Rebalancing?

*Rebalancing means to restore balance to or adjust the balance of (something)*

In LTV, rebalancing means adjusting the protocol's positions to keep the target LTV (Loan-to-Value) ratio. The protocol keeps a constant target LTV ratio to balance risk and leverage effectively. Rebalancing is needed when:

- User actions (deposits or withdrawals) change the vault's leverage.
- Market changes impact asset prices and effective LTV.
- Interest earned from lending affects position values.
- Target LTV shifts need position adjustments.

Without rebalancing, the protocol could become:

- Over-leveraged, which leads to exposing the user to more risks
- Under-leveraged, which can lead to the user missing out on yield opportunities
- Unbalanced, which may create chances for outside actors to take advantage.

### **Auction**

An auction in LTV is a deterministic exchange mechanism where the vault offers to trade specific amounts of borrow and collateral assets in order to restore target LTV.

- Execution terms are public and deterministic (not competitive bidding).
- Participants must provide assets in the direction the vault requires (opposite sign of imbalance).
- Rewards are built into the exchange rate and grow across auction steps, incentivizing timely participation.
- Profits are possible because the vault tilts prices in favor of executors, but whether it is profitable net of gas/market depends on external conditions.

### How auctions help maintain balance

Auctions are the decentralized way to rebalance the vault:

- It does this by removing the need for curators and encouraging community involvement.
- Ensures fair execution with time-based pricing and open competition.
- Reduces costs by grouping operations and limiting slippage.
- Offers MEV resistance by spreading execution by approximately 512-1024 blocks.

### Advantages of the auction-based system

- The system enhances autonomy by eliminating manual interventions in rebalancing. It operates independently using established formulas and is designed to be inclusive for all interested participants.
- The protocol provides public functions (`executeAuctionBorrow`, `executeAuctionCollateral`) that allow any participant, whether human or bot, to execute rebalancing trades and claim rewards. Time-based rewards encourage timely participation, while transparent pricing allows for informed decision-making.
- MEV protection strategies enhance transaction integrity through key methods. Gradual trade execution mitigates risks of frontrunning and sandwich attacks. Time-based pricing reduces arbitrage opportunities, and grouping operations minimizes price impact, promoting a more stable trading environment.

### LTV safety boundaries

The protocol sets a clear limit to the target loan-to-value (LTV) ratio to ensure safety and profit:

`minProfitLTV`

The minimum profit ratio (for example, 60%) is the lowest level where auctions are still profitable. This rule makes sure that auction participants can earn good rewards, under-leveraging does not lead to bad positions, and protocol operation stay financially stable.

`maxSafeLTV`

The maximum LTV ratio (for example 90%) is the highest level before the vault becomes unsafe/risky. This helps prevent over-leveraging, which can cause liquidation, too much risk for all vault participants, and system instability during market changes.

**LTV Hierachy**: `0 < minProfitLTV < targetLTV < maxSafeLTV < 1`

## Auction lifecycle (step by step)

In this section, we will cover the step-by-step how auctions integrate into LTV protocol rebalancing:

### 1. Triggering auctions

When the vault's current LTV deviates from the targetLTV, an auction window opens.

```solidity
// Query individual auction state variables
uint256 startAuction = ltv.startAuction();
int256 futureBorrowAssets = ltv.futureBorrowAssets();
int256 futureCollateralAssets = ltv.futureCollateralAssets();
int256 futureRewardBorrowAssets = ltv.futureRewardBorrowAssets();
int256 futureRewardCollateralAssets = ltv.futureRewardCollateralAssets();

// Or construct AuctionState struct manually if needed
AuctionState memory auctionState = AuctionState({
 futureBorrowAssets: ltv.futureBorrowAssets(),
 futureCollateralAssets: ltv.futureCollateralAssets(),
 futureRewardBorrowAssets: ltv.futureRewardBorrowAssets(),
 futureRewardCollateralAssets: ltv.futureRewardCollateralAssets(),
 startAuction: ltv.startAuction()
});
```

**Auction state interpretation**

- Positive `futureBorrowAssets` - vault's borrow exposure must increase; auction incentivizes users to supply borrow tokens (or equivalently withdraw collateral)
- Negative `futureBorrowAssets` - vault must reduce borrow exposure; auction incentivizes users to repay borrow (by supplying collateral)
- Positive `futureCollateralAssets` - vault requires more collateral backing; users can deposit collateral
- Negative `futureCollateralAssets` - vault is over-collateralized; users can withdraw collateral by supplying borrow token

**Reward variables**:

- **`futureRewardBorrowAssets`** and **`futureRewardCollateralAssets`** represent the reward leg that incentivizes executors
- These rewards tilt the exchange rate in the user's favor as the auction progresses
- Whoever steps in earns profit by pushing the vault back to target LTV

### 2. Auction functions

The ABI defines two key write functions:

**`executeAuctionBorrow(int256 deltaUserBorrowAssets)`**

User provides or receives borrow assets (e.g., WETH).

```solidity
function executeAuctionBorrow(int256 deltaUserBorrowAssets) external returns (int256);
```

Parameter options:

- Positive delta - user wants to withdraw borrow assets from the vault
- Negative delta - user deposits borrow assets into the vault

Return value: Signed integer representing collateral token movement (denominated in the opposite asset from the input delta)

- Positive return -  user must supply collateral tokens
- Negative return - vault gives collateral tokens to user

**`executeAuctionCollateral(int256 deltaUserCollateralAssets)`**

User provides or receives collateral assets (e.g., MagicETH).

```solidity
function executeAuctionCollateral(int256 deltaUserCollateralAssets) external returns (int256);
```

Parameter options:

- Positive delta -  user deposits collateral
- Negative delta -  user withdraws collateral

Return value: Signed integer representing borrow token movement (denominated in the opposite asset from the input delta)

- Positive return - user must supply borrow tokens
- Negative return -  vault gives borrow tokens to user

Both functions emit:

```solidity
event AuctionExecuted(address *executor*, int256 *deltaRealCollateralAssets*, int256 *deltaRealBorrowAssets*);
```

The event shows real executed deltas, which may differ slightly from preview due to rounding, step limits, or oracle slippage.

### 3. Previewing the outcome

Before calling, users can simulate effects:

```solidity
// Preview borrow auction execution
function previewExecuteAuctionBorrow(int256 deltaUserBorrowAssets)
	external view returns (int256);

// Preview collateral auction execution
function previewExecuteAuctionCollateral(int256 deltaUserCollateralAssets)
	external view returns (int256);
```

Usage Examples

```solidity
// Check collateral movement for providing 1000 borrow tokens
int256 collateralMovement = ltv.previewExecuteAuctionBorrow(-1000);

// Positive result: user must supply collateral

// Negative result: user receives collateral

// Check borrow movement for providing 500 collateral tokens
int256 borrowMovement = ltv.previewExecuteAuctionCollateral(-500);

// Positive result: user must supply borrow tokens

// Negative result: user receives borrow tokens
```

This prevents blind execution and enables bots to optimize gas and arbitrage profitability.

### 4. Parameter validation and constraints

The protocol enforces strict validation:

```solidity
// Internal validation logic
bool hasOppositeSign = data.futureBorrowAssets  deltaUserBorrowAssets < 0;

bool deltaWithinAuctionSize;
      {
          int256 availableCollateralAssets = availableDeltaUserCollateralAssets(
              data.futureRewardCollateralAssets, data.auctionStep, data.futureCollateralAssets
          );
          deltaWithinAuctionSize = (
              availableCollateralAssets > 0 && availableCollateralAssets >= -deltaUserCollateralAssets
          ) || (availableCollateralAssets < 0 && availableCollateralAssets <= -deltaUserCollateralAssets);
      }

require(
    hasOppositeSign && deltaWithinAuctionSize,
    IAuctionErrors.NoAuctionForProvidedDeltaFutureCollateral(
        data.futureCollateralAssets, data.futureRewardCollateralAssets, deltaUserCollateralAssets
    )
);

```

Validation rules:

- Opposite sign: User parameter must oppose current auction direction
- Within Auction size: Amount must not exceed available auction capacity (broken into steps over `AMOUNT_OF_STEPS` blocks)
- Step limitations: The Protocol may limit how much of `futureBorrowAssets`/ `futureCollateralAssets` can be cleared in one execution
- Token approval: User must approve tokens before execution
- Auction direction: Execution must align with the vault's rebalancing needs

### 5. Integration with Rebalancing

The vault constantly strives to maintain target LTV.

When the vault drifts:

- If over-leveraged, the auction incentivizes users to add collateral or repay borrow.
- If under-leveraged, the auction incentivizes users to withdraw collateral or take borrow.

The price incentives are built into the preview functions: participants either get discounted borrow assets or discounted collateral in exchange for restoring balance.

**MEV Protection**: Auctions split incentives across steps, discouraging a single actor from front-running and monopolizing rebalances. This prevents MEV extraction while ensuring fair distribution of rebalancing opportunities.

In effect, auctions turn rebalancing into an open-market game, where rational actors keep the system stable while profiting from incentives.

## How Users Are Affected

When a user action improves the vault’s LTV, they may be rewarded (reduced fees or even positive incentives). If their action worsens the LTV, they pay fees. These fees accumulate as auction rewards, funding incentives for others to restore balance.

Normal users typically deposit or withdraw without directly participating in auctions; their actions may still affect LTV and thus trigger reward/fee adjustments.

Arbitrage bots and integrators actively engage in auctions. By supplying or withdrawing assets when the vault is off-target, they earn rewards that scale with auction timing and participation size. This mechanism ensures the vault self-rebalances without needing a curator.

## Examples

### Example 1: LST-ETH vault deposit causing rebalancing auction

**Scenario**: User deposits 1 stETH into an LST-ETH vault with 80%(0..8) target LTV.

**Step 1:** User deposits 1 stETH

- Current vault state (before deposit):
    - Collateral = 5 stETH
    - Borrow = 4 ETH
    - LTV = 4 / 5 = 80% (0.8) - on target
- After deposit:
    - Collateral = 6 stETH (5 + 1)
    - Borrow = 4 ETH (unchanged)
    - LTV = 4 / 6 = 66.67% (0.667) - below the target of 80%, so the vault needs to increase borrow

**Step 2**: Protocol calculates rebalancing needs

The vault must increase borrow and convert borrowed ETH to stETH and add it as collateral until:

```solidity
(Borrow + ΔBorrow) / (Collateral + ΔCollateral) = targetLTV
```

Where `ΔBorrow` is the extra ETH borrowed (and then swapped to stETH and deposited)

Plug numbers in and solve for Δ:

```solidity
(4 + Δ) / (6 + Δ) = 0.8
```

Work it out step-by-step:

```solidity
1. 4 + Δ = 0.8 * (6 + Δ)
2. 4 + Δ = 4.8 + 0.8Δ
3. Δ − 0.8Δ = 4.8 − 4 → 0.2Δ = 0.8
4. Δ = 0.8 / 0.2 = 4
```

So the vault needs to borrow ΔBorrow = 4 ETH, swap it to ~4 stETH, and deposit that collateral.

If the protocol sets auction reward = 1% of borrowed amount: reward = 1% × 4 = **0.04 ETH** (or equivalent in stETH if paid that way).

> Note: The required borrow is larger because each borrowed ETH both increases the numerator (borrow) and, after swap & deposit, the denominator (collateral).
> 

**Step 3**: Auction starts

Implementations can store `futureCollateralAssets` directly or estimate it based on `futureBorrowAssets` Plus the expected swap rate. The main point is that `futureBorrowAssets` is the amount the vault plans to borrow to reach its target.

**Step 4**: User or bot executes the auction

What the executor actually does:

1. Vault borrows 4 ETH from the connected lending market.
2. Executor swaps the 4 ETH to ~4 stETH (ignoring slippage/fees for simplicity).
3. Executor deposits the ~4 stETH as collateral into the vault (or the vault handles the deposit).
4. Executor receives the 4 ETH reward as auction incentive. The reward token and exact distribution can be configured (ETH, stETH, or split).
5. Auction bookkeeping clears `futureBorrowAssets` / `futureRewardBorrowAssets`.

**Step 5: Final vault state**

- Collateral = 6 + 4 ≈ 10 stETH
- Borrow = 4 + 4 = 8 ETH
- Final LTV = 8 / 10 = 80% (0.8 ) - target reached

### Example 2: Withdrawal triggering auction to repay borrow

**Scenario**: Vault target LTV = **0.75**. A user withdraws 50 USD in borrow tokens.

**Step 1: Initial vault state**

- Collateral = 1000 USD
- Borrow = 750 USD
- `futureBorrowAssets = 0`
- `futureCollateralAssets = 0`
- LTV = 750 ÷ 1000 = **0.75** (on target)

**Step 2: User withdrawal**

- User withdraws 50 USD of borrow token
- Temporary state:
    - Collateral = 1000 USD
    - Borrow = 800 USD
- LTV = 800 ÷ 1000 = **0.80** (> target of 0.75)

**Step 3: Rebalancing requirement**

- To restore balance, both borrow and collateral must be reduced.
- Protocol calculates an auction obligation:
    - `futureBorrowAssets = -200 USD`
    - `futureCollateralAssets = -200 USD`

This means the auction will exchange **200 USD of borrow tokens** (to repay debt) for **200 USD of collateral tokens** (released to the executor).

**Step 4: Auction execution**

- Executor calls `executeAuctionBorrow(200)` (repay path).
- Execution flow:
    1. Executor provides 200 USD in borrow tokens.
    2. Vault repays 200 USD debt in the lending protocol.
    3. Vault releases 200 USD worth of collateral to the executor.

**Step 5: Final vault state**

- Collateral = 800 USD
- Borrow = 600 USD
- `futureBorrowAssets = 0`
- `futureCollateralAssets = 0`
- LTV = 600 ÷ 800 = **0.75** (balanced again)

### Example 3: LTV boundary management

**Scenario**: Market volatility pushes vault near safety boundaries.

**Vault Configuration**:

- `minProfitLTV`: 60%
- `targetLTV`: 80%
- `maxSafeLTV`: 90%

**Case A: LTV drops to 55% (below `minProfitLTV`)**

- No auctions are opened (not profitable to execute)
- Vault remains stable but underleveraged
- Rebalancing waits until user actions or prices move LTV back above 60%

**Case B: LTV rises to 92% (above `maxSafeLTV`)**

- Forced auctions trigger immediately to reduce risk
- Safety takes priority over reward optimization
- Rebalancing continues until LTV returns within the safe range

**Case C: Normal operation at 75% LTV**

- Standard auction opened to restore 80% target
- Regular reward/fee mechanics apply
- Efficient rebalancing proceeds as designed

## FAQ

### Do I need to manually join auctions?

**No.** Regular users don’t need to interact with auctions directly. You just deposit or withdraw as usual. If your action pushes the vault away from the target LTV, the protocol may charge a small fee and open an auction to rebalance. If your action improves the LTV, you may even receive a reward.

### Can I lose funds in auctions?

Not as an executor. Auction participation is always designed to be profitable for arbitrageurs:

- Rewards are set to exceed costs
- Slippage is bounded by parameters
- All outcomes can be previewed on-chain before execution

For regular users, auctions don’t take your funds. However, your deposits/withdrawals may incur a fee if they worsen the vault’s LTV, or earn you a bonus if they improve it.

### How do bots make money here?

Bots and arbitrageurs profit by:

- Executing auctions quickly for higher rewards
- Providing the borrow/collateral needed to rebalance
- Collecting a share of the reward proportional to auction progress and size
- Exploiting market price differences during execution

### Does this affect my withdrawals/deposits?

Only marginally. The auction system is designed to:

- Keep user operations smooth
- Ensure fair pricing through transparent incentives
- Maintain vault efficiency and target leverage

Any fees or rewards from rebalancing are already reflected in share price updates, so overall fairness is preserved.
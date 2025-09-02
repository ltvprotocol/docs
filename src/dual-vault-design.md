# Dual-vault design in LTV protocol

The LTV Protocol features a two-vault system. This innovative design allows the creation of leveraged tokenized vaults that keep a constant Loan-to-Value (LTV) ratio. This design makes the protocol **curatorless** and lets it maintain optimal leverage automatically.

## What is the Dual-vault design?

The LTV protocol operates as two interconnected EIP-4626 vaults that work together to create a single leveraged position:

### 1. Collateral vault

The system is designed to manage primary assets, such as ETH and LST tokens. Its main function involves supplying collateral to various external lending protocols. It also handles a variety of collateral tokens, including ETH, WBTC, and other liquid staking tokens.

### 2. Borrow vault

The primary purpose of this system is to manage borrowed assets, such as USDC and DAI. It functions by overseeing debt positions and facilitating borrowing operations. The protocol is also designed to handle stablecoins and other assets that can be borrowed.

## Why two vaults instead of one?

### **The problem with single vaults**

Traditional leveraged vaults have several issues:

- **Complex tracking**: Managing both collateral and debt in one vault is complicated.
- **Rebalancing challenges**: Single vaults find it hard to keep the right leverage ratios.
- **User confusion**: Users are unsure if they are depositing collateral or debt.
- **Limited options**: It's difficult to separate collateral and borrowing activities.

### **The Dual-vault solution**

The dual-vault design addresses these issues by:

- **Clear roles**: Each vault has a specific function.
- **Easier user experience**: Users can clearly choose which type of asset to deposit.
- **Automatic rebalancing**: The vaults work together to keep the target LTV ratio.
- **More flexibility**: It supports different deposit strategies.

## How the Dual-vault system works

### The LTV invariant

The main part of the dual-vault system is the LTV invariant. This is a mathematical rule that ensures the protocol always keeps the target LTV ratio.

```solidity
Target LTV = (Total Borrow Value) / (Total Collateral Value)
```

For example, if the target LTV is 80%, then for every $1000 of collateral, the protocol will maintain $800 of debt.

Derived formulas:

- Target Borrow = Collateral × Target LTV
- Target Collateral = Borrow ÷ Target LTV

### Vault interaction mechanics

The two vaults work together and do not operate independently. They use advanced coordination to communicate and function as a team.

**State synchronization**

```solidity
// Both vaults share the same underlying state
IERC20 public collateralToken;  // e.g wstETH
IERC20 public borrowToken;      // e.g ETH
uint128 public targetLTV;       // e.g 80%

```

**Automatic rebalancing**

When users interact with either vault, the protocol automatically:

1. Calculates the new LTV ratio
2. Determines what rebalancing is needed
3. Triggers auctions to restore target LTV
4. Updates both vaults to maintain consistency

## User experience with Dual vaults

For most users, the experience is simple:

1. Deposit collateral (ETH, LST tokens, etc.) into the collateral vault
2. Receive vault shares representing your leveraged position
3. The borrow vault works automatically in the background through auctions
4. Withdraw when you want to exit your position

You only interact with one vault, while the protocol automatically manages the other vault for you. This makes the dual-vault complexity easy for regular users.

### Deposit options

Users have two different ways to enter the LTV Protocol, but most users will only use one:

**Option 1: Collateral vault deposit (main entry point)**

```solidity
function depositCollateral(uint256 assets, address receiver)
	external returns (uint256 shares)
```

When you deposit collateral tokens such as ETH, WBTC, or LST token, this asset is supplied to the lending protocols. In exchange for your collateral, you receive vault shares that represent leveraged exposure. This process is primarily used by regular users looking to enter the protocol, making it an ideal option for those who want to leverage their existing collateral.

**Option 2: Borrow vault deposit (advanced usage)**

```solidity
function deposit(uint256 assets, address receiver)
	external returns (uint256 shares)
```

When you deposit borrow tokens, such as USDC, DAI, or other stablecoins, the protocol uses these deposits to repay existing debt, effectively improving the loan-to-value (LTV) ratio. In return, you receive vault shares that provide leveraged exposure to your investment. This process is designed for users, bots, arbitrageurs, and integrators. It is best suited for automated systems and traders who participate in auctions.

### Withdrawal options

Similarly, users can withdraw through either vault:

**Collateral withdrawal**

```solidity
function withdrawCollateral(uint256 assets, address receiver, address owner)
	external returns (uint256 shares)
```

You receive collateral tokens such as ETH or WBTC. The protocol then withdraws this collateral from various lending protocols. This setup is particularly beneficial for users who are seeking to retrieve their original collateral.

**Borrow withdrawal**

```solidity
function withdraw(uint256 assets, address receiver, address owner)
	external returns (uint256 shares)
```

Users have the opportunity to borrow tokens such as USDC and DAI. Upon withdrawal of these borrowed tokens from the vault, the protocol increases the user's debt in relation to their collateral to facilitate this transaction. This service is particularly beneficial for users seeking stablecoins for various purposes.

## Technical architecture

### Contract structure

The dual-vault system is implemented through a modular architecture:

```solidity
contract LTV is
    AuctionRead,
    AuctionWrite,
    ERC20Read,
    ERC20Write,
    LowLevelRebalanceRead,
    LowLevelRebalanceWrite,
    BorrowVaultRead,      // ← Borrow vault functionality
    BorrowVaultWrite,     // ← Borrow vault functionality
    CollateralVaultRead,  // ← Collateral vault functionality
    CollateralVaultWrite, // ← Collateral vault functionality
    AdministrationWrite,
    InitializeWrite
{}
```

### Shared state management

Both vaults share the same underlying state variables:

```solidity
abstract contract LTVState {
    // Core tokens
    IERC20 public collateralToken;  // e.g., wstETH
    IERC20 public borrowToken;      // e.g., ETH
    
    // LTV parameters
    uint128 public targetLTV;       // Target leverage ratio
    uint128 public maxSafeLTV;      // Maximum safe LTV
    uint128 public minProfitLTV;    // Minimum profitable LTV
    
    // Future positions (from auctions)
    int256 public futureBorrowAssets;
    int256 public futureCollateralAssets;
    
    // Protocol state
    uint256 public startAuction;
    uint256 public baseTotalSupply;
    
    // Other variables ...
}
```

### Module delegation

The main LTV contract delegates specific operations to specialized modules:

```solidity
// Borrow vault operations
function deposit(uint256 assets, address receiver) external returns (uint256) {
    _delegate(address(modules.borrowVaultModule()), abi.encode(assets, receiver));
}

// Collateral vault operations
function depositCollateral(uint256 assets, address receiver) external returns (uint256) {
    _delegate(address(modules.collateralVaultModule()), abi.encode(assets, receiver));
}
```

## Benefits of the Dual-vault design

1. User choice and flexibility
    - Users can pick how they want to get started.
    - They can use different investment strategies.
    - The system helps them make the most of their capital.
2. Automatic leverage management
    - Users don’t need to manually adjust their positions.
    - The target loan-to-value (LTV) is guaranteed through math.
    - Positions are automatically optimized.
3. Risk management
    - There is a clear distinction between collateral and debt.
    - The system sets automatic safety limits.
    - If needed, forced auctions help bring the LTV back into safe levels.
4. Protocol efficiency
    - The system uses gas effectively with specialized features.
    - It simplifies individual operations.
    - Error handling and recovery are improved.
5. Integration friendly
    - The protocol meets standard compliance requirements.
    - It easily connects with other DeFi protocols.
    - Developers have clear interfaces to work with.

## How auctions work with dual vaults

### Auction coordination

When rebalancing is needed, auctions work across both vaults:

**Borrow auctions**

When the loan-to-value (LTV) ratio falls below the desired level, indicating that the asset is under-leveraged, the vault triggers an auction. In this auction, participants, known as executors, exchange assets with the vault, aiming to restore the LTV to its target level. 

Executors exchange assets with the vault, and their rewards depend on the type of auction:

- Collateral-for-borrow auctions: Executors provide collateral to the vault in exchange for borrowed tokens and are rewarded accordingly.
- Borrow-for-collateral auctions: Executors provide borrowed tokens to the vault in exchange for collateral and receive rewards in collateral.

This auction mechanism incentivizes participants to actively manage the vault’s leverage, helping bring the LTV back toward the target and stabilizing the vault’s financial position.

**Collateral auctions**

When the loan-to-value (LTV) ratio becomes excessively high, indicating that an asset is over-leveraged, the vault initiates an auction. During this event, executors engage in asset exchanges with the vault, aiming to bring the LTV back to the desired target level. Participants in this process are incentivized with rewards, which can be in the form of collateral or borrow tokens, depending on whether they are contributing to collateral or adjusting the borrow side. This mechanism effectively works to reduce leverage and stabilize the LTV towards its target.

### Auction state tracking

The protocol tracks auction progress across both vaults:

```solidity
// Future positions from ongoing auctions

int256 public futureBorrowAssets;      // Pending borrow changes

int256 public futureCollateralAssets;  // Pending collateral changes

// Auction timing

uint256 public startAuction;           // When current auction started
```

## Real-world example

Let's walk through a complete example of how the dual-vault system works:

### Initial setup

- Target LTV: 80%
- Current State: $1000 ETH collateral, $800 USDC debt
- LTV Ratio: 80% (perfect!)

### User action: Collateral deposit

*Note: This example assumes ETH = $2000 for simplicity*

Step 1. Initial State

- Collateral = $1000 (0.5 ETH)
- Borrow = $800 (USDC)
- Target LTV = 0.80
- Current LTV = $800 / $1000 = 0.80 balanced

Step 2. User deposits 1 ETH ($2000) into the collateral vault

- New Collateral = $3000
- Borrow = $800
- LTV = $800 / $3000 = 0.267 - too low (under-leveraged)

Step 3. Protocol detects imbalance

- Target borrow = $3000 * 0.80 = $2400
- Current borrow = $800
- Required additional borrow = $1600
- Protocol sets:
    - `futureBorrowAssets = +1600`
    - `futureCollateralAssets = 0`

Step 4. Borrow auction is created

- Executors can draw $1600 in USDC debt on behalf of the vault.
- In exchange, they receive collateral rewards defined by auction parameters.
- Execution happens step by step until the borrow reaches the safe zone near target LTV.

Step 5. Auction execution

- Executors participate by taking borrow side positions.
- Vault debt rises, collateral remains unchanged (except for rewards released).
- Each execution nudges the vault toward 0.80 LTV without overshooting `maxSafeLTV`.

Step 6. Final State (after auction completes)

- Collateral ≈ $3000 (minus small reward payouts)
- Borrow ≈ $2400
- LTV = $2400 / $3000 = 0.80 back to target

### User Action: Borrow deposit (advanced usage)

*Assume ETH = $2000 for simplicity*

Step 1. Initial State

- Collateral = $3000 (≈1.5 ETH)
- Borrow = $2400 (USDC)
- Target LTV = 0.80
- Current LTV = $2400 / $3000 = 0.80  balanced

Step 2. User deposits 1000 USDC into the borrow vault

- Collateral = $3000
- Borrow = $2400 – 1000 = $1400
- LTV = $1400 / $3000 = 0.467 - too low (under-leveraged)
- Note: depositing borrow tokens reduces outstanding debt.

Step 3. Protocol detects imbalance

- Target borrow = $3000 × 0.80 = $2400
- Current borrow = $1400
- Additional borrow needed = $1000
- Protocol sets:
    - `futureBorrowAssets = +1000`
    - `futureCollateralAssets = 0`

Step 4. Borrow auction is created

- Executors can increase the vault’s debt by drawing up to $1000 in USDC.
- In exchange, they receive collateral rewards.
- Auction executes gradually, staying inside `minProfitLTV` and `maxSafeLTV`.

Step 5. Auction execution

- Executors call `executeAuctionBorrow(…)` to borrow on behalf of the vault.
- Debt rises from $1400 toward $2400.
- Collateral remains unchanged (apart from small reward outflows).

Step 6. Final State (after auction completes)

- Collateral ≈ $3000
- Borrow ≈ $2400
- LTV = $2400 / $3000 = 0.80 restored to target

Auctions move the vault step-by-step toward the target LTV, always staying within the safety boundaries (e.g., between `minProfitLTV` and `maxSafeLTV`). The exact amount borrowed depends on the target math and cannot exceed safety limits in one move.

## Advanced Features

### Low-level functions

The dual-vault design enables sophisticated low-level operations:

```solidity
// Direct borrow manipulation
function executeLowLevelRebalanceBorrow(int256 deltaBorrow)
    external returns (int256, int256)

// Direct collateral manipulation
function executeLowLevelRebalanceCollateral(int256 deltaCollateral)
    external returns (int256, int256)

// Direct share manipulation
function executeLowLevelRebalanceShares(int256 deltaShares)
    external returns (int256, int256)

```

### Cross-vault operations

Users can perform complex operations that span both vaults:

1. Arbitrage: Exploit price differences between vaults
2. MEV Strategies: Extract value from transaction ordering
3. Protocol Integration: Build on top of LTV vaults
4. Advanced Trading: Execute sophisticated strategies

## Security considerations

### State consistency

The dual-vault system maintains strict consistency:

1. Atomic Operations: All vault operations are atomic
2. State Validation: Every operation validates the complete state
3. Boundary Enforcement: LTV boundaries are strictly enforced
4. Safety Procedures: Forced auctions trigger to bring LTV back inside safe bounds

### Access control

Multiple layers of protection:

```solidity
// Function-level controls
mapping(bytes4 => bool) public _isFunctionDisabled;

// Global controls
bool public isDepositDisabled;
bool public isWithdrawDisabled;

// Whitelist controls
IWhitelistRegistry public whitelistRegistry;
bool public isWhitelistActivated;

```

## Conclusion

The dual-vault design is the foundation that makes the LTV Protocol unique. By separating collateral and borrowing operations into distinct but coordinated vaults, the protocol achieves:

- Simplicity: Easy-to-understand interactions for users
- Efficiency: Improved operations and lower gas usage
- Flexibility: Various ways to use the protocol
- Reliability: Mathematical proof of maintaining target loan-to-value ratios
- Innovation: New design for leveraged vaults

This architecture enables the protocol to be truly curatorless while providing users with the benefits of leveraged exposure to their favorite assets. Whether you're a casual user looking for higher yields or an advanced trader building sophisticated strategies, the dual-vault system provides the tools you need.
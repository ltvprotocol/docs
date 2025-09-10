# How low-level functions function in LTV protocol

### What are Low-Level Functions?

Low-level functions in the LTV Protocol let users manage the vault's rebalancing in a detailed way. Unlike high-level functions, such as `deposit()` and `withdraw()`, which simplify user interactions, low-level functions allow users to adjust collateral, borrow, or shares directly while keeping the vault aligned with the target Loan-to-Value (LTV) ratio.

Think of high-level functions like driving a car with the steering wheel and pedals. In contrast, low-level functions are like controlling the engine, transmission, and brakes directly. They allow for precise tuning of how the vault works.

### Why do they exist?

Low-level functions serve several important purposes:

- They can support complex rebalancing strategies that regular deposit and withdrawal cannot.
- They perform rebalancing tasks with minimal gas costs by skipping unnecessary steps.
- They help with complex strategies like arbitrage and MEV (Maximal extractable value).
- They enable other protocols and automated systems to interact directly with the vault’s core features.
- They offer optimized paths for specific operations, like avoiding chaining multiple high-level calls

### Who uses them?

Low-level functions are primarily used by:

- **Bots and Arbitrageurs**: Automated systems that look for profit from price differences.
- **Users**: Skilled traders who know how the protocol works.
- **Integrators**: Other DeFi protocols connecting with LTV vaults.
- **MEV Searchers**: Individuals aiming to gain value from transaction ordering.
- **Protocol Developers**: Teams creating services on top of or alongside LTV.

## High-level vs low-level functions

### High-level functions

The purpose of the system is to provide a user-friendly interface for executing common financial operations, such as `deposit()`, `withdraw()`, `mint()`, and `redeem()` funds. These operations are designed to be simple, allowing users to complete them in a single step. This interface is particularly beneficial for regular users who frequently deposit or withdraw funds. It is also important to note that the gas cost associated with these operations is higher, as it includes additional safety checks and features aimed at enhancing the user experience.

### Low-level functions

Certain functions allow users to control vault mechanics directly. This includes tasks like `executeLowLevelRebalanceBorrow()` and `executeLowLevelRebalanceCollateral()`. These processes are complex and require a strong understanding of the protocol. They are mainly designed for advanced users, automated bots, and integrators who need precise control. These functions have lower gas costs due to their efficient execution paths.

## When Are Low-Level functions used?

Low-level functions are typically used in these scenarios:

1. Arbitrage Opportunities: When there are price differences between the vault's shares and underlying assets
2. MEV Strategies: When searchers want to extract value from transaction ordering
3. Protocol Integration: When other DeFi protocols need to interact with LTV vaults
4. Advanced Rebalancing: When users want to execute specific rebalancing strategies
5. Gas Optimization: When users want to minimize transaction costs for large operations

## Types of Low-level functions

The LTV Protocol provides three main categories of low-level functions, each designed for different rebalancing strategies:

### 1. Borrow-based rebalance

These functions allow you to rebalance the vault by directly manipulating the borrow position. You specify how much you want to change the vault's debt, and the protocol calculates what collateral and shares need to change to maintain the target LTV ratio.

You it when you want to:

- Increase the vault's leverage by adding more debt
- Decrease the vault's leverage by reducing debt
- Execute arbitrage strategies involving borrow assets
- Optimize gas costs for borrow-focused operations

Key functions:

```solidity
// Preview what would happen if you change the borrow position
function previewLowLevelRebalanceBorrow(int256 deltaBorrowAssets)
	external view returns (int256, int256)

// Execute the borrow-based rebalance
function executeLowLevelRebalanceBorrow(int256 deltaBorrow)
	external returns (int256, int256)

// Get the maximum borrow change allowed
function maxLowLevelRebalanceBorrow() external view returns (int256)
```

Code preview:

```solidity
// Example: Increase vault's borrow position by 1000 DAI
int256 deltaBorrowAssets = 1000e18; // 1000 DAI with 18 decimals

// Preview the operation
(int256 deltaCollateral, int256 deltaShares) = vault.previewLowLevelRebalanceBorrow(deltaBorrowAssets);

// Execute if the preview looks good
vault.executeLowLevelRebalanceBorrow(deltaBorrow);
```

What happens behind the scenes:

1. Protocol calculates how much collateral is needed to maintain target LTV
2. Determines how many shares should be minted/burned
3. Executes the necessary lending protocol interactions
4. Updates the vault's internal state

### 2. Collateral-based rebalance

These functions allow you to rebalance the vault by directly manipulating the collateral position. You specify how much collateral you want to add or remove, and the protocol calculates what borrow and shares need to change to maintain the target LTV ratio.

Use it when you want to:

- Add collateral to increase the vault's safety margin
- Remove collateral to free up capital
- Execute arbitrage strategies involving collateral assets
- Optimize gas costs for collateral-focused operations

Key functions:

```solidity
// Preview what would happen if you change the collateral position
function previewLowLevelRebalanceCollateral(int256 deltaCollateralAssets)
	external view returns (int256, int256)

// Execute the collateral-based rebalance
function executeLowLevelRebalanceCollateral(int256 deltaCollateral)
	external returns (int256 deltaBorrow, int256 deltaShares)

// Get the maximum collateral change allowed
function maxLowLevelRebalanceCollateral() external view returns (int256)
```

Code preview:

```solidity
// Example: Add 1 ETH as collateral to the vault
int256 deltaCollateralAssets = 1e18; // 1 ETH with 18 decimals

// Preview the operation
(int256 deltaBorrow, int256 deltaShares) = vault.previewLowLevelRebalanceCollateral(deltaCollateralAssets);

// Execute if the preview looks good
vault.executeLowLevelRebalanceCollateral(deltaCollateralAssets);
```

What happens behind the scenes:

1. Protocol calculates how much additional borrow can be taken with the new collateral
2. Determines how many shares should be minted/burned
3. Executes the necessary lending protocol interactions
4. Updates the vault's internal state

### 3. Share-based rebalance

These functions allow you to rebalance the vault by directly manipulating the share position. You specify how many shares you want to mint or burn, and the protocol calculates what collateral and borrow need to change to maintain the target LTV ratio.

You it when you want to:

- Mint shares to increase your position in the vault
- Burn shares to reduce your position in the vault
- Execute arbitrage strategies involving share prices
- Optimize gas costs for share-focused operations

Key functions:

```solidity
// Preview what would happen if you change the share position
function previewLowLevelRebalanceShares(int256 deltaShares)
	external view returns (int256, int256)

// Execute the share-based rebalance
function executeLowLevelRebalanceShares(int256 deltaShares)
	external returns (int256, int256)

// Get the maximum share change allowed
function maxLowLevelRebalanceShares() external view returns (int256)
```

Code preview:

```solidity
// Example: Mint 1000 vault shares
int256 deltaShares = 1000e18; // 1000 shares with 18 decimals

// Preview the operation
(int256 deltaCollateral, int256 deltaBorrow) = vault.previewLowLevelRebalanceShares(deltaShares);

// Execute if the preview looks good
vault.executeLowLevelRebalanceShares(deltaShares);
```

What happens behind the scenes:

1. Protocol calculates how much collateral and borrow are needed for the new shares
2. Determines the optimal ratio to maintain target LTV
3. Executes the necessary lending protocol interactions
4. Updates the vault's internal state

### 4. Hint Functions

Hint functions are optimized versions of the main rebalance functions that include additional parameters to help the protocol make better decisions about rounding and execution. They can save gas by providing hints about the expected outcome.

Use when:

- You want to optimize gas costs
- You have a good idea of what the operation will result in
- You are executing complex strategies where gas optimization matters
- You are building bots or automated systems

Key functions:

```solidity
// Borrow-based rebalance with hint
function executeLowLevelRebalanceBorrowHint(int256 deltaBorrowAssets, bool isSharesPositiveHint)
	external returns (int256, int256)

// Collateral-based rebalance with hint
function executeLowLevelRebalanceCollateralHint(int256 deltaCollateralAssets, bool isSharesPositiveHint)
	external returns (int256, int256)

// Preview functions with hints
function previewLowLevelRebalanceBorrowHint(int256 deltaBorrowAssets, bool isSharesPositiveHint)
	external view returns (int256, int256)

function previewLowLevelRebalanceCollateralHint(int256 deltaCollateralAssets, bool isSharesPositiveHint)
	external view returns (int256, int256)
```

Code preview:

```solidity
// Example: Execute borrow rebalance with hint that shares will be positive

int256 deltaBorrowAsset = 1000e18;
bool isSharesPositiveHint = true; // Hint that we expect to mint shares

// Execute with hint for gas optimization

vault.executeLowLevelRebalanceBorrowHint(deltaBorrowAsset, isSharesPositiveHint);
```

What the hint Does:

- `isSharesPositiveHint = true`: Tells the protocol you expect to mint shares (positive deltaShares)
- `isSharesPositiveHint = false`: Tells the protocol you expect to burn shares (negative deltaShares)

This helps the protocol choose the most efficient calculation path and rounding strategy

## Important Considerations

### Safety Features

- All low-level functions include safety checks to prevent excessive leverage
- Maximum limits are enforced to protect the protocol
- Reentrancy protection prevents malicious attacks
- Function stopper modifiers allow emergency pausing

### Gas Optimization

- Low-level functions are designed to minimize gas costs
- Hint functions can provide additional gas savings
- Preview functions allow you to check outcomes before execution
- Batch operations can be more efficient than multiple separate calls

### Risk Considerations

- Low-level functions require deep understanding of the protocol
- Incorrect usage can result in suboptimal outcomes
- Market conditions can affect execution results
- Always use preview functions before execution

### Integration Examples

- Arbitrage Bots: Use low-level functions to quickly capitalize on price differences
- MEV Searchers: Use hints and optimized execution paths for better profitability
- Protocol Integrators: Use low-level functions for seamless vault interactions
- Advanced Traders: Use low-level functions for sophisticated strategies

## Conclusion

Low-level functions provide the building blocks for advanced interactions with the LTV Protocol. While they require more technical knowledge than high-level functions, they offer greater flexibility, efficiency, and control. Whether you're building arbitrage bots, integrating with other protocols, or executing sophisticated trading strategies, low-level functions give you the tools you need to interact with the protocol at its most fundamental level.
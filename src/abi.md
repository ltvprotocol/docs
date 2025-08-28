# **Public ABI Description**

## EIP-4626 Standard (Borrow Vault)

### EIP-4626 read methods/view functions

1. `asset() → address`
    - **Type:** `view`
    - **Outputs:** `address` - The address of the asset token used by the borrow vault.
    - **Description:** This function returns the address of the main asset token for the borrow vault.
2. `totalAssets() → uint256`
    - **Type:** `view`
    - **Outputs:** `uint256` - The total amount of borrowed assets in the vault
    - **Description:** This function returns the total borrowed assets in the vault, including real and future auction assets.
3. `totalAssets(bool isDeposit) → uint256`
    - **Type:** `view`
    - **Inputs:** `isDeposit (bool)` - Whether this is for deposit operations (true) or withdrawal operations (false)
    - **Outputs:** `uint256` - Returns the total amount of borrow assets managed by the vault
    - **Description:** This function returns the total borrow assets with context-aware pricing. When `isDeposit = true`, uses optimistic pricing (rounds up collateral, rounds down borrow). When `isDeposit = false,` uses conservative pricing (rounds down collateral, rounds up borrow).
4. `convertToShares(uint256 assets) → uint256`
    - **Type:** `view`
    - **Inputs:** `assets (uint256)` - The amount of borrowed assets to convert
    - **Outputs:** `uint256` - The number of shares based on the assets
    - **Description:** This function returns how many vault shares are obtained for a certain amount of borrowed assets.
5. `convertToAssets(uint256 shares) → uint256`
    - **Type:** `view`
    - **Inputs:** `shares (uint256)` - The number of shares to convert
    - **Outputs:** `uint256` - The amount of borrowed assets based on the shares
    - **Description:**  This function returns how many borrowed assets are obtained for a certain number of vault shares.
6. `maxDeposit(address receiver) → uint256`
    - **Type:** `view`
    - **Inputs:** `receiver (address)` - The address to check deposit limits
    - **Outputs:** `uint256` - The maximum amount of borrowed assets that can be deposited
    - **Description:** The function returns the maximum borrowed assets that the receiver can deposit.
7. `maxMint(address receiver) → uint256`
    - **Type:** `view`
    - **Inputs:** `receiver (address)` - The address to check mint limits
    - **Outputs:** `uint256` - The maximum number of shares that can be minted for the receiver
    - **Description:** This function shows the maximum vault shares that can be minted for the receiver.
8. `maxWithdraw(address owner) → uint256`
    - **Type:** `view`
    - **Inputs:** `owner (address)` - The address to check withdrawal limits
    - **Outputs:** `uint256` - The maximum amount of borrowed assets that can be withdrawn
    - **Description:** This function returns the maximum borrowed assets the owner can withdraw.
9. `maxRedeem(address owner) → uint256`
    - **Type:** `view`
    - **Inputs:** `owner (address)` - The address to check redemption limits
    - **Outputs:** `uint256` - The maximum number of shares that can be redeemed
    - **Description:** This function returns the maximum shares the owner can redeem.
10. `previewDeposit(uint256 assets) → uint256`
    - **Type:** `view`
    - **Inputs:** `assets (uint256)` - The amount of borrowed assets to deposit
    - **Outputs:** `uint256` - The estimated number of shares received
    - **Description:** This function returns the estimates of how many vault shares you get for depositing a specified amount of borrowed assets.

11. `previewMint(uint256 shares) → uint256`

- **Type:** `view`
- **Inputs:** `shares (uint256)` - The number of shares to mint
- **Outputs:** `uint256` - The estimated amount of borrowed assets needed
- **Description:** This function returns the estimates of how many borrowed assets are needed to mint a specified number of shares.
1. `previewWithdraw(uint256 assets) → uint256`
    - **Type:** `view`
    - **Inputs:** `assets (uint256)` - The amount of borrowed assets to withdraw
    - **Outputs:** `uint256` - The estimated number of shares to be burned
    - **Description:** This function returns the estimates of how many shares must be burned to withdraw the specified amount of borrowed assets.
2.  `previewRedeem(uint256 shares) → uint256`
    - **Type:** `view`
    - **Inputs:** `shares (uint256)` - The number of shares to redeem
    - **Outputs:** `uint256` - The estimated amount of borrowed assets to be received
    - **Description:** This function returns the estimates of how many borrowed assets to get for redeeming the specified number of shares.

### EIP-4626 write methods

1. `deposit(uint256 assets, address receiver) → uint256`
    - **Type:** `nonpayable`
    - **Inputs:**
        - `assets (uint256)` - The amount of borrowed assets to deposit
        - `receiver (address)` - The address to receive the minted shares
    - **Outputs:** `uint256` - The number of shares minted for the receiver
    - **Description:** The function deposits borrowed assets and mints vault shares to the receiver. This may trigger rebalancing to maintain the target loan-to-value (LTV) ratio.
2. `mint(uint256 shares, address receiver) → uint256`
    - **Type:** `nonpayable`
    - **Inputs:**
        - `shares (uint256)` - The number of shares to mint
        - `receiver (address)` - The address to receive the minted shares
    - **Outputs:** `uint256` - The amount of borrowed assets used to mint the shares
    - **Description:** This function mints vault shares in exchange for borrowed assets. The required asset amount is calculated based on current exchange rates.
3. `withdraw(uint256 assets, address receiver, address owner) → uint256`
- **Type:** `nonpayable`
- **Inputs:**
    - `assets (uint256)` - The amount of borrowed assets to withdraw
    - `receiver (address)` - The address to receive the withdrawn assets
    - `owner (address)` - The address whose shares will be burned
- **Outputs:** `uint256` - The number of shares burned from the owner
    - **Description:** This function withdraws borrowed assets by burning shares from the owner and transferring the assets to the receiver.

16. `redeem(uint256 shares, address receiver, address owner) → uint256`

- **Type:** `nonpayable`
- **Inputs:**
    - `shares (uint256)` - The number of shares to redeem
    - `receiver (address)` - The address to receive the withdrawn assets
    - `owner (address)` - The address whose shares will be burned
- **Outputs:** `uint256` - The amount of borrowed assets transferred to the receiver
- **Description:** This function redeems shares and transfers the corresponding borrowed assets.

## EIP-4626 Collateral Extension

### EIP-4626 read methods/view function

1. `maxDepositCollateral(address receiver) → uint256`
    - **Type:** `view`
    - **Inputs:** `receiver (address)` - The address to check for how much collateral can be deposited.
    - **Outputs:** `uint256` - The maximum amount of collateral that can be deposited.
    - **Description:** This function returns the maximum collateral assets the receiver can deposit.
2. `maxMintCollateral(address receiver) → uint256`
    - **Type:** `view`
    - **Inputs:** `receiver (address)` - The address to check for how much collateral can be minted.
    - **Outputs:** `uint256` - The maximum number of collateral shares that can be minted.
    - **Description:** This function returns how many collateral shares can be minted for the receiver.
3. `maxRedeemCollateral(address owner) → uint256`
    - **Type:** `view`
    - **Inputs:** `owner (address)` - The address to check for how much collateral can be redeemed.
    - **Outputs:** `uint256` - The maximum number of collateral shares that can be redeemed.
    - **Description:** This function returns how many collateral shares the owner can redeem.
4. `maxWithdrawCollateral(address owner) → uint256`
    - **Type:** `view`
    - **Inputs:** `owner (address)` - The address to check for how much collateral can be withdrawn.
    - **Outputs:** `uint256` - The maximum amount of collateral assets that can be withdrawn.
    - **Description:** This function returns the maximum collateral assets the owner can withdraw.
5. `previewDepositCollateral(uint256 assets) → uint256`
    - **Type:** `view`
    - **Inputs:** `assets (uint256)` - The amount of collateral assets to deposit.
    - **Outputs:** `uint256` - The estimated number of collateral shares to receive.
    - **Description:** This function estimates how many collateral shares to get for depositing a specific amount of collateral assets.
6. `previewMintCollateral(uint256 shares) → uint256`
    - **Type:** `view`
    - **Inputs:** `shares (uint256)` - The number of collateral shares to mint.
    - **Outputs:** uint256 - The estimated amount of collateral assets needed.
    - **Description:**  This function estimates how much collateral is required to mint a specific number of shares.
7. `previewRedeemCollateral(uint256 shares) → uint256`
    - **Type:** `view`
    - **Inputs:** `shares (uint256)` - The number of collateral shares to redeem.
    - **Outputs:** `uint256` - The estimated amount of collateral assets to receive.
    - **Description:** This function returns an estimate of how much collateral to get for redeeming a specific number of shares.
8. `previewWithdrawCollateral(uint256 assets) → uint256`
    - **Type:** `view`
    - **Inputs:** `assets (uint256)` - The amount of collateral assets to withdraw.
    - **Outputs:** `uint256` - The estimated number of collateral shares that will be burned.
    - **Description:** This function returns the estimated number of collateral shares that need to be burned to withdraw a specific amount of collateral assets.
9. `convertToSharesCollateral(uint256 assets) → uint256`
    - **Type:** `view`
    - **Inputs:** `assets (uint256)` - The amount of collateral assets to convert.
    - **Outputs:** `uint256` - The number of collateral shares corresponding to the assets.
    - **Description:** This function calculates how many collateral shares correspond to a given amount of collateral assets.
10. `convertToAssetsCollateral(uint256 shares) → uint256`
    - **Type:** `view`
    - **Inputs:** `shares (uint256)` - The number of collateral shares to convert.
    - **Outputs:** `uint256` - The amount of collateral assets corresponding to the shares.
    - **Description:** This function calculates how many collateral assets correspond to a given number of collateral shares.
11. `totalAssetsCollateral() → uint256`
    - **Type:** `view`
    - **Outputs:** `uint256` - The total amount of collateral assets managed by the vault.
    - **Description:** This function returns the total collateral assets in the vault using conservative pricing `(isDeposit = false)`.
12. `totalAssetsCollateral(bool isDeposit) → uint256`
    - **Type:** `view`
    - **Inputs:** `isDeposit (bool)` - Whether this is for a deposit operation (true) or withdrawal operation (false).
    - **Outputs:** `uint256` - The total amount of collateral assets managed by the vault.
    - **Description:** This function returns the total collateral assets with context-aware pricing. Use `isDeposit = true` for deposit operations (optimistic pricing) and `isDeposit = false` for withdrawal operations (conservative pricing).

### EIP-4626 write methods

1. `depositCollateral(uint256 assets, address receiver) → uint256`
    - **Type:** `nonpayable`
    - **Inputs:**
        - `assets (uint256)` - The amount of collateral assets to deposit.
        - `receiver (address)` - The address to receive the minted collateral shares.
    - **Outputs:** `uint256` - The number of collateral shares minted to the receiver.
    - **Description:** This function deposits collateral assets and mints collateral shares to the receiver.
2. `mintCollateral(uint256 shares, address receiver) → uint256`
    - **Type:** `nonpayable`
    - **Inputs:**
        - `shares (uint256)` - The number of collateral shares to mint.
        - `receiver (address)` - The address to receive the minted shares.
    - **Outputs:** `uint256` - The amount of collateral assets used to mint the shares.
    - **Description:** This function mints collateral shares in exchange for collateral assets.
3. `withdrawCollateral(uint256 assets, address receiver, address owner) → uint256`
    - **Type:** `nonpayable`
    - **Inputs:**
        - `assets (uint256)` - The amount of collateral assets to withdraw.
        - `receiver (address)` - The address to receive the withdrawn collateral assets.
        - `owner (address)` - The address whose collateral shares will be burned.
    - **Outputs:** `uint256` - The number of collateral shares burned from the owner.
    - **Description:** This function withdraws collateral assets by burning collateral shares from the owner.
4. `redeemCollateral(uint256 shares, address receiver, address owner) → uint256`
    - **Type:** `nonpayable`
    - **Inputs:**
        - `shares (uint256)` - The number of collateral shares to redeem.
        - `receiver (address)` - The address to receive the withdrawn collateral assets.
        - `owner (address)` - The address whose collateral shares will be burned.
    - **Outputs:** `uint256` - The amount of collateral assets transferred to the receiver.
    - **Description:** This function redeems collateral shares and transfers the corresponding collateral assets to the receiver.

## ERC-20 Standard

### ERC-20 read methods/view function

1. `balanceOf(address owner) → uint256`
    - **Type:** `view`
    - **Inputs:** `owner (address)` - The address to check the balance for.
    - **Outputs:** `uint256` - The token balance of the owner.
    - **Description:** This function returns the vault share token balance of the specified address.
2. `totalSupply() → uint256`
    - **Type:** `view`
    - **Outputs:** `uint256` - The total supply of vault share tokens.
    - **Description:** This function returns the total supply of vault share tokens.
3. `decimals() → uint8`
    - **Type:** `view`
    - **Outputs:** `uint8` - The number of decimal places for the token.
    - **Description:** This function returns the number of decimal places for the vault share token.
4. `name() → string`
    - **Type:** `view`
    - **Outputs:** `string` - The token name.
    - **Description:** This function returns the name of the vault share token.
5. `symbol() → string`
    - **Type:** `view`
    - **Outputs:** `string` - The token symbol.
    - **Description:** This function returns the symbol of the vault share token.
6. `allowance(address owner, address spender) → uint256`
    - **Type:** `view`
    - **Inputs:**
        - `owner (address)` - The token owner address.
        - `spender (address)` - The spender address.
    - **Outputs:** `uint256` - The remaining allowance amount.
    - **Description:** This function returns the remaining number of tokens that spender can use on behalf of the owner.

### ERC-20 write methods

1. `approve(address spender, uint256 amount) → bool`
    - **Type:** `nonpayable`
    - **Inputs:**
        - `spender (address)` - The address to approve for spending.
        - `amount (uint256)` - The amount of tokens to approve.
    - **Outputs:** `bool` - The success status.
    - **Description:** This function approves the spender to use an amount of tokens on behalf of the caller.
2. `transfer(address recipient, uint256 amount) → bool`
    - **Type:** `nonpayable`
    - **Inputs:**
        - `recipient (address)` - The address to transfer tokens to.
        - `amount (uint256)` - The amount of tokens to transfer.
    - **Outputs:** `bool` - The success status.
    - **Description:** This function transfers amount tokens to the recipient.
3. `transferFrom(address sender, address recipient, uint256 amount) → bool`
    - **Type:** `nonpayable`
    - **Inputs:**
        - `sender (address)` - The address to transfer tokens from.
        - `recipient (address)` - The address to transfer tokens to.
        - `amount (uint256)` - The amount of tokens to transfer.
    - **Outputs:** `bool` - The success status.
    - **Description:** This function transfers amount tokens from sender to recipient using allowance.

## Auction System

### Auction read methods/view function

1. `startAuction() → uint256`
    - **Type:** `view`
    - **Outputs:** `uint256` - The timestamp when the auction started.
    - **Description:** This function returns the timestamp when the current auction started, or 0 if no auction is active.
2. `previewExecuteAuctionBorrow(int256 deltaUserBorrowAssets) → int256`
    - **Type:** `view`
    - **Inputs:** `deltaUserBorrowAssets (int256)` - The change in user borrow assets (positive to receive, negative to provide).
    - **Outputs:** `int256` - The estimated amount of borrow assets user needs to give/receive.
    - **Description:** This function estimates the amount of borrow assets a user needs to give or receive to execute the auction with the specified delta.
3. `previewExecuteAuctionCollateral(int256 deltaUserCollateralAssets) → int256`
    - **Type:** `view`
    - **Inputs:** `deltaUserCollateralAssets (int256)` - The change in user collateral assets (positive to receive, negative to provide).
    - **Outputs:** `int256` - The estimated amount of collateral assets user needs to give/receive.
    - **Description:** This function estimates the amount of collateral assets a user needs to give or receive to execute the auction with the specified delta.

### Auction write methods

1. `executeAuctionBorrow(int256 deltaUserBorrowAssets) → int256`
    - **Type:** `nonpayable`
    - **Inputs:** `deltaUserBorrowAssets (int256)` - The change in user borrow assets (positive to receive, negative to provide).
    - **Outputs:** `int256` - The actual amount of borrow assets transferred.
    - **Description:** This function executes an auction for borrow assets, allowing users to participate in rebalancing for rewards.
2. `executeAuctionCollateral(int256 deltaUserCollateralAssets) → int256`
    - **Type:** `nonpayable`
    - **Inputs:** `deltaUserCollateralAssets (int256)` - The change in user collateral assets (positive to receive, negative to provide).
    - **Outputs:** `int256` - The actual amount of collateral assets transferred.
    - **Description:** This function executes an auction for collateral assets, allowing users to participate in rebalancing for rewards.

## Low level rebalance

### **Low level rebalance read methods/view function**

1. `previewLowLevelRebalanceShares(int256 deltaShares) → (int256 deltaCollateral, int256 deltaBorrow)`
    - **Type:** `view`
    - **Inputs:** `deltaShares (int256)` - The amount of shares to mint or burn (positive for mint).
    - **Outputs:**
        - `deltaCollateral (int256)` - The amount of collateral assets user will receive or provide.
        - `deltaBorrow (int256)` - The amount of borrow assets user will receive or provide.
    - **Description:** This function previews low level rebalance execution with input in shares. Positive deltaShares mints shares, negative burns shares.
2. `previewLowLevelRebalanceBorrow(int256 deltaBorrowAssets) → (int256 deltaCollateral, int256 deltaShares)`
    - **Type:** view
    - **Inputs:** `deltaBorrowAssets (int256)` - The amount of borrow assets to withdraw or deposit (negative to send borrow assets).
    - **Outputs:**
        - `deltaCollateral (int256)` - The amount of collateral assets user will receive or provide.
        - `deltaShares (int256)` - The amount of shares user will receive or burn.
    - **Description:** This function previews low level rebalance execution with input in borrow assets. Negative `deltaCollateral` means protocol sends collateral to user, positive means user provides collateral. Positive `deltaShares` means user receives shares, negative means shares are burned.
3. `previewLowLevelRebalanceCollateral(int256 deltaCollateralAssets) → (int256 deltaBorrow, int256 deltaShares)`
    - **Type:** `view`
    - **Inputs:** `deltaCollateralAssets (int256)` - The amount of collateral assets to withdraw or deposit (positive to send collateral assets).
    - **Outputs:**
        - `deltaBorrow (int256)` - The amount of borrow assets user will receive or provide.
        - `deltaShares (int256)` - The amount of shares user will receive or burn.
    - **Description:** This function previews low level rebalance execution with input in collateral assets. Positive `deltaBorrow` means protocol sends borrow assets to user, negative means user provides borrow assets.
4. `previewLowLevelRebalanceBorrowHint(int256 deltaBorrowAssets, bool isSharesPositiveHint) → (int256 deltaCollateral, int256 deltaShares)`
    - **Type:** `view`
    - **Inputs:**
        - `deltaBorrowAssets (int256)` - The amount of borrow assets to withdraw or deposit (negative to send borrow assets).
        - `isSharesPositiveHint (bool)` - The hint indicating if user expects to mint shares (saves gas).
    - **Outputs:**
        - `deltaCollateral (int256)` - The amount of collateral assets user will receive or provide.
        - `deltaShares (int256)` - The amount of shares user will receive or burn.
    - **Description:** This function is the same as `previewLowLevelRebalanceBorrow` but with a hint to optimize gas usage.
5. `previewLowLevelRebalanceCollateralHint(int256 deltaCollateralAssets, bool isSharesPositiveHint) → (int256 deltaBorrow, int256 deltaShares)`
    - **Type:** `view`
    - **Inputs:**
        - `deltaCollateralAssets (int256)` - The amount of collateral assets to withdraw or deposit (positive to send collateral assets).
        - `isSharesPositiveHint (bool)` - The hint indicating if user expects to mint shares (saves gas).
    - **Outputs:**
        - `deltaBorrow (int256)`- The amount of borrow assets user will receive or provide.
        - `deltaShares (int256)` - The amount of shares user will receive or burn.
    - **Description:** This function is the same as `previewLowLevelRebalanceCollateral` but with a hint to optimize gas usage.
6. `maxLowLevelRebalanceShares() → int256`
    - **Type:** `view`
    - **Outputs:** `int256` - The maximum amount of shares that can be minted or burned in low level rebalance.
    - **Description:** This function returns the maximum amount of shares that can be minted (positive) or burned (negative) in a low level rebalance operation.
7. `maxLowLevelRebalanceBorrow() → int256`
    - **Type:** `view`
    - **Outputs:** `int256` - The maximum amount of borrow assets that can be used in low level rebalance.
    - **Description:** This function returns the maximum amount of borrow assets that can be withdrawn (positive) or deposited (negative) in a low level rebalance operation.
8. `maxLowLevelRebalanceCollateral() → int256`
    - **Type:** `view`
    - **Outputs:** `int256` - The maximum amount of collateral assets that can be used in low level rebalance.
    - **Description:** This function returns the maximum amount of collateral assets that can be withdrawn (positive) or deposited (negative) in a low level rebalance operation.

### Low level rebalance write methods

1. `executeLowLevelBorrow(int256 deltaBorrow) → (int256 deltaCollateral, int256 deltaShares)`
    - **Type:** `nonpayable`
    - **Inputs:** `deltaBorrow (int256)` - The amount of borrow assets to withdraw or deposit (negative to send borrow assets).
    - **Outputs:**
        - `deltaCollateral (int256)` - The amount of collateral assets transferred.
        - `deltaShares (int256)` - The amount of shares minted or burned.
    - **Description:** This function executes low level rebalance with input in borrow assets. This is a core rebalancing function that maintains the target LTV ratio.
2. `executeLowLevelCollateral(int256 deltaCollateral) → (int256 deltaBorrow, int256 deltaShares)`
    - **Type:** `nonpayable`
    - **Inputs:** `deltaCollateral (int256)` - The amount of collateral assets to withdraw or deposit (positive to send collateral assets).
    - **Outputs:**
        - `deltaBorrow (int256)` - The amount of borrow assets transferred.
        - `deltaShares (int256)` - The amount of shares minted or burned.
    - **Description:** This function executes low level rebalance with input in collateral assets.
3. `executeLowLevelShares(int256 deltaShares) → (int256 deltaCollateral, int256 deltaBorrow)`
    - **Type:** `nonpayable`
    - **Inputs:** `deltaShares (int256)` - The amount of shares to mint or burn (positive for mint).
    - **Outputs:**
        - `deltaCollateral (int256)` - The amount of collateral assets transferred.
        - `deltaBorrow (int256)` - The amount of borrow assets transferred.
    - **Description:** This function executes low level rebalance with input in shares.
4. `executeLowLevelBorrowHint(int256 deltaBorrow, bool isSharesPositiveHint) → (int256 deltaCollateral, int256 deltaShares)`
    - **Type:** `nonpayable`
    - **Inputs:**
        - `deltaBorrow (int256)` - The amount of borrow assets to withdraw or deposit (negative to send borrow assets).
        - `isSharesPositiveHint (bool)` - The hint indicating if user expects to mint shares.
    - **Outputs:**
        - `deltaCollateral (int256)` - The amount of collateral assets transferred.
        - `deltaShares (int256)` - The amount of shares minted or burned.
    - **Description:** This function is the same as executeLowLevelBorrow but with a hint to optimize gas usage.
5. `executeLowLevelCollateralHint(int256 deltaCollateral, bool isSharesPositiveHint) → (int256 deltaBorrow, int256 deltaShares)`
    - **Type:** `nonpayable`
    - **Inputs:**
        - `deltaCollateral (int256)` - The amount of collateral assets to withdraw or deposit (positive to send collateral assets).
        - `isSharesPositiveHint (bool)` - The hint indicating if user expects to mint shares.
    - **Outputs:**
        - `deltaBorrow (int256)` - The amount of borrow assets transferred.
        - `deltaShares (int256)` - The amount of shares minted or burned.
    - **Description:** This function is the same as executeLowLevelCollateral but with a hint to optimize gas usage.

## State Representation

### State read methods/view function

1. `borrowToken() → address`
    - **Type:** `view`
    - **Outputs:** `address` - The borrow token contract address.
    - **Description:** This function returns the address of the borrow token (e.g., USDC, DAI).
2. `collateralToken() → address`
    - **Type:** `view`
    - **Outputs:** `address` - The collateral token contract address.
    - **Description:** This function returns the address of the collateral token (e.g., ETH, WBTC).
3. `futureBorrowAssets() → int256`
    - **Type:** `view`
    - **Outputs:** `int256` - The current auction borrow assets.
    - **Description:** This function returns the current amount of borrow assets in the auction system.
4. `futureCollateralAssets() → int256`
    - **Type:** `view`
    - **Outputs:** `int256` - The current auction collateral assets.
    - **Description:** This function returns the current amount of collateral assets in the auction system.
5. `futureRewardBorrowAssets() → int256`
    - **Type:** `view`
    - **Outputs:** `int256` - The current auction reward in borrow assets.
    - **Description:** This function returns the current auction reward denominated in borrow assets.
6. `futureRewardCollateralAssets() → int256`
    - **Type:** `view`
    - **Outputs:** `int256` - The current auction reward in collateral assets.
    - **Description:** This function returns the current auction reward denominated in collateral assets.
7. `getRealBorrowAssets() → uint256`
    - **Type:** `view`
    - **Outputs:** `uint256` - The protocol's current debt in lending protocol.
    - **Description:** This function returns the protocol's current debt position in the underlying lending protocol in borrow assets.
8. `getRealCollateralAssets() → uint256`
    - **Type:** `view`
    - **Outputs:** `uint256` - The protocol's current collateral in lending protocol.
    - **Description:** This function returns the protocol's current collateral position in the underlying lending protocol in collateral assets.
9. `maxSafeLTV() → uint128`
    - **Type:** `view`
    - **Outputs:** `uint128` - The maximum safe loan-to-value ratio.
    - **Description:** This function returns the maximum safe LTV ratio that the protocol will maintain.
10. `maxTotalAssetsInUnderlying() → uint256`
    - **Type:** `view`
    - **Outputs:** `uint256` - The protocol top border in underlying oracle assets.
    - **Description:** This function returns the maximum total assets the protocol can manage in underlying oracle assets.
11. `minProfitLTV() → uint128`
    - **Type:** `view`
    - **Outputs:** `uint128` - The minimum profitable LTV.
    - **Description:** This function returns the minimum profitable LTV ratio for the protocol.
12. `targetLTV() → uint128`
    - **Type:** `view`
    - **Outputs:** `uint128` - The target loan-to-value ratio.
    - **Description:** This function returns the target LTV ratio that the protocol aims to maintain.
13. `oracleConnector() → address`
    - **Type:** `view`
    - **Outputs:** `address` - The oracle connector address.
    - **Description:** This function returns the address of the oracle connector that provides price feeds.
14. `lendingConnector() → address`
    - **Type:** `view`
    - **Outputs:** `address` - The lending protocol connector address.
    - **Description:** This function returns the address of the lending protocol connector (e.g., Aave V3, Morpho Blue).
15. `feeCollector() → address` 
    - **Type:** `view`
    - **Outputs:** `address` - The fee collector address.
    - **Description:** This function returns the address that receives protocol fees.

## Ownership

### Ownership read methods/view function

1. `owner() → address`
    - **Type:** `view`
    - **Outputs:** `address` - The current owner address.
    - **Description:** This function returns the address of the current contract owner.

### Ownership write methods

1. `transferOwnership(address newOwner)`
    - **Type:** `nonpayable`
    - **Inputs:** `newOwner (address)` - The new owner address.
    - **Description:** This function transfers contract ownership to the new owner.
2. `renounceOwnership()`
    - **Type:** `nonpayable`
    - **Description:** This function renounces ownership of the contract, making it ownerless.

## Administration

### Administration write methods

1. `setTargetLTV(uint128 value)`
    - **Type:** `nonpayable`
    - **Inputs:** `value (uint128)` - The new target LTV value.
    - **Outputs:** None
    - **Description:** This function sets the protocol's new target LTV ratio (governor only).
2. `setMaxSafeLTV(uint128 value)`
    - **Type:** `nonpayable`
    - **Inputs:** `value (uint128)` - The new max safe LTV value.
    - **Outputs:** None
    - **Description:** This function sets the protocol's new maximum safe LTV ratio (governor only).
3. `setMinProfitLTV(uint128 value)`
    - **Type:** `nonpayable`
    - **Inputs:** `value (uint128)` - The new min profit LTV value.
    - **Outputs:** None
    - **Description:** This function sets the protocol's new minimum profitable LTV ratio (governor only).
4. `setFeeCollector(address _feeCollector)`
    - **Type:** `nonpayable`
    - **Inputs:** `_feeCollector (address)` - The new fee collector address.
    - **Outputs:** None
    - **Description:** This function sets the protocol's new fee collector address (governor only).
5. `setMaxTotalAssetsInUnderlying(uint256 _maxTotalAssetsInUnderlying)`
    - **Type:** `nonpayable`
    - **Inputs:** `_maxTotalAssetsInUnderlying (uint256)` - The new maximum total assets value.
    - **Outputs:** None
    - **Description:** This function sets the protocol's maximum total assets in underlying oracle assets (governor only).
6. `setMaxDeleverageFee(uint256 value)`
    - **Type:** `nonpayable`
    - **Inputs:** `value (uint256)` - The new max deleverage fee value.
    - **Outputs:** None
    - **Description:** This function sets the protocol's maximum deleverage fee (governor only).
7. `setIsWhitelistActivated(bool activate)`
    - **Type:** `nonpayable`
    - **Inputs:** `activate (bool)` - Whether to activate the whitelist.
    - **Outputs:** None
    - **Description:** This function activates or deactivates the whitelist functionality (governor only).
8. `setWhitelistRegistry(IWhitelistRegistry value)`
    - **Type:** `nonpayable`
    - **Inputs:** `value (IWhitelistRegistry)` - The new whitelist registry address.
    - **Outputs:** None
    - **Description:** This function sets the protocol's whitelist registry address (governor only).
9. `setSlippageProvider(ISlippageProvider _slippageProvider)`
    - **Type:** `nonpayable`
    - **Inputs:** `_slippageProvider (ISlippageProvider)` - The new slippage provider address.
    - **Outputs:** None
    - **Description:** This function sets the protocol's slippage provider address (governor only).
10. `allowDisableFunctions(bytes4[] memory signatures, bool isDisabled)`
    - **Type:** `nonpayable`
    - **Inputs:**
        - `signatures (bytes4[])` - Array of function signatures to enable/disable.
        - `isDisabled (bool)` - Whether to disable the functions.
    - **Outputs:** None
    - **Description:** This function allows the guardian to disable or enable specific functions (guardian only).
11. `setMaxGrowthFee(uint256 _maxGrowthFee)`
    - **Type:** `nonpayable`
    - **Inputs:** `_maxGrowthFee (uint256)` - The new max growth fee value.
    - **Outputs:** None
    - **Description:** This function sets the protocol's maximum growth fee (governor only).
12. `setIsDepositDisabled(bool value)`
    - **Type:** `nonpayable`
    - **Inputs:** `value (bool)` - Whether to disable deposits.
    - **Outputs:** None
    - **Description:** This function disables or enables deposit functionality (guardian only).
13. `setIsWithdrawDisabled(bool value)`
    - **Type:** `nonpayable`
    - **Inputs:** `value (bool)` - Whether to disable withdrawals.
    - **Outputs:** None
    - **Description:** This function disables or enables withdrawal functionality (guardian only).
14. `setLendingConnector(ILendingConnector _lendingConnector)`
    - **Type:** `nonpayable`
    - **Inputs:** `_lendingConnector (ILendingConnector)` - The new lending connector address.
    - **Outputs:** None
    - **Description:** This function sets the lending protocol connector address (owner only).
15. `setOracleConnector(IOracleConnector _oracleConnector)`
    - **Type:** `nonpayable`
    - **Inputs:** `_oracleConnector (IOracleConnector)` - The new oracle connector address.
    - **Outputs:** None
    - **Description:** This function sets the oracle connector address (owner only).
16. `setVaultBalanceAsLendingConnector(address _vaultBalanceAsLendingConnector)`
    - **Type:** `nonpayable`
    - **Inputs:** `_vaultBalanceAsLendingConnector (address)` - The new vault balance as lending connector address.
    - **Outputs:** None
    - **Description:** This function sets the vault balance as lending connector address (owner only).
17. `deleverageAndWithdraw(uint256 closeAmountBorrow, uint256 deleverageFee)`
    - **Type:** `nonpayable`
    - **Inputs:**
        - `closeAmountBorrow (uint256)` - The amount of borrow assets to close.
        - `deleverageFee (uint256)` - The deleverage fee to charge.
    - **Outputs:** None
    - **Description:** This function performs emergency deleveraging and withdrawal of all assets (emergency deleverager only).
18. `updateEmergencyDeleverager(address newEmergencyDeleverager)`
    - **Type:** `nonpayable`
    - **Inputs:** `newEmergencyDeleverager (address)` - The new emergency deleverager address.
    - **Outputs:** None
    - **Description:** This function updates the emergency deleverager address (owner only).
19. `updateGovernor(address newGovernor)`
    - **Type:** `nonpayable`
    - **Inputs:** `newGovernor (address)` - The new governor address.
    - **Outputs:** None
    - **Description:** This function updates the governor address (owner only).
20. `updateGuardian(address newGuardian)`
    - **Type:** `nonpayable`
    - **Inputs:** `newGuardian (address)` - The new guardian address.
    - **Outputs:** None
    - **Description:** This function updates the guardian address (owner only).

## Events

### EIP-4626 Events

1. `Deposit(address indexed sender, address indexed owner, uint256 assets, uint256 shares)`
    - **Description:** This event is emitted when borrow assets are deposited and shares are minted.
    - **Parameters:**
        - `sender (address)` - The address that initiated the deposit.
        - `owner (address)` - The address that receives the minted shares.
        - `assets (uint256)` - The amount of borrow assets deposited.
        - `shares (uint256)` - The number of shares minted.
2. `Withdraw(address indexed sender, address indexed receiver, address indexed owner, uint256 assets, uint256 shares)`
    - **Description:** This event is emitted when borrow assets are withdrawn and shares are burned.
    - **Parameters:**
        - `sender (address)` - The address that initiated the withdrawal.
        - `receiver (address)` - The address that receives the withdrawn assets.
        - `owner (address)` - The address whose shares are burned.
        - `assets (uint256)` - The amount of borrow assets withdrawn.
        - `shares (uint256)` - The number of shares burned.
3. `DepositCollateral(address indexed sender, address indexed owner, uint256 collateralAssets, uint256 shares)`
    - **Description:** This event is emitted when collateral assets are deposited and shares are minted.
    - **Parameters:**
        - `sender (address)` - The address that initiated the deposit.
        - `owner (address)` - The address that receives the minted shares.
        - `collateralAssets (uint256)` - The amount of collateral assets deposited.
        - `shares (uint256)` - The number of shares minted.
4. `WithdrawCollateral(address indexed sender, address indexed receiver, address indexed owner, uint256 collateralAssets, uint256 shares)`
    - **Description:** This event is emitted when collateral assets are withdrawn and shares are burned.
    - **Parameters:**
        - `sender (address)` - The address that initiated the withdrawal.
        - `receiver (address)` - The address that receives the withdrawn assets.
        - `owner (address)` - The address whose shares are burned.
        - `collateralAssets (uint256)` - The amount of collateral assets withdrawn.
        - `shares (uint256)` - The number of shares burned.

### ERC-20 Events

1. `Approval(address indexed owner, address indexed spender, uint256 value)`
    - **Description:** This event is emitted when an approval is granted.
    - **Parameters:**
        - `owner (address)` - The token owner.
        - `spender (address)` - The approved spender.
        - `value (uint256)` - The approved amount.
2. `Transfer(address indexed from, address indexed to, uint256 value)`
    - **Description:** This event is emitted when tokens are transferred.
    - **Parameters:**
        - `from (address)` - The sender address.
        - `to (address)` - The recipient address.
        - `value (uint256)` - The transfer amount.

### Auction Events

1. `AuctionExecuted(address executor, int256 deltaRealCollateralAssets, int256 deltaRealBorrowAssets)`
    - **Description:** This event is emitted when an auction is executed.
    - **Parameters:**
        - `executor (address)` - The address that executed the auction.
        - `deltaRealCollateralAssets (int256)` - The change in real collateral assets.
        - `deltaRealBorrowAssets (int256)` - The change in real borrow assets.

### State Update Events

1. `StateUpdated(int256 oldFutureBorrowAssets, int256 oldFutureCollateralAssets, int256 oldFutureRewardBorrowAssets, int256 oldFutureRewardCollateralAssets, uint256 oldStartAuction, int256 newFutureBorrowAssets, int256 newFutureCollateralAssets, int256 newFutureRewardBorrowAssets, int256 newFutureRewardCollateralAssets, uint256 newStartAuction)`
    - **Description:** This event is emitted when the protocol state is updated.
    - **Parameters:**
        - `oldFutureBorrowAssets (int256)` - The previous future borrow assets.
        - `oldFutureCollateralAssets (int256)` - The previous future collateral assets.
        - `oldFutureRewardBorrowAssets (int256)` - The previous future reward borrow assets.
        - `oldFutureRewardCollateralAssets (int256)` - The previous future reward collateral assets.
        - `oldStartAuction (uint256)` - The previous auction start timestamp.
        - `newFutureBorrowAssets (int256)` - The new future borrow assets.
        - `newFutureCollateralAssets (int256)` - The new future collateral assets.
        - `newFutureRewardBorrowAssets (int256)` - The new future reward borrow assets.
        - `newFutureRewardCollateralAssets (int256)` - The new future reward collateral assets.
        - `newStartAuction (uint256)` - The new auction start timestamp.

### Administration Events

1. `TargetLTVChanged(uint128 oldValue, uint128 newValue)`
    - **Description:** This event is emitted when the target LTV is changed.
    - **Parameters:**
        - `oldValue (uint128)` - The previous target LTV value.
        - `newValue (uint128)` - The new target LTV value.
2. `MaxSafeLTVChanged(uint128 oldValue, uint128 newValue)`
    - **Description:** This event is emitted when the maximum safe LTV is changed.
    - **Parameters:**
        - `oldValue (uint128)` - The previous maximum safe LTV value.
        - `newValue (uint128)` - The new maximum safe LTV value.
3. `MinProfitLTVChanged(uint128 oldValue, uint128 newValue)`
    - **Description:** This event is emitted when the minimum profitable LTV is changed.
    - **Parameters:**
        - `oldValue (uint128)` - The previous minimum profitable LTV value.
        - `newValue (uint128)` - The new minimum profitable LTV value.
4. `FeeCollectorUpdated(address oldValue, address newValue)`
    - **Description:** This event is emitted when the fee collector address is updated.
    - **Parameters:**
        - `oldValue (address)` - The previous fee collector address.
        - `newValue (address)` - The new fee collector address.
5. `MaxTotalAssetsInUnderlyingChanged(uint256 oldValue, uint256 newValue)`
    - **Description:** This event is emitted when the maximum total assets in underlying is changed.
    - **Parameters:**
        - `oldValue (uint256)` - The previous maximum total assets value.
        - `newValue (uint256)` - The new maximum total assets value.
6. `MaxDeleverageFeeChanged(uint256 oldValue, uint256 newValue)`
    - **Description:** This event is emitted when the maximum deleverage fee is changed.
    - **Parameters:**
        - `oldValue (uint256)` - The previous maximum deleverage fee value.
        - `newValue (uint256)` - The new maximum deleverage fee value.
7. `MaxGrowthFeeChanged(uint256 oldValue, uint256 newValue)`
    - **Description:** This event is emitted when the maximum growth fee is changed.
    - **Parameters:**
        - `oldValue (uint256)` - The previous maximum growth fee value.
        - `newValue (uint256)` - The new maximum growth fee value.
8. `IsWhitelistActivatedChanged(bool oldValue, bool newValue)`
    - **Description:** This event is emitted when the whitelist activation status is changed.
    - **Parameters:**
        - `oldValue (bool)` - The previous whitelist activation status.
        - `newValue (bool)` - The new whitelist activation status.
9. `IsDepositDisabledChanged(bool oldValue, bool newValue)`
    - **Description:** This event is emitted when the deposit disabled status is changed.
    - **Parameters:**
        - `oldValue (bool)` - The previous deposit disabled status.
        - `newValue (bool)` - The new deposit disabled status.
10. `IsWithdrawDisabledChanged(bool oldValue, bool newValue)`
    - **Description:** This event is emitted when the withdraw disabled status is changed.
    - **Parameters:**
        - `oldValue (bool)` - The previous withdraw disabled status.
        - `newValue (bool)` - The new withdraw disabled status.
11. `LendingConnectorUpdated(address oldValue, address newValue)`
    - **Description:** This event is emitted when the lending connector address is updated.
    - **Parameters:**
        - `oldValue (address)` - The previous lending connector address.
        - `newValue (address)` - The new lending connector address.
12. `OracleConnectorUpdated(address oldValue, address newValue)`
    - **Description:** This event is emitted when the oracle connector address is updated.
    - **Parameters:**
        - `oldValue (address)` - The previous oracle connector address.
        - `newValue (address)` - The new oracle connector address.
13. `SlippageProviderUpdated(address oldValue, address newValue)`
    - **Description:** This event is emitted when the slippage provider address is updated.
    - **Parameters:**
        - `oldValue (address)` - The previous slippage provider address.
        - `newValue (address)` - The new slippage provider address.
14. `WhitelistRegistryUpdated(address oldValue, address newValue)`
    - **Description:** This event is emitted when the whitelist registry address is updated.
    - **Parameters:**
        - `oldValue (address)` - The previous whitelist registry address.
        - `newValue (address)` - The new whitelist registry address.
15. `VaultBalanceAsLendingConnectorUpdated(address oldValue, address newValue)`
    - **Description:** This event is emitted when the vault balance as lending connector address is updated.
    - **Parameters:**
        - `oldValue (address)` - The previous vault balance as lending connector address.
        - `newValue (address)` - The new vault balance as lending connector address.
16. `GovernorUpdated(address oldValue, address newValue)`
    - **Description:** This event is emitted when the governor address is updated.
    - **Parameters:**
        - `oldValue (address)` - The previous governor address.
        - `newValue (address)` - The new governor address.
17. `GuardianUpdated(address oldValue, address newValue)`
    - **Description:** This event is emitted when the guardian address is updated.
    - **Parameters:**
        - `oldValue (address)` - The previous guardian address.
        - `newValue (address)` - The new guardian address.
18. `EmergencyDeleveragerUpdated(address oldValue, address newValue)`
    - **Description:** This event is emitted when the emergency deleverager address is updated.
    - **Parameters:**
        - `oldValue (address)` - The previous emergency deleverager address.
        - `newValue (address)` - The new emergency deleverager address.

### Ownership Events

1. `OwnershipTransferred(address indexed previousOwner, address indexed newOwner)`
    - **Description:** This event is emitted when ownership is transferred.
    - **Parameters:**
        - `previousOwner (address)` - The previous owner address.
        - `newOwner (address)` - The new owner address.

## Connectors

### Read Connector Address

- `oracleConnector() → address` - Returns oracle connector address.
- `lendingConnector() → address` - Returns lending protocol connector address.

---

## EIP links

- [ERC4626](https://eips.ethereum.org/EIPS/eip-4626)
- [ERC20](https://eips.ethereum.org/EIPS/eip-20)
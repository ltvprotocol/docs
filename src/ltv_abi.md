# **Public ABI Description**

## **EIP-4626 (Vault Standard)**

### **4626 Read Methods**

- `asset() → address` - Returns the vault's underlying asset.
- `totalAssets() → uint256` — Returns the total amount of assets managed by the vault.
- `convertToShares(uint256 assets) → uint256` — Calculates how many `shares` correspond to `assets`.
- `convertToAssets(uint256 shares) → uint256` — Calculates how many `assets` correspond to `shares`.
- `maxDeposit(address receiver) → uint256` — Returns the maximum deposit amount allowed for `receiver`.
- `maxMint(address receiver) → uint256` — Returns the maximum number of `shares` that can be minted for `receiver`.
- `maxWithdraw(address owner) → uint256` — Returns the maximum amount of assets that can be withdrawn by `owner`.
- `maxRedeem(address owner) → uint256` — Returns the maximum number of `shares` that can be redeemed by `owner`.
- `previewDeposit(uint256 assets) → uint256` — Estimates how many `shares` will be received for `assets`.
- `previewMint(uint256 shares) → uint256` — Estimates how many `assets` are needed to mint `shares`.
- `previewWithdraw(uint256 assets) → uint256` — Estimates how many `shares` must be burned to withdraw `assets`.
- `previewRedeem(uint256 shares) → uint256` — Estimates how many `assets` will be received for `shares`.

### **4626 Write Methods**

- `deposit(uint256 assets, address receiver) → uint256` — Deposits `assets` and mints `shares` to `receiver`.
- `mint(uint256 shares, address receiver) → uint256` — Mints `shares` in exchange for `assets`.
- `withdraw(uint256 assets, address receiver, address owner) → uint256` — Withdraws `assets`, burning `shares` from `owner`.
- `redeem(uint256 shares, address receiver, address owner) → uint256` — Redeems `shares` and transfers `assets` to `receiver`.

### **4626 Events**

- `Deposit(address indexed sender, address indexed owner, uint256 assets, uint256 shares)`
- `Withdraw(address indexed sender, address indexed receiver, address indexed owner, uint256 assets, uint256 shares)`
---

## **EIP-4626 Collateral (Collateralized Vault Extension)**

### **Collateral Read Methods**

- `maxDepositCollateral(address receiver) → uint256` — Returns the max collateral deposit allowed for `receiver`.
- `maxMintCollateral(address receiver) → uint256` — Returns the max number of collateral `shares` that can be minted.
- `maxRedeemCollateral(address owner) → uint256` — Returns the max number of collateral `shares` that can be redeemed.
- `maxWithdrawCollateral(address owner) → uint256` — Returns the max amount of collateral that can be withdrawn.
- `previewDepositCollateral(uint256 collateralAssets) → uint256` — Estimates `shares` received for collateral deposit.
- `previewMintCollateral(uint256 shares) → uint256` — Estimates collateral assets needed to mint `shares`.
- `previewRedeemCollateral(uint256 shares) → uint256` — Estimates assets received for redeeming `shares`.
- `previewWithdrawCollateral(uint256 collateralAssets) → uint256` — Estimates `shares` burned for collateral withdrawal.

### **Collateral Write Methods**

- `depositCollateral(uint256 collateralAssets, address receiver) → uint256` — Deposits collateral and mints `shares` to `receiver`.
- `mintCollateral(uint256 shares, address receiver) → uint256` — Mints `shares` in exchange for collateral assets.
- `withdrawCollateral(uint256 collateralAssets, address receiver, address owner) → uint256` — Withdraws collateral assets, burning `shares` from `owner`.
- `redeemCollateral(uint256 shares, address receiver, address owner) → uint256` — Redeems `shares` for collateral assets.

### **Collateral Events**

- `DepositCollateral(address indexed sender, address indexed owner, uint256 collateralAssets, uint256 shares)`
- `WithdrawCollateral(address indexed sender, address indexed receiver, address indexed owner, uint256 collateralAssets, uint256 shares)`

---

## **ERC-20 Standard**

### **ERC-20 Read Methods**

- `balanceOf(address owner) → uint256` — Returns the token balance of `owner`.
- `totalSupply() → uint256` — Returns the total supply of tokens.
- `decimals() → uint8` — Returns the number of decimal places for the token.
- `name() → string` — Returns the token name.
- `symbol() → string` — Returns the token symbol.
- `allowance(address owner, address spender) → uint256` — Returns the remaining number of tokens that `spender` can use on behalf of `owner`.

### **ERC-20 Write Methods**

- `approve(address spender, uint256 amount) → bool` — Approves `spender` to use `amount` tokens.
- `transfer(address recipient, uint256 amount) → bool` — Transfers `amount` tokens to `recipient`.
- `transferFrom(address sender, address recipient, uint256 amount) → bool` — Transfers `amount` tokens from `sender` to `recipient`.

### **ERC-20 Events**

- `Approval(address indexed owner, address indexed spender, uint256 value)`
- `Transfer(address indexed from, address indexed to, uint256 value)`

---

## **Auction System**

### **Auction Read Methods**

- `startAuction() → uint256` — Returns the timestamp when the auction started.
- `previewExecuteAuctionBorrow(int256 deltaUserBorrowAssets) → int256` - estimates amount of borrow assets user needs to give/receive to receive/give `deltaUserBorrowAssets` amount of borrow assets
- `previewExecuteAuctionCollateral(int256 deltaUserCollateralAssets) → int256` - estimates amount of collateral assets user needs to give/receive to receive/give `deltaUserCollateralAssets` amount of collateral assets.

### **Auction Write Methods**

- `executeAuctionBorrow(int256 deltaUserBorrowAssets) → int256` — Executes an auction for borrow assets.
- `executeAuctionCollateral(int256 deltaUserCollateralAssets) → int256` — Executes an auction for collateral assets.

### **Auction Events**

- `AuctionExecuted(address indexed executor, int256 deltaRealCollateralAssets, int256 deltaRealBorrowAssets)`

---

## **State Representation**

### **State Read Methods**

- `borrowToken() → address` — Returns the borrow token contract address.
- `collateralToken() → address` — Returns the collateral token contract address.
- `futureBorrowAssets() → int256` — Returns current auction borrow assets.
- `futureCollateralAssets() → int256` — Returns current auction collateral assets.
- `futureRewardBorrowAssets() → int256` — Returns current auction reward in borrow assets.
- `futureRewardCollateralAssets() → int256` — Returns current auction reward in collateral assets.
- `getRealBorrowAssets() → uint256` — Returns protocol's debt in borrow assets.
- `getRealCollateralAssets() → uint256` — Returns protocol's collateral in collateral assets.
- `maxSafeLTV() → uint128` — Returns the maximum safe loan-to-value ratio.
- `minProfitLTV() → uint128` — Returns the minimum profitable LTV.
- `targetLTV() → uint128` — Returns the target loan-to-value ratio.
- `oracle() → address` - Returns oracle address for collateral and borrow tokens.
- `lendingProtocol() → address` - Returns lending protocol address.

## **State Write Methods**
- `setLendingProtocol(address lendingProtocol)` - sets new lending protocol address.
- `setOracle(address oracle)` - sets new oracle address.
- `setTargetLTV(uint128 value)` - sets protocol new target LTV.

### **State Events**

- `StateUpdated(int256 oldFutureBorrowAssets, int256 oldFutureCollateralAssets, int256 oldFutureRewardBorrowAssets, int256 oldFutureRewardCollateralAssets, uint256 oldStartAuction, int256 newFutureBorrowAssets, int256 newFutureCollateralAssets, int256 newFutureRewardBorrowAssets, int256 newFutureRewardCollateralAssets, uint256 newStartAuction)`
- `MaxSafeLTVChanged(uint128 oldValue, uint128 newValue)`
- `MinProfitLTVChanged(uint128 oldValue, uint128 newValue)`
- `TargetLTVChanged(uint128 oldValue, uint128 newValue)`

---

## **Ownership & Administration**

### **Ownership Read Methods**

- `owner() → address` — Returns the address of the current owner.

### **Ownership Write Methods**

- `transferOwnership(address newOwner)` — Transfers contract ownership to `newOwner`.
- `renounceOwnership()` — Renounces ownership of the contract.

### **Ownership Events**

- `OwnershipTransferred(address indexed previousOwner, address indexed newOwner)`

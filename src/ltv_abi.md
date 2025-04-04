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

## **Low Level Rebalance**

### **Low Level Rebalance Read**

- `previewLowLevelBorrow(int256 deltaBorrow) → (int256 deltaCollateral, int256 deltaShares)` - Preview low level rebalance function execution with input in borrow assets. 

  #### Input:
  deltaBorrow - amount of assets user wants to withdraw or deposit to the protocol in borrow assets(*deltaBorrow* < 0 to send borrow assets).

  #### Output:
  deltaCollateral - amount of assets user will receive or should provide for LTV protocol in collateral assets. If *deltaCollateral* < 0 - protocol will send *-deltaCollateral* collateral assets to the user. Otherwise, user will have *deltaCollateral* assets subtracted from his account after execution

  deltaShares - amount of shares user will receive or burn after execution. If *deltaShares* > 0 - user will receive *deltaShares* shares. Otherwise, *-deltaShares* shares will be burned.

- `previewLowLevelCollateral(int256 deltaCollateral) → (int256 deltaBorrow, int256 deltaShares)` - Preview low level rebalance function execution with input in collateral assets.

  #### Input:
  deltaCollateral - amount of assets user wants to withdraw or deposit to the protocol in collateral assets(*deltaCollateral* > 0 to send collateral assets).

  #### Output:
  deltaBorrow - amount of assets user will receive or should provide for LTV protocol in borrow assets. If *deltaBorrow* > 0 - protocol will send *deltaBorrow* borrow assets to the user. Otherwise, user will have *-deltaBorrow* assets subtracted from his account after execution

  deltaShares - amount of shares user will receive or burn after execution. If *deltaShares* > 0 - user will receive *deltaShares* shares. Otherwise, *-deltaShares* shares will be burned.

- `previewLowLevelShares(int256 deltaShares) → (int256 deltaCollateral, int256 deltaBorrow)` - Preview low level rebalance function execution with input in shares.

  #### Input:
  deltaShares - amount of shares user wants to mint or burn in the protocol (*deltaShares* > 0 for mint).

  #### Output:
  deltaCollateral - amount of assets user will receive or should provide for LTV protocol in collateral assets. If *deltaCollateral* < 0 - protocol will send *-deltaCollateral* collateral assets to the user. Otherwise, user will have *deltaCollateral* assets subtracted from his account after execution

  deltaBorrow - amount of assets user will receive or should provide for LTV protocol in borrow assets. If *deltaBorrow* > 0 - protocol will send *deltaBorrow* borrow assets to the user. Otherwise, user will have *-deltaBorrow* assets subtracted from his account after execution

- `previewLowLevelBorrowHint(int256 deltaBorrow, bool isSharesPositiveHint) → (int256 deltaCollateral, int256 deltaShares)` - Preview low level rebalance function execution with input in borrow assets with hint to avoid extra gas usage.

  #### Input:
  deltaBorrow - amount of assets user wants to withdraw or deposit to the protocol in borrow assets(*deltaBorrow* < 0 to send borrow assets).

  isSharesPositiveHint - should be true if user expects to mint shares. Can save gas spent on calculations.

  #### Output:
  deltaCollateral - amount of assets user will receive or should provide for LTV protocol in collateral assets. If *deltaCollateral* < 0 - protocol will send *-deltaCollateral* collateral assets to the user. Otherwise, user will have *deltaCollateral* assets subtracted from his account after execution

  deltaShares - amount of shares user will receive or burn after execution. If *deltaShares* > 0 - user will receive *deltaShares* shares. Otherwise, *-deltaShares* shares will be burned.

- `previewLowLevelCollateralHint(int256 deltaCollateral, bool isSharesPositiveHint) → (int256 deltaBorrow, int256 deltaShares)` - Preview low level rebalance function execution with input in collateral assets with hint to avoid extra gas usage.

  #### Input:
  deltaCollateral - amount of assets user wants to withdraw or deposit to the protocol in collateral assets(*deltaCollateral* > 0 to send collateral assets).
  
  isSharesPositiveHint - should be true if user expects to mint shares. Can save gas spent on calculations.

  #### Output:
  deltaBorrow - amount of assets user will receive or should provide for LTV protocol in borrow assets. If *deltaBorrow* > 0 - protocol will send *deltaBorrow* borrow assets to the user. Otherwise, user will have *-deltaBorrow* assets subtracted from his account after execution

  deltaShares - amount of shares user will receive or burn after execution. If *deltaShares* > 0 - user will receive *deltaShares* shares. Otherwise, *-deltaShares* shares will be burned.

### **Low Level Rebalance Write**

- `executeLowLevelBorrow(int256 deltaBorrow) → (int256 deltaCollateral, int256 deltaShares)` - Execute low level rebalance with input in borrow assets. 

  #### Input:
  deltaBorrow - amount of assets user wants to withdraw or deposit to the protocol in borrow assets(*deltaBorrow* < 0 to send borrow assets).

  #### Output:
  deltaCollateral - amount of assets user will receive or should provide for LTV protocol in collateral assets. If *deltaCollateral* < 0 - protocol will send *-deltaCollateral* collateral assets to the user. Otherwise, user will have *deltaCollateral* assets subtracted from his account after execution

  deltaShares - amount of shares user will receive or burn after execution. If *deltaShares* > 0 - user will receive *deltaShares* shares. Otherwise, *-deltaShares* shares will be burned.

- `executeLowLevelCollateral(int256 deltaCollateral) → (int256 deltaBorrow, int256 deltaShares)` - Execute low level rebalance with input in collateral assets.

  #### Input:
  deltaCollateral - amount of assets user wants to withdraw or deposit to the protocol in collateral assets(*deltaCollateral* > 0 to send collateral assets).

  #### Output:
  deltaBorrow - amount of assets user will receive or should provide for LTV protocol in borrow assets. If *deltaBorrow* > 0 - protocol will send *deltaBorrow* borrow assets to the user. Otherwise, user will have *-deltaBorrow* assets subtracted from his account after execution

  deltaShares - amount of shares user will receive or burn after execution. If *deltaShares* > 0 - user will receive *deltaShares* shares. Otherwise, *-deltaShares* shares will be burned.

- `executeLowLevelShares(int256 deltaShares) → (int256 deltaCollateral, int256 deltaBorrow)` - Execute low level rebalance with input in shares.

  #### Input:
  deltaShares - amount of shares user wants to mint or burn in the protocol (*deltaShares* > 0 for mint).

  #### Output:
  deltaCollateral - amount of assets user will receive or should provide for LTV protocol in collateral assets. If *deltaCollateral* < 0 - protocol will send *-deltaCollateral* collateral assets to the user. Otherwise, user will have *deltaCollateral* assets subtracted from his account after execution

  deltaBorrow - amount of assets user will receive or should provide for LTV protocol in borrow assets. If *deltaBorrow* > 0 - protocol will send *deltaBorrow* borrow assets to the user. Otherwise, user will have *-deltaBorrow* assets subtracted from his account after execution

- `executeLowLevelBorrowHint(int256 deltaBorrow, bool isSharesPositiveHint) → (int256 deltaCollateral, int256 deltaShares)` - Execute low level rebalance with input in borrow assets with hint to avoid extra gas usage.

  #### Input:
  deltaBorrow - amount of assets user wants to withdraw or deposit to the protocol in borrow assets(*deltaBorrow* < 0 to send borrow assets).

  isSharesPositiveHint - should be true if user expects to mint shares. Can save gas spent on calculations.

  #### Output:
  deltaCollateral - amount of assets user will receive or should provide for LTV protocol in collateral assets. If *deltaCollateral* < 0 - protocol will send *-deltaCollateral* collateral assets to the user. Otherwise, user will have *deltaCollateral* assets subtracted from his account after execution

  deltaShares - amount of shares user will receive or burn after execution. If *deltaShares* > 0 - user will receive *deltaShares* shares. Otherwise, *-deltaShares* shares will be burned.

- `executeLowLevelCollateralHint(int256 deltaCollateral, bool isSharesPositiveHint) → (int256 deltaBorrow, int256 deltaShares)` - Execute low level rebalance with input in collateral assets with hint to avoid extra gas usage.

  #### Input:
  deltaCollateral - amount of assets user wants to withdraw or deposit to the protocol in collateral assets(*deltaCollateral* > 0 to send collateral assets).
  
  isSharesPositiveHint - should be true if user expects to mint shares. Can save gas spent on calculations.

  #### Output:
  deltaBorrow - amount of assets user will receive or should provide for LTV protocol in borrow assets. If *deltaBorrow* > 0 - protocol will send *deltaBorrow* borrow assets to the user. Otherwise, user will have *-deltaBorrow* assets subtracted from his account after execution

  deltaShares - amount of shares user will receive or burn after execution. If *deltaShares* > 0 - user will receive *deltaShares* shares. Otherwise, *-deltaShares* shares will be burned.

## **State Representation**

### **State Read Methods**

- `borrowToken() → address` — Returns the borrow token contract address.
- `collateralToken() → address` — Returns the collateral token contract address.
- `futureBorrowAssets() → int256` — Returns current auction borrow assets.
- `futureCollateralAssets() → int256` — Returns current auction collateral assets.
- `futureRewardBorrowAssets() → int256` — Returns current auction reward in borrow assets.
- `futureRewardCollateralAssets() → int256` — Returns current auction reward in collateral assets.
- `getRealBorrowAssets() → uint256` — Returns protocol's current debt in lending protocol in borrow assets.
- `getRealCollateralAssets() → uint256` — Returns protocol's current collateral in lending protocol in collateral assets.
- `maxSafeLTV() → uint128` — Returns the maximum safe loan-to-value ratio.
- `maxTotalAssetsInUnderlying() → uint256` - protocol top border in underlying oracle assets.
- `minProfitLTV() → uint128` — Returns the minimum profitable LTV.
- `targetLTV() → uint128` — Returns the target loan-to-value ratio.
- `oracleConnector() → address` - Returns oracle connector address.
- `lendingConnector() → address` - Returns lending protocol connector address.
- `feeCollector() → address` - Returns fee collector address.

### **State Events**

- `StateUpdated(int256 oldFutureBorrowAssets, int256 oldFutureCollateralAssets, int256 oldFutureRewardBorrowAssets, int256 oldFutureRewardCollateralAssets, uint256 oldStartAuction, int256 newFutureBorrowAssets, int256 newFutureCollateralAssets, int256 newFutureRewardBorrowAssets, int256 newFutureRewardCollateralAssets, uint256 newStartAuction)`

---

## **Ownership**

### **Ownership Read Methods**

- `owner() → address` — Returns the address of the current owner.

### **Ownership Write Methods**

- `transferOwnership(address newOwner)` — Transfers contract ownership to `newOwner`.
- `renounceOwnership()` — Renounces ownership of the contract.

## Administration 

### **Administration Write Methods** 
- `setTargetLTV(uint128 value)` - Set protocol new target LTV.
- `setMaxSafeLTV(uint128 value)` - Set protocol new max safe LTV.
- `setMainProfitLTV(uint128 value)` - Set protocol new min profit LTV.
- `setFeeCollector(address _feeCollector)` - Set protocol new fee collector.
- `setLendingConnector(address _oracleConnector)` - Set lending protocol connector address. 
- `setMaxGrowthFee(uint256 fee)` - Set protocol max growth fee.
- `setMaxTotalAssetsInUnderlying(uint256 _maxTotalAssetsInUnderlying)` - Set protocol top border in underlying oracle assets.

### **Administration Events**
- `MaxSafeLTVChanged(uint128 oldValue, uint128 newValue)`
- `MinProfitLTVChanged(uint128 oldValue, uint128 newValue)`
- `TargetLTVChanged(uint128 oldValue, uint128 newValue)`


### **Ownership Events**

- `OwnershipTransferred(address indexed previousOwner, address indexed newOwner)`

## **Connectors**

### **Read Connector Address**

- `oracleConnector() → address` - Returns oracle connector address.
- `lendingConnector() → address` - Returns lending protocol connector address.

### **Write Connector Address**

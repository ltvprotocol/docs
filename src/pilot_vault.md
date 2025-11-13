# Pilot Vault - wstETH ↔ ETH Vault (AAVE v3)

The LTV Protocol's first production vault, the **wstETH (Lido) ↔ ETH Vault**, enables leveraged staking on Ethereum by combining Lido's liquid staking with AAVE v3's lending capabilities.

The wstETH ↔ ETH Vault is designed to maximize yield from Ethereum staking through leveraged positions while maintaining a constant target Loan-To-Value (LTV) ratio. 

## Vault Specifications

| Parameter | Value |
|-----------|-------|
| **Collateral Asset** | wstETH (Lido) |
| **Borrow Asset** | ETH |
| **Target Leverage** | 12x |
| **Target APY** | 4.3% (historical 2025 average) |
| **Lending Protocol** | AAVE v3 |
| **Network** | Ethereum Mainnet |

## Launch Process

The wstETH ↔ ETH Vault follows LTV Protocol’s [four-phase launch process](./vaults_launch.md): it begins with internal team capital to verify integrations and system stability, then expands to whitelisted private LPs to test scalability. Next, access opens to 42 NFT holders for controlled community participation. Finally, the vault becomes permissionless and public, with capacity scaling based on risk parameters and available AAVE v3 liquidity.

## Rate-Risk Model

Even at **maximum pool utilization**, AAVE v3’s ETH borrow rate peaks around **10.6% APR**.  
At that rate, starting from **12× leverage**, the vault would only approach liquidation after roughly **92 days** without intervention.  

This creates a significant safety buffer, giving sufficient time to rebalance, unwind, or deleverage before any critical thresholds are reached.  
The vault’s oracle setup further minimizes liquidation risk by using **wstETH’s redemption price** (not market spot), making it resistant to short-term depegs or volatility shocks.

## Stability and Backtesting

Historical simulations demonstrate consistent yield premiums and stability under changing rate conditions.  
Since early 2023, while base ETH staking yield averaged **~3.48% APY**, the same position with **x12 leverage on AAVE v3** maintained an average yield of **~9.23% APY** — roughly **2.6× higher** than baseline staking performance.  

This yield consistency across multiple market regimes validates the model’s resilience and sustainability as a foundation for subsequent vaults.  

![wstETH APY vs wstETH_WETH x12 AAVE v3 vault](wstETH_APY_vs_wstETH_WETH_x12_Aave_v3.png)

## Addresses

### Implementation

| Name | Address |
|------|---------|
| **LTV Beacon Proxy** | `0xa260b049ddd6567e739139404c7554435c456d9e` |

### Core Contracts

| Name | Address |
|------|---------|
| **LTV Implementation** | `0xd7ad059e339d12a1e8feb116da105d45b8ce2961` |
| **Beacon** | `0x2a53522265da2f3cc0be3d824b2f3c47a8fe1fc9` |
| **Beacon Proxy Admin** | `0x7f566425f807e6d2143759ac4a21883d209f6e45` |

### Modules

| Name | Address |
|------|---------|
| **ERC20 Module** | `0x2e8d2de14733d99bf85665eeb7e769bd8616114d` |
| **Borrow Vault Module** | `0xfedc676dbe1c6f7de17435b194c2fb9a8dd6d998` |
| **Collateral Vault Module** | `0x231b3774e5ffeb5dfbbe6644e871265d012c59d2` |
| **Low-Level Rebalance Module** | `0xcaa1e13cc2a5f654b944108d2e8a571d310c36f6` |
| **Auction Module** | `0x4ba3996075e7f7bc5580a123f73ae1d434e6c7bb` |
| **Administration Module** | `0x9cc3a99a5eabd37d986c00099bbcf9654b418a6e` |
| **Initialize Module** | `0x0b0581efe477fdc172196f1353775e34544da812` |
| **Modules Provider** | `0x986b9ff8bd6e2caf14b1a0efb3fffa593ad31af6` |

### Connectors

| Name | Address |
|------|---------|
| **Whitelist Registry** | `0x5ded41f6414a5c1575d91d716aa3b2b9836d46fd` |
| **Vault Balance Connector** | `0x48563844ea0e3218bca30d557487ff637ca915f4` |
| **Slippage Connector** | `0x4e4e690ed7b461387008276e427cab9d100514fa` |
| **Oracle Connector** | `0x3aeafb991094589b6d5f71ddf4ab77c3464bc9b2` |
| **Lending Connector** | `0x3233963016660814482e20b54181f5a96df4dc99` |

### Libraries

| Name | Address |
|------|---------|
| **DepositWithdraw** | `0x656e01a590b8AAda412aa31be2BfF8F91aD00A9A` |
| **MintRedeem** | `0x05d9639c930a9F41981B921daaD8d43Cc05d05A3` |
| **NextStep** | `0x1bc8f3fdC7Cc32df7601B10c03F2fA0717FAA75a` |
| **DeltaSharesAndDeltaRealBorrow** | `0x48cB8763318312E6836f37A66429f26fCd894E7b` |
| **DeltaRealBorrowAndDeltaRealCollateral** | `0x32c6494A272fB170C95D1855917231986e827838` |
| **DeltaSharesAndDeltaRealCollateral** | `0xB5dBb061862c13e51085162236da8ee1AB18387e` |
| **LowLevelRebalanceMath** | `0x9e9f13440Fdd9f30b8693fB04Da4dc0c763d85F1` |          
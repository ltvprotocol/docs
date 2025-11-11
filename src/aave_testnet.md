# Aave Sepolia Testnet

## List of Vaults

### AAVE LTV Vault

Main production vault for testing leveraged yield strategies with WETH and WBTC deposits on Aave V3 Sepolia.

Vault: [0xAa707a2a0e38DE55cA0b61dd32468e357b55b15f](https://sepolia.etherscan.io/address/0xAa707a2a0e38DE55cA0b61dd32468e357b55b15f)

### AAVE LTV Permissioned

Permissioned vault with active whitelist registry for controlled access testing and restricted user participation.

Vault: [0x94e6014565f790fee82890ee647f1855b910bd98](https://sepolia.etherscan.io/address/0x94e6014565f790fee82890ee647f1855b910bd98)

### AAVE LTV Kamikaze

Testing vault for soft liquidation and deleverage-and-withdraw functionality validation under extreme market conditions.

Vault: [0x685d1257671c9a0ec816e4d74b4912af26be98fb](https://sepolia.etherscan.io/address/0x685d1257671c9a0ec816e4d74b4912af26be98fb)

### AAVE LTV Upgradeable Beacon Proxy Vault

Upgradeable beacon proxy implementation of the vault.

Vault: [0xAa707a2a0e38DE55cA0b61dd32468e357b55b15f](https://sepolia.etherscan.io/address/0xAa707a2a0e38DE55cA0b61dd32468e357b55b15f)

## Description

The Aave LTV Vault is a testnet deployment of the LTV Protocol leveraging the Sepolia Aave V3 lending market. It allows users to deposit assets (WETH and WBTC) and earn yield through leveraged positions while maintaining a constant target Loan-To-Value (LTV) ratio. This testnet implementation serves as a sandbox to test LTV logic with Aave as the underlying lending system.

It follows the design principles outlined in the research paper:  
[LTV: Curatorless Leveraged Tokenized Vault with a Constant Target Loan-To-Value Ratio](https://github.com/ltvprotocol/papers/blob/main/LTV_Curatorless_Leveraged_Tokenized_Vault_with_a_Constant_Target_Loan-To-Value_Ratio.pdf)

## Components

### LTV Vault

An EIP-4626-based vault that automates leverage using recursive borrowing. It keeps the LTV ratio near a constant target using an auction incentive mechanism.

Source code: [LTV Vault](https://github.com/ltvprotocol/ltv_v0)

### Aave Lending Connector

This connector interfaces with Aave V3 to manage borrow/collateral positions within the vault.

# How to Use

1. Obtain ETH from the Sepolia Testnet faucets.
2. Transform ETH to WETH using the [WETH contract](https://sepolia.etherscan.io/address/0xC558DBdd856501FCd9aaF1E62eae57A9F0629a3c) or swap into [WBCT](https://sepolia.etherscan.io/address/0x29f2D40B0605204364af54EC677bD022dA425d03)
3. Deposit WETH into the [LTV Vault](https://sepolia.etherscan.io/address/0xAa707a2a0e38DE55cA0b61dd32468e357b55b15f).
4. Wait for the yield generation period to end.
5. Withdraw your WETH or MagicETH from the [LTV Vault](https://sepolia.etherscan.io/address/0xAa707a2a0e38DE55cA0b61dd32468e357b55b15f).

# Pico UI

A minimal and lightweight frontend for interacting with the ghost [testnet vaults](https://testnet.ltv.finance).

Source code: [https://github.com/ltvprotocol/pico_ui](https://github.com/ltvprotocol/pico_ui)
Detailed documentation: [Pico UI Docs](./pico_ui.md)

# Aave Sepolia Testnet Addresses

## v0.1.11

Actual (v0.1.11) tag: [testnet_v0.1.10](https://github.com/ltvprotocol/ltv_v0/releases/tag/testnet_v0.1.11)

### Implementation

| Name | Address |
|------|---------|
| **AAVE LTV Upgradeable Beacon Proxy Vault** | `0xAa707a2a0e38DE55cA0b61dd32468e357b55b15f` |

## v0.1.10

Actual (v0.1.10) tag: [testnet_v0.1.10](https://github.com/ltvprotocol/ltv_v0/releases/tag/testnet_v0.1.10)

### Implementation

| Name | Address |
|------|---------|
| **AAVE LTV Vault** | `0xAa707a2a0e38DE55cA0b61dd32468e357b55b15f` |
| **AAVE LTV Permissioned** | `0x94e6014565f790fee82890ee647f1855b910bd98` |
| **AAVE LTV Kamikaze** | `0x685d1257671c9a0ec816e4d74b4912af26be98fb` |

### Core Contracts

| Name | Address |
|------|---------|
| **Implementation** | `0x480d554D8BBF66d301063CccbA7847f26BD0293f` |
| **Beacon** | `0xfE8f068568Cc84c3818406765da285e7f57B8A80` |

### Modules

| Name | Address |
|------|---------|
| **ERC20 Module** | `0x9fb3133ebb390bc1d46d555b06f2e115ee919f17` |
| **Borrow Vault Module** | `0x1ece1d0c024526995696f9fd321225130700e8d6` |
| **Collateral Vault Module** | `0x1ed1573f1c756ad9b915f5b804a306b62a140c1d` |
| **Low-Level Rebalance Module** | `0xf8cbf81bbb5efc1fac45f8d2f1e99c06c7b69d86` |
| **Auction Module** | `0xec7d68652654c297c5fce832e7445b9158cdb3aa` |
| **Administration Module** | `0x6dc2cb8fdc1e8d8c8dd6e4244882f3ada2b7c33e` |
| **Initialize Module** | `0x29cefdfefd5b24c14bf2a4c7e464d1dcc4fce206` |
| **Modules Provider** | `0xc7349994eb73ba9090980dfc8628849fda4d5a29` |

### Connectors

| Name | Address |
|------|---------|
| **Whitelist Registry** | `0x69dafafca68306c5b664a8bf23196da9350edc59` |
| **Vault Balance Connector** | `0xf3aaab0591e74f9d509296906366704e9c3f6c2a` |
| **Slippage Connector** | `0xd64de8418ee7ef911175a8bd8b4bf4facd72796f` |
| **Oracle Connector** | `0xa19d6038a1d3b2c86b2ba98bf98cc39273644ec1` |
| **Lending Connector** | `0x433d9aa49dd184863ac818ff8aa359047510dc30` |

### Libraries

| Name | Address |
|------|---------|
| **DepositWithdraw** | `0xA3FbBdA4dbAe3e5d9138dBF0055d1eA5327624Ad` |
| **MintRedeem** | `0x676384D764e33032E228823DFAE10dEce86c24E1` |
| **NextStep** | `0x2b90Ed4B9b8B91D57612ef6F8121368fa8d862bA` |
| **DeltaSharesAndDeltaRealBorrow** | `0x8f806DE6E7859164660AB64d84d50449c3Aa4d51` |
| **DeltaRealBorrowAndDeltaRealCollateral** | `0xF1A532acb2e9cc03c3911bEc718fca71c257EB83` |
| **DeltaSharesAndDeltaRealCollateral** | `0x7A6A06bfeddA3c38536Cef100091209BD9F2417E` |
| **LowLevelRebalanceMath** | `0xAdEaD04C1595BF3b091e2796299beD8F59Ad15BC` |

## v0.1.3

### Implementation

https://github.com/ltvprotocol/ltv_v0/releases/tag/testnet_aave_v0.1.3

| Name | Address |
|------|---------|
| **AAVE LTV Vault** | `0xAa707a2a0e38DE55cA0b61dd32468e357b55b15f` |

### Core Contracts

| Name | Address |
|------|---------|
| **Implementation** | `0x850381Ee6363e05630692cEF49e815B320F72B8c` |
| **Beacon** | `0xfE8f068568Cc84c3818406765da285e7f57B8A80` |

### Modules

| Name | Address |
|------|---------|
| **ERC20 Module** | `0x93889Ffe70ec6F772458b8D2e40C0377266F6Bef` |
| **Borrow Vault Module** | `0x64d5acf4db65ae30723b96e5fb455bc2c66f9798` |
| **Collateral Vault Module** | `0xe1F7EC39C820f10B9C114202FACA82c63fC131eB` |
| **Low-Level Rebalance Module** | `0x6cb72186a8F7a305F4262af19BD0a2b6b80594DF` |
| **Auction Module** | `0x9bDF7EBf76741D3948116e39a883693D64Dc9fd5` |
| **Administration Module** | `0xcE5BCE6d3C3f983F4ca61b5e4efeE8e225a7D9c2` |
| **Initialize Module** | `0x599484fda8E1e3d69ca480AcFc77C2Ea36D0f0Bb` |
| **Modules Provider** | `0xd843c8d41a2c78B24af7155Ea407940a002604d9` |

### Connectors

| Name | Address |
|------|---------|
| **Whitelist Registry** | `0x0000000000000000000000000000000000000000` |
| **Vault Balance Connector** | `0xA9D5d9b37278E5440bFbC31c0841ae77143aE943` |
| **Slippage Connector** | `0x96e905b396546b01C7C8c6DaFaD08e694C3d963C` |
| **Oracle Connector** | `0xEcB5c35E49Ca057894e088B8Cf27C7710A0ee1fa` |
| **Lending Connector (Aave)** | `0x87e1d99D8Af73a7DB9d80827076A283E17a1f431` |

### Token Addresses (Aave Sepolia Testnet)

| Name | Address |
|------|---------|
| **Borrow Token** | `0x29f2D40B0605204364af54EC677bD022dA425d03` |
| **Collateral Token** | `0xC558DBdd856501FCd9aaF1E62eae57A9F0629a3c` |
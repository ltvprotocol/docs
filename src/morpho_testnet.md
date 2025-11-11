# Morpho Sepolia Testnet

The Morpho LTV Vault is a testnet deployment of the LTV Protocol leveraging the Sepolia Morpho lending market. It allows users to deposit assets (WETH and USDC) and earn yield through leveraged positions while maintaining a constant target Loan-To-Value (LTV) ratio. This testnet implementation serves as a sandbox to test LTV logic with Morpho as the underlying lending system.

It follows the design principles outlined in the research paper:
[LTV: Curatorless Leveraged Tokenized Vault with a Constant Target Loan-To-Value Ratio](https://github.com/ltvprotocol/papers/blob/main/LTV_Curatorless_Leveraged_Tokenized_Vault_with_a_Constant_Target_Loan-To-Value_Ratio.pdf)

## Components

### LTV Vault

An EIP-4626-based vault that automates leverage using recursive borrowing. It keeps the LTV ratio near a constant target using an auction incentive mechanism.

Source code: [LTV Vault](https://github.com/ltvprotocol/ltv_v0)

### Morpho Lending Connector

This connector interfaces with Morpho to manage borrow/collateral positions within the vault.

# How to Use

1. Obtain ETH from the Sepolia Testnet faucets.
2. Transform ETH to WETH using the [WETH contract](https://sepolia.etherscan.io/address/0x2D5ee574e710219a521449679A4A7f2B43f046ad) or swap into [USDC](https://sepolia.etherscan.io/address/0x1c7D4B196Cb0C7B01d743Fbc6116a902379C7238)
3. Deposit WETH into the [LTV Vault](https://sepolia.etherscan.io/address/0x9A1Fc3ff25083f33373Bbf9617E12892FF19E07A).
4. Wait for the yield generation period to end.
5. Withdraw your WETH or MagicETH from the [LTV Vault](https://sepolia.etherscan.io/address/0x9A1Fc3ff25083f33373Bbf9617E12892FF19E07A).

# Pico UI

A minimal and lightweight frontend for interacting with the ghost [testnet vaults](https://testnet.ltv.finance).

Source code: [https://github.com/ltvprotocol/pico_ui](https://github.com/ltvprotocol/pico_ui)
Detailed documentation: [Pico UI Docs](./pico_ui.md)

# Moroho Sepolia Testnet Addresses

| Name | Address |
|------|---------|
| **Morpho LTV Vault** | `0x9A1Fc3ff25083f33373Bbf9617E12892FF19E07A` |

## v0.1.10

Actual (v0.1.10) tag: [testnet_v0.1.10](https://github.com/ltvprotocol/ltv_v0/releases/tag/testnet_v0.1.10)

### Core Contracts

| Name | Address |
|------|---------|
| **Implementation** | `0x480d554D8BBF66d301063CccbA7847f26BD0293f` |
| **Beacon** | `0xfE8f068568Cc84c3818406765da285e7f57B8A80` |

### Modules

| Name | Address |
|------|---------|
| **ERC20 Module** | `0x9FB3133Ebb390bc1d46d555B06F2E115Ee919F17` |
| **Borrow Vault Module** | `0x1ECE1D0C024526995696F9FD321225130700e8D6` |
| **Collateral Vault Module** | `0x1ED1573f1C756aD9b915f5B804a306b62a140c1D` |
| **Low-Level Rebalance Module** | `0xF8cBf81bbb5EFc1faC45F8d2F1E99C06C7b69D86` |
| **Auction Module** | `0xeC7d68652654c297C5FCE832E7445b9158CDB3aA` |
| **Administration Module** | `0x6DC2cb8fdc1e8D8c8DD6E4244882f3ADA2B7C33E` |
| **Initialize Module** | `0x29ceFDfefd5b24C14Bf2A4c7E464D1dCC4fce206` |
| **Modules Provider** | `0xC7349994eb73Ba9090980DFc8628849fDA4d5A29` |

### Connectors

| Name | Address |
|------|---------|
| **Whitelist Registry** | `0x69DafAfcA68306c5B664A8BF23196DA9350edC59` |
| **Vault Balance Connector** | `0xf3AaaB0591e74F9D509296906366704e9C3f6c2A` |
| **Slippage Connector** | `0xd64De8418ee7EF911175A8bd8B4Bf4FAcD72796F` |
| **Oracle Connector** | `0x1a0b3f2f3823582871c05634d4874d26ad29542c` |
| **Lending Connector** | `0xb241c66c61adb67cd261e71f425bc38cff6f00a4` |

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

## v0.1.4

Actual (v0.1.4) tag: [morpho_testnet_v0.1.4](https://github.com/ltvprotocol/ltv_v0/releases/tag/morpho_testnet_v0.1.4)

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
| **Oracle Connector** | `0xAdF9c5C3e09Ad8F35eAf96c7072Eca84A18b5AE7` |
| **Lending Connector (Morpho)** | `0x2F7E5B3f16120363E9d6C6a46744D3a90D426CB0` |

### Token Addresses (Aave Sepolia Testnet)

| Name | Address |
|------|---------|
| **Borrow Token** | `0x1c7D4B196Cb0C7B01d743Fbc6116a902379C7238` |
| **Collateral Token** | `0x2D5ee574e710219a521449679A4A7f2B43f046ad ` |

# Versions

## Fix oracle connector after initial deploy

| Name | Address |
|------|---------|
| **Oracle Connector** | `0x075ECcc98436AB3C0713Fc25C6e88ac56DdE8b16` |

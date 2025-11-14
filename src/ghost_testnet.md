# Ghost testnet

The LTV Vault is an implementation of an EIP-4626 vault designed for leveraging WETH (Wrapped ETH) deposits to generate yield over time. Users can deposit WETH, hold for a few months, and earn more WETH through a leveraged strategy utilizing the Magic ETH (MAE) token.

The vault borrows WETH and use MagicETH as collateral in the HodlMyBeer Lending protocol and follows the design principles outlined in the research paper: [LTV: Curatorless Leveraged Tokenized Vault with a Constant Target Loan-To-Value Ratio](https://github.com/ltvprotocol/papers/blob/main/LTV_Curatorless_Leveraged_Tokenized_Vault_with_a_Constant_Target_Loan-To-Value_Ratio.pdf)

# Components

## LTV Vault

The LTV Vault is a smart contract that accepts WETH and MagicETH deposits. The vault borrows WETH and keeps MagicETH tokens as collateral in target LTV ratio in HodlMyBeer Lending Protocol. Users can deposit WETH and MagicETH into the vault and receive more WETH and MagicETH after a certain period.

Source code: [LTV Vault](https://github.com/ltvprotocol/ltv_v0)

Ghost Testnet branch: [ghost-testnet](https://github.com/ltvprotocol/ltv_v0/tree/ghost)

Actual (v0.1.1) tag: [ghost_v0.1.1](https://github.com/ltvprotocol/ltv_v0/releases/tag/ghost_v0.1.1)

## WETH (Wrapped Testnet ETH)

WETH is the wrapped version of ETH available on the Sepolia Testnet. Users can obtain testnet ETH from various faucets, such as:

- [Alchemy Faucet](https://www.alchemy.com/faucets/ethereum-sepolia)
- [Infura Faucet](https://www.infura.io/zh/faucet/sepolia)
- [Google Cloud Faucet](https://cloud.google.com/application/web3/faucet/ethereum/sepolia)
- [QuickNode Faucet](https://faucet.quicknode.com/ethereum/sepolia)
- [GetBlock Faucet](https://getblock.io/faucet/eth-sepolia/)

User can tranform Sepolia ETH to WETH using the [WETH contract](https://sepolia.etherscan.io/address/0xfFf9976782d46CC05630D1f6eBAb18b2324d6B14#writeContract)

## Magic ETH (MAE)

Magic ETH (MAE) is a testnet asset that mimics real-world Liquid Staking Tokens (LSTs) such as Rocket Pool ETH (rETH) or Lido Staked ETH (stETH). MAE accrues value over time, making it an effective asset for yield strategies.

Source code: [Magic ETH](https://github.com/ltvprotocol/ltv_v0/blob/ghost/src/ghost/magic/MagicETH.sol)

## Spooky Oracle

The Spooky Oracle provides the real-time redemption price of Magic ETH (MAE) and feeds this data into the LTV Vault. It is maintained by the LTV Protocol Team to ensure accurate pricing for yield calculations and leverage adjustments.

Source code: [Spooky Oracle](https://github.com/ltvprotocol/ltv_v0/blob/ghost/src/ghost/spooky/SpookyOracle.sol)

## HodlMyBeer Lending Protocol

The HodlMyBeer Lending Protocol is a dummy lending system that supports an isolated WETH-to-MAE pool. The LTV Vault interacts with this protocol to borrow WETH and provide MagicETH as collateral and execute leveraged yield strategies.

Source code: [HodlMyBeer Lending Protocol](https://github.com/ltvprotocol/ltv_v0/blob/ghost/src/ghost/hodlmybeer/HodlMyBeerLending.sol)

# How to Use

1. Obtain ETH from the Sepolia Testnet faucets.
2. Transform ETH to WETH using the [WETH contract](https://sepolia.etherscan.io/address/0xfFf9976782d46CC05630D1f6eBAb18b2324d6B14#writeContract).
3. Deposit WETH into the [LTV Vault](https://sepolia.etherscan.io/address/0xe2a7f267124ac3e4131f27b9159c78c521a44f3c#writeProxyContract).
4. Wait for the yield generation period to end.
5. Withdraw your WETH or MagicETH from the [LTV Vault](https://sepolia.etherscan.io/address/0xe2a7f267124ac3e4131f27b9159c78c521a44f3c#writeProxyContract).

# Pico UI

A minimal and lightweight frontend for interacting with the ghost [testnet vaults](https://testnet.ltv.finance).

Source code: [https://github.com/ltvprotocol/pico_ui](https://github.com/ltvprotocol/pico_ui)
Detailed documentation: [Pico UI Docs](./pico_ui.md)

# Contract Addresses

| Name           | Source Code | Address |
|--------------|-------------|---------|
| **LTV Vault** | [Source code](https://github.com/ltvprotocol/ltv_v0/blob/ghost/src/ltv_lendings/GhostLTV.sol) | [0xe2a7f267124ac3e4131f27b9159c78c521a44f3c](https://sepolia.etherscan.io/address/0xe2a7f267124ac3e4131f27b9159c78c521a44f3c) | 
| **Magic ETH** | [Source code](https://github.com/ltvprotocol/ltv_v0/blob/ghost/src/ghost/magic/MagicETH.sol) | [0x8f7b2044f9aa6fbf495c1cc3bde120df9032ae43](https://sepolia.etherscan.io/address/0x8f7b2044f9aa6fbf495c1cc3bde120df9032ae43#code) 
| **Spooky Oracle** | [Source code](https://github.com/ltvprotocol/ltv_v0/blob/ghost/src/ghost/spooky/SpookyOracle.sol) | [0x6074d1d4022521147db1fad7bacc486b35a64df3](https://sepolia.etherscan.io/address/0x6074d1d4022521147db1fad7bacc486b35a64df3)
| **HodlMyBeer Lending Protocol** | [Source code](https://github.com/ltvprotocol/ltv_v0/blob/ghost/src/ghost/hodlmybeer/HodlMyBeerLending.sol) | [0x1dcd756db287354c4607d5d57621cdfb4456e2d4](https://sepolia.etherscan.io/address/0x1dcd756db287354c4607d5d57621cdfb4456e2d4#readProxyContract) 

# Versions

## v0.1.10

Actual (v0.1.10) tag: [testnet_v0.1.10](https://github.com/ltvprotocol/ltv_v0/releases/tag/testnet_v0.1.10)

### Core Contracts

| Name | Address |
|------|---------|
| **Implementation** | `0x480d554d8bbf66d301063cccba7847f26bd0293f` |

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

## v0.1.5

Actual (v0.1.5) tag: [ghost_v0.1.5](https://github.com/ltvprotocol/ltv_v0/releases/tag/ghost_v0.1.5)

### Core Contracts

| Name | Address |
|------|---------|
| **Implementation** | `0x850381Ee6363e05630692cEF49e815B320F72B8c` |

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
| **Oracle Connector** | `0x79928E57C139BbDa14AEbcBe568d9a4B17bBcA00` |
| **Lending Connector** | `0x899645f1AF07511e112f737027BCF13F122aa5A6` |

## v0.1.1
Previous (v0.1.1) tag: [ghost_v0.1.1](https://github.com/ltvprotocol/ltv_v0/releases/tag/ghost_v0.1.1)

Deployed contract implementation: [0x95fe6151ad668984b1c7b193866302b41b90887a](https://sepolia.etherscan.io/address/0x95fe6151ad668984b1c7b193866302b41b90887a)

## v0.1.0
Previous (v0.1.0) tag: [ghost_v0.1.0](https://github.com/ltvprotocol/ltv_v0/releases/tag/ghost_v0.1.0)

Deployed contract implementation: [0xeb81c4fe3d8e3c146a25bf9ea2309977071b6490](https://sepolia.etherscan.io/address/0xeb81c4fe3d8e3c146a25bf9ea2309977071b6490)

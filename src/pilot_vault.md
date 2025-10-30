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

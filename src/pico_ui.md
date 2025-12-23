# Pico UI

A minimal and lightweight frontend for interacting with the LTV Protocol testnet vaults.

**Pico UI** is a simple user interface designed to interact with vaults. It gives access to all main actions: deposit, mint, withdraw, redeem for borrow and collateral tokens. Also it shows balances and main vault information, creates possibility to convenient and simple interact with vaults.

## Capabilities

- Connect with any EIP-6963 compatible browser wallet (e.g., MetaMask, Trust Wallet, Rainbow, and others)
- Deposit assets or mint shares with borrow/collateral tokens
- Withdraw assets or redeem shares with borrow/collateral tokens
- View real-time vault and transaction statuses

## Deployed Testnet Version

[https://testnet.ltv.finance](https://testnet.ltv.finance)


## Release Versions

- **v0.0.14** — Improved position visibility with leveraged token amounts, USD max values, new APY API, and quick action buttons (25%/50%/75%).  
  [View Release](https://github.com/ltvprotocol/pico_ui/releases/tag/v0.0.14)

- **v0.0.13** — UX fixes including better points formatting, disabled actions on invalid inputs, and cleared persistent errors.  
  [View Release](https://github.com/ltvprotocol/pico_ui/releases/tag/v0.0.13)

- **v0.0.12** — Hotfix for button disable logic blocking valid transactions (building on v0.0.11 features).  
  [View Release](https://github.com/ltvprotocol/pico_ui/releases/tag/v0.0.12)

- **v0.0.11** — Points system integration and Mainnet 42 NFT snapshot completion.  
  [View Release](https://github.com/ltvprotocol/pico_ui/releases/tag/v0.0.11)

- **v0.0.10** — Cleaner UX, simpler navigation, and improved flash-loan interactions.  
  [View Release](https://github.com/ltvprotocol/pico_ui/releases/tag/v0.0.10)

- **v0.0.9** — React versions update and deployment improvements.  
  [View Release](https://github.com/ltvprotocol/pico_ui/releases/tag/v0.0.9)

- **v0.0.8** — Mainnet vault addition, full wstETH support, flash-loan mint/redeem, and domain migration to app.ltv.finance.  
  [View Release](https://github.com/ltvprotocol/pico_ui/releases/tag/v0.0.8)

- **v0.0.7** — Major upgrade with bug fixes, multi-network support, low-level vault functions, direct auction execution, and smoother design.  
  [View Release](https://github.com/ltvprotocol/pico_ui/releases/tag/v0.0.7)

- **v0.0.6** — Fixes for rounding/max values/minor UI bugs, plus major refactor for speed and stability.  
  [View Release](https://github.com/ltvprotocol/pico_ui/releases/tag/v0.0.6)

- **v0.0.5** — Fix for clearer messaging when no wallets are detected.  
  [View Release](https://github.com/ltvprotocol/pico_ui/releases/tag/v0.0.5)

- **v0.0.4** — Multi-vault interface and auto-reconnect to wallet.  
  [View Release](https://github.com/ltvprotocol/pico_ui/releases/tag/v0.0.4)

- **v0.0.3** - Fixes wallet duplication/loading & Sepolia switching issues
  [View Release](https://github.com/ltvprotocol/pico_ui/releases/tag/v0.0.3)

- **v0.0.2** — Early testnet release with basic vault actions and initial UI improvements.  
  [View Release](https://github.com/ltvprotocol/pico_ui/releases/tag/v0.0.2)

- **v0.0.1** — Initial stable release with basic vault interaction and wallet connection support.  
  [View Release](https://github.com/ltvprotocol/pico_ui/releases/tag/v0.0.1)

## Prerequisites

- Node.js (v20 or later)

## Setup

1. Clone the repository:
  ```bash
   git clone https://github.com/ltvprotocol/pico_ui.git
   ```
2. Go to directory:
  ```bash
   cd pico_ui
   ```
3. Install dependencies:
  ```bash
   npm install
   ```

## Development

Start the development server:
```bash
npm run dev
```

The application will be available at `http://localhost:5173`

## Building for Production

Build the application:
```bash
npm run build
```

The built files will be in the `dist` directory.
You can deploy it to any static hosting provider (e.g., Vercel, Netlify, Cloudflare Pages).

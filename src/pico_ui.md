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

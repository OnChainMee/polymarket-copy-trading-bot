# Polymarket Copy Trading Bot

Automated copy-trading for [Polymarket](https://polymarket.com). Monitor a chosen trader and mirror their prediction market orders in real time.

## Table of Contents

- [Features](#features)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Configuration](#configuration)
- [Usage](#usage)
- [Geographic Restrictions](#geographic-restrictions)
- [Contributing](#contributing)
- [License](#license)
- [Contact](#contact)

## Features

- **Automated copy trading** — Mirror orders from a selected Polymarket trader as they place them.
- **Real-time monitoring** — Poll the CLOB for the target wallet’s activity at a configurable interval.
- **Configurable behavior** — Set fetch interval, order age cutoff, and retry limits via environment variables.
- **MongoDB-backed history** — Track copied trades for your own records.

## Prerequisites

- **Node.js** 18+ and npm
- **MongoDB** — Local instance or [MongoDB Atlas](https://www.mongodb.com/cloud/atlas)
- **Polygon RPC** — e.g. [Infura](https://infura.io), [Alchemy](https://www.alchemy.com), or another provider
- **Wallets** — The Polymarket wallet you want to copy from, and the wallet that will execute copies (with USDC on Polygon)

## Installation

1. Clone the repository and go to the project directory:

   ```bash
   git clone https://github.com/OnChainMee/polymarket-copy-trading-bot.git
   cd polymarket-copy-trading-bot

   ```

2. Install dependencies:

   ```bash
   npm install
   ```

3. Create a `.env` file in the project root (see [Configuration](#configuration)).

4. Build and run:

   ```bash
   npm run build
   npm start
   ```

For development with hot reload:

```bash
npm run dev
```

## Configuration

Create a `.env` file in the project root with the following variables. Do **not** commit `.env` or share keys.

| Variable | Required | Description | Example / default |
|----------|----------|-------------|-------------------|
| `USER_ADDRESS` | Yes | Polymarket wallet address to copy | `0x...` |
| `PROXY_WALLET` | Yes | Your Polymarket wallet (executes copies) | `0x...` |
| `PRIVATE_KEY` | Yes | Private key for `PROXY_WALLET` (no `0x` prefix) | — |
| `CLOB_HTTP_URL` | Yes | Polymarket CLOB API base URL | `https://clob.polymarket.com/` |
| `CLOB_WS_URL` | Yes | Polymarket CLOB WebSocket URL | `wss://ws-subscriptions-clob.polymarket.com/ws` |
| `MONGO_URI` | Yes | MongoDB connection string | `mongodb+srv://user:pass@cluster.mongodb.net/dbname` |
| `RPC_URL` | Yes | Polygon mainnet RPC URL | `https://polygon-mainnet.g.alchemy.com/v2/YOUR_KEY` |
| `USDC_CONTRACT_ADDRESS` | Yes | USDC on Polygon | `0x2791Bca1f2de4661ED88A30C99A7a9449Aa84174` |
| `FETCH_INTERVAL` | No | Poll interval in seconds | `1` |
| `TOO_OLD_TIMESTAMP` | No | Ignore orders older than this (hours) | `24` |
| `RETRY_LIMIT` | No | Max retries per operation | `3` |

Example `.env` (replace placeholders; never commit real keys):

```env
USER_ADDRESS=0xTargetTraderAddress
PROXY_WALLET=0xYourPolymarketWallet
PRIVATE_KEY=your_private_key_no_0x_prefix

CLOB_HTTP_URL=https://clob.polymarket.com/
CLOB_WS_URL=wss://ws-subscriptions-clob.polymarket.com/ws

MONGO_URI=mongodb+srv://user:password@cluster.mongodb.net/your_db
RPC_URL=https://polygon-mainnet.g.alchemy.com/v2/YOUR_ALCHEMY_KEY
USDC_CONTRACT_ADDRESS=0x2791Bca1f2de4661ED88A30C99A7a9449Aa84174

FETCH_INTERVAL=1
TOO_OLD_TIMESTAMP=24
RETRY_LIMIT=3
```

## Usage

- **Production:** `npm run build` then `npm start`
- **Development:** `npm run dev` (runs with `ts-node`)

On startup the bot will connect to MongoDB, then start monitoring `USER_ADDRESS` and executing copies on `PROXY_WALLET`. Ensure the proxy wallet has USDC on Polygon for placing orders.

## Geographic Restrictions

Polymarket may restrict access from some regions. If you run into IP-based blocks:

- Run the bot from a supported region (e.g. VPS in the Netherlands or another allowed location).
- Use a low-latency provider close to Polymarket’s infrastructure if you care about execution speed.

This project does not endorse any specific VPS provider. Choose a host that meets your legal and performance needs.

## Contributing

Contributions are welcome. Please open an issue to discuss larger changes, then submit a pull request. If you find the project useful, consider giving it a star.

## License

ISC — see [package.json](package.json) for details.

## Contact

For questions or updates: [Telegram](https://t.me/OnChainMee).

# Polymarket Trading Bot (YES/NO Risk-Free Arbitrage)

A professional Python-based trading bot for Polymarket with full credential management, order execution, market analysis, and automated arbitrage trading capabilities.

This repository is optimized for developers, quantitative traders, and researchers looking to understand or deploy **Polymarket arbitrage strategies** with configurable thresholds, fast market scanning, and clean architecture.

---

## 🔍 What Is Polymarket Arbitrage?

On Polymarket, each binary market resolves to **$1.00** for the correct outcome.  
If **YES price + NO price < $1.00**, both sides can be bought to guarantee profit.

### Example
- YES: $0.48  
- NO: $0.49  
- Total cost: $0.97  
- Payout on resolution: $1.00  
- **Guaranteed profit: $0.03 per share**

---

## 🚀 Key Features

- ⚡ Real-time market monitoring (Socket + polling)
- 🔎 Automatic detection of YES/NO mispricing
- 🧮 Profit & position tracking
- 🧠 Configurable arbitrage thresholds
- 📊 Multi-market support (BTC, ETH, SOL, XRP, etc.)
- 🧱 Clean modular architecture
- 📝 Detailed logging & statistics
- 🧪 Safe simulation mode (no real trades by default)
- 🐍 Python implementation available

## 📦 Installation

```bash
# Clone
git clone https://github.com/m4rcu5o/polymarket-trading-bot.git
cd rarb

# Install dependencies
pip install -e .

# Configure
cp .env.example .env
# Edit .env with your settings

# Generate Polymarket API credentials
python -c "
from py_clob_client.client import ClobClient
import os
client = ClobClient('https://clob.polymarket.com', key=os.environ['PRIVATE_KEY'], chain_id=137)
creds = client.create_or_derive_api_creds()
print(f'POLY_API_KEY={creds.api_key}')
print(f'POLY_API_SECRET={creds.api_secret}')
print(f'POLY_API_PASSPHRASE={creds.api_passphrase}')
"

# Approve Polymarket contracts (one-time setup)
python scripts/approve_usdc.py

# Run
rarb run --live --realtime
```

---

## ⚙️ Configuration

Create your environment file:

```bash
# Wallet
PRIVATE_KEY=0x...                    # Your wallet private key
WALLET_ADDRESS=0x...                 # Your wallet address

# Polymarket L2 API Credentials (generate with script above)
POLY_API_KEY=...
POLY_API_SECRET=...
POLY_API_PASSPHRASE=...

# Trading Parameters
MIN_PROFIT_THRESHOLD=0.005           # 0.5% minimum profit
MAX_POSITION_SIZE=100                # Max $100 per trade
MIN_LIQUIDITY_USD=10000              # $10k minimum market liquidity
MAX_DAYS_UNTIL_RESOLUTION=7          # Skip markets resolving later
NUM_WS_CONNECTIONS=6                 # WebSocket connections (250 markets each)
DRY_RUN=true                         # Set to false for live trading

# Dashboard (optional - omit for no auth)
DASHBOARD_USERNAME=admin
DASHBOARD_PASSWORD=...
```
---

## 🧪 Simulation Mode (Default)

This bot **does not place real trades by default**.  
All detected arbitrage opportunities are logged.

To enable real trading:
1. Add Polymarket API credentials to `.env`
2. Implement order signing logic
3. Test with small position sizes

---

## 🧠 Geo-Restrictions

Polymarket blocks US IP addresses for order placement. The recommended architecture:
- Bot server (us-east-1): Low-latency WebSocket connection for price monitoring
- Proxy server (ca-central-1 Montreal): SOCKS5 proxy for order placement

Configure the proxy in your
```bash
SOCKS5_PROXY_HOST=your-proxy-ip
SOCKS5_PROXY_PORT=1080
SOCKS5_PROXY_USER=rarb
SOCKS5_PROXY_PASS=your-password
```
---

## 🐍 Python Version

A fully updated Python implementation is available:

```bash
git checkout python_version
```

---

## ⚠️ Risk Disclaimer

- Arbitrage windows are extremely short
- Slippage & latency may eliminate profit
- Market conditions change rapidly
- Always test before deploying real capital

---

## 📬 Contact

[Telegram](https://t.me/microRustyme)

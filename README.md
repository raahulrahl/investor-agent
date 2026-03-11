<p align="center">
  <img src="https://raw.githubusercontent.com/getbindu/create-bindu-agent/refs/heads/main/assets/light.svg" alt="bindu Logo" width="200">
</p>

<h1 align="center">investor-agent</h1>

<p align="center">
  <strong>Investment analysis agent that fetches market data, financial statements, sentiment indicators, and technical analysis to generate comprehensive investment reports</strong>
</p>

<p align="center">
  <a href="https://github.com/Paraschamoli/investor-agent/actions/workflows/build-and-push.yml?query=branch%3Amain">
    <img src="https://img.shields.io/github/actions/workflow/status/Paraschamoli/investor-agent/build-and-push.yml?branch=main" alt="Build status">
  </a>
  <a href="https://img.shields.io/github/license/Paraschamoli/investor-agent">
    <img src="https://img.shields.io/github/license/Paraschamoli/investor-agent" alt="License">
  </a>
</p>

---

## 📖 Overview

Investor Agent is an AI-powered financial analysis agent built on the [Bindu Agent Framework](https://github.com/getbindu/bindu) and [Agno](https://github.com/agno-agi/agno). It provides comprehensive investment analysis by fetching real-time market data from Yahoo Finance, CNN, Nasdaq, and Google Trends, then synthesizing insights using OpenRouter LLMs.

**Key Capabilities:**
- 📊 **Market Data**: Real-time stock prices, market movers (gainers, losers, most-active)
- 📈 **Technical Analysis**: SMA, EMA, RSI, MACD, Bollinger Bands (requires TA-Lib)
- 🎯 **Sentiment Indicators**: CNN Fear & Greed Index, Crypto Fear & Greed, Google Trends
- 💰 **Financial Statements**: Income statements, balance sheets, cash flows (quarterly/annual)
- 🏢 **Institutional Data**: Institutional holders, insider trades, mutual fund ownership
- 📅 **Earnings Calendar**: Nasdaq earnings announcements with surprise tracking
- 📊 **Options Analysis**: Options chains with filtering by date, strike, and type
- 🤖 **LLM Analysis**: Investment thesis generation via OpenRouter (Claude 3.5 Sonnet default)

---

## 🚀 Quick Start

### Prerequisites

- Python 3.12+
- [uv](https://github.com/astral-sh/uv) package manager
- OpenRouter API key ([get free tier](https://openrouter.ai/keys))
- Mem0 API key (optional, [get free tier](https://app.mem0.ai/dashboard/api-keys))

### Installation

```bash
# Clone the repository
git clone https://github.com/Paraschamoli/investor-agent.git
cd investor-agent

# Install dependencies
uv sync

# Configure environment
cp .env.example .env
```

### Configuration

Edit `.env` and add your API keys:

```bash
OPENROUTER_API_KEY=your_openrouter_key_here
MEM0_API_KEY=your_mem0_key_here  # Optional
```

### Run the Agent

```bash
# Start the agent (default: Claude 3.5 Sonnet)
uv run python -m investor_agent

# Or specify a different model
uv run python -m investor_agent --model anthropic/claude-3.5-sonnet

# Agent will be available at http://localhost:3773
```

---

## 💡 Usage

### Example Queries

```bash
# Comprehensive stock analysis
"Provide a comprehensive investment analysis of Apple (AAPL) for a long-term investor"

# Market sentiment check
"What's the current market sentiment? Check Fear & Greed index and market movers"

# Financial statements
"Get Tesla's quarterly income statement and balance sheet"

# Earnings calendar
"Which companies have earnings announcements this week?"

# Technical analysis
"Calculate RSI and MACD indicators for NVIDIA (NVDA)"

# Institutional analysis
"Show me institutional holders and recent insider trades for Microsoft (MSFT)"

# Options analysis
"Get the options chain for SPY expiring in the next 30 days"

# Portfolio strategy
"Create a diversified long-term investment portfolio with moderate risk tolerance"
```

### JSON-RPC 2.0 API

**Send Message:**
```bash
curl --location 'http://localhost:3773' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{api_key}}' \
--data '{
  "jsonrpc": "2.0",
  "method": "message/send",
  "params": {
    "message": {
      "role": "user",
      "kind": "message",
      "messageId": "550e8400-e29b-41d4-a716-446655440001",
      "contextId": "550e8400-e29b-41d4-a716-446655440002",
      "taskId": "550e8400-e29b-41d4-a716-446655440003",
      "parts": [
        {
          "kind": "text",
          "text": "Analyze Apple (AAPL) stock for potential investment"
        }
      ]
    },
    "skillId": "investment-analysis-v1",
    "configuration": {
      "acceptedOutputModes": ["application/json"]
    }
  },
  "id": "550e8400-e29b-41d4-a716-446655440003"
}'
```

**Check Task Status:**
```bash
curl --location 'http://localhost:3773' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{api_key}}' \
--data '{
  "jsonrpc": "2.0",
  "method": "tasks/get",
  "params": {
    "taskId": "550e8400-e29b-41d4-a716-446655440003"
  },
  "id": "550e8400-e29b-41d4-a716-446655440004"
}'
```

**Response Format:**
```json
{
  "jsonrpc": "2.0",
  "id": "550e8400-e29b-41d4-a716-446655440004",
  "result": {
    "id": "550e8400-e29b-41d4-a716-446655440003",
    "status": {
      "state": "completed",
      "timestamp": "2026-03-06T12:32:09.822685+00:00"
    },
    "history": [...],
    "artifacts": [
      {
        "name": "result",
        "parts": [
          {
            "kind": "text",
            "text": "# Investment Analysis: Apple Inc. (AAPL)..."
          }
        ]
      }
    ]
  }
}
```

### Task States

- `submitted`: Task received, agent initializing
- `working`: Agent processing request and calling tools
- `completed`: Analysis finished, results in artifacts
- `failed`: Error occurred (check error message)

---

## 🔧 Available Tools

The agent uses 13 specialized financial analysis tools:

| Tool | Description | Data Source |
|------|-------------|-------------|
| `get_market_movers` | Gainers, losers, most-active stocks | Yahoo Finance HTML |
| `get_ticker_data` | Stock info, news, recommendations | yfinance |
| `get_price_history` | Historical price data (1d to max) | yfinance |
| `get_financial_statements` | Income, balance, cash flow statements | yfinance |
| `get_institutional_holders` | Institutional and mutual fund ownership | yfinance |
| `get_earnings_history` | Earnings with surprise data | yfinance |
| `get_insider_trades` | Insider trading activity | yfinance |
| `get_options` | Options chain with filtering | yfinance |
| `get_cnn_fear_greed_index` | Market sentiment indicators | CNN |
| `get_crypto_fear_greed_index` | Crypto market sentiment | alternative.me |
| `get_google_trends` | Search interest trends | Google Trends |
| `get_nasdaq_earnings_calendar` | Upcoming earnings by date | Nasdaq API |
| `calculate_technical_indicator` | SMA, EMA, RSI, MACD, BBANDS | TA-Lib |

---

## 🎯 Skills

### investment-analysis-v1

**Primary Capability:**
- Fetch and analyze financial data from Yahoo Finance and other APIs
- Market movers, ticker data, price history, financial statements
- Institutional holders, earnings, insider trades, options chains
- Sentiment indicators and technical analysis

**Secondary Capability:**
- LLM-based analysis using OpenRouter models
- Generates markdown investment reports with thesis
- Mem0 integration for memory (optional)
- Async task handling via bindufy framework

**When to Use:**
- Single stock fundamental analysis
- Market sentiment monitoring
- Earnings calendar tracking
- Institutional flow analysis
- Technical indicator calculation

**When NOT to Use:**
- Real-time trading execution
- Portfolio backtesting
- Algorithmic trading strategies
- Non-financial data analysis

**Performance:**
- Average processing time: 5-15 seconds
- Max concurrent requests: 3
- Memory per request: 512MB

---

## 🐳 Docker Deployment

### Local Docker Setup

```bash
# Build the Docker image
docker build -f Dockerfile.agent -t investor-agent .

# Run the container
docker run -p 3773:3773 \
  -e OPENROUTER_API_KEY=your_key \
  -e MEM0_API_KEY=your_key \
  investor-agent

# Agent will be available at http://localhost:3773
```

### Docker Compose

```bash
# Start with docker-compose
docker-compose up --build

# Run in background
docker-compose up -d

# View logs
docker-compose logs -f

# Stop
docker-compose down
```

---

## 🌐 Deploy to bindus.directory

Make your agent discoverable worldwide and enable agent-to-agent collaboration.

### Setup GitHub Secrets

```bash
# Authenticate with GitHub
gh auth login

# Set deployment secrets
gh secret set BINDU_API_TOKEN --body "<your-bindu-api-key>"
gh secret set DOCKERHUB_TOKEN --body "<your-dockerhub-token>"
```

Get your keys:
- **Bindu API Key**: [bindus.directory](https://bindus.directory) dashboard
- **Docker Hub Token**: [Docker Hub Security Settings](https://hub.docker.com/settings/security)

### Automatic Deployment

The agent uses GitHub Actions for automatic deployment:

```bash
# Push to main branch triggers deployment
git push origin main
```

GitHub Actions will:
1. ✅ Build Docker image for linux/amd64 and linux/arm64
2. ✅ Push to Docker Hub as `para5/investor-agent:latest`
3. ✅ Register agent on bindus.directory
4. ✅ Deploy to Argo CD
5. ✅ Wait for deployment completion
6. ✅ Report deployment URL and health check

**Workflow triggers on:**
- Push to `main` branch
- Changes to `Dockerfile.agent`, `investor_agent/**`, `pyproject.toml`, `.version`
- Manual workflow dispatch

---

## 🛠️ Development

### Project Structure

```
investor-agent/
├── investor_agent/
│   ├── skills/
│   │   └── investment-analysis/
│   │       └── skill.yaml          # Skill configuration
│   ├── __init__.py
│   ├── __main__.py                 # CLI entry point
│   ├── main.py                     # Agent initialization
│   ├── tools.py                    # 13 financial analysis tools
│   └── agent_config.json           # Agent configuration
├── .github/
│   └── workflows/
│       └── build-and-push.yml      # CI/CD pipeline
├── tests/
│   └── test_main.py
├── .env.example
├── .version                         # Version tracking
├── docker-compose.yml
├── Dockerfile.agent
└── pyproject.toml
```

### Running Tests

```bash
# Run all tests
uv run pytest

# With coverage
uv run pytest --cov=investor_agent

# Specific test file
uv run pytest tests/test_main.py
```

### Code Quality

```bash
# Format code
uv run ruff format .

# Lint code
uv run ruff check .

# Type checking
uv run mypy investor_agent
```

### Adding New Tools

1. Define tool function in `investor_agent/tools.py`
2. Add to `InvestmentTools` toolkit in `main.py`
3. Update `allowed_tools` in `skill.yaml`
4. Add tests in `tests/`
5. Update documentation

### Environment Variables

| Variable | Description | Required | Default |
|----------|-------------|----------|---------|
| `OPENROUTER_API_KEY` | OpenRouter API key for LLM | ✅ Yes | - |
| `MEM0_API_KEY` | Mem0 API key for memory | ❌ No | - |
| `PORT` | Server port | ❌ No | 3773 |

---

## 📊 Technical Details

### Architecture

```
User Request (JSON-RPC)
    ↓
Bindufy Server (Port 3773)
    ↓
Handler (Lazy Agent Init)
    ↓
Agno Agent (OpenRouter LLM)
    ↓
InvestmentTools Toolkit (13 tools)
    ↓
External APIs (Yahoo Finance, CNN, Nasdaq, Google Trends)
    ↓
LLM Synthesis
    ↓
Markdown Report (Artifacts)
```

### Data Sources

- **Yahoo Finance**: Stock data via yfinance library + HTML scraping
- **CNN**: Fear & Greed Index API
- **Alternative.me**: Crypto Fear & Greed Index
- **Nasdaq**: Earnings calendar API
- **Google Trends**: pytrends library

### Error Handling

- **API Failures**: Retry with exponential backoff (3 attempts, 2-30 second delays)
- **Rate Limits**: Automatic retry with backoff
- **Invalid Tickers**: Validation and error messages
- **Missing TA-Lib**: Graceful degradation, skip technical indicators
- **Network Errors**: Partial results when possible

### Limitations

- Yahoo Finance data may be delayed 15-20 minutes
- Technical indicators require TA-Lib installation
- No real-time streaming data
- No portfolio backtesting or optimization
- Analysis is educational, not financial advice

---

## 🤝 Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/amazing-feature`
3. Commit your changes: `git commit -m 'Add amazing feature'`
4. Push to the branch: `git push origin feature/amazing-feature`
5. Open a Pull Request

### Development Guidelines

- Follow existing code style (ruff formatting)
- Add tests for new features
- Update documentation
- Ensure all tests pass
- Update `skill.yaml` if capabilities change

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## 🙏 Powered by Bindu

Built with the [Bindu Agent Framework](https://github.com/getbindu/bindu) and [Agno](https://github.com/agno-agi/agno)

**Why Bindu?**
- 🌐 **Internet of Agents**: A2A, AP2, X402 protocols for agent collaboration
- ⚡ **Zero-config setup**: From idea to production in minutes
- 🛠️ **Production-ready**: Built-in deployment, monitoring, and scaling
- 🔌 **Framework agnostic**: Works with Agno, LangChain, CrewAI, and more

**Build Your Own Agent:**
```bash
uvx cookiecutter https://github.com/getbindu/create-bindu-agent.git
```

---

## 📚 Resources

- 📖 [Full Documentation](https://Paraschamoli.github.io/investor-agent/)
- 💻 [GitHub Repository](https://github.com/Paraschamoli/investor-agent/)
- 🐛 [Report Issues](https://github.com/Paraschamoli/investor-agent/issues)
- 💬 [Join Discord](https://discord.gg/3w5zuYUuwt)
- 🌐 [Agent Directory](https://bindus.directory)
- 📚 [Bindu Documentation](https://docs.getbindu.com)
- 🔧 [API Reference](https://docs.getbindu.com/api-reference/all-the-tasks/send-message-to-agent)

---

## ⚠️ Disclaimer

This agent is for educational and informational purposes only. The investment analysis and recommendations provided are generated by AI and should not be considered as financial advice. Always consult with qualified financial professionals before making investment decisions. Past performance does not guarantee future results.

---

<p align="center">
  <strong>Built with 💛 by the team from Amsterdam 🌷</strong>
</p>

<p align="center">
  <a href="https://github.com/Paraschamoli/investor-agent">⭐ Star this repo</a> •
  <a href="https://discord.gg/3w5zuYUuwt">💬 Join Discord</a> •
  <a href="https://bindus.directory">🌐 Agent Directory</a>
</p>

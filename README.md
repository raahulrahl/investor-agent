<p align="center">
  <img src="https://raw.githubusercontent.com/getbindu/create-bindu-agent/refs/heads/main/assets/light.svg" alt="bindu Logo" width="200">
</p>

<h1 align="center">investor-agent</h1>

<p align="center">
  <strong>AI-powered investment analysis agent with comprehensive financial tools</strong>
</p>

<p align="center">
  <a href="https://github.com/Paraschamoli/investor-agent/actions/workflows/main.yml?query=branch%3Amain">
    <img src="https://img.shields.io/github/actions/workflow/status/Paraschamoli/investor-agent/main.yml?branch=main" alt="Build status">
  </a>
  <a href="https://img.shields.io/github/license/Paraschamoli/investor-agent">
    <img src="https://img.shields.io/github/license/Paraschamoli/investor-agent" alt="License">
  </a>
</p>

---

## 📖 Overview

Investor Agent is a comprehensive AI-powered financial analysis agent built on the [Bindu Agent Framework](https://github.com/getbindu/bindu) for the Internet of Agents. It provides real-time market data, technical analysis, sentiment indicators, and investment insights.

**Key Capabilities:**
- � **Real-time Market Data**: Stock prices, historical data, and market movers
- 📈 **Technical Analysis**: SMA, EMA, RSI, MACD, Bollinger Bands
- 🎯 **Market Sentiment**: CNN Fear & Greed Index, Google Trends
- 💰 **Financial Statements**: Income statements, balance sheets, cash flows
- 🏢 **Institutional Data**: Insider trades, institutional holders
- � **Earnings Calendar**: Upcoming earnings announcements
- 📊 **Options Analysis**: Complete options chains with Greeks

---

## 🚀 Quick Start

### Prerequisites

- Python 3.12+
- [uv](https://github.com/astral-sh/uv) package manager
- API keys for OpenRouter and Mem0 (both have free tiers)

### Installation

```bash
# Clone the repository
git clone https://github.com/Paraschamoli/investor-agent.git
cd investor-agent

# Create virtual environment
uv venv --python 3.12.9
source .venv/bin/activate  # On Windows: .venv\Scripts\activate

# Install dependencies
uv sync

# Configure environment
cp .env.example .env
```

### Configuration

Edit `.env` and add your API keys:

| Key | Get It From | Required |
|-----|-------------|----------|
| `OPENROUTER_API_KEY` | [OpenRouter](https://openrouter.ai/keys) | ✅ Yes |
| `MEM0_API_KEY` | [Mem0 Dashboard](https://app.mem0.ai/dashboard/api-keys) | If you want to use Mem0 tools |

### Run the Agent

```bash
# Start the agent
uv run python -m investor_agent

# Agent will be available at http://localhost:3773
```

---

## 💡 Usage

### Example Queries

```bash
# Market analysis
"Analyze Apple (AAPL) stock performance over the last 6 months"

# Technical indicators
"Calculate RSI and MACD indicators for Tesla (TSLA)"

# Market sentiment
"What's the current market sentiment? Check Fear & Greed index and top market movers"

# Financial analysis
"Get the latest quarterly income statement and balance sheet for Microsoft (MSFT)"

# Earnings analysis
"Which companies have earnings announcements this week? Focus on tech stocks"

# Options analysis
"Get the options chain for NVIDIA (NVDA) with expiration dates in the next 30 days"
```

### Input Formats

**Plain Text:**
```
Analyze [TICKER] stock with [specific requirements]
```

**JSON:**
```json
{
  "content": "Get technical analysis for TSLA with RSI and MACD indicators",
  "focus": "technical-analysis"
}
```

### Output Structure

The agent returns structured output with:
- **Market Data**: Real-time prices and historical data
- **Technical Indicators**: Calculated technical analysis metrics
- **Sentiment Analysis**: Market sentiment indicators and trends
- **Financial Reports**: Formatted financial statements
- **Investment Insights**: AI-powered analysis and recommendations

---

## 🔌 API Usage

The agent exposes a RESTful API when running. Default endpoint: `http://localhost:3773`

### Quick Start

For complete API documentation, request/response formats, and examples, visit:

📚 **[Bindu API Reference - Send Message to Agent](https://docs.getbindu.com/api-reference/all-the-tasks/send-message-to-agent)**


### Additional Resources

- 📖 [Full API Documentation](https://docs.getbindu.com/api-reference/all-the-tasks/send-message-to-agent)
- 📦 [Postman Collections](https://github.com/GetBindu/Bindu/tree/main/postman/collections)
- 🔧 [API Reference](https://docs.getbindu.com)

---

## 🎯 Skills

### investment-analysis (v1.0.0)

**Primary Capability:**
- Comprehensive financial analysis and investment insights
- Real-time market data and technical indicators
- Sentiment analysis and earnings tracking

**Features:**
- 📊 Real-time stock price data and historical analysis
- 📈 Technical indicators (SMA, EMA, RSI, MACD, Bollinger Bands)
- 🎯 Market sentiment indicators (Fear & Greed Index, Google Trends)
- 💰 Financial statements (income, balance sheet, cash flow)
- 🏢 Institutional holdings and insider trading data
- 📅 Earnings calendar and surprise analysis
- 📊 Options chain analysis with Greeks
- 🚀 Market movers and sector performance

**Best Used For:**
- Stock analysis and due diligence
- Portfolio risk assessment
- Market timing and entry/exit decisions
- Earnings season preparation
- Options strategy development

**Not Suitable For:**
- Real-time trading execution
- Financial advice (educational purposes only)
- High-frequency trading strategies

**Performance:**
- Average processing time: ~2-5 seconds
- Max concurrent requests: 10
- Memory per request: ~50MB

---

## 🐳 Docker Deployment

### Local Docker Setup

```bash
# Build and run with Docker Compose
docker-compose up --build

# Agent will be available at http://localhost:3773
```

### Docker Configuration

The agent runs on port `3773` and requires:
- `OPENROUTER_API_KEY` environment variable
- `MEM0_API_KEY` environment variable

Configure these in your `.env` file before running.

### Production Deployment

```bash
# Use production compose file
docker-compose -f docker-compose.prod.yml up -d
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

### Deploy

```bash
# Push to trigger automatic deployment
git push origin main
```

GitHub Actions will automatically:
1. Build your agent
2. Create Docker container
3. Push to Docker Hub
4. Register on bindus.directory

---

## 🛠️ Development

### Project Structure

```
investor-agent/
├── investor_agent/
│   ├── skills/
│   │   └── investment-analysis/
│   │       ├── skill.yaml          # Skill configuration
│   │       └── __init__.py
│   ├── __init__.py
│   ├── __main__.py
│   ├── main.py                     # Agent entry point
│   ├── tools.py                   # Financial analysis tools
│   └── agent_config.json           # Agent configuration
├── tests/
│   └── test_main.py
├── .env.example
├── docker-compose.yml
├── Dockerfile.agent
└── pyproject.toml
```

### Running Tests

```bash
make test              # Run all tests
make test-cov          # With coverage report
```

### Code Quality

```bash
make format            # Format code with ruff
make lint              # Run linters
make check             # Format + lint + test
```

### Pre-commit Hooks

```bash
# Install pre-commit hooks
uv run pre-commit install

# Run manually
uv run pre-commit run -a
```

---

## 🤝 Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/amazing-feature`
3. Commit your changes: `git commit -m 'Add amazing feature'`
4. Push to the branch: `git push origin feature/amazing-feature`
5. Open a Pull Request

See [CONTRIBUTING.md](CONTRIBUTING.md) for detailed guidelines.

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## 🙏 Powered by Bindu

Built with the [Bindu Agent Framework](https://github.com/getbindu/bindu)

**Why Bindu?**
- 🌐 **Internet of Agents**: A2A, AP2, X402 protocols for agent collaboration
- ⚡ **Zero-config setup**: From idea to production in minutes
- 🛠️ **Production-ready**: Built-in deployment, monitoring, and scaling

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

---

<p align="center">
  <strong>Built with 💛 by the team from Amsterdam 🌷</strong>
</p>

<p align="center">
  <a href="https://github.com/Paraschamoli/investor-agent">⭐ Star this repo</a> •
  <a href="https://discord.gg/3w5zuYUuwt">💬 Join Discord</a> •
  <a href="https://bindus.directory">🌐 Agent Directory</a>
</p>

#   i n v e s t o r - a g e n t 
 
 

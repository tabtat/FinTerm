# FinTerm MCP Server

FastAPI microservice for market data processing, Gemini-backed analysis, risk scoring, and market-maker quoting.

## 🚀 Quick Start

### Prerequisites

- Python 3.10+ (tested with Python 3.13)
- Git

### 1. Clone and Setup

```bash
git clone <your-repo-url>
cd aivestor

# Create virtual environment
python3 -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt
```

### 2. Environment Configuration

```bash
# Create environment file from example
cp .env.example .env

# Edit .env and add your Gemini API key
nano .env  # or use your preferred editor
```

### 3. Start the Server

```bash
# Start development server
uvicorn app:app --reload

# Server will be available at http://127.0.0.1:8000
```

### 4. Verify Installation

```bash
# Health check
curl http://127.0.0.1:8000/api/v1/mcp/health

# Expected response: {"status":"ok","service":"MCP Server"}
```

## 📊 API Endpoints

### Local Endpoints (No API key required)

- `GET /` - Root endpoint with docs link
- `GET /api/v1/mcp/health` - Health check
- `POST /api/v1/mcp/analyze-risk` - Risk analysis and anomaly detection
- `POST /api/v1/mcp/predict-price` - Price prediction (EMA/Linear Regression)
- `POST /api/v1/mcp/market-maker/quote` - Market maker bid/ask quotes

### Gemini AI Endpoints (Requires API key)

- `GET /api/v1/mcp/models` - List available Gemini models
- `POST /api/v1/mcp/summarize-news` - AI-powered news summarization
- `POST /api/v1/mcp/analyze-chart` - AI chart analysis

## 🧪 Testing with curl

### 1. Risk Analysis

```bash
curl -X POST http://127.0.0.1:8000/api/v1/mcp/analyze-risk \
  -H "Content-Type: application/json" \
  -d '{
    "symbol": "AAPL",
    "data": [
      {"time": "2025-09-10", "close": 110, "volume": 5000},
      {"time": "2025-09-11", "close": 108, "volume": 12000},
      {"time": "2025-09-12", "close": 114, "volume": 9000}
    ]
  }'
```

### 2. Price Prediction (EMA)

```bash
curl -X POST http://127.0.0.1:8000/api/v1/mcp/predict-price \
  -H "Content-Type: application/json" \
  -d '{
    "symbol": "AAPL",
    "method": "ema",
    "ema_span": 5,
    "data": [
      {"close": 110}, {"close": 108}, {"close": 111}, {"close": 112}, {"close": 113}
    ]
  }'
```

### 3. Market Maker Quote

```bash
curl -X POST http://127.0.0.1:8000/api/v1/mcp/market-maker/quote \
  -H "Content-Type: application/json" \
  -d '{
    "mid_price": 100.0,
    "volatility": 0.2,
    "risk_aversion": 0.1,
    "time_horizon": 1.0,
    "inventory": -5.0,
    "kappa": 1.5
  }'
```

### 4. News Summarization (Requires Gemini API)

```bash
curl -X POST http://127.0.0.1:8000/api/v1/mcp/summarize-news \
  -H "Content-Type: application/json" \
  -d '{
    "articles": [
      "Apple reported strong Q3 earnings with iPhone sales exceeding expectations.",
      "Market volatility increased due to inflation concerns."
    ]
  }'
```

## 📖 Interactive Documentation

FastAPI automatically generates interactive API documentation:

- **Swagger UI**: http://127.0.0.1:8000/docs
- **ReDoc**: http://127.0.0.1:8000/redoc

## 🛠 Development

### Project Structure

```
aivestor/
├── app.py              # Main FastAPI application
├── requirements.txt    # Python dependencies
├── .env.example       # Environment template
├── .env               # Environment variables (create from .env.example)
├── README.md          # This file
└── venv/              # Virtual environment (created locally)
```

### Key Features

- **Risk Analysis**: Volatility calculation and volume anomaly detection
- **ML Predictions**: EMA and linear regression price forecasting
- **Market Making**: Avellaneda-Stoikov optimal bid/ask pricing
- **AI Analysis**: Gemini-powered chart and news analysis

## 🔧 Troubleshooting

### Common Issues

**1. Type annotation error (Python < 3.10)**

```
TypeError: unsupported operand type(s) for |: 'type' and 'NoneType'
```

**Solution**: Add `from __future__ import annotations` at the top of `app.py`

**2. Missing Gemini API key**

```
{"detail":"GEMINI_API_KEY is not set"}
```

**Solution**: Set your API key in `.env` file or environment variable

**3. Virtual environment not found**

```
source: no such file or directory: venv/bin/activate
```

**Solution**: Create virtual environment first with `python3 -m venv venv`

**4. Port already in use**

```
OSError: [Errno 48] Address already in use
```

**Solution**: Use different port with `uvicorn app:app --port 8001 --reload`

### Testing Script

Save as `test_all.sh`:

```bash
#!/bin/bash
BASE_URL="http://127.0.0.1:8000"

echo "🔍 Testing Health..."
curl -s $BASE_URL/api/v1/mcp/health | jq

echo -e "\n📊 Testing Risk Analysis..."
curl -s -X POST $BASE_URL/api/v1/mcp/analyze-risk \
  -H "Content-Type: application/json" \
  -d '{"symbol":"AAPL","data":[{"close":110,"volume":5000},{"close":108,"volume":12000}]}' | jq

echo -e "\n🎯 Testing Price Prediction..."
curl -s -X POST $BASE_URL/api/v1/mcp/predict-price \
  -H "Content-Type: application/json" \
  -d '{"symbol":"AAPL","data":[{"close":110},{"close":108},{"close":111}]}' | jq

echo -e "\n💰 Testing Market Maker..."
curl -s -X POST $BASE_URL/api/v1/mcp/market-maker/quote \
  -H "Content-Type: application/json" \
  -d '{"mid_price":100,"volatility":0.2,"risk_aversion":0.1,"time_horizon":1,"inventory":0,"kappa":1.5}' | jq
```

Run with: `chmod +x test_all.sh && ./test_all.sh`

## 🏗 Architecture

The system follows a microservice architecture with clear separation between data ingestion, analysis, and client interfaces. The MCP server acts as the computational engine for market analysis and prediction.

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests
5. Submit a pull request

---

*Database files should reside under the `instance/` folder as per project convention.*

<style>#mermaid-1758069086906{font-family:sans-serif;font-size:16px;fill:#333;}#mermaid-1758069086906 .error-icon{fill:#552222;}#mermaid-1758069086906 .error-text{fill:#552222;stroke:#552222;}#mermaid-1758069086906 .edge-thickness-normal{stroke-width:2px;}#mermaid-1758069086906 .edge-thickness-thick{stroke-width:3.5px;}#mermaid-1758069086906 .edge-pattern-solid{stroke-dasharray:0;}#mermaid-1758069086906 .edge-pattern-dashed{stroke-dasharray:3;}#mermaid-1758069086906 .edge-pattern-dotted{stroke-dasharray:2;}#mermaid-1758069086906 .marker{fill:#333333;}#mermaid-1758069086906 .marker.cross{stroke:#333333;}#mermaid-1758069086906 svg{font-family:sans-serif;font-size:16px;}#mermaid-1758069086906 .label{font-family:sans-serif;color:#333;}#mermaid-1758069086906 .label text{fill:#333;}#mermaid-1758069086906 .node rect,#mermaid-1758069086906 .node circle,#mermaid-1758069086906 .node ellipse,#mermaid-1758069086906 .node polygon,#mermaid-1758069086906 .node path{fill:#ECECFF;stroke:#9370DB;stroke-width:1px;}#mermaid-1758069086906 .node .label{text-align:center;}#mermaid-1758069086906 .node.clickable{cursor:pointer;}#mermaid-1758069086906 .arrowheadPath{fill:#333333;}#mermaid-1758069086906 .edgePath .path{stroke:#333333;stroke-width:1.5px;}#mermaid-1758069086906 .flowchart-link{stroke:#333333;fill:none;}#mermaid-1758069086906 .edgeLabel{background-color:#e8e8e8;text-align:center;}#mermaid-1758069086906 .edgeLabel rect{opacity:0.5;background-color:#e8e8e8;fill:#e8e8e8;}#mermaid-1758069086906 .cluster rect{fill:#ffffde;stroke:#aaaa33;stroke-width:1px;}#mermaid-1758069086906 .cluster text{fill:#333;}#mermaid-1758069086906 div.mermaidTooltip{position:absolute;text-align:center;max-width:200px;padding:2px;font-family:sans-serif;font-size:12px;background:hsl(80,100%,96.2745098039%);border:1px solid #aaaa33;border-radius:2px;pointer-events:none;z-index:100;}#mermaid-1758069086906:root{--mermaid-font-family:sans-serif;}#mermaid-1758069086906:root{--mermaid-alt-font-family:sans-serif;}#mermaid-1758069086906 flowchart-v2{fill:apa;}</style>



<style>#mermaid-1758069168998{font-family:sans-serif;font-size:16px;fill:#333;}#mermaid-1758069168998 .error-icon{fill:#552222;}#mermaid-1758069168998 .error-text{fill:#552222;stroke:#552222;}#mermaid-1758069168998 .edge-thickness-normal{stroke-width:2px;}#mermaid-1758069168998 .edge-thickness-thick{stroke-width:3.5px;}#mermaid-1758069168998 .edge-pattern-solid{stroke-dasharray:0;}#mermaid-1758069168998 .edge-pattern-dashed{stroke-dasharray:3;}#mermaid-1758069168998 .edge-pattern-dotted{stroke-dasharray:2;}#mermaid-1758069168998 .marker{fill:#333333;}#mermaid-1758069168998 .marker.cross{stroke:#333333;}#mermaid-1758069168998 svg{font-family:sans-serif;font-size:16px;}#mermaid-1758069168998 .label{font-family:sans-serif;color:#333;}#mermaid-1758069168998 .label text{fill:#333;}#mermaid-1758069168998 .node rect,#mermaid-1758069168998 .node circle,#mermaid-1758069168998 .node ellipse,#mermaid-1758069168998 .node polygon,#mermaid-1758069168998 .node path{fill:#ECECFF;stroke:#9370DB;stroke-width:1px;}#mermaid-1758069168998 .node .label{text-align:center;}#mermaid-1758069168998 .node.clickable{cursor:pointer;}#mermaid-1758069168998 .arrowheadPath{fill:#333333;}#mermaid-1758069168998 .edgePath .path{stroke:#333333;stroke-width:1.5px;}#mermaid-1758069168998 .flowchart-link{stroke:#333333;fill:none;}#mermaid-1758069168998 .edgeLabel{background-color:#e8e8e8;text-align:center;}#mermaid-1758069168998 .edgeLabel rect{opacity:0.5;background-color:#e8e8e8;fill:#e8e8e8;}#mermaid-1758069168998 .cluster rect{fill:#ffffde;stroke:#aaaa33;stroke-width:1px;}#mermaid-1758069168998 .cluster text{fill:#333;}#mermaid-1758069168998 div.mermaidTooltip{position:absolute;text-align:center;max-width:200px;padding:2px;font-family:sans-serif;font-size:12px;background:hsl(80,100%,96.2745098039%);border:1px solid #aaaa33;border-radius:2px;pointer-events:none;z-index:100;}#mermaid-1758069168998:root{--mermaid-font-family:sans-serif;}#mermaid-1758069168998:root{--mermaid-alt-font-family:sans-serif;}#mermaid-1758069168998 flowchart-v2{fill:apa;}</style>

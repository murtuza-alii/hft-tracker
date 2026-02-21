# HFT Stock Tracker System

A high-frequency trading (HFT) system architecture for real-time market data ingestion, order book management, strategy execution, and risk management.

## Architecture Overview

```
┌─────────────────────────────────────────────────────────────────┐
│  Market Data (Exchange Feeds)                                    │
└─────────────────────────────┬───────────────────────────────────┘
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│  Network Layer (C++) - DPDK, Kernel Bypass, 10GbE/100GbE        │
└─────────────────────────────┬───────────────────────────────────┘
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│  Market Data Ingestion (C++) - ITCH, FIX Protocol Decoder       │
└─────────────────────────────┬───────────────────────────────────┘
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│  In-Memory Order Book (C++) - Lock-Free, Replicated             │
└─────────────────────────────┬───────────────────────────────────┘
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│  Event Pipeline (C++) → Strategy Engine (Python)                 │
└─────────────────────────────┬───────────────────────────────────┘
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│  Risk Engine → Smart Order Router → Execution Layer             │
└─────────────────────────────────────────────────────────────────┘
```

## Project Structure

```
project/
├── src/
│   ├── python/                 # Strategy Engine, Risk, OMS, Monitoring
│   │   ├── strategies/         # Market Making, Stat Arb, ML strategies
│   │   ├── risk_engine/        # Pre-trade risk checks
│   │   ├── smart_order_router/ # Venue selection, order type
│   │   ├── oms/                # Order Management System
│   │   ├── monitoring/         # Prometheus, metrics, alerting
│   │   └── common/             # Shared utilities
│   │
│   └── cpp/                    # Ultra-low latency components
│       ├── network/            # DPDK, kernel bypass
│       ├── market_data/        # ITCH, FIX protocol decoder
│       ├── order_book/         # In-memory order book
│       ├── event_pipeline/     # Lock-free event queue
│       ├── execution/          # FIX protocol, exchange connections
│       └── common/             # Timing, shared structures
│
├── config/                     # Configuration files
├── tests/                      # Unit and integration tests
├── requirements.txt
├── CMakeLists.txt
└── README.md
```

## Tech Stack

| Component | Technology |
|-----------|------------|
| Network Layer | DPDK, Solarflare OpenOnload, 10GbE/100GbE NIC |
| Protocol Decoding | ITCH 5.0, FIX 4.2/4.4 |
| Order Book | Lock-free C++, in-memory replication |
| Event Pipeline | Lock-free ring buffer, CAS operations |
| Strategy Engine | Python, NumPy, Pandas, ML (PyTorch/scikit-learn) |
| Risk Engine | Python |
| Monitoring | Prometheus, InfluxDB |
| Execution | FIX Protocol, exchange gateways |

## Quick Start

### Python Components

```bash
# Create virtual environment
python -m venv venv
venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt
```

### C++ Build

```bash
mkdir build && cd build
cmake ..
cmake --build .
```

## Configuration

- Set exchange credentials and API keys in `config/` (never commit secrets)
- Configure risk limits, position limits, and kill-switch thresholds

## License

Proprietary - Internal Use Only

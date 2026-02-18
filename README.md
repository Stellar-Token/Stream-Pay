# StreamPay

## 🌟 Overview

StreamPay is a decentralized micropayment streaming service that enables content creators to receive real-time payments as users consume their content. Built on the Stellar blockchain, StreamPay leverages sub-second finality to make per-second micropayments viable for articles, music, videos, and other digital content.

### Key Features

- **Real-time Payments**: Creators receive payments every second as content is consumed
- **Low Transaction Costs**: Leverage Stellar's minimal fees (~0.00001 XLM per transaction)
- **Multiple Content Types**: Support for articles, music, videos, podcasts, and live streams
- **Creator Dashboard**: Analytics, earnings tracking, and payout management
- **Flexible Pricing**: Set per-second, per-minute, or per-view rates
- **Instant Withdrawals**: Access earnings immediately without waiting periods
- **Multi-currency Support**: Accept payments in XLM, USDC, and other Stellar assets

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                         Frontend                             │
│                    (Next.js + React)                        │
│  - User Interface                                           │
│  - Content Player                                           │
│  - Creator Dashboard                                        │
│  - Wallet Integration                                       │
└──────────────────┬──────────────────────────────────────────┘
                   │
                   │ REST API / WebSocket
                   │
┌──────────────────▼──────────────────────────────────────────┐
│                         Backend                              │
│                    (NestJS + Node.js)                       │
│  - Authentication & Authorization                           │
│  - Content Management                                       │
│  - Payment Processing                                       │
│  - Analytics & Reporting                                    │
│  - Streaming Session Management                            │
└──────────────────┬──────────────────────────────────────────┘
                   │
                   │ Stellar SDK
                   │
┌──────────────────▼──────────────────────────────────────────┐
│                    Smart Contracts                           │
│                      (Rust/Soroban)                         │
│  - Payment Stream Contract                                  │
│  - Escrow Management                                        │
│  - Creator Verification                                     │
│  - Revenue Distribution                                     │
└──────────────────┬──────────────────────────────────────────┘
                   │
                   │
┌──────────────────▼──────────────────────────────────────────┐
│                    Stellar Network                           │
│  - Transaction Processing                                   │
│  - Asset Management                                         │
│  - Account Management                                       │
└─────────────────────────────────────────────────────────────┘
```

### Technology Stack

#### Frontend
- **Framework**: Next.js 14 (App Router)
- **UI Library**: React 18
- **Styling**: Tailwind CSS
- **State Management**: Zustand / Redux Toolkit
- **Wallet Integration**: @stellar/freighter-api
- **Media Player**: Video.js / Howler.js
- **Real-time**: Socket.io-client

#### Backend
- **Framework**: NestJS
- **Runtime**: Node.js 18+
- **Database**: PostgreSQL
- **Cache**: Redis
- **Queue**: Bull (Redis-based)
- **Stellar SDK**: @stellar/stellar-sdk
- **Authentication**: JWT + Passport
- **Real-time**: Socket.io
- **File Storage**: AWS S3 / IPFS

#### Smart Contracts
- **Language**: Rust
- **Platform**: Stellar Soroban
- **Testing**: Soroban CLI & SDK
- **Build Tool**: Cargo

#### DevOps
- **Containerization**: Docker
- **Orchestration**: Docker Compose / Kubernetes
- **CI/CD**: GitHub Actions
- **Monitoring**: Prometheus + Grafana

## 📁 Project Structure

```
streampay/
├── frontend/                 # Next.js frontend application
│   ├── src/
│   │   ├── app/             # App router pages
│   │   ├── components/      # React components
│   │   ├── hooks/           # Custom React hooks
│   │   ├── lib/             # Utility functions
│   │   ├── stores/          # State management
│   │   └── styles/          # Global styles
│   ├── public/              # Static assets
│   ├── package.json
│   └── next.config.js
│
├── backend/                  # NestJS backend application
│   ├── src/
│   │   ├── modules/         # Feature modules
│   │   │   ├── auth/
│   │   │   ├── content/
│   │   │   ├── payments/
│   │   │   ├── streaming/
│   │   │   └── users/
│   │   ├── common/          # Shared utilities
│   │   ├── config/          # Configuration
│   │   └── main.ts          # Application entry
│   ├── test/                # E2E tests
│   ├── package.json
│   └── nest-cli.json
│
├── contracts/                # Soroban smart contracts
│   ├── payment-stream/      # Payment streaming contract
│   ├── escrow/              # Escrow contract
│   └── shared/              # Shared contract utilities
│
├── docker/                   # Docker configurations
│   ├── frontend.Dockerfile
│   ├── backend.Dockerfile
│   └── docker-compose.yml
│
├── docs/                     # Additional documentation
│   ├── API.md
│   ├── ARCHITECTURE.md
│   └── DEPLOYMENT.md
│
├── scripts/                  # Utility scripts
│   ├── deploy-contracts.sh
│   └── setup-db.sh
│
├── .github/                  # GitHub configurations
│   └── workflows/           # CI/CD workflows
│
├── LICENSE
└── README.md
```

## 🚀 Getting Started

### Prerequisites

Before you begin, ensure you have the following installed:

- **Node.js** (v18 or higher)
- **npm** or **yarn** or **pnpm**
- **Rust** (latest stable version)
- **Docker** and **Docker Compose**
- **PostgreSQL** (v14 or higher)
- **Redis** (v7 or higher)
- **Stellar CLI** (soroban-cli)

### Installation

1. **Clone the repository**

```bash
git clone https://github.com/yourusername/streampay.git
cd streampay
```

2. **Install dependencies**

```bash
# Install frontend dependencies
cd frontend
npm install

# Install backend dependencies
cd ../backend
npm install

# Build smart contracts
cd ../contracts
cargo build --release
```

3. **Environment Configuration**

Create `.env` files in both frontend and backend directories:

**Frontend (.env.local)**
```env
NEXT_PUBLIC_API_URL=http://localhost:3001
NEXT_PUBLIC_STELLAR_NETWORK=testnet
NEXT_PUBLIC_HORIZON_URL=https://horizon-testnet.stellar.org
NEXT_PUBLIC_WS_URL=ws://localhost:3001
```

**Backend (.env)**
```env
# Application
NODE_ENV=development
PORT=3001
API_PREFIX=api/v1

# Database
DATABASE_HOST=localhost
DATABASE_PORT=5432
DATABASE_USER=streampay
DATABASE_PASSWORD=your_password
DATABASE_NAME=streampay_db

# Redis
REDIS_HOST=localhost
REDIS_PORT=6379

# JWT
JWT_SECRET=your_jwt_secret_key
JWT_EXPIRATION=7d

# Stellar
STELLAR_NETWORK=testnet
HORIZON_URL=https://horizon-testnet.stellar.org
STELLAR_MASTER_KEY=your_stellar_secret_key

# AWS S3 (Optional)
AWS_REGION=us-east-1
AWS_ACCESS_KEY_ID=your_access_key
AWS_SECRET_ACCESS_KEY=your_secret_key
S3_BUCKET_NAME=streampay-content

# IPFS (Optional)
IPFS_API_URL=https://ipfs.infura.io:5001
```

4. **Database Setup**

```bash
# Start PostgreSQL and Redis using Docker
docker-compose up -d postgres redis

# Run database migrations
cd backend
npm run migration:run
```

5. **Deploy Smart Contracts**

```bash
cd contracts

# Install Soroban CLI if not already installed
cargo install --locked soroban-cli

# Build contracts
soroban contract build

# Deploy to testnet
./scripts/deploy-contracts.sh
```

6. **Start Development Servers**

```bash
# Terminal 1 - Backend
cd backend
npm run start:dev

# Terminal 2 - Frontend
cd frontend
npm run dev
```

The application will be available at:
- **Frontend**: http://localhost:3000
- **Backend API**: http://localhost:3001
- **API Documentation**: http://localhost:3001/api/docs

## 🔧 Development

### Running Tests

```bash
# Frontend tests
cd frontend
npm run test
npm run test:e2e

# Backend tests
cd backend
npm run test
npm run test:e2e
npm run test:cov

# Smart contract tests
cd contracts
cargo test
```

### Code Formatting & Linting

```bash
# Frontend
cd frontend
npm run lint
npm run format

# Backend
cd backend
npm run lint
npm run format

# Contracts
cd contracts
cargo fmt
cargo clippy
```

### Building for Production

```bash
# Frontend
cd frontend
npm run build

# Backend
cd backend
npm run build

# Contracts
cd contracts
cargo build --release --target wasm32-unknown-unknown
```

## 🐳 Docker Deployment

Build and run all services using Docker Compose:

```bash
# Build images
docker-compose build

# Start all services
docker-compose up -d

# View logs
docker-compose logs -f

# Stop services
docker-compose down
```

## 📚 API Documentation

API documentation is automatically generated using Swagger/OpenAPI and available at:

```
http://localhost:3001/api/docs
```

Key endpoints include:

- `POST /api/v1/auth/register` - User registration
- `POST /api/v1/auth/login` - User authentication
- `GET /api/v1/content` - List content
- `POST /api/v1/content` - Create content
- `POST /api/v1/streaming/start` - Start streaming session
- `POST /api/v1/streaming/stop` - Stop streaming session
- `GET /api/v1/payments/history` - Payment history
- `POST /api/v1/payments/withdraw` - Withdraw earnings

## 🔐 Security Considerations

- All API endpoints require JWT authentication
- Stellar private keys are stored encrypted
- Rate limiting implemented on all endpoints
- Input validation using class-validator
- SQL injection prevention with parameterized queries
- XSS protection with content sanitization
- CORS configured for allowed origins only

## 🌐 Stellar Integration

### Payment Flow

1. User starts consuming content
2. Frontend calculates micropayment amount per second
3. Payment stream initiated via smart contract
4. Funds held in escrow contract
5. Periodic settlements to creator's Stellar account
6. Session ends, final settlement processed

### Smart Contract Functions

- `initialize_stream()` - Create new payment stream
- `deposit_funds()` - Deposit funds to escrow
- `process_payment()` - Process micropayment
- `settle_stream()` - Final settlement
- `withdraw_earnings()` - Creator withdrawal

## 🤝 Contributing

We welcome contributions! Please see our [Contributing Guide](CONTRIBUTING.md) for details.

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

🙌**Contribution Guidelines:**

Assignment required before PR submission
Timeframe: 48-72 hours
PR description must include: Close #[issue-number]
Star the repo⭐
For more context, please refer to the frontend README 🚀

# StellarTech 🌟

## 🎯 Overview

StellarTech is a revolutionary financial platform built on the Stellar blockchain that combines **mass disbursement capabilities** with **AI-driven portfolio management**. We're bridging the gap between the unbanked population and global financial markets, enabling anyone to receive payments and invest in tokenized real-world assets (RWAs) with zero crypto knowledge required.

### 🌍 The Problem We Solve

- **1.7 billion adults** worldwide are unbanked or underbanked
- Traditional investment platforms require minimum balances and banking relationships
- Cross-border payments are slow (3-5 days) and expensive (2-7% fees)
- Emerging market populations lack access to global investment opportunities
- Financial literacy and portfolio management expertise is inaccessible to most

### 💡 Our Solution

**Entry Point → Growth → Wealth Building**

1. **Mass Disbursement**: Organizations disburse payroll, aid, or remittances via SMS
2. **Instant Wallets**: Recipients get auto-created wallets with no crypto knowledge needed
3. **Investment Access**: Buy fractional tokenized stocks, bonds, and carbon credits
4. **AI Optimization**: Personalized portfolio management and rebalancing
5. **Financial Education**: AI-powered coaching and market insights

---

## ✨ Key Features

### 💸 Mass Disbursement Platform
- **Bulk Payments**: CSV upload for thousands of recipients
- **SMS Onboarding**: No app download required
- **Multi-Asset Support**: USDC, local stablecoins, and custom tokens
- **Instant Settlement**: Leverage Stellar's 3-5 second finality
- **Compliance Ready**: Built-in KYC/AML workflows

### 📈 Investment Marketplace
- **Tokenized Assets**: Trade stocks, bonds, carbon credits, and more
- **Fractional Ownership**: Invest as little as $1 in any asset
- **Real-Time Pricing**: Oracle-powered market data
- **P2P Trading**: Direct asset exchange between users
- **Yield Opportunities**: Earn on idle stablecoin balances

### 🤖 AI Portfolio Management
- **Risk Profiling**: Personalized investment strategies
- **Auto-Rebalancing**: Maintain optimal asset allocation
- **Goal-Based Investing**: Target retirement, education, or wealth goals
- **Smart Recommendations**: ML-powered asset suggestions
- **Market Intelligence**: Sentiment analysis and trend detection

### 🔒 Security & Compliance
- **Non-Custodial**: Users control their private keys
- **Biometric Recovery**: No seed phrases to manage
- **Regulatory Compliance**: KYC/AML integration
- **Smart Contract Audits**: Thoroughly tested Soroban contracts
- **End-to-End Encryption**: Secure data handling

---

## 🏗️ Architecture

### Technology Stack

```
┌─────────────────────────────────────────────────────────┐
│                    Frontend Layer                        │
│              Next.js 14 + TypeScript                     │
│         (User App + Admin Dashboard)                     │
└────────────────────┬────────────────────────────────────┘
                     │
┌────────────────────┴────────────────────────────────────┐
│                    Backend Layer                         │
│              NestJS + TypeScript                         │
│  ┌──────────────┬──────────────┬──────────────┐        │
│  │ Disbursement │     AI/ML    │   Trading    │        │
│  │   Module     │    Engine    │   Engine     │        │
│  └──────────────┴──────────────┴──────────────┘        │
└────────────────────┬────────────────────────────────────┘
                     │
┌────────────────────┴────────────────────────────────────┐
│                 Blockchain Layer                         │
│             Stellar Soroban (Rust)                       │
│  ┌──────────────┬──────────────┬──────────────┐        │
│  │ Disbursement │    Asset     │  Portfolio   │        │
│  │   Contract   │  Tokenization│  Management  │        │
│  └──────────────┴──────────────┴──────────────┘        │
└─────────────────────────────────────────────────────────┘
```

### Core Components

#### 1. Smart Contracts (Rust/Soroban)
```
contracts/
├── disbursement/       # Payment distribution logic
├── assets/            # Tokenized RWA management
├── trading/           # Order matching and settlement
└── portfolio/         # AI-triggered rebalancing
```

#### 2. Backend Services (NestJS)
```
apps/api/
├── modules/
│   ├── disbursement/  # SDP integration, SMS, wallet creation
│   ├── assets/        # Asset tokenization, pricing, trading
│   ├── ai/            # Portfolio optimization, risk analysis
│   ├── stellar/       # Soroban client, Horizon API
│   └── users/         # KYC, profiles, authentication
```

#### 3. Frontend Applications (Next.js)
```
apps/web/
├── app/
│   ├── (admin)/       # Organization dashboard
│   ├── (user)/        # Recipient wallet & investment UI
│   └── api/           # API routes
```

---

## 🚀 Getting Started

### Prerequisites

- **Node.js**: v18+ ([Download](https://nodejs.org))
- **Rust**: v1.75+ ([Install](https://www.rust-lang.org/tools/install))
- **Stellar CLI**: Latest ([Install](https://stellar.org/developers/docs))
- **Docker**: For local development (Optional)
- **PostgreSQL**: v14+ (Or use Docker)

### Installation

1. **Clone the repository**
```bash
git clone https://github.com/yourusername/stellartech.git
cd stellartech
```

2. **Install dependencies**
```bash
# Install all packages
npm install

# Or using yarn
yarn install
```

3. **Set up environment variables**
```bash
# Copy example env files
cp .env.example .env
cp apps/api/.env.example apps/api/.env
cp apps/web/.env.example apps/web/.env

# Edit with your configurations
```

4. **Configure Stellar Network**
```bash
# For testnet (development)
export STELLAR_NETWORK=testnet
export STELLAR_RPC_URL=https://soroban-testnet.stellar.org

# Generate keypairs
stellar keys generate admin --network testnet
stellar keys generate deployer --network testnet
```

5. **Build and deploy smart contracts**
```bash
# Build contracts
cd contracts
cargo build --target wasm32-unknown-unknown --release

# Deploy to testnet
stellar contract deploy \
  --wasm target/wasm32-unknown-unknown/release/disbursement.wasm \
  --source deployer \
  --network testnet

# Save contract IDs to .env
```

6. **Set up the database**
```bash
# Run migrations
npm run db:migrate

# Seed initial data
npm run db:seed
```

7. **Start development servers**
```bash
# Terminal 1: Start backend
npm run dev:api

# Terminal 2: Start frontend
npm run dev:web

# Or run both concurrently
npm run dev
```

8. **Access the application**
- Frontend: http://localhost:3000
- API: http://localhost:3001
- API Docs: http://localhost:3001/api/docs

---

## 📋 Environment Variables

### Backend (`apps/api/.env`)
```env
# Database
DATABASE_URL=postgresql://user:password@localhost:5432/stellartech

# Stellar
STELLAR_NETWORK=testnet
STELLAR_RPC_URL=https://soroban-testnet.stellar.org
STELLAR_HORIZON_URL=https://horizon-testnet.stellar.org
STELLAR_PASSPHRASE=Test SDF Network ; September 2015

# Contract Addresses
DISBURSEMENT_CONTRACT_ID=C...
ASSET_CONTRACT_ID=C...
TRADING_CONTRACT_ID=C...

# Services
SMS_PROVIDER=twilio
TWILIO_ACCOUNT_SID=your_account_sid
TWILIO_AUTH_TOKEN=your_auth_token

# AI/ML
OPENAI_API_KEY=your_openai_key
AI_MODEL=gpt-4

# Security
JWT_SECRET=your_jwt_secret
ENCRYPTION_KEY=your_encryption_key
```

### Frontend (`apps/web/.env`)
```env
NEXT_PUBLIC_API_URL=http://localhost:3001
NEXT_PUBLIC_STELLAR_NETWORK=testnet
NEXT_PUBLIC_STELLAR_HORIZON_URL=https://horizon-testnet.stellar.org
```

---

## 🧪 Testing

### Run all tests
```bash
npm test
```

### Test by component
```bash
# Smart contracts
cd contracts && cargo test

# Backend
npm run test:api

# Frontend
npm run test:web

# E2E tests
npm run test:e2e
```

### Test coverage
```bash
npm run test:cov
```

---

## 📦 Deployment

### Smart Contracts (Mainnet)

```bash
# Switch to mainnet
export STELLAR_NETWORK=mainnet

# Deploy contracts
stellar contract deploy \
  --wasm target/wasm32-unknown-unknown/release/disbursement.wasm \
  --source deployer \
  --network mainnet

# Initialize contracts
stellar contract invoke \
  --id C... \
  --source admin \
  --network mainnet \
  -- initialize --admin "G..."
```

### Backend (Cloud Platform)

```bash
# Build production image
docker build -t stellartech-api:latest -f apps/api/Dockerfile .

# Deploy to your platform (AWS, GCP, Azure, Railway, etc.)
# Example: Railway
railway up

# Or use Docker Compose
docker-compose -f docker-compose.prod.yml up -d
```



## 🤝 Contributing

We welcome contributions from the community! Please read our [Contributing Guidelines](CONTRIBUTING.md) before submitting PRs.

### Development Workflow

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request







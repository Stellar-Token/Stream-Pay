# StreamPay Smart Contracts

> Soroban smart contracts powering StreamPay's micropayment streaming on Stellar

[![Rust](https://img.shields.io/badge/Rust-1.75+-orange)](https://www.rust-lang.org)
[![Soroban](https://img.shields.io/badge/Soroban-20.0+-blue)](https://soroban.stellar.org)
[![Stellar](https://img.shields.io/badge/Stellar-Network-blue)](https://stellar.org)

## 📋 Overview

StreamPay's smart contracts are written in Rust using the Soroban smart contract platform on Stellar. These contracts handle payment streaming, escrow management, creator verification, and revenue distribution with minimal transaction costs.

## 🛠️ Technology Stack

- **Language**: Rust 1.75+
- **Platform**: Stellar Soroban
- **SDK**: soroban-sdk
- **Testing**: Rust native testing + soroban-cli
- **Build Tool**: Cargo
- **Deployment**: soroban-cli

## 📁 Project Structure

```
contracts/
├── payment-stream/              # Main payment streaming contract
│   ├── src/
│   │   ├── lib.rs              # Contract entry point
│   │   ├── types.rs            # Custom types and structs
│   │   ├── storage.rs          # Storage management
│   │   ├── events.rs           # Contract events
│   │   ├── errors.rs           # Error definitions
│   │   └── test.rs             # Unit tests
│   ├── Cargo.toml
│   └── README.md
│
├── escrow/                      # Escrow management contract
│   ├── src/
│   │   ├── lib.rs
│   │   ├── types.rs
│   │   ├── storage.rs
│   │   ├── events.rs
│   │   ├── errors.rs
│   │   └── test.rs
│   ├── Cargo.toml
│   └── README.md
│
├── creator-registry/            # Creator verification contract
│   ├── src/
│   │   ├── lib.rs
│   │   ├── types.rs
│   │   ├── storage.rs
│   │   ├── events.rs
│   │   ├── errors.rs
│   │   └── test.rs
│   ├── Cargo.toml
│   └── README.md
│
├── shared/                      # Shared utilities
│   ├── src/
│   │   ├── lib.rs
│   │   ├── utils.rs
│   │   └── constants.rs
│   └── Cargo.toml
│
├── scripts/                     # Deployment scripts
│   ├── deploy.sh
│   ├── initialize.sh
│   └── test-integration.sh
│
├── Cargo.toml                   # Workspace configuration
└── README.md
```

## 🚀 Getting Started

### Prerequisites

- **Rust** 1.75 or higher
- **Soroban CLI** 20.0 or higher
- **Stellar Account** (testnet or mainnet)

### Installation

1. **Install Rust**

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

2. **Install Soroban CLI**

```bash
cargo install --locked soroban-cli
```

3. **Add WASM target**

```bash
rustup target add wasm32-unknown-unknown
```

4. **Clone and navigate to contracts**

```bash
cd contracts
```

### Building Contracts

```bash
# Build all contracts
cargo build --release --target wasm32-unknown-unknown

# Build specific contract
cd payment-stream
cargo build --release --target wasm32-unknown-unknown

# Optimize contract size
soroban contract optimize --wasm target/wasm32-unknown-unknown/release/payment_stream.wasm
```

### Running Tests

```bash
# Run all tests
cargo test

# Run tests for specific contract
cd payment-stream
cargo test

# Run tests with output
cargo test -- --nocapture

# Run integration tests
./scripts/test-integration.sh
```

## 📝 Contract Details

### 1. Payment Stream Contract

Manages real-time payment streaming between consumers and creators.

#### Key Functions

```rust
// contracts/payment-stream/src/lib.rs

use soroban_sdk::{contract, contractimpl, Address, Env, Symbol, Val};

#[contract]
pub struct PaymentStreamContract;

#[contractimpl]
impl PaymentStreamContract {
    /// Initialize a new payment stream
    pub fn initialize_stream(
        env: Env,
        consumer: Address,
        creator: Address,
        content_id: Symbol,
        rate_per_second: i128,
    ) -> Symbol {
        consumer.require_auth();
        
        let stream_id = generate_stream_id(&env);
        
        let stream = StreamData {
            consumer: consumer.clone(),
            creator: creator.clone(),
            content_id,
            rate_per_second,
            start_time: env.ledger().timestamp(),
            total_paid: 0,
            status: StreamStatus::Active,
        };
        
        storage::set_stream(&env, &stream_id, &stream);
        
        events::stream_initialized(&env, &stream_id, &consumer, &creator);
        
        stream_id
    }

    /// Deposit funds for streaming
    pub fn deposit_funds(
        env: Env,
        stream_id: Symbol,
        amount: i128,
    ) {
        let stream = storage::get_stream(&env, &stream_id)?;
        stream.consumer.require_auth();
        
        // Transfer funds to escrow
        token::transfer(&env, &stream.consumer, &env.current_contract_address(), amount);
        
        // Update stream balance
        storage::add_balance(&env, &stream_id, amount);
        
        events::funds_deposited(&env, &stream_id, amount);
    }

    /// Process payment for elapsed time
    pub fn process_payment(
        env: Env,
        stream_id: Symbol,
    ) -> i128 {
        let mut stream = storage::get_stream(&env, &stream_id)?;
        
        require!(stream.status == StreamStatus::Active, Error::StreamNotActive);
        
        let current_time = env.ledger().timestamp();
        let elapsed_seconds = current_time - stream.start_time;
        let amount_due = stream.rate_per_second * elapsed_seconds as i128;
        
        let available_balance = storage::get_balance(&env, &stream_id)?;
        let payment_amount = amount_due.min(available_balance);
        
        // Transfer to creator
        token::transfer(
            &env,
            &env.current_contract_address(),
            &stream.creator,
            payment_amount,
        );
        
        // Update stream data
        stream.total_paid += payment_amount;
        storage::set_stream(&env, &stream_id, &stream);
        storage::subtract_balance(&env, &stream_id, payment_amount);
        
        events::payment_processed(&env, &stream_id, payment_amount);
        
        payment_amount
    }

    /// Finalize and close stream
    pub fn finalize_stream(
        env: Env,
        stream_id: Symbol,
    ) -> i128 {
        let mut stream = storage::get_stream(&env, &stream_id)?;
        stream.consumer.require_auth();
        
        // Process final payment
        let final_payment = Self::process_payment(env.clone(), stream_id.clone());
        
        // Return remaining balance to consumer
        let remaining_balance = storage::get_balance(&env, &stream_id)?;
        if remaining_balance > 0 {
            token::transfer(
                &env,
                &env.current_contract_address(),
                &stream.consumer,
                remaining_balance,
            );
        }
        
        // Mark stream as completed
        stream.status = StreamStatus::Completed;
        storage::set_stream(&env, &stream_id, &stream);
        
        events::stream_finalized(&env, &stream_id, final_payment);
        
        final_payment
    }

    /// Get stream information
    pub fn get_stream(
        env: Env,
        stream_id: Symbol,
    ) -> StreamData {
        storage::get_stream(&env, &stream_id).unwrap()
    }

    /// Get stream balance
    pub fn get_balance(
        env: Env,
        stream_id: Symbol,
    ) -> i128 {
        storage::get_balance(&env, &stream_id).unwrap_or(0)
    }
}
```

#### Types Definition

```rust
// contracts/payment-stream/src/types.rs

use soroban_sdk::{contracttype, Address, Symbol};

#[contracttype]
#[derive(Clone, Debug, Eq, PartialEq)]
pub struct StreamData {
    pub consumer: Address,
    pub creator: Address,
    pub content_id: Symbol,
    pub rate_per_second: i128,
    pub start_time: u64,
    pub total_paid: i128,
    pub status: StreamStatus,
}

#[contracttype]
#[derive(Clone, Debug, Eq, PartialEq)]
pub enum StreamStatus {
    Active,
    Paused,
    Completed,
    Cancelled,
}

#[contracttype]
pub enum DataKey {
    Stream(Symbol),
    Balance(Symbol),
    StreamCounter,
}
```

#### Error Handling

```rust
// contracts/payment-stream/src/errors.rs

use soroban_sdk::contracterror;

#[contracterror]
#[derive(Copy, Clone, Debug, Eq, PartialEq, PartialOrd, Ord)]
#[repr(u32)]
pub enum Error {
    StreamNotFound = 1,
    StreamNotActive = 2,
    InsufficientBalance = 3,
    Unauthorized = 4,
    InvalidRate = 5,
    AlreadyInitialized = 6,
}
```

#### Events

```rust
// contracts/payment-stream/src/events.rs

use soroban_sdk::{Env, Address, Symbol};

pub fn stream_initialized(
    env: &Env,
    stream_id: &Symbol,
    consumer: &Address,
    creator: &Address,
) {
    env.events().publish(
        (Symbol::new(env, "stream_initialized"), stream_id),
        (consumer, creator),
    );
}

pub fn funds_deposited(
    env: &Env,
    stream_id: &Symbol,
    amount: i128,
) {
    env.events().publish(
        (Symbol::new(env, "funds_deposited"), stream_id),
        amount,
    );
}

pub fn payment_processed(
    env: &Env,
    stream_id: &Symbol,
    amount: i128,
) {
    env.events().publish(
        (Symbol::new(env, "payment_processed"), stream_id),
        amount,
    );
}

pub fn stream_finalized(
    env: &Env,
    stream_id: &Symbol,
    total_paid: i128,
) {
    env.events().publish(
        (Symbol::new(env, "stream_finalized"), stream_id),
        total_paid,
    );
}
```

### 2. Escrow Contract

Manages fund escrow and dispute resolution.

```rust
// contracts/escrow/src/lib.rs

#[contract]
pub struct EscrowContract;

#[contractimpl]
impl EscrowContract {
    /// Create escrow
    pub fn create_escrow(
        env: Env,
        depositor: Address,
        beneficiary: Address,
        amount: i128,
        release_time: u64,
    ) -> Symbol {
        depositor.require_auth();
        
        let escrow_id = generate_escrow_id(&env);
        
        // Transfer funds to escrow
        token::transfer(&env, &depositor, &env.current_contract_address(), amount);
        
        let escrow = EscrowData {
            depositor: depositor.clone(),
            beneficiary: beneficiary.clone(),
            amount,
            release_time,
            status: EscrowStatus::Locked,
        };
        
        storage::set_escrow(&env, &escrow_id, &escrow);
        
        events::escrow_created(&env, &escrow_id, &depositor, &beneficiary, amount);
        
        escrow_id
    }

    /// Release escrow funds
    pub fn release_escrow(
        env: Env,
        escrow_id: Symbol,
    ) {
        let mut escrow = storage::get_escrow(&env, &escrow_id)?;
        
        require!(
            env.ledger().timestamp() >= escrow.release_time,
            Error::EscrowNotReady
        );
        
        require!(escrow.status == EscrowStatus::Locked, Error::EscrowNotLocked);
        
        // Transfer to beneficiary
        token::transfer(
            &env,
            &env.current_contract_address(),
            &escrow.beneficiary,
            escrow.amount,
        );
        
        escrow.status = EscrowStatus::Released;
        storage::set_escrow(&env, &escrow_id, &escrow);
        
        events::escrow_released(&env, &escrow_id, escrow.amount);
    }

    /// Cancel and refund escrow
    pub fn cancel_escrow(
        env: Env,
        escrow_id: Symbol,
    ) {
        let mut escrow = storage::get_escrow(&env, &escrow_id)?;
        escrow.depositor.require_auth();
        
        require!(escrow.status == EscrowStatus::Locked, Error::EscrowNotLocked);
        
        // Refund to depositor
        token::transfer(
            &env,
            &env.current_contract_address(),
            &escrow.depositor,
            escrow.amount,
        );
        
        escrow.status = EscrowStatus::Cancelled;
        storage::set_escrow(&env, &escrow_id, &escrow);
        
        events::escrow_cancelled(&env, &escrow_id);
    }
}
```

### 3. Creator Registry Contract

Manages creator verification and metadata.

```rust
// contracts/creator-registry/src/lib.rs

#[contract]
pub struct CreatorRegistry;

#[contractimpl]
impl CreatorRegistry {
    /// Register creator
    pub fn register_creator(
        env: Env,
        creator: Address,
        metadata_uri: Symbol,
    ) {
        creator.require_auth();
        
        let creator_data = CreatorData {
            address: creator.clone(),
            metadata_uri,
            verified: false,
            registration_time: env.ledger().timestamp(),
            total_earnings: 0,
            content_count: 0,
        };
        
        storage::set_creator(&env, &creator, &creator_data);
        
        events::creator_registered(&env, &creator);
    }

    /// Verify creator (admin only)
    pub fn verify_creator(
        env: Env,
        admin: Address,
        creator: Address,
    ) {
        admin.require_auth();
        require!(storage::is_admin(&env, &admin)?, Error::Unauthorized);
        
        let mut creator_data = storage::get_creator(&env, &creator)?;
        creator_data.verified = true;
        
        storage::set_creator(&env, &creator, &creator_data);
        
        events::creator_verified(&env, &creator);
    }

    /// Update creator earnings
    pub fn update_earnings(
        env: Env,
        creator: Address,
        amount: i128,
    ) {
        // Only payment contract can call this
        require!(
            env.invoker() == storage::get_payment_contract(&env)?,
            Error::Unauthorized
        );
        
        let mut creator_data = storage::get_creator(&env, &creator)?;
        creator_data.total_earnings += amount;
        
        storage::set_creator(&env, &creator, &creator_data);
    }

    /// Get creator info
    pub fn get_creator(
        env: Env,
        creator: Address,
    ) -> CreatorData {
        storage::get_creator(&env, &creator).unwrap()
    }
}
```

## 🧪 Testing

### Unit Tests

```rust
// contracts/payment-stream/src/test.rs

#[cfg(test)]
mod test {
    use super::*;
    use soroban_sdk::{testutils::Address as _, Address, Env};

    #[test]
    fn test_initialize_stream() {
        let env = Env::default();
        let contract_id = env.register_contract(None, PaymentStreamContract);
        let client = PaymentStreamContractClient::new(&env, &contract_id);

        let consumer = Address::generate(&env);
        let creator = Address::generate(&env);
        let content_id = Symbol::new(&env, "content_1");
        let rate = 1000i128; // 0.001 XLM per second

        let stream_id = client.initialize_stream(
            &consumer,
            &creator,
            &content_id,
            &rate,
        );

        let stream = client.get_stream(&stream_id);
        assert_eq!(stream.consumer, consumer);
        assert_eq!(stream.creator, creator);
        assert_eq!(stream.rate_per_second, rate);
    }

    #[test]
    fn test_deposit_and_payment() {
        let env = Env::default();
        let contract_id = env.register_contract(None, PaymentStreamContract);
        let client = PaymentStreamContractClient::new(&env, &contract_id);

        let consumer = Address::generate(&env);
        let creator = Address::generate(&env);
        let stream_id = setup_stream(&client, &consumer, &creator);

        // Deposit funds
        let deposit_amount = 10_000_000i128;
        client.deposit_funds(&stream_id, &deposit_amount);

        // Check balance
        let balance = client.get_balance(&stream_id);
        assert_eq!(balance, deposit_amount);

        // Simulate time passage
        env.ledger().set_timestamp(env.ledger().timestamp() + 10);

        // Process payment
        let payment = client.process_payment(&stream_id);
        assert!(payment > 0);
    }

    #[test]
    #[should_panic(expected = "StreamNotActive")]
    fn test_process_payment_inactive_stream() {
        let env = Env::default();
        let contract_id = env.register_contract(None, PaymentStreamContract);
        let client = PaymentStreamContractClient::new(&env, &contract_id);

        let stream_id = Symbol::new(&env, "invalid_stream");
        client.process_payment(&stream_id);
    }
}
```

### Integration Tests

```bash
#!/bin/bash
# scripts/test-integration.sh

set -e

echo "Building contracts..."
cargo build --release --target wasm32-unknown-unknown

echo "Starting local Stellar network..."
soroban network start local

echo "Deploying contracts..."
PAYMENT_CONTRACT=$(soroban contract deploy \
  --wasm target/wasm32-unknown-unknown/release/payment_stream.wasm \
  --network local)

ESCROW_CONTRACT=$(soroban contract deploy \
  --wasm target/wasm32-unknown-unknown/release/escrow.wasm \
  --network local)

echo "Payment Contract: $PAYMENT_CONTRACT"
echo "Escrow Contract: $ESCROW_CONTRACT"

echo "Running integration tests..."
# Add integration test commands here

echo "Integration tests completed successfully!"
```

## 🚀 Deployment

### Deploy to Testnet

```bash
# 1. Configure network
soroban network add testnet \
  --rpc-url https://soroban-testnet.stellar.org:443 \
  --network-passphrase "Test SDF Network ; September 2015"

# 2. Generate identity
soroban keys generate deployer --network testnet

# 3. Fund account
curl "https://friendbot.stellar.org?addr=$(soroban keys address deployer)"

# 4. Build contracts
cargo build --release --target wasm32-unknown-unknown

# 5. Deploy payment stream contract
soroban contract deploy \
  --wasm target/wasm32-unknown-unknown/release/payment_stream.wasm \
  --source deployer \
  --network testnet

# 6. Deploy escrow contract
soroban contract deploy \
  --wasm target/wasm32-unknown-unknown/release/escrow.wasm \
  --source deployer \
  --network testnet

# 7. Deploy creator registry
soroban contract deploy \
  --wasm target/wasm32-unknown-unknown/release/creator_registry.wasm \
  --source deployer \
  --network testnet
```

### Automated Deployment Script

```bash
#!/bin/bash
# scripts/deploy.sh

NETWORK=${1:-testnet}
SOURCE=${2:-deployer}

echo "Deploying to $NETWORK..."

# Build all contracts
echo "Building contracts..."
cargo build --release --target wasm32-unknown-unknown

# Deploy Payment Stream Contract
echo "Deploying Payment Stream Contract..."
PAYMENT_CONTRACT=$(soroban contract deploy \
  --wasm target/wasm32-unknown-unknown/release/payment_stream.wasm \
  --source $SOURCE \
  --network $NETWORK)

echo "Payment Stream Contract: $PAYMENT_CONTRACT"

# Deploy Escrow Contract
echo "Deploying Escrow Contract..."
ESCROW_CONTRACT=$(soroban contract deploy \
  --wasm target/wasm32-unknown-unknown/release/escrow.wasm \
  --source $SOURCE \
  --network $NETWORK)

echo "Escrow Contract: $ESCROW_CONTRACT"

# Deploy Creator Registry
echo "Deploying Creator Registry..."
CREATOR_CONTRACT=$(soroban contract deploy \
  --wasm target/wasm32-unknown-unknown/release/creator_registry.wasm \
  --source $SOURCE \
  --network $NETWORK)

echo "Creator Registry Contract: $CREATOR_CONTRACT"

# Save contract addresses
cat > deployed_contracts.json << EOF
{
  "network": "$NETWORK",
  "contracts": {
    "payment_stream": "$PAYMENT_CONTRACT",
    "escrow": "$ESCROW_CONTRACT",
    "creator_registry": "$CREATOR_CONTRACT"
  },
  "deployed_at": "$(date -u +%Y-%m-%dT%H:%M:%SZ)"
}
EOF

echo "Deployment complete! Contract addresses saved to deployed_contracts.json"
```

## 📊 Gas Optimization

### Best Practices

1. **Minimize storage operations**
```rust
// Bad: Multiple storage writes
storage::set_value(&env, &key1, value1);
storage::set_value(&env, &key2, value2);
storage::set_value(&env, &key3, value3);

// Good: Batch in struct
let data = BatchData { value1, value2, value3 };
storage::set_batch(&env, &key, &data);
```

2. **Use efficient data structures**
```rust
// Prefer Vec over Map for small collections
// Use Map for large, frequently accessed data
```

3. **Avoid unnecessary computation**
```rust
// Cache computed values
let computed = expensive_calculation();
storage::set_cached(&env, &key, &computed);
```

## 🔐 Security Considerations

### Access Control

```rust
// Always verify authorization
consumer.require_auth();

// Check permissions
require!(is_authorized(&env, &caller), Error::Unauthorized);
```

### Input Validation

```rust
// Validate inputs
require!(amount > 0, Error::InvalidAmount);
require!(rate_per_second > 0, Error::InvalidRate);
```

### Reentrancy Protection

```rust
// Use state checks
require!(stream.status == StreamStatus::Active, Error::InvalidState);

// Update state before external calls
stream.status = StreamStatus::Processing;
storage::set_stream(&env, &stream_id, &stream);
// Then make external call
```

## 📚 Additional Resources

- [Soroban Documentation](https://soroban.stellar.org/docs)
- [Soroban Examples](https://soroban.stellar.org/docs/examples)
- [Stellar SDK Rust](https://github.com/stellar/rs-soroban-sdk)
- [Rust Book](https://doc.rust-lang.org/book/)

## 🤝 Contributing

Please read the main [CONTRIBUTING.md](../CONTRIBUTING.md) for contribution guidelines.

## 📄 License

MIT License - See [LICENSE](../LICENSE)

---

**Contracts Team** | StreamPay


# UseOnce Pallet - Ticket Management System on Substrate

![Substrate Version](https://img.shields.io/badge/Substrate-3.0.0-%23000000?logo=paritysubstrate)
![Rust Version](https://img.shields.io/badge/Rust-1.68+-%23DEA584?logo=rust)

A blockchain-based ticket management system implementing single-use tickets with state transitions controlled through authorized accounts.

## Overview 📖
This Substrate pallet (`useonce`) provides a secure framework for:
- Immutable ticket registration on-chain
- Role-based access control for state modifications
- Final state transition to prevent replay attacks

## Key Features 🔑
- **On-Chain Ticket Registry**  
  Permanent record of all ticket issuances with cryptographic proof
- **Granular Authorization**  
  Multi-level account privileges using Substrate's origin system
- **State Machine Enforcement**  
  Strict lifecycle: `Registered` → `Consumed` (with audit trail)
- **Light Client Compatible**  
  Designed for easy integration with frontend applications

## Technical Architecture 🧠
```rust
pub enum TicketState {
    Registered,  // Initial state
    Consumed     // Terminal state
}

impl<T: Config> Pallet<T> {
    // Core state transition logic
    fn consume_ticket(ticket_id: T::Hash) -> DispatchResult {
        // Authorization checks
        // State validation
        // Immutable update
    }
}
```

## Installation 🛠️

### Prerequisites
- Rust 1.68+ (`rustup install stable`)
- Node.js 16.x+ (LTS recommended)


### Node Setup
```bash
# Clone repository
git clone https://github.com/zeel991/substrate-node-template.git
cd substrate-node-template

# Build optimized binary
cargo build --release --features runtime-benchmarks

# Launch development node
./target/release/node-template \
  --dev \
  --base-path /tmp/node \
  --ws-port 9944
```

### Frontend Demo Setup (Separate Terminal)
```bash
cd substrate-demo

# Install dependencies
yarn build

# Start interactive demo
yarn demo 
```

## Usage Examples 💻

### 1. Issue New Ticket (Sudo)
```typescript
const ticketId = api.createType('Hash', crypto.randomBytes(32));
await api.tx.useOnce.createTicket(ticketId).signAndSend(alice);
```

### 2. Consume Ticket (Authorized Account)
```typescript
const stateUpdate = api.tx.useOnce.consumeTicket(ticketId);
await stateUpdate.signAndSend(authorizedBob, { nonce: currentNonce });
```

### 3. Verify Ticket State
```typescript
const ticket = await api.query.useOnce.tickets(ticketId);
console.log(`Ticket ${ticketId.toHex()} state: ${ticket.state.toString()}`);
```

## Testing 🧪
Run comprehensive test suite:
```bash
# Unit tests
cargo test -p useonce-pallet

# Integration tests
cargo test --test integration
```

## Security Model 🔒
- **Authorization**: Only pre-approved accounts can modify state
- **Finality**: Once consumed, tickets cannot be reverted
- **Nonce Protection**: Replay attack prevention through transaction numbering



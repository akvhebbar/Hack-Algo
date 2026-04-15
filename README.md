# 🔗 Atomic Settlement - Multi-Chain Implementation

A comprehensive project demonstrating atomic settlement (escrow) functionality across multiple blockchain networks. This project includes complete implementations for both **Ethereum** and **Algorand** blockchains, featuring real-time wallet integration, gas estimation, and smart contract interaction through a modern web interface.

## 📋 Project Overview

**Atomic Settlement** is a decentralized escrow system that allows secure fund transfers between parties. Funds are locked in a smart contract until specific conditions are met, providing security and transparency to both transacting parties.

### Key Features

- 🔐 **Secure Escrow Mechanism** - Funds locked in smart contracts
- 💰 **Real-time Balance & Gas Calculation** - Live updates from blockchain
- 🦊 **Wallet Integration** - MetaMask support for Ethereum, Algorand wallets for Algorand
- ⚡ **Multi-Chain Support** - Ethereum and Algorand implementations
- 🚀 **Production-Ready UI** - Modern React/TypeScript web interface
- 📊 **Real Contract Interactions** - Not mocked, direct blockchain calls
- ⏱️ **Automatic Refund Safety** - 60-second timeout protection

## 📂 Project Structure

```
Hack-Algo/
├── Atomic Settlement - Ethereum/    # Ethereum blockchain implementation
│   ├── contracts/                   # Solidity smart contracts
│   ├── client/                      # React/TypeScript web UI
│   ├── server/                      # Backend server
│   ├── scripts/                     # Deployment & testing scripts
│   └── README.md                    # Ethereum-specific documentation
│
├── Atomic Settlement - Algorand/    # Algorand blockchain implementation
│   ├── final/bankfirst/             # Algorand implementation
│   └── README.md                    # Algorand-specific documentation
│
└── README.md                        # This file - General overview
```

## 🏗️ Core Concepts

### Atomic Settlement (Escrow)

An atomic settlement ensures that fund transfers happen atomically:
- **Locked State**: Funds are deposited and locked in the smart contract
- **Finalized State**: Merchant confirms receipt and funds are released
- **Refunded State**: If conditions aren't met, funds return to user

This eliminates the need for trust between parties as the smart contract enforces rules immutably.

## 🔍 Key Components

### Smart Contracts

- **AtomicEscrow.sol** (Ethereum) - Core escrow logic
  - `depositFunds()` - User locks funds
  - `finalizeTransaction()` - Release to merchant
  - `refundUser()` - Return to user
  - Auto-refund after 60 seconds if not finalized

### Frontend (React/TypeScript)

- **InitiatePanel** - Payment initiation UI
- **PipelinePanel** - Transaction status pipeline
- **SimulationPanel** - Testing and demonstration
- **Navbar** - Wallet connection interface

### Custom Hooks

- `use-metamask.tsx` - Wallet connection logic
- `use-wallet-balance.ts` - Real-time balance polling
- `use-gas-estimate.ts` - Gas fee calculation
- `use-contract-interaction.ts` - Smart contract calls

## 🚀 Getting Started

### Prerequisites

- Node.js 18+ and npm
- MetaMask browser extension (for Ethereum)
- Ganache CLI or Ganache GUI (for local Ethereum testing)
- Algorand SDK (for Algorand testing)

### Quick Start

Choose your implementation:

1. **[Ethereum Implementation](./Atomic%20Settlement%20-%20Ethereum/README.md)** - For Ethereum blockchain
2. **[Algorand Implementation](./Atomic%20Settlement%20-%20Algorand/README.md)** - For Algorand blockchain

Each has its own detailed setup and running instructions.

## 💡 How It Works

### Transaction Flow

1. **User Connects Wallet** - Authenticates via MetaMask or Algorand wallet
2. **Initiates Payment** - Specifies amount and merchant
3. **Deposits Funds** - Amount + gas locked in smart contract
4. **Transaction Locked** - Funds held securely in escrow
5. **Merchant Action** - Receives funds or transaction times out
6. **Settlement** - Funds finalized or refunded automatically

### Gas Estimation

- Real-time calculation based on network conditions
- 20% safety buffer included
- Updates every 10 seconds
- Validates sufficient user balance

### Balance Management

- Polls blockchain every 3 seconds
- Updates after transactions
- Formatted display (4 decimal places)
- Real-time in UI

## 📊 Technology Stack

| Layer | Technology |
|-------|-----------|
| **Smart Contracts** | Solidity (Ethereum), PyTeal (Algorand) |
| **Frontend** | React 18, TypeScript, Vite |
| **Styling** | Tailwind CSS, shadcn/ui components |
| **Web3** | ethers.js (Ethereum), Algorand SDK |
| **Testing** | Ganache, Hardhat |
| **Dev Tools** | TypeScript, Vite, ESLint |

## 🔧 Development

### Ethereum Workflow

```bash
# Install dependencies
cd "Atomic Settlement - Ethereum"
npm install

# Start Ganache (separate terminal)
ganache

# Deploy contracts
npm run deploy

# Start dev server
npm run dev
```

### Algorand Workflow

```bash
# Install dependencies
cd "Atomic Settlement - Algorand/final/bankfirst"
npm install

# Configure Algorand
npm run configure

# Start dev server
npm run dev
```

## 🧪 Testing

### Local Testing (Ethereum)

- Use Ganache with predefined accounts and private keys
- Deploy contracts to local testnet
- Test with MetaMask connected to Ganache
- Simulate fund transfers and escrow conditions

### Testnet Testing (Algorand)

- Connect to Algorand TestNet
- Use Algorand wallet with testnet funds
- Test atomic swaps and settlement logic

## 📚 Key Files & Documentation

- **IMPLEMENTATION_COMPLETE.md** (Ethereum) - Detailed implementation summary
- **QUICK_START.md** (Ethereum) - 2-minute setup guide
- **METAMASK_SETUP.md** (Ethereum) - MetaMask configuration
- **contracts/AtomicEscrow.sol** - Smart contract source
- **client/src/hooks/** - React hooks for blockchain interaction

## 🎯 Use Cases

1. **Payment Processing** - Secure peer-to-peer payments
2. **Escrow Services** - Third-party payment holding
3. **Atomic Swaps** - Cross-chain token exchanges
4. **Marketplace Settlement** - Buyer protection on decentralized marketplaces
5. **Conditional Payments** - Funds released on condition verification

## 🔐 Security Features

- ✅ Funds locked in smart contract (not transferred during deposit)
- ✅ Automatic 60-second refund timeout
- ✅ Validation of sufficient user balance before transaction
- ✅ Real-time gas estimation prevents under-funding
- ✅ Transaction receipts and status tracking
- ✅ Error handling and rollback capabilities

## 📈 Current Status

- ✅ **Ethereum**: Fully implemented and tested
- ✅ **Smart Contracts**: Deployed and functional
- ✅ **Web UI**: Production-ready interface
- ✅ **MetaMask Integration**: Working with Ganache
- 🔄 **Algorand**: In development
- 📝 **Documentation**: Complete

## 🤝 Contributing

This project demonstrates atomic settlement principles across different blockchain architectures. Each implementation can serve as a template for:

- Building decentralized escrow systems
- Understanding blockchain transaction lifecycle
- Integrating Web3 wallets into applications
- Gas optimization and real-time estimation

## 📄 License

This project is provided as educational material. Refer to individual blockchain framework licenses (Solidity, PyTeal, ethers.js, Algorand SDK).

## 📞 Support & Documentation

For implementation-specific details:
- See [Ethereum README](./Atomic%20Settlement%20-%20Ethereum/README.md)
- See [Algorand README](./Atomic%20Settlement%20-%20Algorand/README.md)

---

**Built with attention to blockchain best practices, security, and user experience.**

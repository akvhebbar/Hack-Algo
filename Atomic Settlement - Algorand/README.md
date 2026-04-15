# 🔗 Atomic Settlement - Algorand Implementation

A complete Algorand-based atomic settlement (escrow) system leveraging the Algorand blockchain's speed and efficiency. This implementation demonstrates atomic swaps, escrow functionality, and real-time wallet integration on the Algorand network.

---

## 📋 Overview

This Algorand implementation provides:

- **Atomic Swaps** - Trustless exchange of assets
- **Smart Contracts** - PyTeal-based escrow logic
- **Wallet Integration** - Algorand wallet connection
- **Real-time Updates** - Balance and transaction tracking
- **TestNet Support** - Test on Algorand TestNet
- **Production Ready** - Full error handling and validation

### Why Algorand?

✅ **Speed** - Sub-second finality  
✅ **Scalability** - High throughput, low latency  
✅ **Cost** - Minimal transaction fees  
✅ **Energy Efficient** - Pure Proof of Stake consensus  
✅ **Developer Friendly** - PyTeal smart contracts  
✅ **Atomic Swaps** - Native support via Atomic Swap Protocol  

---

## 🚀 Quick Start (10 Minutes)

### Prerequisites

- Node.js 18+ and npm
- Algorand SDK for JavaScript/Python
- Algorand TestNet account with funds
- AlgoExplorer or similar faucet for test tokens
- Algorand wallet (AlgoSigner, MyAlgo, Pera Wallet)

### 1. Install Dependencies

```bash
cd "Atomic Settlement - Algorand/final/bankfirst"
npm install
```

### 2. Configure Algorand Network

```bash
npm run configure
```

This sets up connections to:
- **Algorand TestNet** (recommended for development)
- **Algorand MainNet** (for production)
- **Locally running Algorand node** (optional)

### 3. Deploy Smart Contract

```bash
npm run deploy
```

This will:
- Compile PyTeal contracts
- Deploy to configured network
- Show contract addresses
- Save deployment info

### 4. Start Dev Server

```bash
npm run dev
```

Server runs on `http://localhost:5173`

### 5. Connect Wallet

1. Install Algorand wallet (Pera Wallet recommended)
2. Import or create account on TestNet
3. Go to `http://localhost:5173`
4. Click **"Connect Wallet"**
5. Approve connection in wallet
6. ✅ Ready to test!

---

## 📂 Project Structure

```
Atomic Settlement - Algorand/
├── final/
│   └── bankfirst/
│       ├── projects/
│       │   └── bankfirst/
│       │       ├── src/
│       │       │   ├── components/           # React components
│       │       │   │   ├── layout/
│       │       │   │   │   ├── Hero.tsx
│       │       │   │   │   └── Navbar.tsx     # Wallet connection - Algorand
│       │       │   │   ├── panels/
│       │       │   │   │   ├── InitiatePanel.tsx      # Payment initiation
│       │       │   │   │   ├── PipelinePanel.tsx      # Status pipeline
│       │       │   │   │   └── SimulationPanel.tsx    # Testing
│       │       │   │   └── ui/                # shadcn/ui components
│       │       │   │
│       │       │   ├── hooks/                # Custom React hooks
│       │       │   │   ├── use-algorand-wallet.ts    # Wallet connection
│       │       │   │   ├── use-account-balance.ts    # Balance polling
│       │       │   │   ├── use-fee-estimate.ts       # Fee calculation
│       │       │   │   ├── use-contract-call.ts      # Contract interactions
│       │       │   │   ├── use-simulation.ts         # Simulation logic
│       │       │   │   └── use-toast.ts              # Notifications
│       │       │   │
│       │       │   ├── lib/
│       │       │   │   ├── algorand.ts       # Algorand SDK setup
│       │       │   │   ├── contract.ts       # Contract addresses & ABIs
│       │       │   │   ├── utils.ts          # Helper functions
│       │       │   │   └── queryClient.ts    # React Query setup
│       │       │   │
│       │       │   ├── store/
│       │       │   │   └── use-store.ts      # Global state (Zustand)
│       │       │   │
│       │       │   ├── pages/
│       │       │   │   ├── Home.tsx
│       │       │   │   └── not-found.tsx
│       │       │   │
│       │       │   ├── App.tsx               # Main app
│       │       │   ├── main.tsx              # Entry point
│       │       │   └── index.css             # Styles
│       │       │
│       │       ├── contracts/
│       │       │   ├── escrow.py             # PyTeal escrow contract
│       │       │   ├── atomic_swap.py        # Atomic swap logic
│       │       │   └── build/                # Compiled contracts
│       │       │
│       │       ├── scripts/
│       │       │   ├── deploy.ts             # Deployment script
│       │       │   ├── compile.py            # Contract compilation
│       │       │   └── test.ts               # Testing script
│       │       │
│       │       ├── server/
│       │       │   ├── index.ts              # Express server
│       │       │   ├── routes.ts             # API endpoints
│       │       │   ├── algorand-client.ts    # Algorand node connection
│       │       │   └── storage.ts            # Data persistence
│       │       │
│       │       ├── package.json
│       │       ├── tsconfig.json
│       │       ├── vite.config.ts
│       │       ├── tailwind.config.ts
│       │       │
│       │       ├── README.md
│       │       ├── SETUP.md                  # Detailed setup guide
│       │       └── contracts.md              # Contract documentation
│       │
│       └── README.md
│
└── README.md (this file)
```

---

## 🏗️ Smart Contracts - Algorand

### Supported Contract Types

#### 1. **Escrow Contract** (escrow.py)

Standard 2-of-3 escrow supporting atomic settlement:

```python
@Contract
def escrow(
    user: TxnObject,
    merchant: TxnObject,
    escrow_agent: TxnObject,
    amount: uint64
):
    # Deposit phase
    deposit(user, amount)
    
    # Lock phase
    assert txn.sender == user or txn.sender == escrow_agent
    
    # Release phase
    assert is_finalized()
    transfer_to_merchant(merchant, amount)
```

**Key Features:**
- Atomic transactions (all-or-nothing)
- Multi-signature support
- Time-locked releases
- Condition-based transfers

#### 2. **Atomic Swap Contract** (atomic_swap.py)

Cross-asset swap with HashLock/TimeLock:

```python
@Contract
def atomic_swap(
    initiator: Address,
    responder: Address,
    asset_a: uint64,
    asset_b: uint64,
    amount_a: uint64,
    amount_b: uint64,
    hash_lock: bytes[32],
    time_lock: uint64
):
    # Initiate swap
    receive_asset_a(initiator, amount_a)
    
    # Complete swap
    reveal_preimage(hash_lock)
    transfer_asset_a_to_responder()
    transfer_asset_b_to_initiator()
```

**Features:**
- Hash-locked swaps
- Time-locked escrow
- Multi-asset support
- Non-custodial settlement

---

## 💻 Frontend Components

### Algorand Wallet Integration

Supports all major Algorand wallets:

- **Pera Wallet** (formerly Official Wallet)
- **AlgoSigner** (by PureStake)
- **MyAlgo Wallet** (web-based)
- **Exodus** (multi-chain wallet)

### InitiatePanel.tsx (Algorand)

Payment initiation with Algorand specifics:

- **Real Balance Display** - Microalgos (μALGO)
- **Amount Input** - Configurable payment
- **Real Fee Estimation** - Current Algorand fees
- **Total Calculation** - Amount + fees
- **ASA Support** - Standard Assets for custom tokens
- **Timeout Configuration** - Round-based (not seconds)

### Navbar.tsx (Algorand)

Algorand wallet connection:

- **Connect Button** - Opens wallet selector
- **Address Display** - Account address
- **Balance Badge** - μALGO balance
- **Network Indicator** - TestNet/MainNet
- **Account Selector** - Switch between accounts

### PipelinePanel.tsx (Algorand)

Transaction pipeline with Algorand states:

- **Pending** - Transaction submitted
- **Confirmed** - Included in block
- **Finalized** - On confirmed branch
- **Settled** - All transactions complete

---

## 🎣 Custom React Hooks

### use-algorand-wallet.ts

Manages Algorand wallet connection:

```typescript
const {
  address,        // Connected account address
  isConnected,    // Connection state
  connect,        // Select wallet and connect
  disconnect,     // Disconnect wallet
  walletType,     // Type of wallet (Pera, AlgoSigner, etc)
  error          // Connection errors
} = useAlgorandWallet();
```

### use-account-balance.ts

Real-time balance polling on Algorand:

```typescript
const {
  balanceMicroAlgos,      // Raw balance in μALGO
  balanceAlgo,            // Formatted in ALGO
  assets,                 // Array of ASAs held
  loading,
  error,
  refetch
} = useAccountBalance();
```

**Features:**
- Polls every 5 seconds
- ASA (Standard Asset) support
- Tracks multiple assets
- Real round-by-round updates

### use-fee-estimate.ts

Algorand fee calculation:

```typescript
const {
  minFee,              // Minimum fee (1000 microAlgos)
  estimatedFee,        // For specific transaction
  totalCost,           // Amount + fees
  transactionSize,     // TX size in bytes
  loading,
  error
} = useFeeEstimate();
```

### use-contract-call.ts

Smart contract interaction on Algorand:

```typescript
const {
  callContract,        // async (contractId, method, args)
  callAtomic,          // async (txns[]) - Atomic group
  waitForConfirmation, // async (txnId)
  loading,
  error,
  lastTxnId
} = useContractCall();
```

---

## 🔐 Algorand Transaction Security

### Atomic Groups

All settlement transactions use atomic groups:

```typescript
// Multiple transactions, all succeed or all fail
const txns = [
  deposit_txn,
  contract_call_txn,
  settlement_txn
];

const result = await callAtomic(txns);
// All execute together, or none do
```

### Zero-Copy Transactions

Algorand transactions are atomic by nature:
- ✅ No risk of partial execution
- ✅ Built-in atomic swaps
- ✅ Time-lock support via transaction pools

### Rekeying Safety

Contracts use rekeying carefully:
- ✅ Only during setup phase
- ✅ To authorized parties only
- ✅ Never during settlement

---

## 🧪 Testing

### TestNet Testing

1. Get TestNet ALGO from faucet:
   ```
   https://dispenser.testnet.aws.algodev.network/
   ```

2. Run test scenarios:
   ```bash
   npm run test
   ```

3. Test atomic swaps:
   ```bash
   npm run test:swap
   ```

### Use Cases to Test

- **Simple 2-of-2 Escrow** - User → Merchant
- **3-of-3 Escrow** - User → Merchant with Arbitrator
- **ASA Swaps** - Exchange different tokens
- **Hash-locked Swaps** - Time-locked with preimage
- **Timeout Refunds** - Automatic refund after timeout

---

## 📊 Fee Structure

### Algorand Fees

- **Minimum Fee:** 1,000 microAlgos (0.001 ALGO)
- **Per Byte:** 400 microAlgos per additional 128 bytes
- **Smart Contract:** Base fee + additional microAlgos
- **ASA Transfer:** 1,000 microAlgos base

### Cost Comparison

| Operation | Algorand | Ethereum |
|-----------|----------|----------|
| **Simple Transfer** | 0.001 ALGO (~$0.0002) | 50k gas |
| **Contract Call** | 0.005-0.01 ALGO (~$0.001) | 100k gas |
| **ASA Transfer** | 0.001 ALGO | High gas |
| **Atomic Swap** | 0.005 ALGO (~$0.001) | 200k gas |

Algorand is **100-1000x cheaper** than Ethereum!

---

## 🌐 Algorand Network Configuration

### TestNet Configuration

```
Network: Algorand TestNet
Node URL: https://testnet-api.algonode.cloud
Indexer: https://testnet-idx.algonode.cloud
Genesis ID: SGO1GKSj3IqM92NCTXiPAbDzgsC3DUdrNP7yM8Dri7I=
```

### MainNet Configuration

```
Network: Algorand MainNet
Node URL: https://mainnet-api.algonode.cloud
Indexer: https://mainnet-idx.algonode.cloud
Genesis ID: wGHE2Pwdvd7S12BL5FaOP20EGYesN73ktiC1qzkkit8=
```

### Local Node (Optional)

For full local development:

```bash
# Install Algorand node
# Follow: https://developer.algorand.org/docs/run-a-node/setup/

# Start sandbox
goal node status
```

---

## 📈 Transaction Lifecycle

### Escrow Flow

```
1. User connects Algorand wallet
   ↓
2. User enters amount (in ALGO)
   ↓
3. System estimates fee
   ↓
4. User reviews transaction
   ↓
5. Wallet signs transaction
   ↓
6. Transaction broadcast to network
   ↓
7. Pending state (in TX pool)
   ↓
8. Included in block (first confirmation)
   ↓
9. Confirmed (multiple blocks)
   ↓
10A. Merchant claims → Funds released ✅
10B. Timeout → Auto-refund ✅
```

---

## 🚀 Deployment

### TestNet Deployment

```bash
# 1. Compile contracts
npm run compile

# 2. Deploy to TestNet
npx hardhat run scripts/deploy.ts --network testnet

# 3. Save deployment info
npm run save-config
```

### MainNet Deployment

```bash
# 1. Run with extra validation
npx hardhat run scripts/deploy.ts --network mainnet

# 2. Verify contract (optional, use blockchain explorer)

# 3. Update mainnet config
npm run update:mainnet
```

---

## 🐛 Troubleshooting

### "Wallet Not Connecting"

**Solution:** 
- Make sure wallet extension is installed
- Try a different wallet (Pera > AlgoSigner > MyAlgo)
- Restart browser

### "Transaction Rejected"

**Solution:**
- Check account has sufficient balance (amount + fees)
- Verify you're on correct network (TestNet or MainNet)
- Try increasing timeout rounds

### "Contract Call Failed"

**Solution:**
- Check contract is deployed to correct app ID
- Verify contract arguments match signature
- Check account hasn't been rekeyed

### "Timeout on Confirmation"

**Solution:**
- Use indexer to check transaction status
- Algorand network might be congested
- Wait a few blocks and retry

### "Invalid Signature"

**Solution:**
- Make sure you're signing with correct account
- Verify transaction wasn't modified
- Check network hasn't had a fork

---

## 📚 Resources

### Algorand Developer Docs
- [Algorand Docs](https://developer.algorand.org)
- [AVM Documentation](https://developer.algorand.org/docs/get-details/dapps/avm/)
- [PyTeal Documentation](https://pyteal.readthedocs.io/)
- [JavaScript SDK](https://github.com/algorand/js-algorand-sdk)

### Community & Tools
- [AlgoExplorer](https://algoexplorer.io/) - Block explorer
- [RandomSwap](https://dispenser.testnet.aws.algodev.network) - TestNet faucet
- [Algo Faucet](https://testnet-dispenser.algorand.org/) - Alternative faucet
- [PureStake API](https://www.purestake.com/) - Node provider

### Wallet Documentation
- [Pera Wallet Docs](https://wallet.perawallet.app/docs)
- [AlgoSigner Docs](https://github.com/PureStake/algosigner)
- [MyAlgo Docs](https://wallet.myalgo.com/)

---

## 🎯 Next Steps

1. ✅ Install and configure Algorand wallet
2. ✅ Get TestNet ALGO from faucet
3. ✅ Deploy contracts to TestNet
4. ✅ Test escrow flow
5. ✅ Test atomic swaps
6. ✅ Test timeout refunds
7. 📝 Deploy to MainNet
8. 🚀 Launch production system

---

## 💡 Advanced Features

### Custom Assets (ASAs)

Transfer any Algorand Standard Asset:

```typescript
// Create ASA
const txn = algosdk.makeAssetTransferTxnWithSuggestedParams(
  sender,
  assetIndex,     // ASA ID
  receiver,
  0,              // Amount
  params
);
```

### AVM 1.0 & 2.0 Support

- **AVM 1.0** - Basic smart contracts
- **AVM 2.0** - Advanced features, better optimization

### State Proofs

Verify Algorand state on other chains:

```python
# In cross-chain settlement
verify_state_proof(algorand_commitment, mainnet_header)
```

---

## 📊 Mainnet Live

Coming soon:
- Full TestNet validation
- Mainnet contract deployment
- Analytics dashboard
- Cross-chain bridges to Ethereum

---

**Built with Algorand's speed, security, and developer experience in mind.**

*Questions? Check out [Algorand Developer Docs](https://developer.algorand.org) or [GitHub Discussions](https://github.com/algorand/go-algorand/discussions)*

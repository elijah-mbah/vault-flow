# VaultFlow Protocol

[![License: ISC](https://img.shields.io/badge/License-ISC-blue.svg)](https://opensource.org/licenses/ISC)
[![Clarity Version](https://img.shields.io/badge/Clarity-3.0-purple.svg)](https://docs.stacks.co/clarity)
[![Stacks Epoch](https://img.shields.io/badge/Stacks%20Epoch-3.1-orange.svg)](https://docs.stacks.co)

**Advanced collateral-based lending infrastructure enabling seamless capital deployment with automated risk management.**

## Overview

VaultFlow revolutionizes decentralized finance by providing a sophisticated lending ecosystem where users can maximize capital efficiency through intelligent collateral utilization. The protocol features dynamic risk assessment, automated liquidation mechanisms, and flexible borrowing positions designed for both retail and institutional participants.

Built with security-first architecture and optimized for scalability on the Stacks blockchain infrastructure, VaultFlow enables users to:

- **Deposit STX as collateral** to unlock borrowing capacity
- **Borrow against collateral** with flexible terms and competitive rates
- **Maintain optimal capital efficiency** through intelligent position management
- **Participate in liquidations** to earn rewards while securing the protocol

## Key Features

### 🔒 **Secure Collateral Management**

- Over-collateralized lending with configurable ratios (minimum 110%, default 150%)
- Real-time collateral ratio monitoring and risk assessment
- Automated liquidation protection at 130% threshold

### 💰 **Flexible Borrowing**

- Borrow STX tokens against deposited collateral
- Dynamic interest rate calculations based on utilization
- Partial repayment and withdrawal capabilities

### ⚡ **Automated Risk Management**

- Real-time liquidation engine to maintain protocol solvency
- Configurable risk parameters for different market conditions
- Protocol fee structure (1% default, max 10%)

### 🛡️ **Protocol Security**

- Comprehensive parameter validation and safety checks
- Administrative controls with multi-sig governance support
- Battle-tested mathematical models for risk calculations

## Protocol Mechanics

### Collateral Requirements

| Parameter | Default Value | Range | Description |
|-----------|---------------|-------|-------------|
| Minimum Collateral Ratio | 150% | 110% - 500% | Required over-collateralization |
| Liquidation Threshold | 130% | 110% - MCR | Automatic liquidation trigger |
| Protocol Fee | 1% | 0% - 10% | Service fee on borrowing |

### Core Functions

#### User Operations

- `deposit()` - Deposit STX as collateral
- `borrow(amount)` - Borrow STX against collateral
- `repay(amount)` - Repay borrowed amount
- `withdraw(amount)` - Withdraw excess collateral
- `liquidate(user)` - Liquidate under-collateralized positions

#### Information Queries

- `get-user-position(user)` - Retrieve user's portfolio data
- `get-protocol-stats()` - Current protocol statistics

#### Administrative Controls (Owner Only)

- `set-minimum-collateral-ratio(ratio)` - Update collateral requirements
- `set-liquidation-threshold(threshold)` - Adjust liquidation parameters
- `set-protocol-fee(fee)` - Configure service fees

## Getting Started

### Prerequisites

- [Clarinet CLI](https://github.com/hirosystems/clarinet) v1.8.0+
- [Node.js](https://nodejs.org/) v18.0.0+
- [npm](https://www.npmjs.com/) or [yarn](https://yarnpkg.com/)

### Installation

1. **Clone the repository**

   ```bash
   git clone https://github.com/elijah-mbah/vault-flow.git
   cd vault-flow
   ```

2. **Install dependencies**

   ```bash
   npm install
   ```

3. **Run contract checks**

   ```bash
   clarinet check
   ```

### Development Workflow

#### Running Tests

```bash
# Run all tests
npm test

# Run tests with coverage and cost analysis
npm run test:report

# Watch mode for continuous testing
npm run test:watch
```

#### Contract Validation

```bash
# Check contract syntax and types
clarinet check

# Interactive REPL for testing
clarinet console
```

#### Deployment

```bash
# Deploy to testnet
clarinet integrate

# Deploy to mainnet (requires configuration)
clarinet deploy --network mainnet
```

## Usage Examples

### Basic Lending Flow

```clarity
;; 1. Deposit STX as collateral
(contract-call? .vault-flow deposit)

;; 2. Borrow against collateral (ensure adequate ratio)
(contract-call? .vault-flow borrow u1000000) ;; 1 STX

;; 3. Check position status
(contract-call? .vault-flow get-user-position tx-sender)

;; 4. Repay borrowed amount
(contract-call? .vault-flow repay u500000) ;; 0.5 STX

;; 5. Withdraw excess collateral
(contract-call? .vault-flow withdraw u500000) ;; 0.5 STX
```

### Liquidation Example

```clarity
;; Liquidate under-collateralized user
(contract-call? .vault-flow liquidate 'SP1HTBVD3JG9C05J7HDJKDYR99M9GKPQJSDK3P7J4S)
```

### Administrative Operations

```clarity
;; Update collateral requirements (owner only)
(contract-call? .vault-flow set-minimum-collateral-ratio u180) ;; 180%

;; Adjust liquidation threshold (owner only)
(contract-call? .vault-flow set-liquidation-threshold u140) ;; 140%
```

## Architecture

### Contract Structure

```text
vault-flow.clar
├── Constants & Configuration
│   ├── Error codes (u100-u107)
│   ├── Protocol limits
│   └── Default parameters
├── Data Structures
│   ├── loans (individual loan records)
│   └── user-positions (portfolio tracking)
├── Core Functions
│   ├── deposit/withdraw
│   ├── borrow/repay
│   └── liquidation engine
└── Administrative Controls
    ├── Parameter updates
    └── Risk management
```

### Security Model

1. **Over-collateralization**: All loans require 150%+ collateral backing
2. **Liquidation Protection**: Automatic liquidation at 130% prevents bad debt
3. **Parameter Validation**: All inputs validated against protocol limits
4. **Access Controls**: Administrative functions restricted to contract owner

## Risk Management

### Collateral Ratio Monitoring

The protocol continuously monitors collateral ratios to ensure system stability:

```clarity
;; Collateral ratio = (collateral * 100) / debt
;; Must maintain >= minimum-collateral-ratio
```

### Liquidation Process

When positions fall below the liquidation threshold:

1. **Detection**: Automated monitoring identifies at-risk positions
2. **Execution**: Liquidators can claim collateral at a discount
3. **Settlement**: Position is cleared from protocol records
4. **Protection**: Protocol maintains solvency through over-collateralization

## Testing

### Test Coverage

The protocol includes comprehensive test coverage for:

- ✅ Collateral deposit and withdrawal
- ✅ Borrowing and repayment flows
- ✅ Liquidation mechanisms
- ✅ Administrative parameter updates
- ✅ Edge cases and error conditions

### Test Coverage Validation

```bash
# Basic test suite
npm test

# Detailed reporting with gas costs
npm run test:report

# Continuous testing during development
npm run test:watch
```

## Contributing

We welcome contributions to improve VaultFlow Protocol! Please follow these guidelines:

1. **Fork the repository** and create your feature branch
2. **Write tests** for any new functionality
3. **Ensure all tests pass** with `npm test`
4. **Follow Clarity best practices** and maintain code quality
5. **Submit a pull request** with detailed description

### Development Standards

- Follow [Clarity coding standards](https://docs.stacks.co/clarity/language-functions)
- Maintain test coverage above 90%
- Document all public functions with clear descriptions
- Use semantic versioning for releases

## Security Considerations

### Audit Status

🔶 **This protocol is under development and has not been audited. Use at your own risk.**

### Known Considerations

- **Smart Contract Risk**: Potential bugs in contract logic
- **Liquidation Risk**: Market volatility may trigger unexpected liquidations
- **Governance Risk**: Administrative controls require trusted operation
- **Stacks Network Risk**: Dependency on Stacks blockchain infrastructure

### Reporting Vulnerabilities

Please report security vulnerabilities responsibly:

1. **DO NOT** create public issues for security vulnerabilities
2. **Email** security concerns to [security@vaultflow.dev]
3. **Provide** detailed reproduction steps and impact assessment
4. **Allow** reasonable time for investigation and patching

## License

This project is licensed under the ISC License - see the [LICENSE](LICENSE) file for details.

## Resources

- **Documentation**: [VaultFlow Docs](https://docs.vaultflow.dev)
- **Stacks Documentation**: [docs.stacks.co](https://docs.stacks.co)
- **Clarity Language**: [Clarity Reference](https://docs.stacks.co/clarity)
- **Community**: [Discord](https://discord.gg/vaultflow) | [Twitter](https://twitter.com/vaultflow)

## Roadmap

### Phase 1: Core Protocol ✅

- [x] Basic lending and borrowing
- [x] Collateral management
- [x] Liquidation engine
- [x] Test suite

### Phase 2: Enhanced Features 🚧

- [ ] Multi-asset collateral support
- [ ] Dynamic interest rates
- [ ] Governance token integration
- [ ] Advanced liquidation strategies

### Phase 3: Ecosystem Integration 📋

- [ ] DEX integration for automated liquidations
- [ ] Oracle price feeds
- [ ] Cross-chain compatibility
- [ ] Mobile application

---

Built with ❤️ on Stacks by the VaultFlow Team

VaultFlow Protocol - Revolutionizing DeFi lending on Bitcoin through Stacks

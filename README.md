# Bitcoin-Backed Stablecoin Vault Protocol

A decentralized finance protocol enabling minting of stablecoins against STX collateral on Stacks L2 while maintaining Bitcoin compliance.

## Overview

This smart contract implements a non-custodial collateralized debt position (CDP) system where:

- Users lock STX (Stacks tokens) as collateral
- Mint algorithmically stabilized tokens against collateral
- Maintains minimum over-collateralization ratio (MCR)
- Automated liquidations preserve protocol solvency
- Governance-controlled risk parameters
- Bitcoin-finalized transaction security

## Key Features

### 1. STX Collateralization Engine

- Native STX token deposits via Stacks L2
- Real-time collateral valuation using price feeds
- Over-collateralization requirements (150%+)

### 2. Dynamic Risk Management

- Configurable parameters:
  - Minimum Collateral Ratio (MCR)
  - Liquidation Ratio (LR)
  - Stability Fees
- Governance-controlled via on-chain voting

### 3. Decentralized Price Feeds

- Multi-oracle support with validity checks
- Sanity bounds for price inputs ($0.01 - $10M/BTC)
- Oracle authorization/revocation functions

### 4. Liquidation Engine

- Permissionless liquidation triggers
- Collateral seizure mechanism
- Atomic position closure
- 10% liquidation buffer (MCR 150% vs LR 120%)

### 5. Protocol Safety

- Emergency shutdown mechanism
- Time-based fee accrual
- Collateral ratio monitoring
- Governance circuit breakers

## Technical Specifications

| Component               | Details                          |
| ----------------------- | -------------------------------- |
| Blockchain              | Stacks L2 (Bitcoin-secured)      |
| Smart Contract Language | Clarity 2.0                      |
| Collateral Asset        | STX (μSTX precision)             |
| Price Oracle            | BTC/USD (Cents precision)        |
| Risk Parameters         | MCR: 150%, LR: 120%, Fee: 2% APR |
| Governance              | On-chain token governance        |

## Contract Architecture

```solidity
+-----------------------+
|   Vault Management    |
|-----------------------|
| - create-vault        |
| - mint-stablecoin     |
| - repay-debt          |
| - withdraw-collateral |
+-----------------------+

+-----------------------+
|  Risk & Liquidation   |
|-----------------------|
| - liquidate           |
| - update-price        |
| - check-ratios        |
+-----------------------+

+-----------------------+
|   Governance Engine   |
|-----------------------|
| - set-risk-parameters |
| - manage-oracles      |
| - emergency-shutdown  |
+-----------------------+
```

## Usage

### Prerequisites

- Stacks L2 wallet (Hiro Wallet recommended)
- STX tokens for collateral
- Testnet STX for development

### Contract Interaction

1. **Initialize Protocol**

```clarity
(contract-call? .stablecoin-vault initialize initial-btc-price)
```

2. **Create Vault**

```clarity
(contract-call? .stablecoin-vault create-vault 500000000) ;; 500 STX
```

3. **Mint Stablecoins**

```clarity
(contract-call? .stablecoin-vault mint-stablecoin 10000) ;; $100
```

4. **Monitor Position**

```clarity
(contract-call? .stablecoin-vault get-vault-health user-address)
```

5. **Liquidate Position**

```clarity
(contract-call? .stablecoin-vault liquidate undercollateralized-address)
```

## Security Considerations

### Protocol Risks

- Oracle manipulation vectors
- Collateral volatility exposure
- Governance attack surfaces
- Smart contract vulnerabilities

### Best Practices

- Maintain 25%+ collateral buffer above MCR
- Monitor price feed validity status
- Use multi-sig for governance operations
- Regular protocol parameter reviews

## Governance

### Controlled Parameters

| Parameter                | Default | Range       |
| ------------------------ | ------- | ----------- |
| Minimum Collateral Ratio | 150%    | 101%-1000%  |
| Liquidation Ratio        | 120%    | 100%-MCR    |
| Stability Fee            | 2% APR  | 0%-100% APR |
| Oracle Authorization     | N/A     | Whitelist   |

```clarity
;; Governance proposal example
(contract-call? .stablecoin-vault set-risk-parameters u175 u130 u3)
```

# Bitcoin Analytics Protocol (BAP) - Smart Contract Documentation

![Stacks Layer 2](https://img.shields.io/badge/Blockchain-Stacks%20Layer%202-orange)
![Fungible Token](https://img.shields.io/badge/Standard-SIP010-blue)

Decentralized governance and staking protocol for Bitcoin-aligned analytics infrastructure

## Overview

The Bitcoin Analytics Protocol (BAP) is a Stacks Layer 2 smart contract system enabling:

- STX staking with tiered reward mechanisms
- On-chain governance through proposal voting
- Dynamic reward distribution with lock-up bonuses
- Emergency protocol controls with Bitcoin-compliant security

## Key Features

### 1. Multi-Tier Staking System

- Configurable lock periods (0, 30, 60 days)
- Base reward rate (5%) + bonus rate (1-1.5x multiplier)
- Minimum stake: 1,000,000 µSTX (1 STX)
- Cooldown-protected unstaking (1440 blocks/~24hrs)

### 2. On-Chain Governance

- Proposal creation with minimum voting power threshold
- Stake-weighted voting system
- Configurable voting periods (100-2880 blocks)
- Automatic vote tallying and execution controls

### 3. Tiered Reward System

| Tier | Minimum STX | Multiplier | Features Enabled   |
| ---- | ----------- | ---------- | ------------------ |
| 1    | 1 STX       | 1x         | Basic voting       |
| 2    | 5 STX       | 1.5x       | Enhanced analytics |
| 3    | 10 STX      | 2x         | Premium features   |

### 4. Security Architecture

- Bitcoin-finalized transaction validation
- Emergency mode activation
- Contract pause/unpause controls
- Cooldown-protected asset withdrawals

## Technical Specification

### Core Components

- **ANALYTICS-TOKEN**: SIP-010 fungible reward token
- **User Positions**: Track staking/earning positions
- **Proposals Map**: Governance proposal storage
- **Tier Configuration**: Reward parameters storage

### State Variables

```clarity
(define-data-var contract-paused bool false)
(define-data-var emergency-mode bool false)
(define-data-var stx-pool uint u0)
(define-data-var base-reward-rate uint u500)  // 5%
(define-data-var minimum-stake uint u1000000) // 1 STX
```

## Function Reference

### Staking Operations

| Function           | Parameters            | Description                        |
| ------------------ | --------------------- | ---------------------------------- |
| `stake-stx`        | (amount, lock-period) | Initiate STX stake with lock-up    |
| `initiate-unstake` | (amount)              | Start cooldown for withdrawal      |
| `complete-unstake` | -                     | Finalize withdrawal after cooldown |

### Governance Functions

| Function           | Parameters            | Description                    |
| ------------------ | --------------------- | ------------------------------ |
| `create-proposal`  | (description, period) | Submit new governance proposal |
| `vote-on-proposal` | (proposal-id, vote)   | Cast stake-weighted vote       |

### Contract Control

| Function              | Description                    |
| --------------------- | ------------------------------ |
| `pause-contract`      | Emergency pause all operations |
| `resume-contract`     | Resume normal operations       |
| `initialize-contract` | Admin setup function           |

## Reward Calculation

### Formula

```
Rewards = (Staked Amount × Base Rate × Multiplier × Blocks Staked) / 14,400,000
```

### Multiplier Components

1. **Tier Multiplier**: 1x-2x based on total stake
2. **Lock Period Bonus**:
   - No lock: 1x
   - 30 days: 1.25x
   - 60 days: 1.5x

## Governance Process

1. **Proposal Creation**

   - Minimum 1M voting power required
   - 256-character description limit
   - Configurable voting period (100-2880 blocks)

2. **Voting**

   - Weighted by staked amount + tier bonuses
   - Requires active staking position
   - Votes cannot be changed once cast

3. **Execution**
   - Minimum 1M votes threshold
   - Automatic status tracking
   - Time-locked execution capability

## Security Model

### Protection Mechanisms

- Cooldown-protected withdrawals
- Contract pause functionality
- Owner-restricted admin functions
- Bitcoin-block anchored timelocks

### Emergency Protocols

1. Immediate contract pausing
2. STX pool freezing
3. Governance proposal suspension
4. Owner override capabilities

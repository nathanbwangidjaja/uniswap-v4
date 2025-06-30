# Hooked on Uniswap: A Deep Dive into V1–V4

**Authors**: David Chang, Ashley Garcia, Xincheng Zhang, Nathan Wangidjaja  
**Date**: Spring 2025  
**Length**: 55 pages  
**Link to full paper**: [Uniswap_Final_Paper.pdf](https://github.com/nathanbwangidjaja/uniswap-v4/blob/main/Uniswap_Final_Paper__1_%20(1).pdf)  

---

## Contents

- [Overview](#overview)
- [Uniswap V1: Constant Product AMM](#uniswap-v1-constant-product-amm)
- [Uniswap V2: ERC-20 Pairs & Oracles](#uniswap-v2-erc-20-pairs--oracles)
- [Uniswap V3: Concentrated Liquidity](#uniswap-v3-concentrated-liquidity)
- [Uniswap V4: Hooks & Flash Accounting](#uniswap-v4-hooks--flash-accounting)
- [Hook Implementations](#hook-implementations)
- [Mathematical Derivations](#mathematical-derivations)
- [References](#references)

---

## Overview

Uniswap is one of the most widely used decentralized exchanges in DeFi. Over four versions, it has evolved from a basic AMM to a programmable platform capable of hosting complex market logic and interacting with broader on-chain financial primitives.

Our goal with this paper was to document this progression, unpack the underlying math and smart contract design, and propose a few implementation ideas of our own along the way.

---

## Uniswap V1: Constant Product AMM

- Introduced `x * y = k` pricing model
- Only supported ETH–ERC20 pairs
- Liquidity provision and swaps handled through Vyper-based contracts
- 0.3% fee integrated into swap function
- Factory/exchange contract structure

---

## Uniswap V2: ERC-20 Pairs & Oracles

- Support for arbitrary ERC-20 <> ERC-20 pairs
- Price oracles via time-weighted averages
- Flash swaps: borrow tokens and return in same transaction
- Architectural split into core (Pair, Factory) and periphery (Router, Library)
- Initial oracle vulnerabilities addressed through block timestamping

---

## Uniswap V3: Concentrated Liquidity

- Liquidity can be placed within a custom price range `[pa, pb]`
- Tick-based system with geometric progression: price(i) = 1.0001^i
- Each liquidity position is an NFT (ERC-721)
- Support for multiple fee tiers (0.05%, 0.3%, 1%)
- Geometric TWAP oracles, improved capital efficiency
- Significant reduction in impermanent loss risk when used properly

---

## Uniswap V4: Hooks & Flash Accounting

- Hooks enable external smart contracts to intercept and modify protocol logic
- Flash accounting using EIP-1153 for gas savings
- Singleton `PoolManager.sol` manages all pools centrally
- Permissions encoded in hook addresses (bitmask logic)
- Pools support runtime customization (swap logic, fee logic, limit orders, etc.)

---

## Hook Implementations

We cover and provide examples for several hook-based extensions:

- **TWAMM (Time-Weighted Average Market Maker)**  
  Breaks large orders into infinitely small virtual trades for smooth execution.

- **Dynamic Fee Hook**  
  Adjusts LP fees in real time based on on-chain volatility.

- **Limit Order Hook**  
  Integrates off-chain order books with on-chain execution via beforeSwap callbacks.

- **Stop-Loss Hook**  
  Embeds stop-loss logic directly into the AMM lifecycle without external keepers.

---

## Mathematical Derivations

Included throughout the paper are:

- AMM formulas for swap calculations across all versions
- Δx / Δy calculations for price movement between ticks
- Liquidity formulas and their relationship to virtual reserves
- TWAP and geometric mean price calculations
- Dynamic fee logic derivation for volatility-aware hooks

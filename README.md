# 🐶 PuppyRaffle Security Audit

A comprehensive security review of the PuppyRaffle smart contract protocol conducted by **Nandini Gupta**.

---

## 📋 About the Project

PuppyRaffle is a Solidity-based raffle protocol that allows users to enter a lottery by paying an entrance fee. After the raffle duration ends, a winner is selected and awarded both a prize pool and a unique puppy NFT.

The protocol includes functionality for:

- User raffle participation
- Refunds for active entrants
- Winner selection
- Fee collection
- NFT minting for winners

---

## 🎯 Audit Objective

The goal of this audit was to identify vulnerabilities that could impact:

- User funds
- Protocol revenue
- Fairness of winner selection
- Availability of the raffle system
- Gas efficiency
- Code quality and maintainability

---

## 🔍 Scope

### Contracts Reviewed

| Contract | Description |
|-----------|-------------|
| PuppyRaffle.sol | Main raffle protocol |

### Solidity Version

```solidity
pragma solidity ^0.7.1;
```

---

## 📊 Audit Summary

| Severity | Count |
|-----------|---------|
| High | 3 |
| Medium | 4 |
| Low | 1 |
| Gas | 2 |
| Informational | 6 |
| **Total Findings** | **16** |

---

## 🚨 High Severity Findings

### H-1 Reentrancy Attack in `refund()`
A malicious entrant can repeatedly call the refund function through a fallback mechanism and drain the raffle contract balance.

**Impact:** Loss of all participant funds.

---

### H-2 Weak Randomness in `selectWinner()`
The protocol uses predictable on-chain values (`msg.sender`, `block.timestamp`, `block.difficulty`) to generate randomness.

**Impact:** Attackers can influence or predict raffle outcomes.

---

### H-3 Integer Overflow of `totalFees`
The protocol uses Solidity 0.7.x, where arithmetic overflow is unchecked.

**Impact:** Protocol fees can become permanently inaccessible.

---

## ⚠️ Medium Severity Findings

### M-1 Denial of Service Through Duplicate Player Checks
Nested loops used for duplicate detection create significant gas growth as the player list expands.

### M-2 Withdrawal Griefing via Forced ETH Transfers
An attacker can use `selfdestruct()` to force ETH into the contract and block fee withdrawals.

### M-3 Unsafe `uint256 → uint64` Fee Casting
Fee values can be truncated when exceeding the maximum value of `uint64`.

### M-4 Smart Contract Winners Can Block Raffle Completion
Prize transfers may fail if the winning contract cannot receive ETH.

---

## 🟡 Low Severity Findings

### L-1 Ambiguous Return Value in `getActivePlayerIndex()`
The function returns `0` both when a player is not found and when the player exists at index `0`.

---

## ⛽ Gas Optimization Findings

### G-1 Use `constant` and `immutable`
Several state variables can be optimized for lower gas costs.

### G-2 Cache Storage Variables in Loops
Repeated storage reads increase gas consumption unnecessarily.

---

## ℹ️ Informational Findings

- Wide Solidity pragma version
- Outdated Solidity compiler version
- Missing zero-address validations
- CEI pattern violations
- Usage of magic numbers
- Unused internal function

---

## 🛠 Key Security Concepts Demonstrated

This review covers:

- Reentrancy attacks
- Randomness manipulation
- Integer overflow vulnerabilities
- Unsafe type casting
- Denial-of-Service attacks
- Withdrawal griefing
- CEI (Checks-Effects-Interactions)
- Gas optimization techniques
- Solidity best practices

---

## 🧪 Testing

All critical and medium-severity findings include Proof-of-Concept tests written in Foundry to demonstrate exploitability and validate impact.

---

## 📚 Methodology

The audit was performed using:

- Manual code review
- Static analysis
- Threat modeling
- Attack path analysis
- Proof-of-Concept exploit development
- Foundry testing framework

---

## 👩‍💻 Auditor

**Nandini Gupta**

Blockchain Security Researcher | Smart Contract Auditor | Solidity Developer

- Smart Contract Security Reviews
- Foundry Testing & Fuzzing
- Solidity Development
- Web3 Security Research

---

## ⚠️ Disclaimer

This audit represents a security assessment of the codebase reviewed at a specific point in time. No audit can guarantee the absence of vulnerabilities, and additional issues may exist beyond those identified in this report.

---
trigger: model_decision
description: Used to handle the wrapping/unwrapping of WIP (Wrapped IP) tokens within Story.
---

### TL;DR

The **WipClient** facilitates the conversion and management of **Wrapped IP (WIP)** tokens. Much like WETH on Ethereum, WIP is an ERC-20 compatible version of the native IP token, required for protocol actions like paying licensing fees, staking dispute bonds, and distributing royalties.

---

## Core WIP Operations

### 1. Wrapping & Unwrapping

* **deposit**: Converts native **IP** tokens into **WIP**. The resulting WIP is deposited into the caller's wallet.
* **withdraw**: Unwraps **WIP** back into native **IP** tokens.

> **Note:** Many Story Protocol SDK methods (like `raiseDispute` or `payRoyaltyOnBehalf`) include `wipOptions` to handle these conversions automatically, but the WipClient provides manual control.

---

## Token Management (ERC-20 Standards)

Since WIP follows the ERC-20 standard, the client includes the standard methods for balance tracking and movement:

### Permissions & Security

* **approve**: Grants a `spender` (such as a Story Protocol module or an AI agent in your "factory") permission to use a specific amount of your WIP.
* **balanceOf**: A read-only method to check the current WIP holdings of any given address.

### Movement

* **transfer**: Sends a specified `amount` of WIP directly to a recipient (`to`).
* **transferFrom**: Allows an authorized `spender` to move WIP from a `from` address to a `to` address, provided that an allowance was previously set via `approve`.

---

## Technical Summary Table

| Method | Type | Input Highlights |
| --- | --- | --- |
| **deposit** | Write | `amount` (Native IP) |
| **withdraw** | Write | `amount` (WIP) |
| **approve** | Write | `spender`, `amount` |
| **balanceOf** | Read | `addr` (Target Address) |
| **transfer** | Write | `to`, `amount` |

---

## Implementation Example: Wrapping IP

```typescript
import { parseEther } from "viem";

// Wrap 10 native IP into 10 WIP to prepare for licensing fees
const response = await client.wipClient.deposit({
  amount: parseEther("10"), 
});

// Check balance afterward
const balance = await client.wipClient.balanceOf("0xYourWalletAddress...");
console.log(`Current WIP Balance: ${balance}`);

```

### Application for Creative Platform

For your **Creative Platform** IP marketplace, the `WipClient` ensures that your financial logic remains consistent. Because **Creative Bank** and your other fintech tools are already operating on **Base** with USDC, you can use the `WipClient` on Story Protocol to bridge the user experience—wrapping native IP into a standard format that your AI agents and marketplace contracts can easily trade, stake, or distribute as rewards.
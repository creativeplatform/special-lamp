---
trigger: model_decision
description: The RoyaltyClient manages the flow of revenue within the Story Protocol IP graph.
---

### TL;DR

The **RoyaltyClient** manages the flow of revenue within the Story Protocol IP graph. It enables entities to pay royalties to IP Assets, check claimable balances in IP Royalty Vaults, and execute complex claims from derivative child IPs back to ancestors.

---

## Core Royalty Methods

### 1. payRoyaltyOnBehalf

Allows a caller to send revenue tokens to a receiver IP. This is essential for both external users (paying for IP usage) and derivative IPs (paying their parents).

* **Flexible Payers**: If an external user is paying, `payerIpId` is set to the `zeroAddress`.
* **WIP Management**: Includes options for **Auto-Wrap** (converting IP to WIP) and **Auto-Approve** to simplify the transaction UX.

### 2. claimAllRevenue & batchClaimAllRevenue

These methods allow IP owners or royalty token holders to pull accumulated revenue from the protocol.

* **Recursive Claims**: Claims can be made from your own **IP Royalty Vault** and all specified **childIpIds**.
* **Automation**: The `batchClaimAllRevenue` method can use **Multicall** to process claims for multiple ancestor IPs in a single transaction.
* **Post-Claim Logic**: `autoTransferAllClaimedTokensFromIp` ensures that once an IP Account claims the funds, they are automatically forwarded to the owner's wallet.

### 3. claimableRevenue

A read-only method to check how much revenue is currently sitting in an IP's Royalty Vault for a specific token (e.g., $WIP or $USDC).

> **Note**: This only checks the vault balance; it does not account for unclaimed revenue still residing in the broader Royalty Module or parent/child contracts.

---

## Technical Reference & Infrastructure

| Method | Purpose | Key Parameters |
| --- | --- | --- |
| **getRoyaltyVaultAddress** | Locates the proxy contract for an IP's vault. | `ipId` |
| **transferToVault** | Moves revenue from a policy to the IP's vault. | `royaltyPolicy`, `ancestorIpId` |
| **payRoyaltyOnBehalf** | Executes a payment into the royalty graph. | `receiverIpId`, `token`, `amount` |

### Workflow Example: Derivative Payment

When **IPA B** (a derivative) earns revenue, it must pay **IPA A** (the parent) based on their PIL terms:

```typescript
const payRoyalty = await client.royalty.payRoyaltyOnBehalf({
  receiverIpId: "0xParentIPA...", 
  payerIpId: "0xChildIPA...", 
  token: WIP_TOKEN_ADDRESS,
  amount: parseEther("5"), // 5 $WIP
});

```

### Claiming Example: Ancestor Collection

```typescript
const claimRevenue = await client.royalty.claimAllRevenue({
  ancestorIpId: "0xParentIPA...",
  claimer: "0xParentIPA...", // Usually the IP Account address
  currencyTokens: [WIP_TOKEN_ADDRESS],
  childIpIds: ["0xChildIPA_1", "0xChildIPA_2"],
  royaltyPolicies: ["0xRoyaltyPolicyLAP..."],
});

```
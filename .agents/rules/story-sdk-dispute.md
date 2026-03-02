---
trigger: model_decision
description: The DisputeClient provides a decentralized framework for addressing IP-related conflicts on Story Protocol.
---

### TL;DR

The **DisputeClient** provides a decentralized framework for addressing IP-related conflicts on Story Protocol. It utilizes an optimistic oracle system (UMA) where users can raise disputes with evidence (CID), stake bonds ($WIP), and resolve claims based on specific violation tags like "IMPROPER_REGISTRATION" or "CONTENT_STANDARDS_VIOLATION."

---

## Core Dispute Methods

### 1. raiseDispute

Initiates a formal challenge against an IP ID.

* **Key Requirements**: A Content Identifier (**CID**) from IPFS containing evidence and a **Bond** (staked in $WIP).
* **Bond Logic**: If you win, you get your bond back plus 50% of the opponent's bond. The other 50% pays the reviewers.
* **Liveness**: Sets the time window (typically 30 days) for the opposing party to present a counter-dispute.

### 2. disputeAssertion (Counter-Dispute)

Allows an IP owner to counter a raised dispute by providing their own counter-evidence.

* **Assertion ID**: You must map the `disputeId` to an `assertionId` using `disputeIdToAssertionId` before calling this.
* **Staking**: The counter-party must match the original bond amount to proceed.

### 3. Resolve & Cancel

* **resolveDispute**: Finalizes the process after a judgment has been rendered by the oracle.
* **cancelDispute**: Allows the initiator to withdraw an ongoing dispute before it is judged.

---

## Dispute Violation Tags

When raising a dispute, you must select a whitelisted `DisputeTargetTag`:

| Tag | Description |
| --- | --- |
| **IMPROPER_REGISTRATION** | The IP being registered already exists or is stolen. |
| **IMPROPER_USAGE** | Violation of the terms defined in the PIL (Programmable IP License). |
| **IMPROPER_PAYMENT** | Missing royalty or licensing payments associated with the IP. |
| **CONTENT_STANDARDS** | Violation of "No-Hate," "No-Pornography," or "Suitable-for-All-Ages" rules. |
| **IN_DISPUTE** | A temporary status tag applied while a judgment is pending. |

---

## Infringement Propagation

### tagIfRelatedIpInfringed

This method ensures that if a **Parent IP** is found to be infringing, the **Derivative IPs** (children) can be systematically tagged as well. This maintains the integrity of the IP graph across the entire **Creative Platform** ecosystem.

---

## Implementation Example: Raising a Dispute

```typescript
const response = await client.dispute.raiseDispute({
  targetIpId: "0xInfringingIP...",
  cid: "QmEvidenceHash...", // IPFS CID
  targetTag: DisputeTargetTag.IMPROPER_USAGE,
  bond: parseEther("0.1"), 
  liveness: 2592000, // 30 days in seconds
});
// Result: Dispute ID is returned for tracking.

```
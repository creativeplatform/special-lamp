---
trigger: model_decision
description: The GroupClient provides the infrastructure to organize individual IP Assets into collective "Group IPs."
---

### TL;DR

The **GroupClient** provides the infrastructure to organize individual IP Assets into collective "Group IPs." This enables collaborative licensing, shared metadata management, and automated royalty distribution across all member IPs through a centralized group reward pool.

---

## Core Group Management

### 1. Group Creation & Registration

* **registerGroup**: Registers a new Group IPA. Requires a `groupPool` address, which defines the logic for splitting royalties (e.g., an `EvenSplitGroupPool`).
* **registerGroupAndAttachLicense**: Creates a group and immediately binds it to specific [PIL Terms](https://www.google.com/search?q=/concepts/licensing-module/license-terms).
* **registerGroupAndAttachLicenseAndAddIps**: A comprehensive method to initialize a group, attach its license, and populate it with member IPs in a single transaction.

### 2. Multi-Action Integration

The GroupClient simplifies the onboarding process by combining multiple protocol steps into "Convenience Methods":

* **mintAndRegisterIpAndAttachLicenseAndAddToGroup**: Mints a new NFT, registers it as an IPA, attaches license terms, and adds it to an existing group.
* **registerIpAndAttachLicenseAndAddToGroup**: Performs the same flow for an existing NFT `tokenId`.

---

## Membership Operations

### Adding and Removing IPs

* **addIpsToGroup**: Adds specified `ipIds` to a group. You can define a `maxAllowedRewardSharePercentage` to cap the earnings of new members.
* **removeIpsFromGroup**: Removes member IPs from the group.

> **Note:** These functions must be executed by the Group IP owner or an authorized operator.

---

## Royalty & Reward Management

### 1. Revenue Collection

* **collectRoyalties**: Pulls earned revenue into the group pool, making it available for distribution.
* **collectAndDistributeGroupRoyalties**: A dual-action method that collects royalties for the group and immediately pushes the distributed shares into each member IP's individual royalty vault.

### 2. Claiming Rewards

* **getClaimableReward**: A read-only method to check the current balance of revenue tokens (like $WIP) available for specific member IPs.
* **claimReward**: Executes the payout to member IPs, triggering on-chain events for the distribution.

---

## Technical Summary Table

| Method | Target | Key Parameters |
| --- | --- | --- |
| **registerGroup** | Group Creator | `groupPool` |
| **addIpsToGroup** | Existing Group | `groupIpId`, `ipIds`, `maxAllowedRewardShare` |
| **collectRoyalties** | Financials | `groupIpId`, `currencyToken` |
| **claimReward** | Member IPs | `groupIpId`, `currencyToken`, `memberIpIds` |

---

### Application for Creative Platform

For your **Creative Platform** IP marketplace, you can use these methods to build "Creative Collectives" or "Collabs." When multiple artists contribute to a single project, you can register them as a group to ensure that any revenue generated from the project's license is automatically split according to the `groupPool` logic.

**Would you like me to help you configure a custom "Split Pool" contract for a collaborative music or AI video project?**
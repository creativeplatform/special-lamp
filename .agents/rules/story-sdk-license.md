---
trigger: model_decision
description: Manage IP licensing, enabling developers to attach terms, mint tokens, and register Programmable IP Licenses (PIL).
---

### TL;DR

The **LicenseClient** manages IP licensing, enabling developers to attach terms, mint tokens, and register Programmable IP Licenses (PIL). It supports diverse licensing models, from Non-Commercial Social Remixing to Commercial Use, with customizable fees and revenue sharing.

---

## Core Licensing Methods

### IP Integration

* **attachLicenseTerms**: Links specific license terms (by ID) to an IP Asset.
* **registerPilTermsAndAttach**: A one-step method to register new PIL terms and immediately link them to an IP.

### Minting & Tokens

* **mintLicenseTokens**: Mints tokens to a `receiver` granting usage rights. Tokens can be "public" (attached to IP) or "private" (not attached).
* **predictMintingLicenseFee**: Pre-computes the required fee (currency and amount) before minting.
* **setMaxLicenseTokens**: Sets a hard cap on the total tokens available for a specific license.

### PIL Registration (Convenience Functions)

| Method | Use Case | Key Params |
| --- | --- | --- |
| **registerPILTerms** | General PIL registration | `transferable`, `royaltyPolicy`, `commercialUse` |
| **registerCommercialUsePIL** | Standard commercial license | `defaultMintingFee`, `currency` |
| **registerCommercialRemixPIL** | Commercial derivatives | `commercialRevShare`, `royaltyPolicyAddress` |
| **registerCCAttributionPIL** | Creative Commons-style | `currency`, `royaltyPolicyAddress` |

---

## Configuration & Data Retrieval

### Licensing Config

Allows granular control over how an IP's licenses behave via the `setLicensingConfig` method:

* **mintingFee**: Fee paid to the licensor upon minting.
* **commercialRevShare**: Percentage of revenue shared (0-100%).
* **licensingHook**: Smart contract address for custom validation logic (e.g., `TotalLicenseTokenLimitHook`).
* **disabled**: If `true`, stops all new minting and derivative attachments.

### Getters

* **getLicenseTerms**: Returns full details of a specific `licenseTermsId` (e.g., expiration, attribution requirements, currency).
* **getLicensingConfig**: Retrieves the specific active configuration for an IP’s license terms.

---

## Implementation Example: Commercial Remix

```typescript
const response = await client.license.registerCommercialRemixPIL({
    currency: "0x1514...", // $WIP Address
    defaultMintingFee: parseEther("1"), 
    commercialRevShare: 10, // 10% Share
});

// Response: { licenseTermsId: 5n, txHash: "0x..." }

```
---
trigger: model_decision
description: IPAssetClient allows you to create, get, and list IP Assets within Story.
---

### TL;DR

The **IPAssetClient** is the core interface for managing IP Assets on Story Protocol. It enables registering NFTs as IP Assets, defining their metadata, attaching licensing terms, and establishing derivative relationships (parent-child links) to manage royalties and usage rights across the IP graph.

---

## Core IP Asset Methods

### 1. registerIpAsset

Registers an IP Asset (IPA) by either linking an existing NFT or minting a new one.

* **NFT Options**:
* `type: "minted"`: Uses an existing `nftContract` and `tokenId`.
* `type: "mint"`: Mints a new NFT via an `spgNftContract`.


* **License & Royalties**: You can optionally attach license terms (e.g., Creative Commons) and set royalty shares for recipients during the registration transaction.
* **Metadata**: Requires URIs and hashes for both the IP-specific metadata and the underlying NFT metadata.

### 2. registerDerivativeIpAsset

A specialized method to register an IPA that is based on one or more "parent" IPs.

* **Automatic Linking**: It can mint a license token from the parent on your behalf (via `derivData`) or consume existing license tokens you already own.
* **Royalty Logic**: Requires setting `maxRts` (Maximum Royalty Tokens), typically set to `100,000,000` for simplicity.
* **Prerequisite**: The parent IPs must already be registered as IP Assets on Story Protocol.

### 3. linkDerivative

Connects an *already registered* IP Asset to a parent IP to formally establish a derivative relationship.

* **Methods**:
* **With License Tokens**: Burn existing license tokens to prove you have the right to link.
* **Without License Tokens**: Provide parent IDs and license terms; the protocol will attempt to mint the required tokens during the linking process.



---

## Key Data Types

| Type | Key Fields | Description |
| --- | --- | --- |
| **IpMetadata** | `ipMetadataURI`, `ipMetadataHash` | Off-chain data describing the creative work. |
| **RoyaltyShare** | `recipient`, `percentage` | Defines who receives what portion of future earnings. |
| **DerivDataInput** | `parentIpIds`, `licenseTermsIds` | Parameters for creating a derivative link. |

---

## Implementation Example: Registering a New IP

```typescript
const response = await client.ipAsset.registerIpAsset({
  nft: { type: "mint", spgNftContract: "0x..." },
  ipMetadata: {
    ipMetadataURI: "https://ipfs.io/ipfs/...",
    ipMetadataHash: toHex("metadata", { size: 32 }),
    nftMetadataURI: "https://ipfs.io/ipfs/...",
    nftMetadataHash: toHex("nft-metadata", { size: 32 }),
  }
});
// IPA ID is returned as response.ipId

```
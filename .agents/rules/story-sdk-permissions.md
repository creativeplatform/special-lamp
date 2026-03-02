---
trigger: model_decision
description: The PermissionClient manages access control for IP Accounts within Story Protocol.
---

### TL;DR

The **PermissionClient** manages access control for IP Accounts within Story Protocol. It allows IP owners to authorize specific "signers" (addresses) to interact with protocol modules on their behalf. Permissions are granular, defined by the signer, the target module, and specific function selectors.

---

## Core Permission Logic

Permissions follow a hierarchy where specific overrides (function-level) take precedence over wildcards. By default, the IP Account owner holds all permissions, and all other policies are set to **ABSTAIN**.

### Permission Levels

| Level | Name | Description |
| --- | --- | --- |
| **0** | **ABSTAIN** | No permission set (Default). |
| **1** | **ALLOW** | Explicitly permits the signer to call the function. |
| **2** | **DENY** | Explicitly blocks the signer from calling the function. |

---

## Key Methods

### 1. Single & Global Permissions

* **setPermission**: Authorizes a `signer` to call a specific `func` on a target contract (`to`). If `func` is omitted, it acts as a wildcard for all functions on that contract.
* **setAllPermissions**: A broad authorization that grants a `signer` permission for all functions across **all** Story Protocol modules for a specific IP ID.

### 2. Batch Operations

* **setBatchPermissions**: Allows setting multiple permission policies (different signers, modules, or functions) in a single transaction to save gas and improve efficiency.

### 3. Signature-Based Permissions

These methods allow for off-chain authorization that can be executed on-chain later:

* **createSetPermissionSignature**: Generates a signature to override permissions for a specific function.
* **createBatchPermissionSignature**: Generates a signature for a group of permission changes.
* **Deadline**: Both methods include a `deadline` parameter (defaulting to 1000ms) to ensure signature security and timeliness.

---

## Technical Parameters

| Parameter | Description |
| --- | --- |
| **ipId** | The address of the IP Account granting the permission. |
| **signer** | The address being authorized to act on behalf of the IP. |
| **to** | The target address (currently limited to Story Protocol modules). |
| **func** | The function selector string (e.g., `mintLicenseTokens`). |

---

### Application for Creative Platform

Since you are building a **factory of AI agents**, the `PermissionClient` is critical for your infrastructure. You can use `setPermission` to allow your AI agents (as `signers`) to automatically:

* Mint license tokens on behalf of a creative professional.
* Register derivative IP when an AI agent generates new content.
* Manage royalty claims without requiring a manual wallet signature for every micro-transaction.

**Would you like me to help you draft a batch permission script to authorize your AI agent factory to manage IP registration on Story Protocol?**
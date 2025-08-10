# Digital Asset Marketplace

A secure and transparent platform for trading digital assets with **escrow protection**, ensuring safe transactions for buyers and sellers.
The contract supports **asset creation**, **marketplace listings**, **royalty settings**, and **verified ownership checks**.

---

## Features

* **Mint Digital Assets** – Create unique, metadata-linked assets with categories (art, music, video, document) and royalty percentages.
* **Escrow-Backed Sales** – Protects both buyers and sellers during asset transfers.
* **Marketplace Listings** – List assets for sale with custom pricing and payment tokens.
* **Ownership Verification** – Public read-only checks for asset ownership.
* **Metadata Validation** – Ensures correct asset details and prevents invalid or incomplete entries.
* **Royalty Support** – Allows creators to earn a percentage from future resales.

---

## Data Structures

### Constants

| Name                | Description                                                       |
| ------------------- | ----------------------------------------------------------------- |
| `CONTRACT_OWNER`    | Deployer address with administrative privileges.                  |
| `ERR_*` codes       | Standardized error responses for validation and state checks.     |
| `platform-fee-rate` | Platform commission (2.5% default in basis points).               |
| `escrow-duration`   | Time window (in blocks) before escrow expires (default 24 hours). |

### Maps

* **`digital-assets`** – Stores asset details (creator, current owner, metadata, category, royalties, verification status).
* **`marketplace-listings`** – Tracks listed assets for sale (price, seller, payment token, status).
* **`escrow-transactions`** – Records escrow agreements (buyer, seller, amount, status, disputes).

---

## Public Functions

### `mint-digital-asset`

Creates a new asset with metadata, category, and royalty percentage.
**Parameters:**

* `asset-name` (string ≤ 100 chars)
* `description` (string ≤ 300 chars)
* `metadata-uri` (string ≤ 200 chars)
* `asset-category` (1 = art, 2 = music, 3 = video, 4 = document)
* `royalty-percentage` (basis points; max 1000 = 10%)

**Returns:** `asset-id` (uint) of the newly created asset.

---

### `list-asset-for-sale`

Lists an owned asset for sale.
**Parameters:**

* `asset-id` (uint)
* `price` (uint, > 0)
* `payment-token` (string ≤ 20 chars)

**Returns:** `true` if listed successfully.

---

## Read-Only Functions

### `get-asset-details`

Returns metadata and ownership info for a given asset ID.

### `get-listing-details`

Returns sale details for a given asset ID.

### `verify-ownership`

Checks if a given principal is the current owner of an asset.

---

## Error Codes

| Code   | Meaning                    |
| ------ | -------------------------- |
| `u100` | Unauthorized action.       |
| `u101` | Asset not found.           |
| `u102` | Invalid price.             |
| `u103` | Insufficient balance.      |
| `u104` | Asset not listed for sale. |
| `u105` | Invalid buyer.             |
| `u106` | Escrow expired.            |
| `u107` | Invalid metadata.          |

---

## Security & Validation

* Assets cannot be listed by non-owners.
* All metadata is validated for length and format.
* Escrow ensures payment is secured before asset transfer.
* Categories and royalty percentages are capped for compliance.

---

## Example Workflow

1. **Mint** an asset with `mint-digital-asset`.
2. **List** the asset using `list-asset-for-sale`.
3. **Buyer** initiates a purchase, triggering **escrow**.
4. **Transfer** completes after escrow conditions are met.

---

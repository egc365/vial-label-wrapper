# ISO-Compliant Numbering for Vial IDs

## Goal

Every vial gets a unique, scannable ID that maps to a database record. The ID must be ISO-compliant where a standard applies, and human-auditable where required.

## Relevant standards

- **ISO/IEC 15417** — Code 128 barcode symbology (1D). Good for longer alphanumeric strings.
- **ISO/IEC 16022** — DataMatrix (2D). Compact, high data density, good for small vial surfaces. Preferred for 2 mL vials.
- **ISO 13485** — Medical devices QMS (if product is regulated as a device).
- **21 CFR Part 11** — Electronic records (if FDA-regulated).
- **USP <1118>** — Application of barcodes in healthcare.

> Note: exact standard applicability depends on the product classification (research chemical, supplement, pharmaceutical). Confirm with regulatory counsel before production.

## Proposed ID format

```
[PREFIX]-[YYYYMMDD]-[SEQ:6]
```

Example: `VIAL-20261012-000001`

- **PREFIX:** product or line code (e.g., `VIAL`, `SYR`, `CBD`).
- **YYYYMMDD:** production date.
- **SEQ:** zero-padded 6-digit sequence, unique per day per prefix.

This gives 1,000,000 unique IDs per prefix per day — far more than needed.

## Barcode content

The barcode encodes the full ID string. No other data (lot, expiry) is required in the barcode itself — that lives in the database record linked by the ID.

Optional: print the ID in human-readable text below or beside the barcode for manual audit. This is cosmetic for the machine but useful for humans.

## Database record fields (minimum)

| Field | Type | Notes |
|-------|------|-------|
| id | string | The barcode value |
| lot | string | Production lot |
| expiry | date | ISO 8601 |
| product | string | SKU or name |
| fill_volume_ml | float | Actual fill |
| station | string | Which arm/station produced it |
| timestamp | datetime | UTC |
| status | enum | pass / fail / rework |

## Uniqueness guarantee

Sequence numbers are allocated by a central service (or a local SQLite DB with a lock) before printing. The printer never generates IDs itself — it receives them. This prevents duplicates across restarts or multiple printers.

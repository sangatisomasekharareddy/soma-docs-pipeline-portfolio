# Configuring Barcode Preprocessing Rules

## Overview

In warehouse operations, barcodes from different suppliers often vary in format and structure. These inconsistencies can disrupt scanning and downstream processing.

The Barcode Preprocessing UI allows you to define rules to standardize and transform scanned barcodes before processing. :contentReference[oaicite:0]{index=0}

---

## Processing Flow

1. Match scanned barcode against rules (sequence order)
2. Apply update logic
3. Stop further evaluation
4. Pass updated barcode for processing

---

## Key Capabilities

- Standardize barcode formats
- Extract or restructure barcode segments
- Validate barcode input in real time
- Reject invalid barcodes
- Adapt to new formats without code changes :contentReference[oaicite:1]{index=1}

---

## Match Functions

| Function       | Description |
|---------------|------------|
| Min Length    | Matches based on minimum length |
| Exact Length  | Matches exact barcode length |
| Begins With   | Matches prefix |
| Contains      | Matches substring |
| Regex Match   | Matches regex pattern |

---

## Update Functions

| Function        | Description |
|----------------|------------|
| Substring      | Extract part of barcode |
| Strip String   | Remove specific text |
| Strip Regex    | Remove using regex |
| Insert At      | Insert string at position |
| Regex Replace  | Advanced transformations |

---

## Example

**Input Barcode:** `ARTSSKU1234567`  
**Output:** `SKU123`

---

## Key Behavior

- Rules execute in sequence
- First match stops evaluation
- Updates run sequentially :contentReference[oaicite:2]{index=2}

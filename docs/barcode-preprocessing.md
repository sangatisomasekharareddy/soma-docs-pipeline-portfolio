# Configuring Barcode Preprocessing Rules

## Overview
In warehouse operations, a wide range of SKUs and suppliers results in barcodes of various formats, structures, and qualifiers. Not all scanned barcodes arrive in a format that matches your Warehouse Management System (WMS) requirements. These variations can lead to inconsistencies during scanning and downstream processing.

The **Barcode Preprocessing (BarcodePreProcView)** UI allows you to configure rules for preprocessing scanned barcodes. You can define barcode matching conditions and specify logic used to transform barcode values. When a scanned barcode meets the defined criteria, the system applies the configured preprocessing rules and updates the barcode before further processing. 

Note: 

- Rules can be configured only in Redwood UI format.
- Rules are user group-specific. 
  
---

## Processing Flow

When a barcode is scanned, the system processes it in the following order: 
1. Match the scanned barcode against configured preprocessing rules (by sequence). 
2. Apply the corresponding update logic (if matched). 
3. Stop further rule evaluation. 
4. Pass the updated barcode for downstream processing.

---

## Key Capabilities

Use barcode preprocessing rules to: 
- Standardize barcode formats across suppliers. 
- Extract or restructure barcode segments from the scanned barcode. 
- Validate barcode input in real time.  
- Reject invalid barcodes before they disrupt operations, reducing manual intervention and errors.  
- Adapt to new barcode formats without code changes.

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

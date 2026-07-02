# Configuring Barcode Preprocessing Rules 

## Overview 
In warehouse operations, a wide range of SKUs and suppliers results in barcodes of various formats, structures, and qualifiers. Not all scanned barcodes arrive in a format that matches your Warehouse Management System (WMS) requirements, leading to inconsistencies during scanning and downstream processing. 

The **Barcode Preprocessing (`BarcodePreProcView`)** UI allows you to configure rules for preprocessing scanned barcodes. You can define barcode matching conditions and specify the logic used to transform barcode values. When a scanned barcode meets the defined criteria, the system applies the configured preprocessing rules and updates the barcode before further processing. 

> **Note:** Rules can be configured **only** in the Redwood UI format and are user group-specific. 

---

## Processing Flow 
When a barcode is scanned, the system processes it in the following sequential order: 

1. Match the scanned barcode against configured preprocessing rules (ordered by sequence). 
2. Apply the corresponding update logic (if a match is found). 
3. Stop further rule evaluation immediately upon matching. 
4. Pass the updated barcode to downstream processing.

---

## Key Capabilities 
Use barcode preprocessing rules to: 
- Standardize barcode formats across different suppliers. 
- Extract or restructure specific barcode segments from the scanned input. 
- Validate barcode inputs in real time.  
- Reject invalid barcodes before they disrupt operations, reducing manual intervention and errors.  
- Adapt to new barcode formats dynamically without demanding code changes. 

---

## Configure Barcode Preprocessing Rules 

In the Barcode Preprocessing UI, you can set criteria determining when to preprocess scanned barcodes. Follow these steps to configure the rules: 

1. Navigate to the **Barcode Preprocessing UI**. 
2. Click **Create**. 
3. On the Create form, enter or select the following fields/toggles as required: 
   * **Sequence Nbr:** Defines the order in which barcode preprocessing rules are evaluated. 
   * **Prefix Text:** Specifies the starting text used to identify matching barcodes. 
   * **Match Function:** Determines how the system evaluates the barcode. You can select one of the following functions:

      | Match Function | Behavior |
      | :--- | :--- |
      | **Min Length** | Matches if the scanned barcode meets the minimum length. On selection, enter the minimum length value. |
      | **Exact Length** | Matches if the scanned barcode is of the exact specified length. On selection, enter the exact length value. |
      | **Begins With** | Matches if the scanned barcode starts with the configured text. On selection, enter the string to match with the barcode's beginning. |
      | **Contains** | Matches if the scanned barcode contains a specific string. On selection, enter the text pattern to match. |
      | **Regex Match** | Matches if the scanned barcode fits the provided regular expression. On selection, enter a regular expression in the `regex` field. |

      > **Note:** All text entries are case-sensitive. 

    * **Reject Barcode toggle:** When enabled, the system rejects the matching scanned barcode and displays an error during scanning. Enable this toggle to block invalid barcodes. 

    > **Note:** Either **Sequence Nbr** or **Match Function** is mandatory for each barcode preprocessing rule. You can also configure both fields simultaneously. 

4. Click **Save**. 

You can create multiple rules with different sequence numbers.
The system matches the scanned barcodes against barcode preprocessing rules in sequence. Once the scanned barcode matches a rule, any associated update logic is executed and no further rules are evaluated. 
Barcode Preprocessing Update rules for a matching sequence define how the barcode is to be updated or handled next. 

---

## Configure Barcode Preprocessing Updates 

In the **Barcode Preprocessing Updates** screen, you can configure the actual logic used to transform the scanned barcodes. 

To define how a matched barcode should be updated: 
1. In the Barcode Preprocessing UI, select an existing barcode preprocessing rule. 
2. Click **More Actions** > **Barcode Preprocessing Updates**. 
3. Click **Create**. 
4. On the Create form, enter or select the following fields: 
   * **Sequence Nbr:** Determines the update execution order. 
   * **Update Function:** Logic used to transform the scanned barcode. You can select from the following choices: 

      | Update Function | Behavior | Example |
      | :--- | :--- | :--- |
      | **Substring** | Extracts a portion of the barcode. Enter `startindex` (inclusive) and `endindex` (exclusive) values.<br><br>*Note: Index value starts with 0. If the provided end index is greater than the last index, the system captures up to the final character.* | **Scanned Barcode:** `ARTSKU1234567`<br>**Startindex:** 3<br>**Endindex:** 9<br>**Result:** `SKU123` (from 3rd to 8th character) |
      | **Strip String** | Removes all occurrences of a specified string. Enter the exact string value to remove. | **Scanned Barcode:** `ARTSSKUARTS123`<br>**Strip String:** `ARTS`<br>**Result:** `SKU123` |
      | **Strip Regex** | Removes parts of the scanned barcode using regex. Enter a regular expression in the `regex` field to strip matching portions. Standard regex patterns apply. | **Scanned Barcode:** `ARTSKU123456`<br>**Regex:** `^(.{3})`<br>**Result:** `SKU123456` (first three characters removed) |
      | **Insert At** | Inserts a string at a specified position. Enter the `Position` (0-indexed) and the `String to insert`. If the position exceeds the length, it appends to the last character's position. | **Scanned Barcode:** `ART12345`<br>**Position:** 3<br>**String to insert:** `SKU`<br>**Result:** `ARTSKU12345` |
      | **Regex Replace** | Performs advanced transformations using regex groups. Enter a regular expression in the `regex` field. *Useful for constructing multi-field barcodes.* | **Scanned Barcode:** `SSBPLPN2201`<br>**Regex:** `^SSBP(.{7})`<br>**String to replace:** `SSMFB[A]$1!`<br>**Result:** `SSMFB[A]LPN2201!` |

5. Click **Save**. 

You can create multiple updates with different sequence numbers. 

> **Execution Behavior:** Barcode Preprocessing Updates execute in strict sequence order. The output of one update function becomes the direct input for the next.

---

## Examples 

### Example 1: Extract Substring 
Extract a portion from the scanned barcode. 

**Input Barcode:** `ARTSSKU1234567` 

#### Barcode Preprocessing Rule
| Group | Sequence Nbr | Prefix Text | Reject Barcode | Match Function | Parameters |
| :--- | :--- | :--- | :--- | :--- | :--- |
| Group1 | 1 | `ART` | `false` | `min_length` | `{"length": 6}` |

The system matches the scanned barcode `ARTSSKU1234567` based on the prefix and minimum length rule. 

#### Barcode Preprocessing Update (for Sequence 1)
| Match Rule | Sequence Nbr | Update Function | Configuration |
| :--- | :--- | :--- | :--- |
| Company/Facility/1 | 1 | `substring` | `startindex = 4`, `endindex = 10` |

The update rule extracts characters from position 4 to 9. 


**Final Output after updates:** `SKU123` 

---

### Example 2: Reject Invalid Barcode 
Reject all barcodes starting with "SSITEM". 

#### Barcode Preprocessing Rule
| Group | Sequence Nbr | Prefix Text | Reject Barcode |
| :--- | :--- | :--- | :--- |
| Group2 | 1 | `SSITEM` | `true` |

If the scanned barcode is `SSITEM012XYZ`, the system matches the prefix and rejects the barcode immediately, throwing an error.

---

### Example 3: Multi-Step Processing 
When a barcode (e.g., `SKUSLPN12345`) is scanned during a mobile transaction, the system evaluates it against each rule sequentially until a match is found. 

#### Barcode Preprocessing Rules
| Group | Sequence Nbr | Prefix Text | Reject Barcode | Match Function | Parameters |
| :--- | :--- | :--- | :--- | :--- | :--- |
| Group3 | 1 | `SKU` | `false` | `min_length` | `{"length": 14}` |
| Group3 | 2 | `SKU` | `false` | `exact_length` | `{"length": 20}` |
| Group3 | 3 | `SKU` | `false` | `begins_with` | `{"string": "SKUNR"}` |
| Group3 | 4 | `SKU` | `false` | `contains` | `{"contains": "SKU-SKU"}` |
| Group3 | 5 | `SKU` | `false` | `regex_match` | `{"regex": "S.S"}` |

For the barcode **`SKUSNLPN12345`**, the system evaluates the rules from sequence 1 to 5: 
1. Sequences 1 to 4 do not match this barcode according to their configuration. 
2. **Sequence 5 matches** this barcode because the regular expression criteria applies (`regex_match: {"regex":"S.S"}`). 

The system triggers the associated barcode preprocessing update rules for Sequence 5.

#### Barcode Preprocessing Update (for Sequence 5)
| Match Rule | Sequence Nbr | Update Function | Configuration |
| :--- | :--- | :--- | :--- |
| Company/Facility/5 | 1 | `strip_string` | `string = SN` |
| Company/Facility/5 | 2 | `substring` | `startindex = 0`, `endindex = 8` |

The barcode updates sequentially: 
1. **Sequence 1:** Removes all occurrences of "SN", turning the string into `SKULPN12345`. 
2. **Sequence 2:** Extracts the first 8 characters from that result (`startindex = 0` to `endindex = 8`). 

* **Input barcode:** `SKUSNPLN12345`
* **Final barcode after updates:** `SKULPN12`

---

### Example 4: Conditional Extraction Based on Supplier Prefix 
You receive SKUs with multiple supplier barcodes, each featuring a unique prefix and different part number formats: 
* **Supplier 1:** Barcodes start with prefix "S1-", with the part number immediately following (e.g., `S1-123456`). 
* **Supplier 2:** Barcodes start with prefix "S2", with the part number immediately following (e.g., `S2123456`). 

The system recognizes the supplier based on the prefix and extracts the core part numbers. 

#### Barcode Preprocessing Rules
| Group | Sequence Nbr | Prefix Text | Reject Barcode | Match Function | Parameters |
| :--- | :--- | :--- | :--- | :--- | :--- |
| Group4 | 1 | `S1-` | `false` | `min_length` | `{"length": 6}` |
| Group4 | 2 | `S2` | `false` | `min_length` | `{"length": 6}` |

#### Barcode Preprocessing Update (for Sequence 1)
| Match Rule | Sequence Nbr | Update Function | Configuration |
| :--- | :--- | :--- | :--- |
| Company/Facility/1 | 1 | `strip_string` | `string = S1-` |

#### Barcode Preprocessing Update (for Sequence 2)
| Match Rule | Sequence Nbr | Update Function | Configuration |
| :--- | :--- | :--- | :--- |
| Company/Facility/2 | 1 | `strip_string` | `string = S2` |

* **For Supplier 1 barcode `S1-123456`:** The rule matches the "S1-" prefix, executes update sequence 1 to strip the "S1-" string, and outputs **`123456`**. 
* **For Supplier 2 barcode `S2123456`:** The rule matches the "S2" prefix, executes update sequence 1 to strip the "S2" string, and outputs **`123456`**.

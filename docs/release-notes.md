

```markdown
# What's New

## Introduce JSON Input Interfaces for UOM and UOM Literals
To streamline Unit of Measure (UOM) data creation and simplify automation, we are introducing UOM and UOM Literals Input Interfaces in JSON format. You can utilize these input interfaces to create UOMs and associated UOM Literals via APIs.

### Key Details
* **Format Support:** Both interfaces are supported only in JSON format.
* **Permissions:** Users (excluding Admin and Management roles) must have the `lgfapi / lgfapi Create Access` group permission to utilize these APIs.

### UOM Input Interface
Use this interface to create new UOMs.

* **API URL:** `POST .../wms/lgfapi/v10/stage/uom`
* **Processing Order:** The API processes entries with `"base_uom_flg" = "true"` (base UOMs) first, followed by entries with `"base_uom_flg" = "false"` (non-base UOMs).

### UOM Literals Input Interface
Use this interface to create localized terminology (literals) for existing UOMs. It supports multiple languages (e.g., English, Chinese, Spanish, French, Korean).

* **API URL:** `POST .../wms/lgfapi/v10/stage/uom_terminology`

> **Note:** For detailed API documentation and sample payloads, refer to the *WMS REST API Guide*.


---

## Display Additional Item Property during Mobile Transactions 
You can now display additional item properties during **Receive Single SKU** and **Sort and Receive** mobile transactions. This enhancement provides additional information to identify scanned items, which helps you with the following: 

* **Speeds up item identification** during receiving. 
* **Reduces reliance** on primary item codes. 
* **Improves accuracy** in high-volume operations. 


### New Screen Parameter 
We have introduced a new selection-type screen parameter `extra-item-property-disp` to the Receive Single SKU and Sort and Receive mobile transactions. Use this parameter to configure the item property to display. 

### Parameter Behavior and Choices 
The following table details the screen parameter behavior: 

| Screen Parameter Choice | Behavior |
| :--- | :--- |
| **None or Blank** | *Default:* The system does not display an additional item property. |
| **Item Code** | When an item is scanned, the system displays the selected item property. |
| **Item Alternate Code** | When an item is scanned, the system displays the selected item property. |
| **Item Part A** | When an item is scanned, the system displays the selected item property. |
| **Item External Style** | When an item is scanned, the system displays the selected item property. |
| **Item Description** | When an item is scanned, the system displays the selected item property. |
| **Item Facility Description** | When an item is scanned, the system displays the selected item property. |
| **Item Short Description** | When an item is scanned, the system displays the selected item property. |
| **Item Description 2** | When an item is scanned, the system displays the selected item property. |
| **Season Code** | When an item is scanned, the system displays the selected item property. |
| **Item Description 3** | When an item is scanned, the system displays the selected item property. |
| **Item Barcode** | When an item is scanned, the system displays the selected item property. |


### Important Considerations 

* **Transaction Support:** The screen parameter value `Item Description` is **not supported** for the Sort and Receive transaction. 
* **Missing Configuration:** If an item is not configured with the selected item property, the system does not display the additional property. 
* **Fallback Logic:** If `Item Facility Description` is blank for an item, the system displays the respective item description instead.


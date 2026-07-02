# Bulk Create Item-Driven Cycle Count Tasks 
Use this API to create item-driven cycle count tasks in bulk, without executing a cycle count task creation template. 

This API creates item-driven cycle count tasks for specified items across one or more locations, or across all eligible locations if no locations are provided. 

---

## Prerequisites 
* Valid user credentials (username and password). 
* Required access permission: lgfapi / lgfapi Create Access. 
* Valid item, facility, and company identifiers. 

---

## URL 
`POST .../wms/lgfapi/v10/entity/location/cc_item_task/bulk_create`

---

## Authentication
Basic Authentication (username and password).

---

## Request Body Parameters 

### Parameters
| Parameter | Type | Required | Description |
| :--- | :--- | :--- | :--- |
| `item_id_list` | Array (Integer) | **Yes** | Valid item IDs to create item-driven cycle count tasks. You can pass a single or multiple item IDs. |
| `facility_id` | Integer | *Conditional* | Facility context by ID. |
| `facility_id__code` | String | *Conditional* | Facility context by code. |
| `company_id` | Integer | *Conditional* | Company context by ID. |
| `company_id__code` | String | *Conditional* | Company context by code. |

> **Note:** > * Provide either `facility_id` or `facility_id__code`.
> * Provide either `company_id` or `company_id__code`.

### Options
| Parameter | Type | Required | Description |
| :--- | :--- | :--- | :--- |
| `location_id_list` | Array (Integer) | No | Valid active or reserve location IDs. You can pass a single or multiple location IDs. If location IDs are not provided, the API creates item-driven cycle count tasks for the specified items at all active and reserve locations where they exist. |
| `priority` | Integer | **Yes** | Task Priority. |
| `description` | String | **Yes** | Valid item-driven cycle count task type description. |
| `commit_frequency` | Integer | No | Frequency at which the changes are applied to each resource or group of resources being processed.<br><br>**Supported values:**<br>• `0` (Default) = Roll back on first error.<br>• `1` / Any integer > 0 = Commit per number of objects passed. |
| `max_num_of_tasks` | Integer | No | Maximum number of tasks to create. This parameter is ignored when `location_id_list` is specified. |

---


## Request Body
```json
{ 
    "parameters":  
    { 
        "item_id_list": [227989, 227990, 227988], 
        "facility_id": 648, 
        "company_id": 369 
    }, 
    "options":  
    { 
        "location_id_list": [132873, 136818, 136798], 
        "priority": 20, 
        "description": "CC-LOCN-BY-ITEM", 
        "commit_frequency": 1, 
        "max_num_of_tasks": 1 
    } 
}

---

## Sample Success Response (200 OK) 
The system returns the successful and failed task creation count.
```json
{ 
    "record_count": 3, 
    "success_count": 3, 
    "failure_count": 0, 
    "data": null, 
    "details": null 
}

---

## Error Codes
| Status Code | Description |
| :--- | :--- |
| 400 | Invalid request |
| 401 | Authentication failed (invalid username or password) |
| 403 | Forbidden (insufficient permissions) |
| 404 | Resource not found |
| 500 | Internal server error |

### Sample Error Response 
If a specific item in the list fails, the details array identifies the specific ID and error reason. 
```json
{ 
  "record_count": 3, 
  "success_count": 2, 
  "failure_count": 1, 
  "details": [ 
    { 
      "item_id": 227990, 
      "error": "Invalid location ID" 
    } 
  ] 
} 



# Bulk Create Item-Driven Cycle Count Tasks

## Overview

Creates item-driven cycle count tasks in bulk across specified or all locations. :contentReference[oaicite:3]{index=3}

---

## Endpoint
POST /wms/lgfapi/v10/entity/location/cc_item_task/bulk_create

---

## Authentication

Basic Authentication (username + password)

---

## Request Body

```json
{
  "parameters": {
    "item_id_list": [227989,227990],
    "facility_id": 648,
    "company_id": 369
  },
  "options": {
    "location_id_list": [132873],
    "priority": 20,
    "description": "CC-LOCN-BY-ITEM"
  }
}

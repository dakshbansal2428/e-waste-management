# E-Waste Management API Documentation

## Overview

This document provides comprehensive documentation for the E-Waste Management API endpoints. The API allows users to manage e-waste collection, tracking, and recycling operations.

---

## Base URL

```
http://localhost:5000/api
```

---

## Endpoints

### 1. Get All E-Waste Items

Retrieve a list of all e-waste items in the system.

**Endpoint:** `GET /api/items`

**Description:** Fetches all registered e-waste items with their details and status.

**Request:**
```http
GET /api/items HTTP/1.1
Host: localhost:5000
Content-Type: application/json
```

**Query Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `status` | string | No | Filter by status (e.g., 'pending', 'collected', 'recycled') |
| `page` | integer | No | Pagination page number (default: 1) |
| `limit` | integer | No | Items per page (default: 10) |

**Response (Success - 200 OK):**
```json
{
  "success": true,
  "data": [
    {
      "id": "1",
      "itemName": "Old Smartphone",
      "category": "Electronics",
      "weight": 0.15,
      "condition": "Non-functional",
      "collectionDate": "2025-12-20",
      "location": "New Delhi",
      "status": "collected",
      "recyclableComponents": [
        "Gold",
        "Copper",
        "Plastic",
        "Glass"
      ],
      "createdAt": "2025-12-15T10:30:00Z",
      "updatedAt": "2025-12-20T14:45:00Z"
    },
    {
      "id": "2",
      "itemName": "Broken Laptop",
      "category": "Electronics",
      "weight": 2.5,
      "condition": "Partially functional",
      "collectionDate": "2025-12-22",
      "location": "Mumbai",
      "status": "pending",
      "recyclableComponents": [
        "Aluminum",
        "Copper",
        "Rare Earth Elements",
        "Plastic"
      ],
      "createdAt": "2025-12-18T08:15:00Z",
      "updatedAt": "2025-12-22T09:20:00Z"
    }
  ],
  "pagination": {
    "currentPage": 1,
    "totalPages": 5,
    "totalItems": 47,
    "itemsPerPage": 10
  },
  "timestamp": "2025-12-31T08:17:19Z"
}
```

**Response (Error - 404 Not Found):**
```json
{
  "success": false,
  "error": "No items found",
  "timestamp": "2025-12-31T08:17:19Z"
}
```

**Response (Error - 500 Internal Server Error):**
```json
{
  "success": false,
  "error": "Database connection failed",
  "details": "Unable to retrieve items from database",
  "timestamp": "2025-12-31T08:17:19Z"
}
```

---

### 2. Create a New E-Waste Item

Register a new e-waste item in the system.

**Endpoint:** `POST /api/items`

**Description:** Creates a new e-waste item entry with detailed information.

**Request:**
```http
POST /api/items HTTP/1.1
Host: localhost:5000
Content-Type: application/json

{
  "itemName": "Desktop Computer",
  "category": "Electronics",
  "weight": 8.5,
  "condition": "Non-functional",
  "location": "Bangalore",
  "recyclableComponents": [
    "Copper",
    "Aluminum",
    "Rare Earth Elements",
    "Glass",
    "Plastic"
  ],
  "description": "Old desktop computer with multiple hardware components"
}
```

**Request Body Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `itemName` | string | Yes | Name of the e-waste item |
| `category` | string | Yes | Category (e.g., 'Electronics', 'Appliances', 'Components') |
| `weight` | number | Yes | Weight in kilograms |
| `condition` | string | Yes | Condition (e.g., 'Functional', 'Partially functional', 'Non-functional') |
| `location` | string | Yes | Location/City name |
| `recyclableComponents` | array | Yes | List of recyclable materials in the item |
| `description` | string | No | Additional description of the item |

**Response (Success - 201 Created):**
```json
{
  "success": true,
  "message": "E-waste item created successfully",
  "data": {
    "id": "48",
    "itemName": "Desktop Computer",
    "category": "Electronics",
    "weight": 8.5,
    "condition": "Non-functional",
    "collectionDate": null,
    "location": "Bangalore",
    "status": "pending",
    "recyclableComponents": [
      "Copper",
      "Aluminum",
      "Rare Earth Elements",
      "Glass",
      "Plastic"
    ],
    "description": "Old desktop computer with multiple hardware components",
    "createdAt": "2025-12-31T08:17:19Z",
    "updatedAt": "2025-12-31T08:17:19Z"
  },
  "timestamp": "2025-12-31T08:17:19Z"
}
```

**Response (Error - 400 Bad Request):**
```json
{
  "success": false,
  "error": "Validation failed",
  "details": {
    "itemName": "Item name is required",
    "weight": "Weight must be a positive number",
    "location": "Location is required"
  },
  "timestamp": "2025-12-31T08:17:19Z"
}
```

**Response (Error - 500 Internal Server Error):**
```json
{
  "success": false,
  "error": "Failed to create e-waste item",
  "details": "Database error occurred while saving the item",
  "timestamp": "2025-12-31T08:17:19Z"
}
```

---

### 3. Update E-Waste Item Status

Update the status or details of an existing e-waste item.

**Endpoint:** `PUT /api/items/:id`

**Description:** Updates the status (e.g., from 'pending' to 'collected' or 'recycled') or other details of an e-waste item.

**Request:**
```http
PUT /api/items/48 HTTP/1.1
Host: localhost:5000
Content-Type: application/json

{
  "status": "collected",
  "collectionDate": "2025-12-31",
  "notes": "Item collected from warehouse location"
}
```

**Request Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `id` | string | Yes (URL) | The ID of the item to update |

**Request Body Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `status` | string | No | New status ('pending', 'collected', 'recycled', 'disposed') |
| `collectionDate` | string | No | Collection date in YYYY-MM-DD format |
| `weight` | number | No | Updated weight in kilograms |
| `condition` | string | No | Updated condition status |
| `notes` | string | No | Additional notes about the update |

**Response (Success - 200 OK):**
```json
{
  "success": true,
  "message": "E-waste item updated successfully",
  "data": {
    "id": "48",
    "itemName": "Desktop Computer",
    "category": "Electronics",
    "weight": 8.5,
    "condition": "Non-functional",
    "collectionDate": "2025-12-31",
    "location": "Bangalore",
    "status": "collected",
    "recyclableComponents": [
      "Copper",
      "Aluminum",
      "Rare Earth Elements",
      "Glass",
      "Plastic"
    ],
    "notes": "Item collected from warehouse location",
    "createdAt": "2025-12-31T08:17:19Z",
    "updatedAt": "2025-12-31T08:17:19Z"
  },
  "timestamp": "2025-12-31T08:17:19Z"
}
```

**Response (Error - 404 Not Found):**
```json
{
  "success": false,
  "error": "Item not found",
  "details": "E-waste item with ID 48 does not exist",
  "timestamp": "2025-12-31T08:17:19Z"
}
```

**Response (Error - 400 Bad Request):**
```json
{
  "success": false,
  "error": "Validation failed",
  "details": {
    "status": "Invalid status value. Allowed values: pending, collected, recycled, disposed",
    "collectionDate": "Date must be in YYYY-MM-DD format"
  },
  "timestamp": "2025-12-31T08:17:19Z"
}
```

**Response (Error - 500 Internal Server Error):**
```json
{
  "success": false,
  "error": "Failed to update e-waste item",
  "details": "Database error occurred while updating the item",
  "timestamp": "2025-12-31T08:17:19Z"
}
```

---

## Status Codes

| Code | Description |
|------|-------------|
| 200 | OK - Request successful |
| 201 | Created - Resource created successfully |
| 400 | Bad Request - Invalid input or validation error |
| 404 | Not Found - Resource not found |
| 500 | Internal Server Error - Server-side error |

---

## Common Error Responses

### Validation Error Format
```json
{
  "success": false,
  "error": "Validation failed",
  "details": {
    "fieldName": "Error message for this field"
  },
  "timestamp": "2025-12-31T08:17:19Z"
}
```

### Server Error Format
```json
{
  "success": false,
  "error": "Error message",
  "details": "Detailed description of the error",
  "timestamp": "2025-12-31T08:17:19Z"
}
```

---

## Best Practices

1. **Always validate input data** before sending requests to the API.
2. **Use appropriate HTTP methods**: GET for retrieval, POST for creation, PUT for updates.
3. **Handle pagination** when retrieving large datasets using page and limit parameters.
4. **Check timestamps** to understand when items were created or last updated.
5. **Implement error handling** for all possible response codes.
6. **Use status filters** to retrieve items in specific states efficiently.

---

## Examples

### Example 1: Retrieve all collected items
```bash
curl -X GET "http://localhost:5000/api/items?status=collected&page=1&limit=5" \
  -H "Content-Type: application/json"
```

### Example 2: Create a new e-waste item
```bash
curl -X POST "http://localhost:5000/api/items" \
  -H "Content-Type: application/json" \
  -d '{
    "itemName": "Old Monitor",
    "category": "Electronics",
    "weight": 5.2,
    "condition": "Non-functional",
    "location": "Delhi",
    "recyclableComponents": ["Glass", "Copper", "Plastic"],
    "description": "CRT Monitor from 2005"
  }'
```

### Example 3: Update item status to recycled
```bash
curl -X PUT "http://localhost:5000/api/items/48" \
  -H "Content-Type: application/json" \
  -d '{
    "status": "recycled",
    "collectionDate": "2025-12-31",
    "notes": "Item successfully recycled"
  }'
```

---

## Support

For issues or questions regarding the API, please contact the development team or open an issue in the repository.

**Last Updated:** 2025-12-31 08:17:19 UTC

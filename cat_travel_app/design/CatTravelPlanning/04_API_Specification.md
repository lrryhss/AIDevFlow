# CatTravelPlanning: API Specification

**Version:** 1.0  
**Date:** 2025-07-21  
**Status:** Draft  
**Author(s):** Gemini
**Base URL:** `/api/v1/planning`

## 1. Overview

This document provides the detailed API specification for the Cat Travel Planning component. The API allows users to search for accommodations and manage travel checklists.

## 2. Authentication

All endpoints require authentication via a JWT token provided in the `Authorization` header.

**Format**: `Authorization: Bearer <jwt_token>`

## 3. Common Response Formats

### Success Response
```json
{
  "status": "success",
  "data": { ... } 
}
```

### Error Response
```json
{
  "status": "error",
  "error": {
    "code": "ERROR_CODE",
    "message": "Human-readable message"
  }
}
```

## 4. Endpoints

### 4.1 Accommodation Search

#### Search for cat-friendly accommodations
- **Endpoint:** `GET /accommodations/search`
- **Description:** Searches for cat-friendly accommodations based on location and dates.
- **Authentication:** Required
- **Query Parameters**:
  - `location` (string)
  - `start_date` (string, ISO 8601 format)
  - `end_date` (string, ISO 8601 format)
  - `filters` (object, e.g., `{"amenities": ["cat-sitting"]}`)
- **Response (200 OK)**:
```json
{
  "status": "success",
  "data": [
    {
      "id": "uuid",
      "name": "The Cat-Friendly Hotel",
      "pet_policy": {
        "allows_cats": true,
        "fee_per_night": 25,
        "max_cats_allowed": 2
      }
    }
  ]
}
```

### 4.2 Checklist Management

#### Get all checklists for a user
- **Endpoint:** `GET /checklists`
- **Description:** Retrieves all travel checklists for the authenticated user.
- **Authentication:** Required
- **Response (200 OK)**:
```json
{
  "status": "success",
  "data": [
    {
      "id": "uuid",
      "trip_name": "Summer Vacation",
      "items": [
        {"id": "uuid", "name": "Carrier", "is_checked": true},
        {"id": "uuid", "name": "Food", "is_checked": false}
      ]
    }
  ]
}
```

#### Create a checklist
- **Endpoint:** `POST /checklists`
- **Description:** Creates a new travel checklist.
- **Authentication:** Required
- **Request Body**:
```json
{
  "trip_name": "string",
  "items": ["Carrier", "Food", "Litter"]
}
```
- **Response (201 Created)**:
```json
{
  "status": "success",
  "data": {
    "id": "uuid",
    "trip_name": "string"
  }
}
```

### 4.3 Regulatory Guidance

#### Get regulatory guidance
- **Endpoint:** `GET /guidance`
- **Description:** Retrieves regulatory guidance for a specific destination and transport type.
- **Authentication:** Required
- **Query Parameters**:
  - `destination_country` (string)
  - `transport_type` (string, e.g., `AIRLINE`)
- **Response (200 OK)**:
```json
{
  "status": "success",
  "data": {
    "guidance_text": "string",
    "source_url": "string"
  }
}
```

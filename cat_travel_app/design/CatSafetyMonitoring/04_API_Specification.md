# CatSafetyMonitoring: API Specification

**Version:** 1.0  
**Date:** 2025-07-21  
**Status:** Draft  
**Author(s):** Gemini
**Base URL:** `/api/v1/safety`

## 1. Overview

This document provides the detailed API specification for the Cat Safety & Monitoring component. The API is designed to be RESTful and follows the standards outlined in the Design Principles document.

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

### 4.1 Tag Management

#### Register a new tag
- **Endpoint:** `POST /tags/register`
- **Description:** Associates a new tracking tag with a user and their cat.
- **Authentication:** Required
- **Request Body**:
```json
{
  "tag_serial_number": "string",
  "cat_id": "uuid"
}
```
- **Response (201 Created)**:
```json
{
  "status": "success",
  "data": {
    "id": "uuid",
    "cat_id": "uuid",
    "status": "INACTIVE"
  }
}
```

#### Get all tags for a user
- **Endpoint:** `GET /tags`
- **Description:** Retrieves a list of all tracking tags associated with the authenticated user.
- **Authentication:** Required
- **Response (200 OK)**:
```json
{
  "status": "success",
  "data": [
    {
      "id": "uuid",
      "cat_id": "uuid",
      "status": "ACTIVE",
      "battery_level": 85
    }
  ]
}
```

### 4.2 Location Tracking

#### Get location history for a tag
- **Endpoint:** `GET /locations/{tag_id}`
- **Description:** Retrieves the location history for a specific tag.
- **Authentication:** Required
- **Query Parameters**:
  - `start_date` (string, ISO 8601 format)
  - `end_date` (string, ISO 8601 format)
  - `page` (int, default: 1)
  - `limit` (int, default: 100)
- **Response (200 OK)**:
```json
{
  "status": "success",
  "data": {
    "items": [
      {
        "latitude": 40.7128,
        "longitude": -74.0060,
        "recorded_at": "2025-07-21T10:00:00Z"
      }
    ],
    "pagination": {
      "page": 1,
      "limit": 100,
      "total": 500
    }
  }
}
```

### 4.3 Geofence Management

#### Create a geofence
- **Endpoint:** `POST /geofences`
- **Description:** Creates a new geofence for the authenticated user.
- **Authentication:** Required
- **Request Body**:
```json
{
  "name": "string",
  "latitude": 40.7128,
  "longitude": -74.0060,
  "radius_meters": 100
}
```
- **Response (201 Created)**:
```json
{
  "status": "success",
  "data": {
    "id": "uuid",
    "name": "string",
    "is_active": true
  }
}
```

#### Get all geofences for a user
- **Endpoint:** `GET /geofences`
- **Description:** Retrieves all geofences for the authenticated user.
- **Authentication:** Required
- **Response (200 OK)**:
```json
{
  "status": "success",
  "data": [
    {
      "id": "uuid",
      "name": "string",
      "latitude": 40.7128,
      "longitude": -74.0060,
      "radius_meters": 100,
      "is_active": true
    }
  ]
}
```

#### Update a geofence
- **Endpoint:** `PUT /geofences/{geofence_id}`
- **Description:** Updates an existing geofence.
- **Authentication:** Required
- **Request Body**:
```json
{
  "name": "string",
  "radius_meters": 150,
  "is_active": false
}
```
- **Response (200 OK)**:
```json
{
  "status": "success",
  "data": {
    "id": "uuid",
    "name": "string",
    "radius_meters": 150,
    "is_active": false
  }
}
```

#### Delete a geofence
- **Endpoint:** `DELETE /geofences/{geofence_id}`
- **Description:** Deletes a geofence.
- **Authentication:** Required
- **Response (204 No Content)**

## 5. WebSocket Events

- **Purpose**: To provide real-time updates to the client without the need for polling.
- **Connection**: The client will establish a WebSocket connection to `/ws/safety` after authenticating.

### Event: `location.update`
- **Description**: Sent when a new location is recorded for a tag the user owns.
```json
{
  "event": "location.update",
  "data": {
    "tag_id": "uuid",
    "latitude": 40.7129,
    "longitude": -74.0061,
    "recorded_at": "2025-07-21T10:01:00Z"
  }
}
```

### Event: `tag.status.update`
- **Description**: Sent when the status or battery level of a tag changes.
```json
{
  "event": "tag.status.update",
  "data": {
    "tag_id": "uuid",
    "status": "LOW_BATTERY",
    "battery_level": 19
  }
}
```

## 6. Rate Limiting

- **Authenticated Users**: 1000 requests per minute.
- **WebSocket Connections**: 1 connection per user.

# CatTravelLogistics: API Specification

**Version:** 1.0  
**Date:** 2025-07-21  
**Status:** Draft  
**Author(s):** Gemini
**Base URL:** `/api/v1/logistics`

## 1. Overview

This document provides the detailed API specification for the Cat Travel Logistics component. The API allows users to browse, rent, and purchase cat travel containers.

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

### 4.1 Container Catalog

#### Get all containers
- **Endpoint:** `GET /containers`
- **Description:** Retrieves a list of all available travel containers.
- **Authentication:** Required
- **Query Parameters**:
  - `type` (string, `RENTAL` or `PURCHASE`)
- **Response (200 OK)**:
```json
{
  "status": "success",
  "data": [
    {
      "id": "uuid",
      "name": "string",
      "type": "RENTAL",
      "rental_price_per_day_cents": 2000,
      "image_url": "string"
    }
  ]
}
```

#### Get a single container
- **Endpoint:** `GET /containers/{container_id}`
- **Description:** Retrieves the details of a specific travel container.
- **Authentication:** Required
- **Response (200 OK)**:
```json
{
  "status": "success",
  "data": {
    "id": "uuid",
    "name": "string",
    "description": "string",
    "type": "PURCHASE",
    "price_cents": 15000,
    "dimensions_cm": {"length": 50, "width": 30, "height": 30},
    "compliance_standards": ["IATA"],
    "image_url": "string"
  }
}
```

### 4.2 Orders

#### Create an order
- **Endpoint:** `POST /orders`
- **Description:** Creates a new order for a container rental or purchase.
- **Authentication:** Required
- **Request Body**:
```json
{
  "container_id": "uuid",
  "order_type": "RENTAL", // RENTAL or PURCHASE
  "rental_start_date": "2025-08-01", // Required for RENTAL
  "rental_end_date": "2025-08-10",   // Required for RENTAL
  "shipping_address": {
    "street": "123 Main St",
    "city": "Anytown",
    "state": "CA",
    "zip": "12345"
  }
}
```
- **Response (201 Created)**:
```json
{
  "status": "success",
  "data": {
    "order_id": "uuid",
    "status": "PENDING",
    "total_price_cents": 18000,
    "payment_intent_client_secret": "string" // To be used by the frontend to complete payment
  }
}
```

#### Get user's order history
- **Endpoint:** `GET /orders`
- **Description:** Retrieves the order history for the authenticated user.
- **Authentication:** Required
- **Response (200 OK)**:
```json
{
  "status": "success",
  "data": [
    {
      "id": "uuid",
      "container_name": "string",
      "order_type": "PURCHASE",
      "status": "DELIVERED",
      "total_price_cents": 15000,
      "created_at": "2025-07-15T14:30:00Z"
    }
  ]
}
```

### 4.3 Payments (Internal)

#### Payment Gateway Callback
- **Endpoint:** `POST /payments/callback`
- **Description:** An internal webhook endpoint for the payment gateway to notify the system of the transaction status. This endpoint should be protected and only accessible by the payment gateway (e.g., via IP whitelisting or a secret header).
- **Authentication:** Varies by payment gateway (e.g., signature verification).
- **Request Body**: (Varies by payment gateway)
- **Response (200 OK)**

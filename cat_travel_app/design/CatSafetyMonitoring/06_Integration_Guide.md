# CatSafetyMonitoring: Integration Guide

**Version:** 1.0  
**Date:** 2025-07-21  
**Status:** Draft  
**Author(s):** Gemini

## 1. Overview

This document provides guidance on integrating the Cat Safety & Monitoring component with external systems. It details the integration points, data formats, and protocols required for successful integration.

## 2. Integration with 3rd Party GPS Tracking Service

- **Purpose**: To receive real-time location and status updates from the physical tracking tags.
- **Integration Method**: The GPS tracking service will push data to a secure, internal-facing API endpoint on our Location Service.
- **Endpoint**: `POST /ingest/location`
- **Authentication**: The GPS service will be provided with a long-lived API key that must be included in the `X-API-Key` header of each request.
- **Data Format (JSON)**:
```json
{
  "tag_serial_number": "string",
  "latitude": 40.7128,
  "longitude": -74.0060,
  "accuracy_meters": 2.5,
  "recorded_at": "2025-07-21T10:00:00Z",
  "status": {
    "battery_level": 85,
    "is_connected": true
  }
}
```
- **Error Handling**: If the ingestion endpoint is unavailable, the GPS service should implement a retry mechanism with exponential backoff.

## 3. Integration with 3rd Party Push Notification Service

- **Purpose**: To send alerts (e.g., geofence breaches, low battery) to the user's mobile device.
- **Integration Method**: The Notification Service will make API calls to the push notification provider (e.g., Firebase Cloud Messaging, Apple Push Notification Service).
- **Authentication**: The Notification Service will use server keys to authenticate with the push notification provider.
- **Data Format (JSON)**:
```json
{
  "to": "<device_token>",
  "priority": "high",
  "notification": {
    "title": "Cat Safety Alert",
    "body": "Your cat has left the 'Hotel Room' safe zone!"
  },
  "data": {
    "alert_type": "GEOFENCE_EXIT",
    "geofence_id": "uuid",
    "tag_id": "uuid"
  }
}
```
- **Delivery Guarantees**: The push notification service should guarantee at-least-once delivery.

## 4. Integration with User and Cat Profile Services

- **Purpose**: To associate tracking tags with the correct user and cat.
- **Integration Method**: The Tag Management Service will make internal API calls to the User Service and Cat Profile Service.
- **Endpoints**:
    - `GET /users/{user_id}`: To validate that a user exists.
    - `GET /cats/{cat_id}`: To validate that a cat profile exists and belongs to the user.
- **Data Consistency**: The system will rely on the user and cat services as the source of truth for user and cat information.

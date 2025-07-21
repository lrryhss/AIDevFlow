# CatSafetyMonitoring: Component Design

**Version:** 1.0  
**Date:** 2025-07-21  
**Status:** Draft  
**Author(s):** Gemini

## 1. Introduction

This document provides a detailed design of the individual components within the Cat Safety & Monitoring system. It breaks down each microservice identified in the System Architecture document, detailing its responsibilities, APIs, and interactions with other components.

## 2. Location Service

- **Description**: This service is the central hub for all location-related data. It ingests, processes, stores, and serves location information from the tracking tags.
- **Responsibilities**:
    - Ingest location data from the 3rd Party GPS Tracking Service.
    - Validate and process raw location data.
    - Store location data in the `location_history` table.
    - Publish `location_updated` events to the message queue.
    - Provide an API to retrieve location history for a tag.
- **API Endpoints**:
    - `POST /ingest/location`: Internal endpoint for the GPS service to push location data.
    - `GET /locations/{tag_id}`: Retrieves the location history for a specific tag, with pagination and date filters.
- **Data Storage**: Interacts with the `location_history` and `tracking_tags` tables in the PostgreSQL database.
- **Dependencies**: 3rd Party GPS Tracking Service, PostgreSQL, Redis (for caching recent locations), Message Queue.

## 3. Geofence Service

- **Description**: This service manages all aspects of geofencing, from creation to alert generation.
- **Responsibilities**:
    - Provide CRUD operations for geofences.
    - Store and manage geofence definitions in the `geofences` table.
    - Consume `location_updated` events from the message queue.
    - Evaluate the cat's current location against all active geofences for that user.
    - Publish `geofence_breached` events (with type ENTER or EXIT) to the message queue if a boundary is crossed.
- **API Endpoints**:
    - `POST /geofences`: Creates a new geofence.
    - `GET /geofences/{user_id}`: Retrieves all geofences for a user.
    - `PUT /geofences/{geofence_id}`: Updates an existing geofence.
    - `DELETE /geofences/{geofence_id}`: Deletes a geofence.
- **Data Storage**: Interacts with the `geofences` and `geofence_alerts` tables.
- **Dependencies**: Message Queue, PostgreSQL.

## 4. Notification Service

- **Description**: A stateless service that acts as a bridge between the backend and the user's device for alerts.
- **Responsibilities**:
    - Consume `geofence_breached` and `low_battery` events from the message queue.
    - Format the event data into a user-friendly notification message.
    - Integrate with a 3rd Party Push Notification Service (e.g., Firebase Cloud Messaging, Apple Push Notification Service) to send alerts to the correct user device.
    - Handle notification delivery status and retries.
- **API Endpoints**: None (internal service).
- **Dependencies**: Message Queue, 3rd Party Push Notification Service.

## 5. Tag Management Service

- **Description**: Manages the lifecycle and metadata of the physical tracking tags.
- **Responsibilities**:
    - Handle the registration and activation of new tags.
    - Associate tags with user and cat profiles.
    - Periodically receive and update the battery level and connectivity status of tags.
    - Publish `low_battery` events to the message queue when the battery level drops below a certain threshold (e.g., 20%).
- **API Endpoints**:
    - `POST /tags/register`: Registers a new tag with a user and cat.
    - `GET /tags/{user_id}`: Retrieves all tags associated with a user.
    - `GET /tags/status/{tag_id}`: Gets the current status and battery level of a specific tag.
    - `POST /tags/ingest/status`: Internal endpoint for the GPS service to push tag status updates.
- **Data Storage**: Interacts with the `tracking_tags` table.
- **Dependencies**: PostgreSQL, Message Queue.

## 6. API Gateway

- **Description**: The single, unified entry point for the mobile client.
- **Responsibilities**:
    - **Authentication**: Verify JWT tokens on incoming requests.
    - **Rate Limiting**: Protect backend services from abuse by enforcing request limits.
    - **Request Routing**: Route incoming requests to the appropriate microservice (Location, Geofence, or Tag Management).
    - **API Composition**: Potentially aggregate results from multiple services for a single client request.
- **Configuration**:
    - Routes will be configured to map API endpoints to the internal service addresses.
    - Authentication and rate-limiting plugins will be enabled.

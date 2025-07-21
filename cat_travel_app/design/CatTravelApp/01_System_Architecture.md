# Cat Travel App: System Architecture

**Version:** 1.0  
**Date:** 2025-07-21  
**Status:** Draft  
**Author(s):** Gemini

## 1. Introduction

This document outlines the high-level system architecture for the Cat Travel App. It illustrates how the three core components—`CatTravelPlanning`, `CatTravelLogistics`, and `CatSafetyMonitoring`—are organized and interact within a unified microservices architecture.

## 2. Architectural Goals

- **Modularity**: Each core component is an independent, deployable microservice, allowing for separate development, scaling, and maintenance.
- **Scalability**: The architecture is designed to scale each component based on its specific load.
- **Resilience**: The use of a message queue and an API gateway improves the system's resilience to the failure of individual services.
- **Unified User Experience**: Despite being composed of multiple services, the application provides a seamless and unified experience to the end-user.

## 3. Architectural Principles

- **Microservices**: The system is divided into services based on business capabilities.
- **Centralized Entry Point**: An API Gateway is used to provide a single, consistent entry point for the mobile client.
- **Asynchronous Communication**: A message queue is used for inter-service communication for non-blocking, event-driven workflows.
- **Shared Data Layer**: While each service has its own schema, they all reside within the same PostgreSQL database, allowing for easier data management and potential for data sharing if needed.

## 4. System Components

- **Mobile Application (Client)**: The single user-facing application for iOS and Android.
- **API Gateway**: The central gateway that routes requests to the appropriate service.
- **Planning Service**: Manages accommodation search and travel checklists.
- **Logistics Service**: Manages container rental and purchase.
- **Safety Service**: Manages real-time location tracking and geofencing.
- **Shared Services**: Common services such as User Authentication, which are used by all other components.

## 5. High-Level Architecture Diagram

```mermaid
graph TD
    subgraph "User Layer"
        MobileApp[Mobile Application]
    end

    subgraph "External Services"
        AccommodationAPI[Accommodation APIs]
        PaymentGateway[Payment Gateway]
        InventorySystem[Inventory System]
        GPSService[GPS Tracking Service]
        PushService[Push Notification Service]
    end

    subgraph "Application Layer"
        APIGateway[API Gateway]

        subgraph "Core Microservices"
            PlanningService[Planning Service]
            LogisticsService[Logistics Service]
            SafetyService[Safety & Monitoring Service]
        end

        subgraph "Shared Services"
            AuthService[Authentication Service]
        end

        subgraph "Data & Events"
            DB[(PostgreSQL Database)]
            Cache[(Redis Cache)]
            MessageQueue[Message Queue]
        end
    end

    MobileApp --> APIGateway

    APIGateway --> AuthService
    APIGateway --> PlanningService
    APIGateway --> LogisticsService
    APIGateway --> SafetyService

    PlanningService --> DB
    PlanningService --> Cache
    PlanningService --> AccommodationAPI

    LogisticsService --> DB
    LogisticsService --> PaymentGateway
    LogisticsService --> InventorySystem

    SafetyService --> DB
    SafetyService --> Cache
    SafetyService --> MessageQueue
    SafetyService --> GPSService

    MessageQueue --> SafetyService
    MessageQueue --> PushService

    classDef user fill:#e1f5fe,stroke:#01579b
    classDef external fill:#fff3e0,stroke:#e65100
    classDef app fill:#f3e5f5,stroke:#4a148c
    classDef data fill:#e0f2f1,stroke:#004d40

    class MobileApp user
    class AccommodationAPI,PaymentGateway,InventorySystem,GPSService,PushService external
    class APIGateway,PlanningService,LogisticsService,SafetyService,AuthService app
    class DB,Cache,MessageQueue data
```

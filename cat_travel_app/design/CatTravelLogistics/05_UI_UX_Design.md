# CatTravelLogistics: UI/UX Design

**Version:** 1.0  
**Date:** 2025-07-21  
**Status:** Draft  
**Author(s):** Gemini

## 1. Overview

This document outlines the UI/UX design for the Cat Travel Logistics feature. The design aims to provide a simple and trustworthy e-commerce experience within the app.

## 2. Wireframes and Mockups

*(Note: This section would typically contain visual wireframes.)*

### 2.1 Container Catalog Screen
- **Description**: A screen displaying a grid or list of available travel containers.
- **Elements**:
    - Tabs or filters to switch between "Rent" and "Purchase".
    - Each item in the list will show a primary image, the container name, price, and a short description.
    - A search bar to find containers by name.

### 2.2 Container Details Screen
- **Description**: A screen showing all the details for a selected container.
- **Elements**:
    - A gallery of images.
    - The container name, price, and a full description.
    - A section detailing compliance standards (e.g., "IATA Compliant").
    - For rentals, a calendar to select start and end dates.
    - A prominent "Add to Cart" or "Rent Now" button.

### 2.3 Checkout Flow
- **Description**: A multi-step process to complete the transaction.
- **Steps**:
    1.  **Shipping Information**: A form to enter or select a shipping address.
    2.  **Payment**: A secure form (potentially a webview hosted by the payment gateway) to enter credit card details.
    3.  **Review and Confirm**: A final screen summarizing the order, including the container, price, and shipping address, with a "Confirm Order" button.

## 3. User Flows

### 3.1 Purchasing a Container

```mermaid
sequenceDiagram
    participant U as User
    participant App as Mobile App
    participant API as API Gateway
    participant LS as Logistics Service
    participant PG as Payment Gateway

    U->>App: Browses catalog and selects a container
    App->>API: GET /containers/{id}
    API-->>App: Container details
    U->>App: Taps "Purchase"
    App->>API: POST /orders (type: PURCHASE)
    API->>LS: Create order
    LS-->>API: Returns order_id and payment_intent
    API-->>App: Order details
    App->>PG: User enters payment info
    PG-->>App: Payment successful
    App->>API: Confirms payment
    API->>LS: Update order status to CONFIRMED
    LS-->>App: Order confirmation
    App->>U: Displays order confirmation
```

### 3.2 Renting a Container

```mermaid
stateDiagram-v2
    [*] --> Catalog
    Catalog --> Details: User selects a container
    Details --> DateSelection: User taps "Rent Now"
    DateSelection --> Shipping: User selects rental dates
    Shipping --> Payment: User enters shipping address
    Payment --> Review: User enters payment details
    Review --> Confirmation: User confirms order
    Confirmation --> [*]: Order complete
```

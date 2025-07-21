# CatTravelLogistics: Component Design

**Version:** 1.0  
**Date:** 2025-07-21  
**Status:** Draft  
**Author(s):** Gemini

## 1. Introduction

This document provides a detailed design of the components within the Cat Travel Logistics system. It focuses on the responsibilities of the Logistics Service and its interactions with external systems.

## 2. Logistics Service

- **Description**: The central microservice responsible for managing the entire lifecycle of container rentals and purchases.
- **Responsibilities**:
    - **Catalog Management**: Provides an API to manage the product catalog stored in the `containers` table.
    - **Inventory Check**: Integrates with the external Inventory Management System to check the availability of containers in real-time before displaying them to the user.
    - **Order Processing**: Creates and manages orders in the `orders` table. It handles the logic for both rentals and purchases.
    - **Payment Orchestration**: Initiates payment requests to the Payment Gateway and handles the callbacks to update the transaction status.
    - **Transaction Recording**: Records the outcome of each payment attempt in the `transactions` table.
- **API Endpoints**:
    - `GET /containers`: Retrieves the list of available containers.
    - `GET /containers/{container_id}`: Retrieves the details of a specific container.
    - `POST /orders`: Creates a new order.
    - `GET /orders`: Retrieves the order history for the authenticated user.
    - `POST /payments/callback`: An internal endpoint for the Payment Gateway to send transaction status updates.
- **Dependencies**: PostgreSQL, Payment Gateway, Inventory Management System.

## 3. Mobile Application (Client)

- **Description**: The frontend component that the user interacts with.
- **Responsibilities**:
    - **UI/UX**: Renders the container catalog, product details, and the checkout flow.
    - **Secure Payment Input**: Uses a secure method (e.g., a webview or the payment gateway's SDK) to collect payment information, ensuring that sensitive card details are never sent to the application's backend.
    - **API Communication**: Interacts with the Logistics Service via the API Gateway.

## 4. External System Interfaces

### 4.1 Inventory Management System Interface
- **Purpose**: To ensure that the app only displays and allows users to order containers that are in stock.
- **Methods**:
    - `checkAvailability(container_id, quantity)`: Checks if a certain quantity of a container is available for purchase.
    - `checkRentalAvailability(container_id, start_date, end_date)`: Checks if a container is available for rent during a specific period.
    - `reserveItem(container_id, quantity)`: Temporarily reserves an item while the user is completing the payment.
    - `confirmSale(order_id, container_id, quantity)`: Confirms the sale of an item, permanently decrementing the stock.

### 4.2 Payment Gateway Interface
- **Purpose**: To securely process payments.
- **Methods**:
    - `createPaymentIntent(amount, currency, order_id)`: Initiates a payment transaction and returns a client secret or token.
    - `handleCallback(payload)`: Processes the webhook/callback from the payment gateway to get the final status of the transaction.

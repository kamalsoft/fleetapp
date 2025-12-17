# architecture.md

# Synapse Fleet - Architecture Document

**Version:** 1.0
**Date:** 2025-12-17

## 1. Introduction

This document outlines the high-level architecture for Synapse Fleet. It describes the major components, their interactions, the technology stack, and the guiding principles behind the architectural decisions.

## 2. Architectural Goals

- **Scalability:** The system must handle a growing number of vehicles, users, and data points without performance degradation.
- **Reliability:** The system must be highly available with minimal downtime.
- **Maintainability:** The architecture should be easy to understand, modify, and extend.
- **Security:** All data, especially sensitive location and driver information, must be secure in transit and at rest.

## 3. Architectural Style

We will adopt a **Microservices Architecture**. This approach breaks down the application into smaller, independent services, each responsible for a specific business capability.

**Benefits:**

- Services can be developed, deployed, and scaled independently.
- Allows for technology diversity (choosing the right tool for the job).
- Improves fault isolation; an issue in one service is less likely to bring down the entire system.

## 4. System Components

Synapse Fleet is composed of the following core microservices:

- **Vehicle Service:** Manages all vehicle-related data (VIN, make, model, maintenance schedules).
- **IAM Service (Identity & Access Management):** Manages companies, users, roles, permissions, and API keys.
- **Driver Service:** Manages the full driver lifecycle: recruiting, onboarding, profiles, qualifications, documents, HOS logs, and settlements.
- **Tracking Service:** Ingests and processes real-time GPS data from telematics devices.
- **FleetOps Service:** Handles core operations: creating and managing places, service orders, and payloads.
- **Integrations Service:** Manages and dispatches webhooks to external systems.
- **Video Service:** Ingests, stores, and processes video footage from dash cams, running AI models to detect events.
- **Vendor Service:** Manages third-party vendors, their services, and the lifecycle of their invoices.
- **Analytics Service:** Processes historical and real-time data to generate predictive insights (e.g., maintenance forecasts).
- **Notification Service:** Sends alerts and notifications (email, SMS, push) to users.
- **API Gateway:** A single entry point for all client requests (Web App, Mobile App), handling routing, authentication, and rate limiting.

### Component Diagram (Mermaid)

```mermaid
graph TD
    subgraph Clients
        WebApp
        MobileApp
    end

    subgraph FMS Backend
        API_Gateway
        subgraph Microservices
            VehicleService
            IAMService
            DriverService
            TrackingService
            FleetOpsService
            IntegrationsService
            AnalyticsService
            VendorService
            VideoService
            NotificationService
        end
    end

    subgraph Databases
        PostgreSQL
        TimescaleDB
        DataLake[Data Lake (e.g., S3)]
        DataWarehouse[Data Warehouse (e.g., Snowflake)]
    end

    subgraph External
        TelematicsDevice
        DashCamDevice
        MapsAPI
    end

    Clients --> API_Gateway
    API_Gateway --> VehicleService
    API_Gateway --> DriverService
    API_Gateway --> IAMService
    API_Gateway --> TrackingService
    API_Gateway --> FleetOpsService
    API_Gateway --> IntegrationsService
    API_Gateway --> AnalyticsService
    API_Gateway --> VendorService
    API_Gateway --> VideoService
    API_Gateway --> NotificationService

    TelematicsDevice -- GPS Data --> TrackingService
    DashCamDevice -- Video Stream --> VideoService
    FleetOpsService -- Uses --> MapsAPI

    VehicleService --> PostgreSQL
    DriverService --> PostgreSQL
    IAMService --> PostgreSQL
    AnalyticsService --> PostgreSQL
    VendorService --> PostgreSQL
    VideoService --> DataLake
    FleetOpsService --> PostgreSQL
    TrackingService --> TimescaleDB

    TrackingService -- Telemetry Data --> DataLake
    PostgreSQL -- Transactional Data --> DataLake
    DataLake -- ETL --> DataWarehouse
    AnalyticsService -- Model Training --> DataWarehouse
```

## 5. Technology Stack

- **Backend Services:** Node.js with TypeScript / Go / Java (depending on service needs)
- **Databases:**
  - **PostgreSQL:** For relational data (vehicles, drivers, jobs).
  - **TimescaleDB (PostgreSQL extension):** For time-series GPS data.
- **Frontend (Web):** React / Vue.js
- **Mobile App:** React Native / Flutter
- **Infrastructure:** Docker, Kubernetes
- **Cloud Provider:** AWS / Azure / Google Cloud
- **Communication:** REST APIs, gRPC for internal service-to-service communication.

## 6. Deployment Strategy

The application will be containerized using Docker and orchestrated with Kubernetes. This allows for automated scaling, rolling updates, and high availability. A CI/CD pipeline (e.g., Jenkins, GitLab CI) will be used to automate testing and deployment.

## 7. Communication Patterns

- **Synchronous Communication (REST/gRPC):** For client-to-server requests and internal request/response interactions where an immediate response is required. A **Service Mesh** (e.g., Istio, Linkerd) can be layered on top to manage traffic, observability, and security.
- **Asynchronous Communication (Message Queue - e.g., RabbitMQ/Kafka):** For events that do not require an immediate response and benefit from decoupling services. Examples include:
  - The `Tracking Service` publishes a `VehicleLocationUpdated` event.
  - The `FleetOps Service` publishes an `order.completed` event, which triggers driver settlement calculations in the `Driver Service`.
  - The `Analytics Service` and `Integrations Service` consume these events to trigger their respective workflows.

For services with complex state, like `FleetOps Service`, an **Event Sourcing** pattern can be adopted to treat all state changes as a sequence of events, providing a full audit log and enabling powerful analytics.

The application will be containerized using Docker and orchestrated with Kubernetes. This allows for automated scaling, rolling updates, and high availability. A CI/CD pipeline (e.g., Jenkins, GitLab CI) will be used to automate testing and deployment.

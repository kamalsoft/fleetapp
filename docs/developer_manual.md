# developer_manual.md

# Synapse Fleet - Developer Manual

Welcome, developer! This guide will help you get your development environment set up and start contributing to the Synapse Fleet project.

## 1. Development Environment Setup

1.  **Prerequisites:**

    - Git
    - Docker and Docker Compose
    - Node.js (v18.x or later)
    - A code editor (e.g., VS Code)

2.  **Clone the Repository:**

    ```bash
    git clone <repository_url>
    cd fms-backend
    ```

3.  **Local Environment:**
    The entire local environment (microservices, databases) is managed by Docker Compose.
    ```bash
    # Start all services in the background
    docker-compose up -d
    ```
    This will build and start containers for each microservice and the required databases.

## 2. Source Code Structure

The backend monorepo is organized by service:

```
/fms-backend
├── /services
│   ├── /tracking-service   # GPS data ingestion
│   ├── /vehicle-service    # Vehicle management
│   └── ...                 # Other microservices
├── /libs                   # Shared libraries and utilities
├── docker-compose.yml      # Local environment definition
└── ...
```

## 3. Coding Standards

- **Language:** All new backend code should be written in TypeScript.
- **Formatting:** We use Prettier for automated code formatting. Run `npm run format` before committing.
- **Linting:** We use ESLint to enforce code quality. Run `npm run lint` to check for issues.
- **Commit Messages:** Follow the Conventional Commits specification.

## 4. Build and Deployment

The project uses a CI/CD pipeline on GitLab. When you push a new branch, the pipeline will automatically:

1.  Run linting and unit tests.
2.  Build a Docker image for the changed service.
3.  Deploy the image to a staging environment for review.

Merging to the `main` branch will trigger a deployment to the production environment after all tests pass.

## 5. API Security Best Practices

- **Use Environment Variables for Secrets:** Never hard-code API keys, database credentials, or other secrets in the source code. Use environment variables, as shown in the `docker-compose.yml` file. For production, use a secret management service like AWS Secrets Manager or HashiCorp Vault.
- **Input Validation:** Validate and sanitize all user input to prevent injection attacks (SQLi, XSS). Use libraries like `class-validator` in the Node.js services.
- **Rate Limiting:** Implement rate limiting at the API Gateway to protect against brute-force attacks and denial-of-service (DoS) attempts.
- **Principle of Least Privilege:** When a service communicates with the database or another service, ensure it has only the minimum permissions necessary to perform its function.

## 6. Webhook Development

- **Subscribing:** To receive events, create a new webhook in the application dashboard by providing your endpoint URL and selecting the events you want to subscribe to (e.g., `order.created`, `order.completed`).
- **Payloads:** All webhook POST requests will contain a JSON body with the event type and the full data payload of the relevant resource.
- **Verification:** Each webhook request will include a unique signature in its header (e.g., `X-Fleet-Signature`). You should use your webhook's secret key to verify this signature to ensure the request originated from our system.
- **Responding:** Your endpoint should respond with a `200 OK` status code as quickly as possible. Any complex logic should be handled asynchronously to avoid timeouts. If our system does not receive a `200 OK`, it will attempt to retry the delivery.

## 7. Ecosystem Integrations

Our platform is designed to be the central hub of your operations. We provide several ways to connect with the broader logistics ecosystem:

- **REST API:** Use our comprehensive REST API for custom development and data retrieval.
- **Webhooks:** Subscribe to real-time events to keep your systems in sync.
- **Pre-built Connectors:** We offer out-of-the-box integrations for popular systems like:
  - **Transportation Management Systems (TMS):** Automatically sync orders and dispatch information.
  - **Fuel Cards:** Reconcile fuel purchases and detect fraud.
  - **ERP Systems:** Connect fleet data with your company's financial and resource planning.

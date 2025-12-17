# User Story: Analytics Service Setup

**As a** developer,
**I want** the Analytics Service container and basic structure set up,
**so that** it can be connected to the message queue to consume events from other services.

---

### Acceptance Criteria:

- The `Analytics Service` is created in the monorepo and added to `docker-compose.yml`.
- The service successfully subscribes to `VehicleLocationUpdated` events from the message queue.
- Consumed events are logged by the service for verification purposes.

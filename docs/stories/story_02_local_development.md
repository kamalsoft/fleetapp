# User Story: Local Development Environment

**As a** developer,
**I want** a `docker-compose.yml` file that spins up all required backend services and databases,
**so that** I can run and test the entire application stack on my local machine with a single command.

---

### Acceptance Criteria:

- Running `docker-compose up -d` successfully starts containers for all MVP services (IAM, Vehicle, Driver, Tracking, FleetOps) and the database.
- Services can communicate with each other over the Docker network.

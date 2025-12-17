# User Story: Add a New Vehicle

**As a** fleet manager,
**I want** to add a new vehicle to my fleet by providing its details (e.g., VIN, make, model),
**so that** I can manage and track my assets within the system.

---

### Acceptance Criteria:

- A secure endpoint `POST /api/v1/vehicles` exists.
- The endpoint validates the incoming data (e.g., VIN format).
- A new vehicle record is created in the `VEHICLES` table.

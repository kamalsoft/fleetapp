# User Story: User Registration

**As a** new fleet manager,
**I want** to register for an account using my email and a password,
**so that** I can gain access to the platform.

---

### Acceptance Criteria:

- An API endpoint `POST /api/v1/iam/register` exists.
- A new user record is created in the database with a securely hashed password.
- A successful registration returns a success message.

# User Story: User Login

**As a** registered user,
**I want** to log in with my email and password,
**so that** I can receive a secure token to access the application's features.

---

### Acceptance Criteria:

- An API endpoint `POST /api/v1/iam/login` exists.
- Upon successful authentication, the API returns a valid JSON Web Token (JWT).
- An attempt with invalid credentials returns an authentication error.

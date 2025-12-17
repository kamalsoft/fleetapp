# User Story: Generate API Key

**As a** Developer or Administrator,
**I want** to generate a secure API key from my account settings,
**so that** I can programmatically access my fleet data for custom integrations.

---

### Acceptance Criteria:

- A secure endpoint `POST /api/v1/iam/api-keys` exists to create a new API key.
- The generated API key is associated with the authenticated user's account.
- The full API key secret is displayed to the user **only once** upon creation for security.
- The user can view a list of their generated keys (e.g., key name, creation date, first few characters) but not the full secret.
- The user can revoke (delete) an existing API key.

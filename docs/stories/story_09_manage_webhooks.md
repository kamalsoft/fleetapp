# User Story: Manage Webhooks

**As a** Developer or Administrator,
**I want** to create and manage webhooks through my account dashboard,
**so that** I can subscribe to real-time events and integrate my own applications with the fleet management system.

---

### Acceptance Criteria:

- A user can navigate to a "Webhooks" or "Developer" section in their account settings.
- A user can create a new webhook by providing a destination URL and selecting from a list of available event types (e.g., `order.created`, `vehicle.moving`).
- Upon creation, the system provides a secret key for signature verification that is displayed only once.
- The user can view a list of their existing webhooks, showing the URL, status, and subscribed events.
- The user can edit an existing webhook's URL and event subscriptions.
- The user can delete a webhook to stop receiving events.
- The user can view a log of recent delivery attempts for a webhook, including success/failure status.

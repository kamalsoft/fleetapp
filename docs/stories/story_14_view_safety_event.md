# User Story: Review AI-Detected Safety Event

**As a** Safety Manager or Fleet Manager,
**I want** to review a video clip and details of an AI-detected harsh braking event,
**so that** I can understand the context of the event and coach my driver to improve safety.

---

### Acceptance Criteria:

- An AI-detected harsh braking event appears in a "Safety Inbox" or "Events" feed in the web application.
- Each event in the feed displays the event type ("Harsh Braking"), driver name, vehicle name, and timestamp.
- Clicking on an event opens a detailed view that includes:
  - A video player showing the clip of the event (e.g., 10 seconds before and 10 seconds after).
  - A map showing the location where the event occurred.
  - Key event data: speed at the time of the event, G-force measurement.
- The user can change the status of the event (e.g., "Needs Review", "Coached", "Dismissed").
- The user can add notes to the event for record-keeping (e.g., "Coached driver on maintaining safe following distance").
- The system logs who reviewed the event and when.

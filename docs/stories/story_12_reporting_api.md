# User Story: Reporting API Endpoints

**As a** frontend developer,
**I want** secure API endpoints to retrieve aggregated vehicle analytics for a given date range,
**so that** I can build reports and charts in the web application.

---

### Acceptance Criteria:

- An endpoint `GET /api/v1/analytics/reports/mileage` exists to return mileage data for a specified vehicle and date range.
- An endpoint `GET /api/v1/analytics/reports/idling` exists to return idle time data for a specified vehicle and date range.
- The endpoints return data sourced from the `DAILY_VEHICLE_STATS` table.

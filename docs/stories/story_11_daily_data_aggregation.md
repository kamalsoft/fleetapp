# User Story: Daily Data Aggregation

**As a** system,
**I want** to run a daily scheduled job to process the previous day's telematics data,
**so that** I can calculate and store aggregated metrics like total distance traveled and total idle time for each vehicle.

---

### Acceptance Criteria:

- A new table, `DAILY_VEHICLE_STATS`, exists to store the aggregated data.
- A scheduled task runs once every 24 hours.
- The task queries the `GPS_DATA` table, calculates total mileage and idle duration for each vehicle for the previous day, and stores the results in the new table.
- The job is resilient and logs any errors encountered during processing.

# use_cases.md

# Synapse Fleet - Use Cases

**Version:** 1.0
**Date:** 2025-12-17

## 1. Actors

- **Fleet Manager:** Responsible for the overall operational efficiency and safety of the fleet.
- **Dispatcher:** Responsible for assigning jobs and managing routes.
- **Driver:** Responsible for operating the vehicle and completing service orders.
- **System:** The Synapse Fleet platform.

## 2. Use Cases

### UC-01: Track a Vehicle in Real-Time

- **Actor:** Fleet Manager, Dispatcher
- **Description:** The user wants to see the current location and status of a specific vehicle on a map.
- **Preconditions:** The user is logged in. The vehicle is equipped with an active telematics device.
- **Basic Flow:**
  1.  User navigates to the "Live Map" screen.
  2.  System displays a map with icons for all active vehicles.
  3.  User clicks on a vehicle icon.
  4.  System displays a panel with the vehicle's ID, driver name, current speed, and address.
- **Alternative Flows:**
  - **4a. Vehicle is offline:** If the system has not received data from the vehicle for more than 5 minutes, it displays the vehicle's last known location and a "Device Offline" status.

### UC-02: Schedule Preventive Maintenance

- **Actor:** Fleet Manager
- **Description:** The user wants to schedule an upcoming maintenance service for a vehicle based on its mileage.
- **Preconditions:** The user is logged in. The vehicle exists in the system.
- **Basic Flow:**
  1.  System automatically detects that a vehicle (e.g., Truck #101) has passed its 15,000-mile service interval.
  2.  System creates a "Maintenance Due" alert for the Fleet Manager.
  3.  Fleet Manager clicks the alert.
  4.  System displays the vehicle's maintenance screen.
  5.  Fleet Manager creates a new service order for an "Oil Change".
  6.  Fleet Manager assigns the service to a date and a vendor.
  7.  System marks the alert as "Scheduled".

### UC-03: View Driver's Hours of Service (HOS) Log

- **Actor:** Fleet Manager, Compliance Officer
- **Description:** The user wants to review a driver's electronic log for a specific day to ensure compliance with HOS regulations.
- **Preconditions:** The user is logged in with permissions to view HOS logs. The driver has been recording their status.
- **Basic Flow:**
  1.  User navigates to the "Compliance" section and selects "Driver Logs".
  2.  User selects a driver from a list and chooses a date.
  3.  System retrieves and displays the driver's log for that day, showing a graph of their status changes (Off Duty, Sleeper Berth, Driving, On Duty).
  4.  System also displays a summary of remaining drive time, shift time, and cycle time.
  5.  User can view, print, or export the log.
- **Alternative Flows:**
  - **3a. Log contains violations:** If the log has any HOS violations (e.g., exceeded drive time), the system clearly highlights these violations in red on the graph and in a summary list.
  - **3b. Driver has uncertified logs:** The system displays a prominent warning that the driver has logs from previous days that have not yet been certified.

### UC-04: Fulfill a Multi-Stop Service Order

- **Actor:** Dispatcher, Driver
- **Description:** A dispatcher creates a service order with one pickup and two drop-off locations, which a driver then completes.
- **Preconditions:** The user is logged in. The driver, vehicle, and locations (places) exist in the system.
- **Basic Flow:**
  1.  **Dispatcher** creates a new `Service Order`.
  2.  **Dispatcher** adds a `Payload` to the order with a "Pickup" location and a "Dropoff" location.
  3.  **Dispatcher** adds a second `Payload` to the order with the same "Pickup" location and a different "Dropoff" location.
  4.  **Dispatcher** assigns a Driver and Vehicle to the `Service Order`.
  5.  **Driver** receives the order on their mobile app.
  6.  **Driver** navigates to the pickup location and marks the payloads as "Picked Up".
  7.  **Driver** navigates to the first drop-off location and marks the first payload as "Delivered".
  8.  **Driver** navigates to the second drop-off location and marks the second payload as "Delivered".
  9.  **System** automatically marks the `Service Order` as "Completed".
- **Alternative Flows:**
  - **7a. Partial Delivery:** If a payload cannot be delivered, the driver can mark it as "Delivery Failed" and add notes. The order remains in progress.

### UC-05: Proactively Manage Vehicle Health

- **Actor:** Fleet Manager
- **Description:** A fleet manager receives a predictive maintenance alert for a vehicle and schedules service before a component fails, preventing costly downtime.
- **Preconditions:** The user is logged in. The Analytics Service is running and has sufficient data.
- **Basic Flow:**
  1.  **System** (Analytics Service) analyzes telematics and maintenance history for a vehicle.
  2.  **System** predicts a high probability of a brake failure within the next 3,000 miles.
  3.  **System** creates a "Predictive Maintenance Alert" for the Fleet Manager, detailing the component, confidence score, and supporting data.
  4.  **Fleet Manager** reviews the alert and clicks "Schedule Service".
  5.  **System** pre-populates a maintenance order for a "Brake System Inspection".
  6.  **Fleet Manager** assigns the service to a vendor, preventing an on-road failure.

### UC-06: Onboard a New Driver and Manage Their Documents

- **Actor:** Fleet Manager, Driver
- **Description:** A fleet manager onboards a new driver, who then uploads their required documents via the mobile app.
- **Preconditions:** The user is logged in.
- **Basic Flow:**
  1.  **Fleet Manager** creates a new driver profile in the Synapse Fleet dashboard, setting their status to "Recruiting".
  2.  **Fleet Manager** sends an invitation to the driver to download the mobile app.
  3.  **Driver** downloads the app and logs in.
  4.  **Driver** is prompted to upload their documents (e.g., CDL, Medical Card) by taking photos with their phone.
  5.  **Driver** submits the documents.
  6.  **Fleet Manager** receives a notification to review the documents.
  7.  **Fleet Manager** reviews and approves the documents, setting an expiry date for each.
  8.  **Fleet Manager** changes the driver's status to "Active".
- **Alternative Flows:**
  - **7a. Document Rejected:** If a document is blurry or incorrect, the Fleet Manager can reject it with a note, prompting the driver to re-upload.

### UC-07: Add Deduction to Driver Settlement

- **Actor:** Fleet Manager, Accountant
- **Description:** A fleet manager adds a one-time deduction to a driver's settlement before finalizing payroll.
- **Preconditions:** The user is logged in with permissions to manage settlements. A draft settlement for a driver exists.
- **Basic Flow:**
  1.  **User** navigates to the "Settlements" section and selects the driver's draft settlement for the current pay period.
  2.  **User** clicks the "Add Deduction" button.
  3.  **User** enters a description (e.g., "Cash Advance") and an amount for the deduction.
  4.  **User** saves the deduction.
  5.  **System** creates a new record in the `SETTLEMENT_DEDUCTIONS` table linked to the settlement.
  6.  **System** recalculates the `total_deductions` and `net_pay` on the parent `DRIVER_SETTLEMENTS` record.
  7.  The new deduction appears as a line item on the settlement details page.
- **Alternative Flows:**
  - **4a. Invalid Amount:** If the user enters a non-numeric or negative value for the amount, the system displays an error and prevents saving.

### UC-08: Onboard a Maintenance Vendor

- **Actor:** Fleet Manager
- **Description:** A fleet manager adds a new trusted repair shop to the system so they can be assigned maintenance work.
- **Preconditions:** The user is logged in with permissions to manage vendors.
- **Basic Flow:**
  1.  **User** navigates to the "Vendors" section and clicks "Add Vendor".
  2.  **User** enters the vendor's name, contact information, address, and specialty (e.g., "Tire Repair", "Engine Work").
  3.  **User** saves the new vendor profile.
  4.  **System** creates a new record in the `VENDORS` table.
  5.  The new vendor is now available to be assigned to maintenance work orders.

### UC-09: Process a Vendor Invoice

- **Actor:** Fleet Manager, Accountant
- **Description:** A user receives an invoice from a vendor for completed maintenance and processes it for payment.
- **Preconditions:** The user is logged in. The vendor exists, and a corresponding maintenance log for the work exists.
- **Basic Flow:**
  1.  **User** navigates to the "Vendors" section and selects the "Invoices" tab.
  2.  **User** clicks "Add Invoice" and selects the vendor and the related maintenance job.
  3.  **User** enters the invoice number, amount, and due date, and can optionally upload a scan of the invoice.
  4.  **System** saves the invoice with a status of "Pending".
  5.  When payment is made, the user can update the status to "Paid".

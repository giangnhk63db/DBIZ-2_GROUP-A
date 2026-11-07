# **Screen List: Verified Vehicle Report**

Screen ID pattern: S-\<feature\>-\<nn\>, for example S-F01-02 is the second screen of feature F-01. Screens shared by every feature (sign in, settings) use S-F00-\<nn\>.

| Screen ID | Screen name | Actor | Feature | Shown when (purpose) | Entry points | Use cases / FRs served | Priority | Screen spec |
| :---: | :---: | :---: | :---: | ----- | ----- | ----- | :---: | ----- |
| S-F00-01 | Home | Car Owner | F-00 | Shown when the car owner is on the Tasco Mobility system home screen. Offers node Verified Vehicle Report to create/open an existing vehicle profile. | The precondition is that car owner must have signed in and had account in Tasco Mobility system  | UC-1.01; FR-01-01-FE | Must |  |
| S-F01-01 | Create Vehicle  Report (Enter VIN) | Car Owner | F-01 | Shown after the car owner clicks the node Create Verified Vehicle Report. The owner enters a 17-character VIN. This is also how a new vehicle is added: a vehicle enters the system only through this step. | S-F00-01 | UC-1.01, UC-2.10; FR-01-01-FE, FR-01-01-BE | Must |  |
| S-F01-02 | Vehicle Profile | Car Owner | F-01 | Shown after the VIN is accepted and the vehicle profile is created, and whenever the car owner opens an existing vehicle. The one scrolling screen displays 3 main blocks: vehicle identification and  status.  Vehicle identification includes VIN, engine number, make, model, year of manufacture, paint color, number of seats.  Vehicle status includes mileage, maintenance & repair history, battery health (only for electric cars), legal status (inspection, unpaid traffic fines, mortgage). The owner sees the full overview by scrolling, with no extra navigation for short information fields. Market data including preliminary market price and 12-week line charts | S-F01-01 (after the VIN is accepted); S-F00-01 (existing vehicle) | UC-1.01, UC-1.07, UC-1.08, UC-1.09, UC-1.10, FR-01-01-FE, FR-01-09-FE, FR-01-10-INT, FR-01-11-INT, FR-01-12-INT, FR-01-13-FE, FR-01-13-BE | Must |  |
| S-F01-03 | Verified Minimum Mileage | Car Owner | F-01 | Shown when the car owner clicks “View details” in section “Minimum mileage” of S-F01-02. Displays the toll history timeline and the verified minimum mileage retrieved from VETC. If no passage record exists, it shows the “mileage unverified” status. | S-F01-02 (Section “Minimum mileage” | UC-1.03; FR-01-03-FE, FR-01-03-INT | Must |  |
| S-F01-05 | Maintenance History | Car Owner | F-01 | Shown when the car owner clicks “View all” in section “Maintenance & repair history” of S-F01-02. Displays the full maintenance timeline from Carpla Service. The owner uploads an external invoice (PDF, XML or image) via the Upload invoice modal and sees the updated timeline. | S-F01-02 (section “Maintenance & repair history”) | UC-1.04, UC-1.05; FR-01-04-FE, FR-01-04-BE, FR-01-05-FE, FR-01-05-BE, FR-01-06-BE, FR-01-07-INT, FR-01-07-BE | Must |  |
| S-F01-06 | Battery Health (SoH) | Car Owner | F-01 | Shown when the car owner clicks “View all” in the section “Battery health”. Displays the Battery State of Health (SoH) calculated from charging session history. If charging data is insufficient, it shows the Error notice. | S-F01-02 (section “Maintenance & repair history”) | UC-1.06; FR-01-08-FE, FR-01-08-INT, FR-01-08-BE | Could |  |
| S-F02-01 | Market Price & 12-Week Trend | Car Owner | F-02 | Shown when the car owner clicks “View trend” in section “Market data” of S-F01-02. Displays the latest weekly market price with its confidence level and the 12-week trend chart. Right after a new vehicle is added, it shows the “Calculating Price” message until the price history is ready. | S-F01-02 (“Market Price section”)  | UC-2.01, UC-2.02, UC-2.10; FR-02-09-FE, FR-02-09-BE, FR-02-10-FE, FR-02-10-BE | Must |  |
| S-F03-01 | Submit Sale Request | Car Owner | F-03 | Shown when the car owner clicks the fixed “Submit sale request” button lying on the final of the Verified Vehicle Report (S-F01-02). The selling form is pre-filled from the verified report and updated legal status to fill in. The owner just enters current mileage, desired price and phone number, then clicks Submit. After submission, the screen shows the screen submitted successfully. | S-F01-02  | UC-3.01, UC-3.02; FR-03-01-FE, FR-03-02-BE | Must |  |
| S-F03-02 | Sale Request Review & Quote | Carpla Staff | F-03 | Shown when Carpla staff receive a sale request in Carpla CRM and open it. Displays the packaged verified report: car owner declared information and  fully verified vehicle report. The staff inputs a suggested quote range and sends it to the car owner. | S-F03-01 | UC-3.07, UC-3.08; FR-03-04-FE, FR-03-04-BE, FR-03-05-FE, FR-03-05-BE | Must |  |
| S-F03-03 | Sale Request & Preliminary Quote | Car Owner | F-03 | Displays the preliminary quote once Carpla staff has sent it, otherwise that the request is in the status of waiting for Carpla. | S-F03-02, S-F03-01 | UC-3.09; FR-03-06-FE, FR-03-06-BE | Should |  |

## 

## **Modals and overlays**

| Modal | Opened from screen | Purpose | Note |
| ----- | ----- | ----- | ----- |
| Error notification Validate FE)  | S-F01-01, S-F01-04, S-F03-01, S-F03-02 | Tells the user that the input failed the client-side check (wrong format, missing or invalid field). The user fixes the input and retries. No request is sent to the server. | Error notification (FE) displays in the format in-line; right below the field  |
| Error notification (Validate BE) | S-F01-01, S-F01-03, S-F01-04, S-F01-06, S-F03-01 | Tells the user that the system or an external service could not complete the request after the maximum retries. Shows a message and a Retry or Close button. | Error notification (BE) displays in the format popups |
| Success notification  | S-F01-01, S-F01-04, S-F03-01, S-F03-02 | Confirms that the system completed successfully and saved the action. Shows a message and a Close button. | Success notification displays in the format popups |

## **Use cases without a screen**

| Use case | Priority | Why there is no screen |
| ----- | ----- | ----- |
| UC-2.03 Check Verified Mileage Plausibility | Must | Step of the weekly market price job started by the Weekly Timer. |
| UC-2.04 Calculate Weekly Market Price | Must | Started by the Weekly Timer. |
| UC-2.05 Sync Listings from Partner Marketplaces | Must | Background job. |
| UC-2.06 Sync 180-Day Listing History | Could | Background job. |
| UC-2.07 Normalize and Clean Listings | Must | Background job. |
| UC-2.08 Select Comparable Vehicles | Must | Background job. |
| UC-2.09 Calculate Market Price and Confidence | Must | Background job. |
| UC-2.11 Stop Market Price Tracking | Could | Could priority, not designed in this release. |
| UC-3.06 Package & Forward Verified Report and Market Price to Carpla Staff | Must | Started by the system after the legal re-check. The car owner only gets a notification when forwarding fails (AD-UC-3.06). |
| UC-3.03, UC-3.04, UC-3.05 | Should | Run inside the legal re-check (UC-3.02) when the sale request is submitted. The car owner only sees the result of the submit on S-F03-01.. |

## 

## **Checks before you save**

* \[x\] Every Screen ID is unique. No ID is used for two different screens.  
* \[x\] No row is a state (error, empty, loading, success) or a confirmation modal.  
* \[x\] Every Must use case has at least one screen, or a note saying it has no user interface (for example a time-triggered activity).  
* \[x\] Every actor action in the activity diagrams maps to a screen in this list.  
* \[x\] Every screen serves at least one FR. A screen that serves none is out of scope or a missing FR.


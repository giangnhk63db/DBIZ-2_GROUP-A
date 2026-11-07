# W1 Function List: 
L2 = a business behaviour the client signs off, with its EARS criterion and function-point type. L3 = one work item in one layer (FE = front end, BE = back end, INT = integration), sized S = 4 h, M = 8 h, L = 16 h. Initial priority is set here.

## L2 functions

| Feature | L2 ID | L2 function name | EARS acceptance criterion | Source trace | Initial priority | FP type |
|---|---|---|---|---|---|---|
| F-01 | FR-01-01 | Enter VIN to initialize Report | WHEN the car owner submits a 17 character VIN, the system SHALL validate the VIN format and create a new digital vehicle profile; IF the VIN format is invalid, the system SHALL reject the submission and display an error. | UC-1.01; CTX: flow-02; SEQ: S-1.01, S-1.02, S-1.04, S-1.03 | Must | EI |
| F-01 | FR-01-02 | View Vehicle Identification | WHEN the car owner opens vehicle identification, the system SHALL fetch and display verified license plate, brand, and engine data from VETC; IF the VETC connection fails, the system SHALL display a service unavailable alert. | UC-1.02; CTX: flow-05; SEQ: S-1.05, S-1.08, S-1.06, S-1.07 | Must | EQ |
| F-01 | FR-01-03 | View Verified Minimum Mileage | WHEN the car owner navigates to mileage section, the system SHALL retrieve passage history from VETC and display verified minimum ODO; IF no passage records exist, the system SHALL show mileage unverified status. | UC-1.03; CTX: flow-05, flow-12; SEQ: S-1.09, S-1.12, S-1.10, S-1.11 | Must | EQ |
| F-01 | FR-01-04 | View Maintenance and Repair History in Carpla Service | WHEN the car owner accesses maintenance history, the system SHALL fetch and display official service records from Carpla Service; IF no official record is found, the system SHALL display an empty history prompt. | UC-1.04; CTX: flow-13, flow-05; SEQ: S-2.01, S-2.04, S-2.02, S-2.03 | Must | EQ |
| F-01 | FR-01-05 | Upload External Maintenance and Repair Invoices | WHEN the car owner uploads an external repair invoice file, the system SHALL accept PDF, XML, or image formats and store the upload; IF the file format or size is invalid, the system SHALL reject the file. | UC-1.05; CTX: flow-03, flow-05; SEQ: S-2.05, S-2.06 | Should | EI |
| F-01 | FR-01-06 | Extract invoice data via OCR or parsing | WHEN an external invoice file is uploaded, the system SHALL extract seller tax code, invoice number, and total amount; IF parsing fails, the system SHALL flag the file for manual verification. | UC-1.05; CTX: flow-03; SEQ: S-2.06 | Could | NC |
| F-01 | FR-01-07 | Cross check invoice authenticity with Electronic Invoice Portal | WHEN invoice metadata is extracted, the system SHALL verify authenticity with the Electronic Invoice Portal; IF valid, update the maintenance timeline; IF invalid, reject invoice and show error message. | UC-1.05; CTX: flow-17, flow-18, flow-05; SEQ: S-2.07, S-2.08, S-2.09, S-2.10, S-2.11 | Could | EO |
| F-01 | FR-01-08 | View Battery State of Health | WHERE the vehicle is an electric vehicle, the system SHALL calculate and display battery SoH based on charging session history; IF charging data is insufficient, the system SHALL display an estimation pending notice. | UC-1.06; CTX: flow-05; SEQ: S-3.01, S-3.05, S-3.02, S-3.03, S-3.04 | Could | EO |
| F-01 | FR-01-09 | Trigger Refresh Legal Status | WHEN the car owner clicks the Refresh button, the system SHALL dispatch parallel verification requests to government portals; IF a request is already in progress, the system SHALL disable duplicate clicks. | UC-1.07; CTX: flow-04; SEQ: S-4.01 | Must | EI |
| F-01 | FR-01-10 | Check Inspection Status | WHEN triggered by legal refresh, the system SHALL query the Vehicle Inspection Portal using plate and VIN; IF the portal responds, the system SHALL return validity period; IF timeout, return status pending. | UC-1.08; CTX: flow-23, flow-24; SEQ: S-4.02, S-4.03 | Must | EQ |
| F-01 | FR-01-11 | Check Unpaid Traffic Fine Status | WHEN triggered by legal refresh, the system SHALL query the Traffic Fine Lookup Portal using license plate; IF violations exist, return fine details; IF none, return clear status. | UC-1.09; CTX: flow-19, flow-20; SEQ: S-4.04, S-4.05 | Should | EQ |
| F-01 | FR-01-12 | Check Mortgage Status | WHEN triggered by legal refresh, the system SHALL query Secured Transaction Registry Portal using VIN; IF active mortgage exists, attach warning details; IF released, return unencumbered status. | UC-1.10; CTX: flow-21, flow-22; SEQ: S-4.06, S-4.07 | Must | EQ |
| F-01 | FR-01-13 | Display updated comprehensive legal status | WHEN all parallel legal verification queries return, the system SHALL aggregate results and render the consolidated legal status; IF any query fails, the system SHALL display partial results with retry prompt. | UC-1.07; SEQ: S-4.08, S-4.09 | Must | EO |
| F-02 | FR-02-01 | Calculate weekly market price | WHEN the weekly timer fires, the system SHALL, for every tracked car, check that the verified minimum mileage divided by max(car age in years, 1) reaches 2,000 km per year, then calculate and store a market price and a confidence level. IF the mileage is below this threshold, the system SHALL record INSUFFICIENT_DATA for that car and skip its price calculation. IF the calculation fails for a car, the system SHALL keep its previous weekly price, log the failure and continue with the remaining cars. | UC-2.03, 2.04; SEQ: S-6.01, S-6.16, S-6.17, S-6.18 | Must | EO |
| F-02 | FR-02-02 | Sync listings from partner marketplaces | WHEN the weekly sync job executes, the system SHALL fetch active vehicle listings from Nextgen VN and Chợ Tốt APIs and persist them with source ID and sync timestamp; IF an external partner API times out after 3 retries (6 hours apart), the system SHALL proceed with the responsive partner, write an ERROR log entry with HTTP status, and flag partial sync status. IF both of marketplace partner failed, the system SHALL return API_failed. | UC-2.05; CTX: flow-15, flow-16; SEQ: S-6.02, S-6.03, S-6.04, S-6.09, S-6.10, S-6.05, S-6.06, S-6.07, S-6.08 | Must | EI |
| F-02 | FR-02-03 | Select comparable vehicles | WHEN selecting valuation datasets, the system SHALL filter listings matching exact brand, model, manufacturing year (within ±1 year), and mileage (within 20% km comparable to the minimum mileage of car); IF zero matching listings satisfy the criteria, the system SHALL widen the manufacturing year and mileage difference according to the widening rule and IF zero matching listings satisfy this table, the system SHALL display "Insufficient data". | UC-2.08; SEQ: S-6.11 → S-6.13 | Must | NC |
| F-02 | FR-02-04 | Normalize and clean listings | WHEN new listings are stored, the system SHALL normalize their fields and remove invalid listings before they are used for pricing. IF a listing has a missing or invalid mandatory field, the system SHALL exclude it from pricing. | UC-2.07; SEQ: S-6.12 → S-6.15 | Must | NC |
| F-02 | FR-02-05 | Calculate the confidence | WHEN the median and P25-P75 range have been calculated for a vehicle, the system SHALL assign a confidence level of High, Medium or Low based on the size of the comparable sample and the comparison level (Level 0, 1 or 2) used to reach it, and save it in the weekly price record; IF fewer than 30 comparable listings remain after Level 2, the system SHALL NOT assign a confidence level and SHALL record INSUFFICIENT_DATA. | UC-2.08; SEQ: S-6.20 → S-6.24 | Should | NC |
| F-02 | FR-02-06 | Calculate market price | WHEN at least 30 comparable listings have been selected for a vehicle after price outliers are removed, the system SHALL calculate the median price and the P25-P75 range, round them to the nearest million VND, and save them as the weekly price record; IF fewer than 30 comparable listings remain after Level 2, the system SHALL record INSUFFICIENT_DATA and SHALL NOT calculate a price. | UC-2.09; SEQ: S-6.25 → S-6.27 | Must | NC |
| F-02 | FR-02-07 | Sync 180-day listing history (first-time signed-up car) | WHEN a car model is signed up for the first time, the system SHALL retrieve 180 days of listing history for that model from both partners. IF a partner returns no history, the system SHALL continue with the other partner and record the gap. | UC-2.10, 2.06; CTX: flow-8, flow-15, flow-16; SEQ: S-7.01 → S-7.03, S-7.06 → S-7.10 | Could | EI |
| F-02 | FR-02-08 | Backfill 12-week price history | WHEN the 180-day listing history of a newly signed-up car model has been retrieved, the system SHALL retrieve the car’s verified minimum mileage for each of the previous 12 weeks from VETC and calculate the weekly market price using comparable listings at that mileage; IF VETC has no mileage record for a week, the system SHALL leave that week without a price. | UC-2.03; CTX: flow-12; SEQ: S-7.11 → S-7.13 | Could | EO |
| F-02 | FR-02-09 | Backfill 12-week price history | WHEN the 180-day listing history of a newly signed-up car model has been retrieved, the system SHALL calculate the weekly market price for each of the previous 12 weeks and store them as historical price records. IF a week has no listings in the history, the system SHALL leave that week without a price. | UC-2.07, 2.08, 2.09; CTX: flow-08; SEQ: S-7.14 → S.7.21 | Could | EO |
| F-02 | FR-02-10 | View market price | WHEN a car owner opens the market price view for a tracked car, the system SHALL display the latest weekly market price and its confidence level. IF no weekly price exists for the car yet, the system SHALL display a message "Calculating" | UC-2.01; CTX: flow-08; SEQ: S-5.01 → S-5.04, S-5.05 → S-5.15 | Must | EO |
| F-02 | FR-02-11 | View 12-week price trend chart | WHEN the Car Owner opens the market price view, the system SHALL render a chart with one price point per week for the 12 most recent weeks. IF fewer than 12 weekly prices exist, the system SHALL display "No historical data available" | UC-2.02; CTX: flow-08; SEQ: S-5.16 → S-5.18, S-5.18 → S-5.22 | Should | EO |
| F-02 | FR-02-12 | Stop market price tracking | WHEN a car owner stops tracking a car, the system SHALL exclude that car from all further weekly calculations. IF the car is not currently tracked, the system SHALL make no change. | UC-2.11; SEQ: S-8.01, S-8.02 → S-8.05 | Could | EI |
| F-03 | FR-03-01 | Submit sale request | WHEN a car owner submits a sale request for a verified report, the system SHALL create the sale request and trigger the legal status re-check and the packaging for Carpla staff. IF the report has not been initialized for the VIN, the system SHALL reject the request and display an error. | UC-3.01; CTX: flow-01; SEQ: S-9.01, S-9.02 | Must | EI |
| F-03 | FR-03-02 | Re-check legal status before forwarding | WHEN a sale request is submitted, the system SHALL query the Vehicle Inspection Portal, the Traffic Fine Lookup Portal and the Secured Transaction Registry Portal, and attach the results to the request. IF a portal has no record for the vehicle or does not respond (timeout or error), the system SHALL attach that check as “No data” and still forward the request. | UC-3.02 (incl. 3.03, 3.04, 3.05); CTX: flow-19, flow-20, flow-21, flow-22, flow-23, flow-24; SEQ: S-9.03 → S-9.10 | Should | NC |
| F-03 | FR-03-03 | Package and forward report and market price to Carpla staff | WHEN a sale request is submitted, the system SHALL package the verified report and the latest market price and make the package available to Carpla staff. IF no market price exists for the car, the system SHALL forward the package with the price marked as unavailable. | UC-3.06; CTX: flow-09, flow-10; SEQ: S-9.11 → S-9.13 | Must | EO |
| F-03 | FR-03-04 | Review verified report | WHEN Carpla staff open a sale request, the system SHALL display the packaged verified report and market price. IF the package is incomplete, the system SHALL display which parts are missing. | UC-3.07; CTX: flow-09, flow-10; SEQ: S-10.01, S-10.02 | Must | EQ |
| F-03 | FR-03-05 | Send preliminary quote to car owner | WHEN Carpla staff submit a quote for a sale request, the system SHALL store it and make it visible to the car owner. IF the quote amount is missing or invalid, the system SHALL reject the submission and display an error. | UC-3.08, 3.09; CTX: flow-11, flow-07; SEQ: S-10.04, S-10.05, S-10.06 | Should | EI |
| F-03 | FR-03-06 | View preliminary quote | WHEN a car owner opens a sale request, the system SHALL display the preliminary quote once it has been sent. IF no quote has been sent yet, the system SHALL display that the request is waiting for Carpla. | UC-3.09; CTX: flow-07; SEQ: S-10.07, S-10.08 | Should | EQ |

## L3 work items

| L3 ID | L2 ID | Layer | L3 work item | Size | Base hours | EARS precise? | Hidden work? |
|---|---|---|---|---|---|---|---|
| FR-01-01-FE | FR-01-01 | FE | Display VIN input form with client-side format validation and feedback | S | 4 | Yes | No |
| FR-01-01-BE | FR-01-01 | BE | Validate VIN checksum logic and initialize digital vehicle record in database | M | 8 | Yes | No |
| FR-01-02-FE | FR-01-02 | FE | Display vehicle identification card on the report screen | S | 4 | Yes | No |
| FR-01-02-INT | FR-01-02 | INT | Implement internal API to query core vehicle identity data from VETC | S | 4 | Yes | No |
| FR-01-03-FE | FR-01-03 | FE | Display verified minimum ODO badge | S | 4 | Yes | No |
| FR-01-03-INT | FR-01-03 | INT | Implement API connector to query passage logs from VETC and compute minimum ODO | M | 8 | Yes | No |
| FR-01-04-FE | FR-01-04 | FE | Display interactive maintenance history timeline interface | S | 4 | Yes | No |
| FR-01-04-BE | FR-01-04 | BE | API: Retrieve data of maintenance and repair history from Carpla Service (Carpla Service belongs to Carpla) | S | 4 | Yes | No |
| FR-01-05-FE | FR-01-05 | FE | Display file uploader supporting PDF, XML, images with progress indicator | S | 4 | No | No |
| FR-01-05-BE | FR-01-05 | BE | API: Store file uploaded | S | 4 | No | Yes |
| FR-01-06-BE | FR-01-06 | BE | Build OCR processing pipeline and XML parsing worker for invoice metadata | M | 8 | Yes | Yes |
| FR-01-07-INT | FR-01-07 | INT | API: connect to National Electronic Invoice Portal with alt error handling | M | 8 | Yes | No |
| FR-01-07-BE | FR-01-07 | BE | Merge validated external invoice into vehicle history or return rejection response | S | 4 | Yes | No |
| FR-01-08-FE | FR-01-08 | FE | Display battery state of health (SoH) | S | 4 | No | No |
| FR-01-08-INT | FR-01-08 | INT | API: integrate with charging station partners to fetch charging session data from Tasco stations | M | 8 | No | No |
| FR-01-08-BE | FR-01-08 | BE | Calculate using degradation algorithm to compare baseline vs current charging duration | M | 8 | No | No |
| FR-01-09-FE | FR-01-09 | FE | Display legal status section header with refresh buttons and loading state | S | 4 | Yes | No |
| FR-01-10-INT | FR-01-10 | INT | API: Look up data in National Vehicle Inspection Portal | M | 8 | No | No |
| FR-01-11-INT | FR-01-11 | INT | API: look up data in Traffic Police Fine Lookup Portal | M | 8 | Yes | No |
| FR-01-12-INT | FR-01-12 | INT | API: look up data in National Secured Transaction Registry Portal | M | 8 | Yes | No |
| FR-01-13-BE | FR-01-13 | BE | Store legal report aggregator combining inspection, fines, and mortgage payloads for the next displaying | S | 4 | Yes | Yes |
| FR-01-13-FE | FR-01-13 | FE | Display consolidated legal status card rendering inspection badges, fines, and mortgage tags | S | 4 | Yes | No |
| FR-02-01-BE | FR-02-01 | BE | Check mileage plausibility (verified minimum mileage / max(age, 1) vs 2,000 km/year) and route implausible cars to INSUFFICIENT_DATA and scheduled weekly job with failure alert and re-run | S | 4 | Yes | Yes |
| FR-02-02-INT1 | FR-02-02 | INT | Integrate Nextgen VN listings | L | 16 | Yes | No |
| FR-02-02-INT2 | FR-02-02 | INT | Integrate Chợ Tốt listings | L | 16 | Yes | No |
| FR-02-02-BE2 | FR-02-02 | BE | Store credentials, rate limit and retry for partner APIs | M | 8 | No | Yes |
| FR-02-05-BE | FR-02-03 | BE | Filter for vehicles of the same brand, model, mileage and year to manufacture and widen the mileage and year to manufacture according to widening rule if there is not enough data | M | 8 | Yes | No |
| FR-02-03-BE | FR-02-04 | BE | Normalize fields and remove invalid listings | L | 16 | No | Yes |
| FR-02-04-BE | FR-02-05 | BE | Assign confidence level (High / Medium / Low) from comparable sample size and comparison level, and save it with the weekly price record | S | 4 | Yes | No |
| FR-02-06-BE | FR-02-06 | BE | Calculate median and P25-P75 range (rounded to nearest million VND) from comparable listings and save the weekly price record | L | 16 | Yes | No |
| FR-02-07-BE | FR-02-07 | BE | Detect first sign-up | S | 4 | Yes | No |
| FR-02-07-INT | FR-02-07 | INT | Fetch 180-day listing history from Nextgen VN and Chợ Tốt | M | 8 | Yes | No |
| FR-02-08-INT | FR-02-08 | INT | Read historical VETC readings for the backfill | M | 8 | Yes | No |
| FR-02-08-BE | FR-02-09 | BE | Rebuild 12 weekly prices from listing history | M | 8 | Yes | No |
| FR-02-09-FE | FR-02-10 | FE | Display market price with confidence level | S | 4 | Yes | No |
| FR-02-09-BE | FR-02-10 | BE | API: read latest weekly price and confidence | S | 4 | Yes | No |
| FR-02-10-FE | FR-02-11 | FE | Display 12-week trend chart | M | 8 | Yes | No |
| FR-02-10-BE | FR-02-11 | BE | API: read prices in the latest 12-week | S | 4 | Yes | No |
| FR-02-11-FE | FR-02-12 | FE | Display stop tracking screen | S | 4 | Yes | No |
| FR-02-11-BE | FR-02-12 | BE | API: deactivate tracking | S | 4 | Yes | No |
| FR-03-01-FE | FR-03-01 | FE | Display submit sale request button; form for car owner to fill mileage, desired price; and sending processing notification | M | 8 | Yes | No |
| FR-03-02-BE | FR-03-02 | BE | Refresh the legal status of the vehicle | S | 4 | Yes | No |
| FR-03-03-BE | FR-03-03 | BE | Assemble the package and notify Carpla staff | M | 8 | Yes | No |
| FR-03-04-FE | FR-03-04 | FE | Display screen receive sale request and verified report, market price | S | 4 | Yes | No |
| FR-03-04-BE | FR-03-04 | BE | API: read the package for staff including mileage, phone number, desired price, verified report, market valuation data | S | 4 | Yes | No |
| FR-03-05-FE | FR-03-05 | FE | Enter and send preliminary quote | S | 4 | Yes | No |
| FR-03-05-BE | FR-03-05 | BE | API: store quote, notify car owner | M | 8 | Yes | No |
| FR-03-06-FE | FR-03-06 | FE | Display preliminary quote and button book an inspection | S | 4 | Yes | No |
| FR-03-06-BE | FR-03-06 | BE | API: read the quote and book an inspection | M | 8 | Yes | No |

31 L2 functions, 49 L3 work items, 320 base hours. No row is above the 16-hour ceiling.

NC (not countable) L2 rows: FR-01-06, FR-02-03, FR-02-04, FR-02-05, FR-02-06, FR-03-02. They score zero function points but carry real work, which is why the function-point count is only a cross-check.

# W0 Scope:
L2 = a business behaviour the client signs off, with its EARS criterion and function-point type. L3 = one work item in one layer (FE = front end, BE = back end, INT = integration), sized S = 4 h, M = 8 h, L = 16 h. Initial priority is set here.

## Table 1. Use case inventory

| Use case ID | Use case name | Primary actor | Assigned feature |
|---|---|---|---|
| UC-1.01 | Enter VIN to initialize Report | Car Owner | F-01 |
| UC-1.02 | View Vehicle Identification | Car Owner | F-01 |
| UC-1.03 | View Verified Minimum Mileage | Car Owner | F-01 |
| UC-1.04 | View Maintenance & Repair History in Carpla Service | Car Owner | F-01 |
| UC-1.05 | Upload External Maintenance & Repair Invoices | Car Owner | F-01 |
| UC-1.06 | View Battery State of Health (SoH) | Car Owner | F-01 |
| UC-1.07 | Refresh Legal Status | Car Owner | F-01 |
| UC-1.08 | View Inspection Status | Car Owner | F-01 |
| UC-1.09 | View Unpaid Traffic Fine Status | Car Owner | F-01 |
| UC-1.10 | View Mortgage Status | Car Owner | F-01 |
| UC-2.01 | View Market Price | Car Owner | F-02 |
| UC-2.02 | View 12-Week Price Trend Chart | Car Owner | F-02 |
| UC-2.03 | Check Verified Mileage Plausibility | Weekly timer | F-02 |
| UC-2.04 | Calculate Weekly Market Price | Weekly timer | F-02 |
| UC-2.05 | Sync Listings from Partner Marketplaces | Weekly timer | F-02 |
| UC-2.06 | Sync 180-Day Listing History | Weekly timer | F-02 |
| UC-2.07 | Normalize and Clean Listings | Weekly timer | F-02 |
| UC-2.08 | Select Comparable Vehicles | Weekly timer | F-02 |
| UC-2.09 | Calculate Market Price and Confidence | Weekly timer | F-02 |
| UC-2.10 | Backfill 12-Week Price History | Car Owner | F-02 |
| UC-2.11 | Stop Market Price Tracking | Car Owner | F-02 |
| UC-3.01 | Submit Sale Request | Car Owner | F-03 |
| UC-3.02 | Re-check Legal Status | Car Owner | F-03 |
| UC-3.03 | Check Inspection Status | Car Owner | F-03 |
| UC-3.04 | Check Unpaid Traffic Fine Status | Car Owner | F-03 |
| UC-3.05 | Check Mortgage Status | Car Owner | F-03 |
| UC-3.06 | Package & Forward Verified Report and Market Price to Carpla Staff | Car Owner | F-03 |
| UC-3.07 | Review Verified Report | Carpla Staff | F-03 |
| UC-3.08 | Send Preliminary Quote to Car Owner | Carpla Staff | F-03 |
| UC-3.09 | View Preliminary Quote | Car Owner | F-03 |

Use cases listed: 30. All use cases are assigned to a feature.

## Table 2. Feature list (flows into W1)

| Feature ID | Feature name (business noun) | Scope in one sentence | Primary actors served | Owner | Use cases covered |
|---|---|---|---|---|---|
| F-01 | Vehicle Passport | A car owner creates a vehicle profile from its VIN and builds a verified record of identity, repair invoices and legal status | Car Owner | Owner A | UC-1.01 to UC-1.10 |
| F-02 | Market Price | Every week the system collects partner-marketplace listings and shows the car owner a market price with a 12-week price chart | Car Owner, Weekly Timer | Owner B | UC-2.01 to UC-2.11 |
| F-03 | Sell Request & Preliminary Quote | The owner asks to sell; the system re-checks legal status and sends the verified report including vehicle identification, status and market price to Carpla staff, who return a preliminary quote | Car Owner, Carpla Staff | Owner C | UC-3.01 to UC-3.09 |

Feature count: 3 (allowed 3 to 6): PASS.

## Table 3. Won't-have (recorded, never dropped silently)

| Excluded from scope | How it surfaced | Reason for exclusion |
|---|---|---|
| Sale Alert when there is good notifications from market based on market price | Not on the diagrams | This feature did not match with insight of car owner. Car owner only want to sell when they have available need. |
| Maintainance Alert | Not on the diagrams | This feature did not create much profit for Carpla.  Defining the exact number of km car has driven is too difficult. If car owner has to enter it manually regularly, it creates more trouble for car owner rather than value. |

## Table 4. Data functions

| Data group | ILF / EIF | Why | Function points |
|---|---|---|---|
| Verified vehicle report | ILF | The system directly creates, stores, and updates verified vehicle digital records and passport status linked to registered car owners in its internal database. | 10 |
| Uploaded maintenance invoice | ILF | The system stores and manages external invoice files, extracted line items, and confirmed repair timeline entries uploaded by car owners. | 10 |
| Market listing | EIF | Read only reference dataset crawled or synced from partner car sales marketplaces, utilized strictly by valuation algorithms without TASCO Mobility maintaining original postings. | 7 |
| Weekly market price | ILF | The system calculates, records, and maintains weekly market price snapshots and 12 week historical price tracking curves within its own database. | 10 |
| Sale request and preliminary quote | ILF | The system stores transaction requests initiated by owners, Carpla staff evaluation notes, and issued preliminary quotation records. | 10 |
| VETC vehicle data | EIF | Read only reference data queried via API from the VETC system to verify license plates, engine numbers, and minimum passage odometer readings. | 7 |
| Carpla Service records | EIF | Read only official maintenance and repair service history retrieved directly via API from the external Carpla Service CRM system. | 7 |
| Electronic invoice portal data | EIF | Read only authentication records queried from the General Department of Taxation electronic invoice portal to verify invoice legitimacy. | 7 |
| EV charging station data | EIF | Read only charging telemetry and session logs retrieved from Tasco electric vehicle charging infrastructure to calculate battery state of health. | 7 |
| Vehicle inspection data | EIF | Read only technical compliance and certification dates queried on demand from the National Vehicle Inspection Portal. | 7 |
| Traffic fine data | EIF | Read only traffic violation records and penalty status queried directly from the Traffic Police Department portal. | 7 |
| Secured transaction registry data | EIF | Read only legal encumbrance and bank mortgage status queried on demand from the National Secured Transaction Registry Portal. | 7 |
| Partner marketplace listings | EIF | Read only external listing feeds synced periodically from designated partner car portals for market pricing comparison. | 7 |
| **Total data-function points** | | | **103** |

ILF = Internal Logical File (data the system maintains itself). EIF = External Interface File (data the system only reads from another system).

|**Parameters — Fixed for the whole class**|||||||||||
| - | - | - | - | - | - | - | - | - | - | :- |
||||||||||||
|**INPUT**|Labour-market data, productivity assumptions and the team capacity, fixed for the whole cohort.||||||||||
|**OUTPUT**|Every derived number in W1, W2, W4 and W5 traces back to a cell on this sheet.||||||||||
|**WHY FIXED**|If each team invented its own rates the exercise would test guessing rather than the model, and results would not be comparable at marking. The point of a published parameter is that the subjectivity sits in ONE auditable place instead of forty scattered ones.||||||||||
||||||||||||
|**1. Effort size to base hours (applied at level 3)**|||||||||||
|**Size**|**Hours**||**Note**||||||||
|S|4||*Ceiling for one L3 row is 16 h — above that it is not yet an L3 row*||||||||
|M|8||*Matches the test: ask a developer; an answer of 4–16 h is correct granularity*||||||||
|L|16||||||||||
||||||||||||
|**2. Technical layer to AI leverage and default seniority**|||||||||||
|**Layer**|**AI leverage factor**|**Default band**|**Note**||||||||
|FE|0\.35|Junior|*Screens, forms, repeated patterns — AI generates these well*||||||||
|BE|0\.7|Senior|*Business logic, state, concurrency*||||||||
|INT|1|Senior|*Third-party APIs, credentials, sandboxes — AI helps least here*||||||||
||||||||||||
|**3. Hourly cost by seniority band (VND / hour)**|||||||||||
|**Band**|**Rate**||**Source**||||||||
|Junior|200,000 VND||*ITviec 2025-26 median 25.35m VND/month x 1.35 overhead / 176 h*||||||||
|Senior|300,000 VND||*ITviec 2025-26 median 39.9m VND/month x 1.35 / 176 h*||||||||
|Expert|400,000 VND||*ITviec 2025-26 median 54.9m VND/month x 1.35 / 176 h*||||||||
||||||||||||
|**4. Verification factor (driven by acceptance-criterion quality)**|||||||||||
|**EARS precise?**|**Factor**||**Note**||||||||
|Yes|0\.15||*Clear criteria — fast to verify, often automatable*||||||||
|No|0\.4||*Vague criteria — a person must reason the answer out from scratch*||||||||
||||||||||||
|**5. Function point weights (IFPUG average complexity)**|||||||||||
|**FP type**|**Weight**||**What it counts**||||||||
|EI|4||*External Input — data crosses the boundary inward and changes stored data*||||||||
|EO|5||*External Output — data leaves the boundary WITH derived values or calculation*||||||||
|EQ|4||*External inQuiry — retrieval only, no derived data, nothing stored changes*||||||||
|ILF|10||*Internal Logical File — a group of data the system itself maintains*||||||||
|EIF|7||*External Interface File — data the system reads but never maintains*||||||||
|NC|0||*Not countable — a business rule or non-functional item inside another transaction*||||||||
|Hours per unadjusted function point|3||*Class parameter, PRE-AI gross hours. Published industry figures range very widely; calibrate this from your own delivered projects rather than importing a benchmark*||||||||
||||||||||||
||||||||||||


|**6. Team capacity for DBIZ3**|||||||||||
| - | - | - | - | - | - | - | - | - | - | - |
|Team members|5||||||||||
|Weeks|12||||||||||
|Productive hours per person per week|5||*Hours actually spent building, not hours present*||||||||
|**Total capacity (person-hours)**|300||*Hard ceiling for the whole scope: Must + Should + Could*||||||||
||||||||||||
|**7. Thresholds**|||||||||||
|Must, maximum share of scope effort|60%||*DSDM: above 60% the project enters the risk zone*||||||||
|Could, target share of scope effort|20%||*The contingency pool, expressed as scope rather than money*||||||||
|Hidden technical work, minimum share|10%||*Real projects run 20–30%; below 10% something is missing*||||||||
|Maximum hours for one L3 row|16||||||||||
|Minimum number of features|3||||||||||
|Maximum number of features|6||||||||||
|Estimate variance that triggers a review|30%||*If bottom-up and function-point estimates differ by more than this, something is mis-sized*||||||||
||||||||||||
|**8. Formulas**|||||||||||
|**Person-hours  =  base hours x AI leverage factor  +  base hours x verification factor**|||||||||||
|**Cost  =  person-hours x hourly rate for the seniority band**|||||||||||
|**Function-point cross-check  =  unadjusted function points x hours per FP  (compare against total base hours)**|||||||||||
|*Note: once a ~20% Could pool exists as scope contingency, do NOT add a further 20% money contingency on top — that counts the same buffer twice.*|||||||||||
||||||||||||
|**9. Estimation basis — which method drives the hours**|||||||||||
|**Estimation basis**|Bottom-up||*Switch this and the whole workbook recalculates on the other method.*||||||||
|<p>*Bottom-up: base hours come from the S / M / L label on each L3 row.*</p><p>*Function-point: the L2 total comes from its function-point count x hours per FP, then S / M / L acts only as the RELATIVE weight that distributes those hours across the L3 rows. NC rows have no function points, so they always fall back to bottom-up — that is the 0 function point problem, and it is why the two methods can never fully replace each other.*</p>|||||||||||
|<p></p><p></p>|||||||||||
|<p></p><p></p>|||||||||||

|**W0 — Establish scope and split into features**||
| - | :- |
|||
|**INPUT**||
|**OUTPUT**||
|**PROCEDURE**||
|**RULE**||
|||
|**Table 1 · Use case inventory — list every use case before clustering**||
|**Use case ID**||
|UC-1.01||
|UC-1.02||
|UC-1.03||
|UC-1.04||
|UC-1.05||
|UC-1.06||
|UC-1.07||
|UC-1.08||
|UC-1.09||
|UC-1.10||
|UC-2.01||
|UC-2.02||
|UC-2.03||
|UC-2.04||
|UC-2.05||
|UC-2.06||
|UC-2.07||
|UC-2.08||
|UC-2.09||
|UC-2.10||
|UC-2.11||
|UC-3.01||
|UC-3.02||
|UC-3.03||
|UC-3.04||
|UC-3.05||
|UC-3.06||
|UC-3.07||
|UC-3.08||
|UC-3.09||
|Use cases listed||
|||


|**Table 2 · Feature list — this is what flows into W1**||
| - | :- |
|**Feature ID**||
|F-01||
|F-02||
|F-03||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|Feature count||
|||
|**Table 3 · Won't-have — record it here, never drop it silently**||
|**Excluded from scope**||
|Sale Alert when there is good notifications from market based on market price||
|Maintainance Alert||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|**Table 4 · Data functions — what the system maintains (ILF) or only reads (EIF)**||
|**Data group**||
|Verified vehicle report||
|Uploaded maintenance invoice||
|Market listing||
|Weekly market price||
|Sale request and preliminary quote||
|VETC vehicle data||
|Carpla Service records||



|Electronic invoice portal data|
| - |
|EV charging station data|
|Vehicle inspection data|
|Traffic fine data|
|Secured transaction registry data|
|Partner marketplace listings|
||
||


|||
| - | :- |
|||
|The Use Case Diagram, the CVCA and the Context Diagram, updated after the user meeting. Every diagram element must already carry an ID.||
|(a) a complete use-case inventory, (b) a small set of features, each a folder under specs/, (c) an explicit Won't-have list, (d) the data functions the system maintains or reads. Feature IDs feed column A of W1.||
|<p>1\. List EVERY use case in Table 1 — names only, no discussion yet.</p><p>2\. Cluster them into features in Table 2. A feature name is a business noun the client recognises, never a screen name or a technical module.</p><p>3\. Coverage check against the CVCA: every primary actor must be served by at least one feature.</p><p>4\. Record anything excluded in Table 3, with the reason.</p><p>5\. In Table 4, list the data the system MAINTAINS (ILF) and the data it only READS from another system (EIF). Read these off the Context Diagram boundary.</p>||
|Merge any feature expected to hold fewer than 3 L2 rows. A feature that no use case and no CVCA flow points at does not belong in the release.||
|||
|||
|**Use case name**||
|Enter VIN to initialize Report||
|View Vehicle Identification||
|View Verified Minimum Mileage||
|View Maintenance & Repair History in Carpla Service||
|Upload External Maintenance & Repair Invoices||
|View Battery State of Health (SoH)||
|Refresh Legal Status||
|View Inspection Status||
|View Unpaid Traffic Fine Status||
|View Mortgage Status||
|View Market Price||
|View 12-Week Price Trend Chart||
|Check Verified Mileage Plausibility||
|Calculate Weekly Market Price||
|Sync Listings from Partner Marketplaces||
|Sync 180-Day Listing History||
|Normalize and Clean Listings||
|Select Comparable Vehicles||
|Calculate Market Price and Confidence||
|Backfill 12-Week Price History||
|Stop Market Price Tracking||
|Submit Sale Request||
|Re-check Legal Status||
|Check Inspection Status||
|Check Unpaid Traffic Fine Status||
|Check Mortgage Status||
|Package & Forward Verified Report and Market Price to Carpla Staff||
|Review Verified Report||
|Send Preliminary Quote to Car Owner||
|View Preliminary Quote||
|30||
|||


||
| - |
|**Feature name (business noun)**|
|Vehicle Passport|
|Market Price|
|Sell Request & Preliminary Quote|
||
||
||
||
||
||
||
||
||
|3|
||
||
|**How it surfaced**|
|Not on the diagrams|
|Not on the diagrams|
||
||
||
||
||
||
||
||
||
||
|**ILF / EIF**|
|ILF|
|ILF|
|EIF|
|ILF|
|ILF|
|EIF|
|EIF|



|EIF|
| - |
|EIF|
|EIF|
|EIF|
|EIF|
|EIF|
||
||


||||
| - | - | :- |
||||
||||
||||
||||
||||
||||
||||
|**Primary actor**|**Assigned feature**||
|Car Owner|F-01||
|Car Owner|F-01||
|Car Owner|F-01||
|Car Owner|F-01||
|Car Owner|F-01||
|Car Owner|F-01||
|Car Owner|F-01||
|Car Owner|F-01||
|Car Owner|F-01||
|Car Owner|F-01||
|Car Owner|F-02||
|Car Owner|F-02||
|Weekly timer|F-02||
|Weekly timer|F-02||
|Weekly timer|F-02||
|Weekly timer|F-02||
|Weekly timer|F-02||
|Weekly timer|F-02||
|Weekly timer|F-02||
|Car Owner|F-02||
|Car Owner|F-02||
|Car Owner|F-03||
|Car Owner|F-03||
|Car Owner|F-03||
|Car Owner|F-03||
|Car Owner|F-03||
|Car Owner|F-03||
|Carpla Staff|F-03||
|Carpla Staff|F-03||
|Car Owner|F-03||
|All assigned|||
||||


|||
| - | - |
|**Scope in one sentence**|**Primary actors served**|
|A car owner creates a vehicle profile from its VIN and builds a verified record of identity, repair invoices and legal status|Car Owner|
|Every week the system collects partner-marketplace listings and shows the car owner a market price with a 12-week price chart|Car Owner, Weekly Timer|
|The owner asks to sell; the system re-checks legal status and sends the verified report including vehicle identification, status and market price to Carpla staff, who return a preliminary quote|Car Owner, Carpla Staff|
|||
|||
|||
|||
|||
|||
|||
|||
|||
|PASS||
|||
|||
|**Reason for exclusion**||
|<p>This feature did not match with insight of car owner.</p><p>Car owner only want to sell when they have available need.</p>||
|<p>This feature did not create much profit for Carpla.</p><p>Defining the exact number of km car has driven is too difficult. If car owner has to enter it manually regularly, it creates more trouble for car owner rather than value.</p>||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|**Why**|**Function points**|
|The system directly creates, stores, and updates verified vehicle digital records and passport status linked to registered car owners in its internal database.|10|
|The system stores and manages external invoice files, extracted line items, and confirmed repair timeline entries uploaded by car owners.|10|
|Read only reference dataset crawled or synced from partner car sales marketplaces, utilized strictly by valuation algorithms without TASCO Mobility maintaining original postings.|7|
|The system calculates, records, and maintains weekly market price snapshots and 12 week historical price tracking curves within its own database.|10|
|The system stores transaction requests initiated by owners, Carpla staff evaluation notes, and issued preliminary quotation records.|10|
|Read only reference data queried via API from the VETC system to verify license plates, engine numbers, and minimum passage odometer readings.|7|
|Read only official maintenance and repair service history retrieved directly via API from the external Carpla Service CRM system.|7|



|Read only authentication records queried from the General Department of Taxation electronic invoice portal to verify invoice legitimacy.|7|
| - | -: |
|Read only charging telemetry and session logs retrieved from Tasco electric vehicle charging infrastructure to calculate battery state of health.|7|
|Read only technical compliance and certification dates queried on demand from the National Vehicle Inspection Portal.|7|
|Read only traffic violation records and penalty status queried directly from the Traffic Police Department portal.|7|
|Read only legal encumbrance and bank mortgage status queried on demand from the National Secured Transaction Registry Portal.|7|
|Read only external listing feeds synced periodically from designated partner car portals for market pricing comparison.|7|
|||
|**Total data-function points**|**103**|



|||
| - | - |
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||


|||
| - | - |
|**Owner**|**Use cases covered**|
|Owner A|UC-1.01 UC-1.02 UC-1.03 UC-1.04 UC-1.05 UC-1.06 UC-1.07 UC-1.08 UC-1.09 UC-1.10|
|Owner B|UC-2.01 UC-2.02 UC-2.03 UC-2.04 UC-2.05 UC-2.06 UC-2.07 UC-2.08 UC-2.09 UC-2.10 UC-2.11|
|Owner C|UC-3.01 UC-3.02 UC-3.03 UC-3.04 UC-3.05 UC-3.06 UC-3.07 UC-3.08 UC-3.09|
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||

|**W1 — Decompose features into the function list**||||
| - | :- | :- | :- |
|||||
|**INPUT**|The feature list from W0 and the five design diagrams: Use Case, Sequence, Context (DFD L0), CVCA, Architecture + ADR.|||
|**OUTPUT**|Every function at two levels. L2 = a business behaviour the client signs off, carrying the EARS criterion and the function-point type. L3 = one work item in one technical layer. No money on this sheet.|||
|**PROCEDURE**|<p>1\. Sweep the five diagrams, ONE DIAGRAM PER PASS. Record every candidate with the diagram element it came from.</p><p>2\. Merge duplicates — keep one row, accumulate the sources in column E.</p><p>3\. Write one EARS sentence per L2 row.</p><p>4\. Classify each L2 row by function-point type: EI, EO, EQ, or NC when it is a rule or a non-functional item living inside another transaction.</p><p>5\. Split each L2 into L3 rows by layer (FE / BE / INT) and give each a size.</p><p>6\. Set an initial MoSCoW priority by instinct. W4 is where it is tightened.</p>|||
|**WATCH FOR**|Many valuable rows are NC — 'prevent double booking', 'enforce authorisation', 'deploy automatically'. They score zero function points and are often the most expensive rows in the sheet. That gap is the reason the function-point count is a CROSS-CHECK, never the estimate itself.|||
|**ADD / REMOVE**|Type into the next blank row to add a function; clear a row's contents to remove it. Every other sheet follows automatically.|||
|||||
|**Feature**|**L2 ID**|**L2 function name**||
|F-01|FR-01-01|Enter VIN to initialize Report||
|F-01|FR-01-01|Enter VIN to initialize Report||
|F-01|FR-01-02|View Vehicle Identification||
|F-01|FR-01-02|View Vehicle Identification||
|F-01|FR-01-03|View Verified Minimum Mileage||
|F-01|FR-01-03|View Verified Minimum Mileage||
|F-01|FR-01-04|View Maintenance and Repair History in Carpla Service||
|F-01|FR-01-04|View Maintenance and Repair History in Carpla Service||
|F-01|FR-01-05|Upload External Maintenance and Repair Invoices||
|F-01|FR-01-05|Upload External Maintenance and Repair Invoices||
|F-01|FR-01-06|Extract invoice data via OCR or parsing||
|F-01|FR-01-07|Cross check invoice authenticity with Electronic Invoice Portal||
|F-01|FR-01-07|Cross check invoice authenticity with Electronic Invoice Portal||
|F-01|FR-01-08|View Battery State of Health||
|F-01|FR-01-08|View Battery State of Health||
|F-01|FR-01-08|View Battery State of Health||
|F-01|FR-01-09|Trigger Refresh Legal Status||
|F-01|FR-01-10|Check Inspection Status||
|F-01|FR-01-11|Check Unpaid Traffic Fine Status||
|F-01|FR-01-12|Check Mortgage Status||
|F-01|FR-01-13|Display updated comprehensive legal status||
|F-01|FR-01-13|Display updated comprehensive legal status||
|F-02|FR-02-01|Calculate weekly market price||
|F-02|FR-02-02|Sync listings from partner marketplaces||
|F-02|FR-02-02|Sync listings from partner marketplaces||
|F-02|FR-02-02|Sync listings from partner marketplaces||



|F-02|FR-02-03|Select comparable vehicles|
| - | - | - |
|F-02|FR-02-04|Normalize and clean listings|
|F-02|FR-02-05|Calculate the confidence|
|F-02|FR-02-06|Calculate market price|
|F-02|FR-02-07|Sync 180-day listing history (first-time signed-up car)|
|F-02|FR-02-07|Sync 180-day listing history (first-time signed-up car)|
|F-02|FR-02-08|Backfill 12-week price history|
|F-02|FR-02-09|Backfill 12-week price history|
|F-02|FR-02-10|View market price|
|F-02|FR-02-10|View market price|
|F-02|FR-02-11|View 12-week price trend chart|
|F-02|FR-02-11|View 12-week price trend chart|
|F-02|FR-02-12|Stop market price tracking|
|F-02|FR-02-12|Stop market price tracking|
|F-03|FR-03-01|Submit sale request|
|F-03|FR-03-02|Re-check legal status before forwarding|
|F-03|FR-03-03|Package and forward report and market price to Carpla staff|
|F-03|FR-03-04|Review verified report|
|F-03|FR-03-04|Review verified report|
|F-03|FR-03-05|Send preliminary quote to car owner|
|F-03|FR-03-05|Send preliminary quote to car owner|
|F-03|FR-03-06|View preliminary quote|
|F-03|FR-03-06|View preliminary quote|
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||


||||
| - | - | - |
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||


||||
| - | - | - |
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||
|**Totals**||L2 functions: 31   |   L3 work items: 49|



|**Integrity**||All L3 rows have a numeric Base hours value|
| - | - | - |



||||
| - | - | :- |
||||
||||
||||
||||
||||
||||
||||
|**EARS acceptance criterion**|**Source trace**||
|WHEN the car owner submits a 17 character VIN, the system SHALL validate the VIN format and create a new digital vehicle profile; IF the VIN format is invalid, the system SHALL reject the submission and display an error.|<p>UC-1.01</p><p>CTX: flow-02</p><p>SEQ: S-1.01, S-1.02, S-1.04</p>||
|WHEN the car owner submits a 17 character VIN, the system SHALL validate the VIN format and create a new digital vehicle profile; IF the VIN format is invalid, the system SHALL reject the submission and display an error.|<p>UC-1.01</p><p>CTX:</p><p>SEQ: S-1.03</p>||
|WHEN the car owner opens vehicle identification, the system SHALL fetch and display verified license plate, brand, and engine data from VETC; IF the VETC connection fails, the system SHALL display a service unavailable alert.|<p>UC-1.02</p><p>CTX: flow-05</p><p>SEQ: S-1.05, S-1.08</p>||
|WHEN the car owner opens vehicle identification, the system SHALL fetch and display verified license plate, brand, and engine data from VETC; IF the VETC connection fails, the system SHALL display a service unavailable alert.|<p>UC-1.02</p><p>CTX: flow-05</p><p>SEQ: S-1.06, S-1.07</p>||
|WHEN the car owner navigates to mileage section, the system SHALL retrieve passage history from VETC and display verified minimum ODO; IF no passage records exist, the system SHALL show mileage unverified status.|<p>UC-1.03</p><p>CTX: flow-05</p><p>SEQ: S-1.09, S-1.12</p>||
|WHEN the car owner navigates to mileage section, the system SHALL retrieve passage history from VETC and display verified minimum ODO; IF no passage records exist, the system SHALL show mileage unverified status.|<p>UC-1.03</p><p>CTX: flow-12</p><p>SEQ: S-1.10, S-1.11</p>||
|WHEN the car owner accesses maintenance history, the system SHALL fetch and display official service records from Carpla Service; IF no official record is found, the system SHALL display an empty history prompt.|<p>UC-1.04</p><p>CTX: flow-13, flow-05</p><p>SEQ: S-2.01, S-2.04</p>||
|WHEN the car owner accesses maintenance history, the system SHALL fetch and display official service records from Carpla Service; IF no official record is found, the system SHALL display an empty history prompt.|<p>UC-1.04</p><p>CTX: flow-13, flow-05</p><p>SEQ: S-2.02, S-2.03</p>||
|WHEN the car owner uploads an external repair invoice file, the system SHALL accept PDF, XML, or image formats and store the upload; IF the file format or size is invalid, the system SHALL reject the file.|<p>UC-1.05</p><p>CTX: flow-03, flow-05</p><p>SEQ: S-2.05</p>||
|WHEN the car owner uploads an external repair invoice file, the system SHALL accept PDF, XML, or image formats and store the upload; IF the file format or size is invalid, the system SHALL reject the file.|<p>UC-1.05</p><p>CTX: flow-03, flow-05</p><p>SEQ: S-2.06</p>||
|WHEN an external invoice file is uploaded, the system SHALL extract seller tax code, invoice number, and total amount; IF parsing fails, the system SHALL flag the file for manual verification.|<p>UC-1.05</p><p>CTX: flow-03</p><p>SEQ: S-2.06</p>||
|WHEN invoice metadata is extracted, the system SHALL verify authenticity with the Electronic Invoice Portal; IF valid, update the maintenance timeline; IF invalid, reject invoice and show error message.|<p>UC-1.05</p><p>CTX: flow-17, flow-18</p><p>SEQ: S-2.07, S-2.08</p>||
|WHEN invoice metadata is extracted, the system SHALL verify authenticity with the Electronic Invoice Portal; IF valid, update the maintenance timeline; IF invalid, reject invoice and show error message.|<p>UC-1.05</p><p>CTX: flow-05</p><p>SEQ: S-2.09, S-2.10, S-2.11</p>||
|WHERE the vehicle is an electric vehicle, the system SHALL calculate and display battery SoH based on charging session history; IF charging data is insufficient, the system SHALL display an estimation pending notice.|<p>UC-1.06</p><p>CTX: flow-05</p><p>SEQ: S-3.01, S-3.05</p>||
|WHERE the vehicle is an electric vehicle, the system SHALL calculate and display battery SoH based on charging session history; IF charging data is insufficient, the system SHALL display an estimation pending notice.|<p>UC-1.06</p><p>CTX: flow-05</p><p>SEQ: S-3.02, S-3.03</p>||
|WHERE the vehicle is an electric vehicle, the system SHALL calculate and display battery SoH based on charging session history; IF charging data is insufficient, the system SHALL display an estimation pending notice.|<p>UC-1.06</p><p>CTX:</p><p>SEQ: S-3.04</p>||
|WHEN the car owner clicks the Refresh button, the system SHALL dispatch parallel verification requests to government portals; IF a request is already in progress, the system SHALL disable duplicate clicks.|<p>UC-1.07</p><p>CTX: flow-04</p><p>SEQ: S-4.01</p>||
|WHEN triggered by legal refresh, the system SHALL query the Vehicle Inspection Portal using plate and VIN; IF the portal responds, the system SHALL return validity period; IF timeout, return status pending.|<p>UC-1.08</p><p>CTX: flow-23, flow-24</p><p>SEQ: S-4.02, S-4.03</p>||
|WHEN triggered by legal refresh, the system SHALL query the Traffic Fine Lookup Portal using license plate; IF violations exist, return fine details; IF none, return clear status.|<p>UC-1.09</p><p>CTX: flow-19, flow-20</p><p>SEQ: S-4.04, S-4.05</p>||
|WHEN triggered by legal refresh, the system SHALL query Secured Transaction Registry Portal using VIN; IF active mortgage exists, attach warning details; IF released, return unencumbered status.|<p>UC-1.10</p><p>CTX: flow-21, flow-22</p><p>SEQ: S-4.06, S-4.07</p>||
|WHEN all parallel legal verification queries return, the system SHALL aggregate results and render the consolidated legal status; IF any query fails, the system SHALL display partial results with retry prompt.|<p>UC-1.07</p><p>CTX:</p><p>SEQ: S-4.08</p>||
|WHEN all parallel legal verification queries return, the system SHALL aggregate results and render the consolidated legal status; IF any query fails, the system SHALL display partial results with retry prompt.|<p>UC-1.07</p><p>CTX:</p><p>SEQ: S-4.09</p>||
|WHEN the weekly timer fires, the system SHALL, for every tracked car, check that the verified minimum mileage divided by max(car age in years, 1) reaches 2,000 km per year, then calculate and store a market price and a confidence level. IF the mileage is below this threshold, the system SHALL record INSUFFICIENT\_DATA for that car and skip its price calculation. IF the calculation fails for a car, the system SHALL keep its previous weekly price, log the failure and continue with the remaining cars.|<p>UC-2.03, UC-2.04</p><p>CTX:</p><p>SEQ: S-6.01, S-6.16, S-6.17, S-6.18</p>||
|WHEN the weekly sync job executes, the system SHALL fetch active vehicle listings from Nextgen VN and Chợ Tốt APIs and persist them with source ID and sync timestamp; IF an external partner API times out after 3 retries (6 hours apart), the system SHALL proceed with the responsive partner, write an ERROR log entry with HTTP status, and flag partial sync status. IF both of marketplace partner failed, the system SHALL return API\_failed.|<p>UC-2.05</p><p>CTX: flow-15, flow-16</p><p>SEQ: S-6.02, S-6.03, S-6.04, S-6.09, S-6.10</p>||
|WHEN the weekly calculation starts, the system SHALL fetch active vehicle listings from Nextgen VN and Chợ Tốt APIs and persist them with source ID and  timestamp. IF an external partner API times out after 3 retries (6 hours apart), the system SHALL mark that partner as not valid for the current week, write an ERROR log entry with HTTP status, and flag partial sync status. WHEN the sync of both partners has finished, the system SHALL determine the valid data sources for the current week as the partners that responded, and SHALL pass only the listings from those sources to the weekly calculation. IF no partner is valid, the system SHALL return API\_failed.|<p>UC-2.05</p><p>CTX: flow-15, flow-16</p><p>SEQ: S-6.05, S-6.06, S-6.07, S-6.09, S-6.10</p>||
|WHEN the weekly calculation starts, the system SHALL fetch active vehicle listings from Nextgen VN and Chợ Tốt APIs and persist them with source ID and  timestamp. IF an external partner API times out after 3 retries (6 hours apart), the system SHALL mark that partner as not valid for the current week, write an ERROR log entry with HTTP status, and flag partial sync status. WHEN the sync of both partners has finished, the system SHALL determine the valid data sources for the current week as the partners that responded, and SHALL pass only the listings from those sources to the weekly calculation. IF no partner is valid, the system SHALL return API\_failed.|<p>UC-2.05</p><p>CTX: flow-15, flow-16</p><p>SEQ: S-6.08</p>||



|WHEN selecting valuation datasets, the system SHALL filter listings matching exact brand, model, manufacturing year (within ±1 year), and mileage (within 20% km comparable to the minimum mileage of car); IF zero matching listings satisfy the criteria, the system SHALL widen the manufacturing year and mileage difference according to the widening rule and IF zero matching listings satisfy this table, the system SHALL display "Insufficient data".|<p>UC-2.08</p><p>CTX:</p><p>SEQ: S-6.11 → S-6.13</p>|
| - | - |
|WHEN new listings are stored, the system SHALL normalize their fields and remove invalid listings before they are used for pricing. IF a listing has a missing or invalid mandatory field, the system SHALL exclude it from pricing.|<p>UC-2.07</p><p>CTX:</p><p>SEQ: S-6.12 → S-6.15</p>|
|WHEN the median and P25-P75 range have been calculated for a vehicle, the system SHALL assign a confidence level of High, Medium or Low based on the size of the comparable sample and the comparison level (Level 0, 1 or 2) used to reach it, and save it in the weekly price record; IF fewer than 30 comparable listings remain after Level 2, the system SHALL NOT assign a confidence level and SHALL record INSUFFICIENT\_DATA.|<p>UC-2.08</p><p>CTX:</p><p>SEQ: S-6.20 → S-6.24</p>|
|WHEN at least 30 comparable listings have been selected for a vehicle after price outliers are removed, the system SHALL calculate the median price and the P25-P75 range, round them to the nearest million VND, and save them as the weekly price record; IF fewer than 30 comparable listings remain after Level 2, the system SHALL record INSUFFICIENT\_DATA and SHALL NOT calculate a price.|<p>UC-2.09</p><p>CTX:</p><p>SEQ: S-6.25 → S-6.27</p>|
|WHEN a car model is signed up for the first time, the system SHALL retrieve 180 days of listing history for that model from both partners. IF a partner returns no history, the system SHALL continue with the other partner and record the gap.|<p>UC-2.10</p><p>CTX: flow-8</p><p>SEQ: S-7.01 → S-7.03</p>|
|WHEN a vehicle model is registered on the platform for the first time, the system SHALL query 180 days of historical listings from Nextgen VN and Chợ Tốt; IF a partner endpoint returns an empty payload or error status, the system SHALL process records from the available partner and log a DATA\_GAP warning with the missing date range.|<p>UC-2.06</p><p>CTX: flow-15, flow-16</p><p>SEQ: S-7.06 → S-7.10</p>|
|WHEN the 180-day listing history of a newly signed-up car model has been retrieved, the system SHALL retrieve the car’s verified minimum mileage for each of the previous 12 weeks from VETC and calculate the weekly market price using comparable listings at that mileage; IF VETC has no mileage record for a week, the system SHALL leave that week without a price.|<p>UC-2.03</p><p>CTX: flow-12</p><p>SEQ: S-7.11 → S-7.13</p>|
|WHEN the 180-day listing history of a newly signed-up car model has been retrieved, the system SHALL calculate the weekly market price for each of the previous 12 weeks and store them as historical price records.  IF a week has no listings in the history, the system SHALL leave that week without a price.|<p>UC-2.07, UC-2.08, UC-2.09</p><p>CTX: flow-08</p><p>SEQ: S-7.14 → S.7.21</p>|
|WHEN a car owner opens the market price view for a tracked car, the system SHALL display the latest weekly market price and its confidence level. IF no weekly price exists for the car yet, the system SHALL display a message "Calculating"|<p>UC-2.01</p><p>CTX: flow-08</p><p>SEQ: S-5.01 → S-5.04</p>|
|WHEN a car owner opens the market price view for a tracked car, the system SHALL display the latest weekly market price and its confidence level. IF no weekly price exists for the car yet, the system SHALL display a message "Calculating"|<p>UC-2.01</p><p>CTX: flow-08</p><p>SEQ: S-5.05 → S-5.15</p>|
|<p>WHEN the Car Owner opens the market price view, the system SHALL render a chart with one price point per week for the 12 most recent weeks.</p><p>IF fewer than 12 weekly prices exist, the system SHALL display "No historical data available"</p>|<p>UC-2.02</p><p>CTX: flow-08</p><p>SEQ: S-5.16 → S-5.18</p>|
|<p>WHEN the Car Owner opens the market price view, the system SHALL render a chart with one price point per week for the 12 most recent weeks.</p><p>IF fewer than 12 weekly prices exist, the system SHALL display "No historical data available"</p>|<p>UC-2.02</p><p>CTX: flow-08</p><p>SEQ: S-5.18 → S-5.22</p>|
|WHEN a car owner stops tracking a car, the system SHALL exclude that car from all further weekly calculations. IF the car is not currently tracked, the system SHALL  make no change.|<p>UC-2.11</p><p>CTX:</p><p>SEQ: S-8.01</p>|
|WHEN a car owner stops tracking a car, the system SHALL exclude that car from all further weekly calculations. IF the car is not currently tracked, the system SHALL  make no change.|<p>UC-2.11</p><p>CTX:</p><p>SEQ: S-8.02 → S-8.05</p>|
|WHEN a car owner submits a sale request for a verified report, the system SHALL create the sale request and trigger the legal status re-check and the packaging for Carpla staff. IF the report has not been initialized for the VIN, the system SHALL reject the request and display an error.|<p>UC-3.01</p><p>CTX: flow-01</p><p>SEQ: S-9.01, S-9.02</p>|
|WHEN a sale request is submitted, the system SHALL query the Vehicle Inspection Portal, the Traffic Fine Lookup Portal and the Secured Transaction Registry Portal, and attach the results to the request. IF a portal has no record for the vehicle or does not respond (timeout or error), the system SHALL attach that check as “No data” and still forward the request.|<p>UC-3.02 (incl. UC-3.03, 3.04, 3.05)</p><p>CTX: flow-19, flow-20, flow-21, flow-22, flow-23, flow-24</p><p>SEQ: S-9.03 → S-9.10</p>|
|WHEN a sale request is submitted, the system SHALL package the verified report and the latest market price and make the package available to Carpla staff. IF no market price exists for the car, the system SHALL forward the package with the price marked as unavailable.|<p>UC-3.06</p><p>CTX: flow-09, flow-10</p><p>SEQ: S-9.11 → S-9.13</p>|
|WHEN Carpla staff open a sale request, the system SHALL display the packaged verified report and market price. IF the package is incomplete, the system SHALL display which parts are missing.|<p>UC-3.07</p><p>CTX: flow-09, flow-10</p><p>SEQ: S-10.01, S-10.02</p>|
|WHEN Carpla staff open a sale request, the system SHALL display the packaged verified report and market price. IF the package is incomplete, the system SHALL display which parts are missing.|<p>UC-3.07</p><p>CTX: flow-09, flow-10</p><p>SEQ: S-10.02</p>|
|WHEN Carpla staff submit a quote for a sale request, the system SHALL store it and make it visible to the car owner. IF the quote amount is missing or invalid, the system SHALL reject the submission and display an error.|<p>UC-3.08</p><p>CTX: flow-11</p><p>SEQ: S-10.04, S-10.05</p>|
|WHEN Carpla staff submit a quote for a sale request, the system SHALL store it and make it visible to the car owner. IF the quote amount is missing or invalid, the system SHALL reject the submission and display an error.|<p>UC-3.09</p><p>CTX: flow-07</p><p>SEQ: S-10.06</p>|
|WHEN a car owner opens a sale request, the system SHALL display the preliminary quote once it has been sent. IF no quote has been sent yet, the system SHALL display that the request is waiting for Carpla.|<p>UC-3.09</p><p>CTX: flow-07</p><p>SEQ: S-10.07, S-10.08</p>|
|WHEN a car owner opens a sale request, the system SHALL display the preliminary quote once it has been sent. IF no quote has been sent yet, the system SHALL display that the request is waiting for Carpla.|<p>UC-3.09</p><p>CTX: flow-07</p><p>SEQ: S-10.08</p>|
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||


|||
| - | - |
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||


|||
| - | - |
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||


|||
| - | - |


||||||
| - | - | - | - | :- |
||||||
||||||
||||||
||||||
||||||
||||||
||||||
|**Initial priority**|**FP type**|**L3 ID**|**Layer**||
|Must|EI|FR-01-01-FE|FE||
|Must|EI|FR-01-01-BE|BE||
|Must|EQ|FR-01-02-FE|FE||
|Must|EQ|FR-01-02-INT|INT||
|Must|EQ|FR-01-03-FE|FE||
|Must|EQ|FR-01-03-INT|INT||
|Must|EQ|FR-01-04-FE|FE||
|Must|EQ|FR-01-04-BE|BE||
|Should|EI|FR-01-05-FE|FE||
|Could|EI|FR-01-05-BE|BE||
|Could|NC|FR-01-06-BE|BE||
|Could|EO|FR-01-07-INT|INT||
|Should|EO|FR-01-07-BE|BE||
|Could|EO|FR-01-08-FE|FE||
|Could|EO|FR-01-08-INT|INT||
|Could|EO|FR-01-08-BE|BE||
|Must|EI|FR-01-09-FE|FE||
|Must|EQ|FR-01-10-INT|INT||
|Should|EQ|FR-01-11-INT|INT||
|Must|EQ|FR-01-12-INT|INT||
|Must|EO|FR-01-13-BE|BE||
|Must|EO|FR-01-13-FE|FE||
|Must|EO|FR-02-01-BE|BE||
|Must|EI|FR-02-02-INT1|INT||
|Must|EI|FR-02-02-INT2|INT||
|Should|EI|FR-02-02-BE2|BE||



|Must|NC|FR-02-05-BE|BE|
| - | - | - | - |
|Must|NC|FR-02-03-BE|BE|
|Should|NC|FR-02-04-BE|BE|
|Must|NC|FR-02-06-BE|BE|
|Could|EI|FR-02-07-BE|BE|
|Could|EI|FR-02-07-INT|INT|
|Could|EO|FR-02-08-INT|INT|
|Could|EO|FR-02-08-BE|BE|
|Must|EO|FR-02-09-FE|FE|
|Must|EO|FR-02-09-BE|BE|
|Should|EO|FR-02-10-FE|FE|
|Should|EO|FR-02-10-BE|BE|
|Could|EI|FR-02-11-FE|FE|
|Could|EI|FR-02-11-BE|BE|
|Must|EI|FR-03-01-FE|FE|
|Should|NC|FR-03-02-BE|BE|
|Must|EO|FR-03-03-BE|BE|
|Must|EQ|FR-03-04-FE|FE|
|Must|EQ|FR-03-04-BE|BE|
|Should|EI|FR-03-05-FE|FE|
|Should|EI|FR-03-05-BE|BE|
|Should|EQ|FR-03-06-FE|FE|
|Should|EQ|FR-03-06-BE|BE|
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||


|||||
| - | - | - | - |
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||


|||||
| - | - | - | - |
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||


|||||
| - | - | - | - |


||
| - |
||
||
||
||
||
||
||
|**L3 work item**|
|Display VIN input form with client-side format validation and feedback|
|Validate VIN checksum logic and initialize digital vehicle record in database|
|Display vehicle identification card on the report screen|
|Implement internal API to query core vehicle identity data from VETC|
|Display verified minimum ODO badge|
|Implement API connector to query passage logs from VETC and compute minimum ODO|
|Display interactive maintenance history timeline interface|
|API: Retrieve data of maintenance and repair history from Carpla Service (Carpla Service belongs to Carpla)|
|Display file uploader supporting PDF, XML, images with progress indicator|
|API: Store file uploaded|
|Build OCR processing pipeline and XML parsing worker for invoice metadata|
|API: connect to National Electronic Invoice Portal with alt error handling|
|Merge validated external invoice into vehicle history or return rejection response|
|Display battery state of health (SoH)|
|API: integrate with charging station partners to fetch charging session data from Tasco stations|
|Calculate using degradation algorithm to compare baseline vs current charging duration|
|Display legal status section header with refresh buttons and loading state|
|API: Look up data in National Vehicle Inspection Portal|
|API: look up data in Traffic Police Fine Lookup Portal|
|API: look up data in National Secured Transaction Registry Portal|
|Store legal report aggregator combining inspection, fines, and mortgage payloads for the next displaying|
|Display consolidated legal status card rendering inspection badges, fines, and mortgage tags|
|Check mileage plausibility (verified minimum mileage / max(age, 1) vs 2,000 km/year) and route implausible cars to INSUFFICIENT\_DATA and scheduled weekly job with failure alert and re-run|
|Integrate Nextgen VN listings|
|Integrate Chợ Tốt listings|
|Store credentials, rate limit and retry for partner APIs|



|Filter for vehicles of the same brand, model, mileage and year to manufacture and widen the mileage and year to manufacture according to widening rule if there is not enough data|
| - |
|Normalize fields and remove invalid listings|
|Assign confidence level (High / Medium / Low) from comparable sample size and comparison level, and save it with the weekly price record|
|Calculate median and P25-P75 range (rounded to nearest million VND) from comparable listings and save the weekly price record|
|Detect first sign-up|
|Fetch 180-day listing history from Nextgen VN and Chợ Tốt|
|Read historical VETC readings for the backfill|
|Rebuild 12 weekly prices from listing history|
|Display market price with confidence level|
|API: read latest weekly price and confidence|
|Display 12-week trend chart|
|API: read prices in the latest 12-week|
|Display stop tracking screen|
|API: deactivate tracking|
|Display submit sale request button; form for car owner to fill mileage, desired price; and sending processing notification|
|Refresh the legal status of the vehicle|
|Assemble the package and notify Carpla staff|
|Display screen receive sale request and verified report, market price|
|API: read the package for staff including mileage, phone number, desired price, verified report, market valuation data|
|Enter and send preliminary quote|
|API: store quote, notify car owner|
|Display preliminary quote and button book an inspection|
|API: read the quote and book an inspection|
||
||
||
||
||
||
||
||
||
||
||
||


||
| - |
||
||
||
||
||
||
||
||
||
||
||
||
||
||
||
||
||
||
||
||
||
||
||
||
||
||
||
||


||
| - |
||
||
||
||
||
||
||
||
||
||
||
||
||
||
||
||
||
||
||
||
||
||
||
||
||
||
||
||
||


||
| - |


|||
| - | - |
|||
|||
|||
|||
|||
|||
|||
|**Size**|**Base hours**|
|S|4|
|M|8|
|S|4|
|S|4|
|S|4|
|M|8|
|S|4|
|S|4|
|S|4|
|S|4|
|M|8|
|M|8|
|S|4|
|S|4|
|M|8|
|M|8|
|S|4|
|M|8|
|M|8|
|M|8|
|S|4|
|S|4|
|S|4|
|L|16|
|L|16|
|M|8|



|M|8|
| - | -: |
|L|16|
|S|4|
|L|16|
|S|4|
|M|8|
|M|8|
|M|8|
|S|4|
|S|4|
|M|8|
|S|4|
|S|4|
|S|4|
|M|8|
|S|4|
|M|8|
|S|4|
|S|4|
|S|4|
|M|8|
|S|4|
|M|8|
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||


|||
| - | - |
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||


|||
| - | - |
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
||**320**|



|||
| - | - |


||||||
| - | - | - | - | - |
||||||
||||||
||||||
||||||
||||||
||||||
||||||
|**EARS precise?**|**Hidden work?**|**New L2**|**L2 index**|**FP value**|
|Yes|No|1|1|4|
|Yes|No|0|1|0|
|Yes|No|1|2|4|
|Yes|No|0|2|0|
|Yes|No|1|3|4|
|Yes|No|0|3|0|
|Yes|No|1|4|4|
|Yes|No|0|4|0|
|No|No|1|5|4|
|No|Yes|0|5|0|
|Yes|Yes|1|6|0|
|Yes|No|1|7|5|
|Yes|No|0|7|0|
|No|No|1|8|5|
|No|No|0|8|0|
|No|No|0|8|0|
|Yes|No|1|9|4|
|No|No|1|10|4|
|Yes|No|1|11|4|
|Yes|No|1|12|4|
|Yes|Yes|1|13|5|
|Yes|No|0|13|0|
|Yes|Yes|1|14|5|
|Yes|No|1|15|4|
|Yes|No|0|15|0|
|No|Yes|0|15|0|



|Yes|No|1|16|0|
| - | - | -: | -: | -: |
|No|Yes|1|17|0|
|Yes|No|1|18|0|
|Yes|No|1|19|0|
|Yes|No|1|20|4|
|Yes|No|0|20|0|
|Yes|No|1|21|5|
|Yes|No|1|22|5|
|Yes|No|1|23|5|
|Yes|No|0|23|0|
|Yes|No|1|24|5|
|Yes|No|0|24|0|
|Yes|No|1|25|4|
|Yes|No|0|25|0|
|Yes|No|1|26|4|
|Yes|No|1|27|0|
|Yes|No|1|28|5|
|Yes|No|1|29|4|
|Yes|No|0|29|0|
|Yes|No|1|30|4|
|Yes|No|0|30|0|
|Yes|No|1|31|4|
|Yes|No|0|31|0|
||||||
||||||
||||||
|||||0|
|||||0|
|||||0|
|||||0|
|||||0|
|||||0|
|||||0|
|||||0|
|||||0|



|||||0|
| - | - | - | - | -: |
|||||0|
|||||0|
|||||0|
|||||0|
|||||0|
|||||0|
|||||0|
|||||0|
|||||0|
|||||0|
|||||0|
|||||0|
|||||0|
|||||0|
|||||0|
|||||0|
|||||0|
|||||0|
|||||0|
|||||0|
|||||0|
|||||0|
|||||0|
|||||0|
|||||0|
|||||0|
|||||0|
|||||0|



|||||0|
| - | - | - | - | -: |
|||||0|
|||||0|
|||||0|
|||||0|
|||||0|
|||||0|
|||||0|
|||||0|
|||||0|
|||||0|
|||||0|
|||||0|
|||||0|
|||||0|
|||||0|
|||||0|
|||||0|
|||||0|
|||||0|
|||||0|
|||||0|
|||||0|
|||||0|
|||||0|
|||||0|
|||||0|
|||||0|
|||||0|
|||||**109**|


|**W2 — Costing, item by item**|||||
| - | - | - | :- | :- |
||||||
|**INPUT**|Every L3 row from W1, plus the rates and factors from Parameters. Nothing is typed on this sheet.||||
|**OUTPUT**|Cost of each L3 work item, a subtotal per L2 function, and a function-point cross-check of the whole estimate. These feed W2\_Rollup.||||
|**PROCEDURE**|<p>The sheet is fully derived. The team's job is to READ it, in this order:</p><p>1\. Find the most expensive L3 row. Is it expensive because it is large, or because it is hard?</p><p>2\. Compare two rows with the same size label but different layers — the gap is the AI leverage effect.</p><p>3\. Find a row where EARS precise? is No and read its verification hours.</p><p>4\. Read the function-point cross-check at the bottom and explain the variance.</p>||||
|**TEACHING POINT**|Flip one cell in W1 column M from Yes to No and watch the cost move. That is the cheapest possible proof that a vague specification is measurably more expensive.||||
||||||
|**L3 ID**|**Feature**|**L2 ID**|**Layer**||
|FR-01-01-FE|F-01|FR-01-01|FE||
|FR-01-01-BE|F-01|FR-01-01|BE||
|FR-01-02-FE|F-01|FR-01-02|FE||
|FR-01-02-INT|F-01|FR-01-02|INT||
|FR-01-03-FE|F-01|FR-01-03|FE||
|FR-01-03-INT|F-01|FR-01-03|INT||
|FR-01-04-FE|F-01|FR-01-04|FE||
|FR-01-04-BE|F-01|FR-01-04|BE||
|FR-01-05-FE|F-01|FR-01-05|FE||
|FR-01-05-BE|F-01|FR-01-05|BE||
|FR-01-06-BE|F-01|FR-01-06|BE||
|FR-01-07-INT|F-01|FR-01-07|INT||
|FR-01-07-BE|F-01|FR-01-07|BE||
|FR-01-08-FE|F-01|FR-01-08|FE||
|FR-01-08-INT|F-01|FR-01-08|INT||
|FR-01-08-BE|F-01|FR-01-08|BE||
|FR-01-09-FE|F-01|FR-01-09|FE||
|FR-01-10-INT|F-01|FR-01-10|INT||
|FR-01-11-INT|F-01|FR-01-11|INT||
|FR-01-12-INT|F-01|FR-01-12|INT||
|FR-01-13-BE|F-01|FR-01-13|BE||
|FR-01-13-FE|F-01|FR-01-13|FE||
|FR-02-01-BE|F-02|FR-02-01|BE||
|FR-02-02-INT1|F-02|FR-02-02|INT||
|FR-02-02-INT2|F-02|FR-02-02|INT||
|FR-02-02-BE2|F-02|FR-02-02|BE||
|FR-02-05-BE|F-02|FR-02-03|BE||
|FR-02-03-BE|F-02|FR-02-04|BE||
|#REF!|#REF!|#REF!|#REF!||
|FR-02-04-BE|F-02|FR-02-05|BE||
|FR-02-06-BE|F-02|FR-02-06|BE||
|FR-02-07-BE|F-02|FR-02-07|BE||
|FR-02-07-INT|F-02|FR-02-07|INT||
|FR-02-08-INT|F-02|FR-02-08|INT||



|FR-02-08-BE|F-02|FR-02-09|BE|
| - | - | - | - |
|FR-02-09-FE|F-02|FR-02-10|FE|
|FR-02-09-BE|F-02|FR-02-10|BE|
|FR-02-10-FE|F-02|FR-02-11|FE|
|FR-02-10-BE|F-02|FR-02-11|BE|
|FR-02-11-FE|F-02|FR-02-12|FE|
|FR-02-11-BE|F-02|FR-02-12|BE|
|FR-03-01-FE|F-03|FR-03-01|FE|
|FR-03-02-BE|F-03|FR-03-02|BE|
|FR-03-03-BE|F-03|FR-03-03|BE|
|FR-03-04-FE|F-03|FR-03-04|FE|
|FR-03-04-BE|F-03|FR-03-04|BE|
|FR-03-05-FE|F-03|FR-03-05|FE|
|FR-03-05-BE|F-03|FR-03-05|BE|
|FR-03-06-FE|F-03|FR-03-06|FE|
|FR-03-06-BE|F-03|FR-03-06|BE|
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||


|||||
| - | - | - | - |
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|**TOTAL**||||
|||||
|**Cost per L2 function — the client-facing 'cost of one function'**||||
|**L2 ID**|**L2 function name**|**Feature**|**FP type**|
|FR-01-01|Enter VIN to initialize Report|F-01|EI|
|FR-01-02|View Vehicle Identification|F-01|EQ|
|FR-01-03|View Verified Minimum Mileage|F-01|EQ|
|FR-01-04|View Maintenance and Repair History in Carpla Service|F-01|EQ|
|FR-01-05|Upload External Maintenance and Repair Invoices|F-01|EI|
|FR-01-06|Extract invoice data via OCR or parsing|F-01|NC|
|FR-01-07|Cross check invoice authenticity with Electronic Invoice Portal|F-01|EO|
|FR-01-08|View Battery State of Health|F-01|EO|
|FR-01-09|Trigger Refresh Legal Status|F-01|EI|



|FR-01-10|Check Inspection Status|F-01|EQ||
| - | - | - | - | :- |
|FR-01-11|Check Unpaid Traffic Fine Status|F-01|EQ||
|FR-01-12|Check Mortgage Status|F-01|EQ||
|FR-01-13|Display updated comprehensive legal status|F-01|EO||
|FR-02-01|Calculate weekly market price|F-02|EO||
|FR-02-02|Sync listings from partner marketplaces|F-02|EI||
|FR-02-03|Select comparable vehicles|F-02|NC||
|FR-02-04|Normalize and clean listings|F-02|NC||
|FR-02-05|Calculate the confidence|F-02|NC||
|FR-02-06|Calculate market price|F-02|NC||
|FR-02-07|Sync 180-day listing history (first-time signed-up car)|F-02|EI||
|FR-02-08|Backfill 12-week price history|F-02|EO||
|FR-02-09|Backfill 12-week price history|F-02|EO||
|FR-02-10|View market price|F-02|EO||
|FR-02-11|View 12-week price trend chart|F-02|EO||
|FR-02-12|Stop market price tracking|F-02|EI||
|FR-03-01|Submit sale request|F-03|EI||
|FR-03-02|Re-check legal status before forwarding|F-03|NC||
|FR-03-03|Package and forward report and market price to Carpla staff|F-03|EO||
|FR-03-04|Review verified report|F-03|EQ||
|FR-03-05|Send preliminary quote to car owner|F-03|EI||
|FR-03-06|View preliminary quote|F-03|EQ||
||||||
||||||
||||||
||||||
||||||
||||||
||||||
||||||
||||||
||||||
|**Function-point cross-check — a second estimate from a different direction**|||||
|Transactional function points (EI / EO / EQ)|109\.0|*Counted once per L2 row, from W1 column Q*|||
|Data function points (ILF / EIF)|103\.0|*From W0 Table 4 — counted once for the whole product*|||
|Unadjusted function points|212\.0|*Transactional + data*|||
|Hours per FP (class parameter)|3\.0|*Pre-AI gross hours*|||
|Functional work — function-point estimate (h)|636\.0|*What function points can see*|||
|Non-functional (NC) work — bottom-up (h)|56\.0|*The 0 function point problem: concurrency, authorisation, deployment. IFPUG needs a separate method (SNAP) for this*|||
|**Function-point basis total (h)**|692\.0|*Functional + non-functional*|||
|**Bottom-up basis total (h)**|320\.0|*Sum of W1 column L*|||
|**Variance**|-53.8%|REVIEW - the two methods disagree by more than the tolerance. Look for an over-sized row, or a feature whose hours-per-FP ratio in W2\_Rollup sits far from its siblings.|||
|Active estimation basis|Bottom-up|*Set on Parameters section 9*|||
|Base hours actually in use|#REF!|*What the cost columns are built from right now*|||
|*Read it this way: function points size the FUNCTIONAL work only, so the NC line is added separately rather than folded in. Switching the basis on Parameters section 9 changes which of the two totals drives every cost in the workbook; NC rows stay on bottom-up either way, because there is no function-point number for them to come from.*|||||



|||||||
| - | - | - | - | - | - |
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|**Base hours**|**AI factor**|**AI-adj. hours**|**Verif. factor**|**Verif. hours**|**Person-hours**|
|4\.0|0\.35|1\.4|0\.15|0\.6|2\.0|
|8\.0|0\.70|5\.6|0\.15|1\.2|6\.8|
|4\.0|0\.35|1\.4|0\.15|0\.6|2\.0|
|4\.0|1\.00|4\.0|0\.15|0\.6|4\.6|
|4\.0|0\.35|1\.4|0\.15|0\.6|2\.0|
|8\.0|1\.00|8\.0|0\.15|1\.2|9\.2|
|4\.0|0\.35|1\.4|0\.15|0\.6|2\.0|
|4\.0|0\.70|2\.8|0\.15|0\.6|3\.4|
|4\.0|0\.35|1\.4|0\.40|1\.6|3\.0|
|4\.0|0\.70|2\.8|0\.40|1\.6|4\.4|
|8\.0|0\.70|5\.6|0\.15|1\.2|6\.8|
|8\.0|1\.00|8\.0|0\.15|1\.2|9\.2|
|4\.0|0\.70|2\.8|0\.15|0\.6|3\.4|
|4\.0|0\.35|1\.4|0\.40|1\.6|3\.0|
|8\.0|1\.00|8\.0|0\.40|3\.2|11\.2|
|8\.0|0\.70|5\.6|0\.40|3\.2|8\.8|
|4\.0|0\.35|1\.4|0\.15|0\.6|2\.0|
|8\.0|1\.00|8\.0|0\.40|3\.2|11\.2|
|8\.0|1\.00|8\.0|0\.15|1\.2|9\.2|
|8\.0|1\.00|8\.0|0\.15|1\.2|9\.2|
|4\.0|0\.70|2\.8|0\.15|0\.6|3\.4|
|4\.0|0\.35|1\.4|0\.15|0\.6|2\.0|
|4\.0|0\.70|2\.8|0\.15|0\.6|3\.4|
|16\.0|1\.00|16\.0|0\.15|2\.4|18\.4|
|16\.0|1\.00|16\.0|0\.15|2\.4|18\.4|
|8\.0|0\.70|5\.6|0\.40|3\.2|8\.8|
|8\.0|0\.70|5\.6|0\.15|1\.2|6\.8|
|16\.0|0\.70|11\.2|0\.40|6\.4|17\.6|
|#REF!|#REF!|#REF!|#REF!|#REF!|#REF!|
|4\.0|0\.70|2\.8|0\.15|0\.6|3\.4|
|16\.0|0\.70|11\.2|0\.15|2\.4|13\.6|
|4\.0|0\.70|2\.8|0\.15|0\.6|3\.4|
|8\.0|1\.00|8\.0|0\.15|1\.2|9\.2|
|8\.0|1\.00|8\.0|0\.15|1\.2|9\.2|



|8\.0|0\.70|5\.6|0\.15|1\.2|6\.8|
| -: | -: | -: | -: | -: | -: |
|4\.0|0\.35|1\.4|0\.15|0\.6|2\.0|
|4\.0|0\.70|2\.8|0\.15|0\.6|3\.4|
|8\.0|0\.35|2\.8|0\.15|1\.2|4\.0|
|4\.0|0\.70|2\.8|0\.15|0\.6|3\.4|
|4\.0|0\.35|1\.4|0\.15|0\.6|2\.0|
|4\.0|0\.70|2\.8|0\.15|0\.6|3\.4|
|8\.0|0\.35|2\.8|0\.15|1\.2|4\.0|
|4\.0|0\.70|2\.8|0\.15|0\.6|3\.4|
|8\.0|0\.70|5\.6|0\.15|1\.2|6\.8|
|4\.0|0\.35|1\.4|0\.15|0\.6|2\.0|
|4\.0|0\.70|2\.8|0\.15|0\.6|3\.4|
|4\.0|0\.35|1\.4|0\.15|0\.6|2\.0|
|8\.0|0\.70|5\.6|0\.15|1\.2|6\.8|
|4\.0|0\.35|1\.4|0\.15|0\.6|2\.0|
|8\.0|0\.70|5\.6|0\.15|1\.2|6\.8|
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||


|||||||
| - | - | - | - | - | - |
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|**#REF!**||**#REF!**||**#REF!**|**#REF!**|
|||||||
|||||||
|**L3 items**|**Person-hours**|**Cost (VND)**|**Effective priority**|||
|2|8\.8|2,440,000|Must|||
|2|6\.6|1,780,000|Must|||
|2|11\.2|3,160,000|Must|||
|2|5\.4|1,420,000|Must|||
|2|7\.4|1,920,000|Should|||
|1|6\.8|2,040,000|Could|||
|2|12\.6|3,780,000|Could|||
|3|23\.0|6,600,000|Could|||
|1|2\.0|400,000|Must|||


|1|11\.2|3,360,000|Must|||
| -: | -: | -: | - | - | - |
|1|9\.2|2,760,000|Should|||
|1|9\.2|2,760,000|Must|||
|2|5\.4|1,420,000|Must|||
|1|3\.4|1,020,000|Must|||
|3|45\.6|13,680,000|Must|||
|1|6\.8|2,040,000|Must|||
|1|17\.6|5,280,000|Must|||
|1|3\.4|1,020,000|Should|||
|1|13\.6|4,080,000|Must|||
|2|12\.6|3,780,000|Could|||
|1|9\.2|2,760,000|Could|||
|1|6\.8|2,040,000|Could|||
|2|5\.4|1,420,000|Must|||
|2|7\.4|1,820,000|Should|||
|2|5\.4|1,420,000|Could|||
|1|4\.0|800,000|Must|||
|1|3\.4|1,020,000|Should|||
|1|6\.8|2,040,000|Must|||
|2|5\.4|1,420,000|Must|||
|2|8\.8|2,440,000|Should|||
|2|8\.8|2,440,000|Should|||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||


|||||
| - | - | - | - |
|||||
|||||
|||||
|||||
|||||
|||||
|**Band**|**Rate**|**Cost (VND)**|**Effective priority**|
|Junior|200,000|400,000|Must|
|Senior|300,000|2,040,000|Must|
|Junior|200,000|400,000|Must|
|Senior|300,000|1,380,000|Must|
|Junior|200,000|400,000|Must|
|Senior|300,000|2,760,000|Must|
|Junior|200,000|400,000|Must|
|Senior|300,000|1,020,000|Must|
|Junior|200,000|600,000|Should|
|Senior|300,000|1,320,000|Should|
|Senior|300,000|2,040,000|Could|
|Senior|300,000|2,760,000|Could|
|Senior|300,000|1,020,000|Could|
|Junior|200,000|600,000|Could|
|Senior|300,000|3,360,000|Could|
|Senior|300,000|2,640,000|Could|
|Junior|200,000|400,000|Must|
|Senior|300,000|3,360,000|Must|
|Senior|300,000|2,760,000|Should|
|Senior|300,000|2,760,000|Must|
|Senior|300,000|1,020,000|Must|
|Junior|200,000|400,000|Must|
|Senior|300,000|1,020,000|Must|
|Senior|300,000|5,520,000|Must|
|Senior|300,000|5,520,000|Must|
|Senior|300,000|2,640,000|Must|
|Senior|300,000|2,040,000|Must|
|Senior|300,000|5,280,000|Must|
|#REF!|#REF!|#REF!|#REF!|
|Senior|300,000|1,020,000|Should|
|Senior|300,000|4,080,000|Must|
|Senior|300,000|1,020,000|Could|
|Senior|300,000|2,760,000|Could|
|Senior|300,000|2,760,000|Could|



|Senior|300,000|2,040,000|Could|
| - | -: | -: | - |
|Junior|200,000|400,000|Must|
|Senior|300,000|1,020,000|Must|
|Junior|200,000|800,000|Should|
|Senior|300,000|1,020,000|Should|
|Junior|200,000|400,000|Could|
|Senior|300,000|1,020,000|Could|
|Junior|200,000|800,000|Must|
|Senior|300,000|1,020,000|Should|
|Senior|300,000|2,040,000|Must|
|Junior|200,000|400,000|Must|
|Senior|300,000|1,020,000|Must|
|Junior|200,000|400,000|Should|
|Senior|300,000|2,040,000|Should|
|Junior|200,000|400,000|Should|
|Senior|300,000|2,040,000|Should|
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||


|||||
| - | - | - | - |
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||**#REF!**||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||


|||||
| - | - | - | - |
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||

|**W2 — Roll-up and threshold check**||||||
| - | - | :- | :- | :- | :- |
|||||||
|**INPUT**|W2\_Costing, which reads W1 and W4. Nothing is typed here except the three shared observations at the bottom.|||||
|**OUTPUT**|Per-feature totals, the three headline figures for Session 8, and a pass/fail on every threshold — shown twice: as first written in W1, and after the W4 decisions.|||||
|**PROCEDURE**|<p>1\. The integrator merges the branches and commits.</p><p>2\. Recalculate. Fix nothing yet.</p><p>3\. Just observe and assess whether the cost analysis is good enough</p>|||||
|||||||
|**Per feature (using the effective priority, i.e. after W4)**||||||
|**Feature**|**Name**|**L2**||||
|F-01|Vehicle Passport|13||||
|F-02|Market Price|12||||
|F-03|Sell Request & Preliminary Quote|6||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
||**PROJECT TOTAL**|**31**||||
|*Columns M-O compare the two sizing methods FEATURE BY FEATURE. Data functions (ILF / EIF) are counted once for the whole product, so column N holds transactional function points only — read column O as a RELATIVE figure. Features should cluster around a similar hours-per-FP ratio; the one sitting far from its siblings is where somebody mis-sized something, or where NC rows are carrying work that function points cannot see.*||||||
|||||||
|**The three headline figures — always say which one you mean**||||||
|**Scenario**|**Scope**|**Person-hours**||||
|**Committed**|Must only|168\.4||||
|**Expected**|Must + Should|216\.8||||
|**Ceiling**|Must + Should + Could|293\.2||||
|||||||
|**Threshold check — as first written (W1) vs after the W4 decisions**||||||
|**Indicator**|**As written**|**After W4**||||
|Must share of in-scope effort|0\.0%|57\.4%||||
|Could share (contingency pool)|0\.0%|26\.1%||||
|In-scope effort vs team capacity (p-h)|#VALUE!|293\.2||||
|Hidden technical work, share of effort|0\.0%|0\.0%||||
|L3 rows above the hour ceiling|0\.0|0\.0||||
|Feature count|3\.0|3\.0||||
|||||||
|**Three observations the team carries into W4:**||||||
|||||||
|||||||
|||||||


||||||||||
| - | - | - | - | - | - | :- | :- | :- |
||||||||||
||||||||||
||||||||||
||||||||||
||||||||||
||||||||||
|**L3**|**Base h (in use)**|**Must (p-h)**|**Should**|**Could**|**Won't**||||
|22|124\.0|59\.8|16\.6|42\.4|0\.0||||
|18|144\.0|92\.4|10\.8|34\.0|0\.0||||
|9|52\.0|16\.2|21\.0|0\.0|0\.0||||
||||||||||
||||||||||
||||||||||
||||||||||
||||||||||
||||||||||
||||||||||
||||||||||
||||||||||
|**49**|**320.0**|**168.4**|**48.4**|**76.4**|**0.0**||||
||||||||||
||||||||||
||||||||||
|**Cost (VND)**|**Meaning**||||||||
|48,520,000|*What the team guarantees to deliver — the Minimum Usable SubseT*||||||||
|61,940,000|*The figure given to the client; the base cash flow for Session 8*||||||||
|84,360,000|*If the Could contingency pool is never consumed*||||||||
||||||||||
||||||||||
|**Threshold**|**Verdict (after W4)**||||||||
|60%|PASS||||||||
|20%|PASS||||||||
|300|PASS||||||||
|10%|MISSING - revisit access control, deployment, tests||||||||
|0|PASS||||||||
|3-6|PASS||||||||
||||||||||
||||||||||
||||||||||
||||||||||
||||||||||


|||||||
| - | - | - | - | - | - |
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|**In-scope p-h**|**In-scope cost**|**Must %**|**Bottom-up h**|**Transactional FP**|**Bottom-up h per FP**|
|118\.8|33,840,000|50\.3%|124\.0|51\.0|2\.4|
|137\.2|40,360,000|67\.3%|144\.0|37\.0|3\.9|
|37\.2|10,160,000|43\.5%|52\.0|21\.0|2\.5|
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|**293.2**|**84,360,000**|**57.4%**|**320.0**|**109.0**|**2.9**|
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||

|**W3 — Cross-team review**||||
| - | - | :- | :- |
|||||
|**INPUT**|The owning team's W0–W2 sheets, plus the machine signals already computed in W2\_Rollup.|||
|**OUTPUT**|A completed eight-criterion checklist and a defect log with at least two concrete, evidenced defects. The defect log is the input to W4.|||
|**WHO FILLS WHAT**|BLUE cells = the OWNING team, filled before handing the file over. ORANGE cells = the REVIEWING team. Green cells are computed. Neither team writes in the other's colour.|||
|**PROCEDURE**|<p>1\. Teams swap files in a ring: A reviews B, B reviews C, C reviews A.</p><p>2\. The reviewer reads the machine signals in column D — three criteria are already answered.</p><p>3\. The reviewer judges the five human criteria IN THIS ORDER: C2, then C3, C4, C5, then C8. Every finding must quote the offending cell.</p><p>4\. The reviewer presents. The owning team may ONLY ask clarifying questions — no rebuttal here.</p>|||
|**RULE**|A review that reports 'looks fine' scores zero. Minimum two defects with evidence.|||
|||||
|**Owning team**|Team ...|**Reviewing team**||
|||||
|**Checklist — eight quality criteria**||||
|**#**|**Criterion**|**Judged by**||
|**C1**|Two-way traceability|Machine + person||
|**C2**|Scope boundary explicit (Won't-have with a reason)|Person||
|**C3**|MECE — no duplicates, no missing CRUD|Person||
|**C4**|State and time transitions have a trigger|Person||
|**C5**|Hidden technical work|Machine + person||
|**C6**|Granularity 4-16 hours|Machine||
|**C7**|Layer label FE / BE / INT present|Machine + person||
|**C8**|Independently testable EARS sentence|Person||
|||||
|**Defect log**||||
|**#**|**Location (L2 / L3 ID)**|**Criterion**||
|1|FR-01-02|C3 MECE||
|2|FR-03-04|C8 EARS||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||


||||
| - | - | - |
||||
||||
||||
||||


||||
| - | - | :- |
||||
||||
||||
||||
||||
||||
||||
|Team ...|||
||||
||||
|**Machine signal (computed)**|**Owning team's self-assessment**||
|Features: 3  |  see \_coverage.md|||
|Won't-have rows recorded: 2|||
|*Not machine-checkable — read it*|||
|*Read it — look for CLOCK-triggered functions*|||
|Hidden share: 0.0%  (floor 10%)|||
|L3 rows above ceiling: 0|||
|L3 rows missing a layer: 0|||
|Rows flagged EARS not precise: 8|||
||||
||||
|**Evidence — quote the offending cell**|**Reviewer's proposed bucket**||
|Column C overlaps the constraint already implied by FR-01-01|Fix now - merge or cross-reference||
|Column M says No; the criterion does not state which totals the summary contains|Mark [NEEDS CLARIFICATION]||
||||
||||
||||
||||
||||
||||
||||
||||


|||
| - | - |
|||
|||
|||
|||


||||
| - | - | - |
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||
|**Reviewer verdict and evidence**|**Reviewer: pass?**|**Note**|
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||
|**Owning team's response**|**Owning team's decision**|**Person-hours affected**|
|Accepted - kept separate, rationale added|Fix now|6\.6|
|Accepted - question raised with the client|Clarify|5\.4|
||||
||||
||||
||||
||||
||||
||||
||||


||||
| - | - | - |
||||
||||
||||
||||

|**W4 — Repair and prioritise**|||||
| - | - | :- | :- | :- |
||||||
|**INPUT**|Initial priority from W1, the defect log from W3, and the team capacity from Parameters.||||
|**OUTPUT**|An effective priority for every L2 function, each change carrying the rule it followed and a written justification. The effective priority flows back into W2\_Costing and re-drives every total.||||
|**PROCEDURE**|<p>1\. Sort every W3 defect into one of three buckets: Fix now / Clarify / Reject. Rejecting is allowed, but the reason must be written down.</p><p>2\. Repair STRUCTURE first: merge duplicates, add clock-triggered functions, add missing hidden work, split rows above the ceiling.</p><p>3\. Recalculate and read the new numbers.</p><p>4\. CUT: first move rows out to Won't-have, then demote Must -> Should -> Could. One sentence of justification per change.</p><p>5\. Recalculate and confirm every threshold passes.</p>||||
|**THE RULE ON THE BOARD**|Cut by lowering priority, NEVER by lowering effort. Effort changes only when the technical scope of the row genuinely changes, and what was dropped must be written down.||||
||||||
||||||
|**Decision rules — every priority change must name the rule it followed**|||||
|**Rule**|**MoSCoW meaning**|**Test question**|||
|**R1 · Must**|The release has no viable use without it|If it is absent on delivery day, is the release unusable?|||
|**R2 · Should**|Important, but a workaround exists|Painful to leave out, yet the solution still works?|||
|**R3 · Could**|Desirable, low impact if dropped|Would anyone notice within the first month?|||
|**R4 · Won't**|Agreed as out of scope for this release|Did the client accept deferring it, and is it in W0 Table 3?|||
||||||
||||||
||||||
|**Feature**|**L2 ID**|**L2 function name**|||
|F-01|FR-01-01|Enter VIN to initialize Report|||
|F-01|FR-01-02|View Vehicle Identification|||
|F-01|FR-01-03|View Verified Minimum Mileage|||
|F-01|FR-01-04|View Maintenance and Repair History in Carpla Service|||
|F-01|FR-01-05|Upload External Maintenance and Repair Invoices|||
|F-01|FR-01-06|Extract invoice data via OCR or parsing|||
|F-01|FR-01-07|Cross check invoice authenticity with Electronic Invoice Portal|||
|F-01|FR-01-08|View Battery State of Health|||
|F-01|FR-01-09|Trigger Refresh Legal Status|||
|F-01|FR-01-10|Check Inspection Status|||
|F-01|FR-01-11|Check Unpaid Traffic Fine Status|||
|F-01|FR-01-12|Check Mortgage Status|||



|F-01|FR-01-13|Display updated comprehensive legal status|
| - | - | - |
|F-02|FR-02-01|Calculate weekly market price|
|F-02|FR-02-02|Sync listings from partner marketplaces|
|F-02|FR-02-03|Select comparable vehicles|
|F-02|FR-02-04|Normalize and clean listings|
|F-02|FR-02-05|Calculate the confidence|
|F-02|FR-02-06|Calculate market price|
|F-02|FR-02-07|Sync 180-day listing history (first-time signed-up car)|
|F-02|FR-02-08|Backfill 12-week price history|
|F-02|FR-02-09|Backfill 12-week price history|
|F-02|FR-02-10|View market price|
|F-02|FR-02-11|View 12-week price trend chart|
|F-02|FR-02-12|Stop market price tracking|
|F-03|FR-03-01|Submit sale request|
|F-03|FR-03-02|Re-check legal status before forwarding|
|F-03|FR-03-03|Package and forward report and market price to Carpla staff|
|F-03|FR-03-04|Review verified report|
|F-03|FR-03-05|Send preliminary quote to car owner|
|F-03|FR-03-06|View preliminary quote|
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||
|**Control panel — updates the moment column G changes**|||



|**Indicator**|**Current**|**Threshold**|
| :-: | :-: | :-: |
|Must share of in-scope effort|57\.4%|60\.0%|
|Could share (contingency pool)|26\.1%|20\.0%|
|In-scope effort vs capacity (p-h)|293\.2|300\.0|
|Expected cost (Must + Should)|61,940,000 VND||


||||||
| - | - | - | - | :- |
||||||
||||||
||||||
||||||
||||||
||||||
||||||
||||||
||**Kano category that usually maps here**||||
||Must-be — invisible when present, fatal when absent||||
||One-dimensional — more of it, more satisfaction||||
||Attractive — a delighter, never a Must||||
||Indifferent — asked for, but nobody actually cares||||
||||||
||||||
||||||
|**Initial priority**|**Rule applied**|**Kano category**|**New priority**||
|Must|||||
|Must|||||
|Must|||||
|Must|||||
|Should|||||
|Could|||||
|Could|||||
|Could|||||
|Must|||||
|Must|||||
|Should|||||
|Must|||||


|Must||||
| - | - | - | - |
|Must||||
|Must||||
|Must||||
|Must||||
|Should||||
|Must||||
|Could||||
|Could||||
|Could||||
|Must||||
|Should||||
|Could||||
|Must||||
|Should||||
|Must||||
|Must||||
|Should||||
|Should||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||


|**Verdict**||||
| :-: | :-: | :-: | :-: |
|PASS||||
|PASS||||
|PASS||||
|*The figure given to the client and used for NPV in Session 8*||||


|||||
| - | - | - | :- |
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|||||
|**Effective priority**|**Justification (required if changed)**|**Person-hours**||
|Must||8\.8||
|Must||6\.6||
|Must||11\.2||
|Must||5\.4||
|Should||7\.4||
|Could||6\.8||
|Could||12\.6||
|Could||23\.0||
|Must||2\.0||
|Must||11\.2||
|Should||9\.2||
|Must||9\.2||



|Must||5\.4|
| - | - | -: |
|Must||3\.4|
|Must||45\.6|
|Must||6\.8|
|Must||17\.6|
|Should||3\.4|
|Must||13\.6|
|Could||12\.6|
|Could||9\.2|
|Could||6\.8|
|Must||5\.4|
|Should||7\.4|
|Could||5\.4|
|Must||4\.0|
|Should||3\.4|
|Must||6\.8|
|Must||5\.4|
|Should||8\.8|
|Should||8\.8|
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||


||||
| :-: | :-: | :-: |
||||
||||
||||
||||


||
| - |
||
||
||
||
||
||
||
||
||
||
||
||
||
||
||
||
|**Cost (VND)**|
|2,440,000|
|1,780,000|
|3,160,000|
|1,420,000|
|1,920,000|
|2,040,000|
|3,780,000|
|6,600,000|
|400,000|
|3,360,000|
|2,760,000|
|2,760,000|



|1,420,000|
| -: |
|1,020,000|
|13,680,000|
|2,040,000|
|5,280,000|
|1,020,000|
|4,080,000|
|3,780,000|
|2,760,000|
|2,040,000|
|1,420,000|
|1,820,000|
|1,420,000|
|800,000|
|1,020,000|
|2,040,000|
|1,420,000|
|2,440,000|
|2,440,000|
||
||
||
||
||
||
||
||
||
||
||


||
| :-: |
||
||
||
||

|**W5 — Baseline and handover**|||
| - | :- | :- |
||||
|**INPUT**|The three headline figures from W2\_Rollup and the assumption set from Parameters.||
|**OUTPUT**|Git tag fl-v1.0, business-case.md (three figures plus assumptions), tasks.md seeded risk-first, and one consolidated open-questions table.||
|**PROCEDURE**|<p>1\. The commit message records that all thresholds pass.</p><p>2\. Fill sections 1 and 2 below; export them as business-case.md.</p><p>3\. In section 3, filter Effective priority = Must and sort Risk order ascending — that is the order for tasks.md.</p><p>4\. Complete section 4 and confirm nothing is left dangling.</p>||
||||
|**1. Three scenarios**|||
|**Scenario**|**Person-hours**||
|**Pessimistic — Must only**|168\.4||
|**Base — Must + Should**|216\.8||
|**Optimistic — full scope**|293\.2||
|*The three MoSCoW tiers ARE the three scenarios — no invented plus-or-minus 20%.*|||
||||
|**2. Assumptions**|||
|**Assumption**|**Value**||
|Junior rate|200,000 VND||
|Senior rate|300,000 VND||
|Expert rate|400,000 VND||
|AI leverage — FE / BE / INT|0\.35 / 0.7 / 1||
|Verification factor — precise / vague|0\.15 / 0.4||
|Hours per function point|3||
|Team capacity (person-hours)|300||
|Money contingency on the Must set|10%||
||||
|**3. tasks.md seed — filter Effective priority = Must, sort Risk order ascending**|||
|**L3 ID**|**Layer**||
|FR-01-01-FE|FE||
|FR-01-01-BE|BE||
|FR-01-02-FE|FE||
|FR-01-02-INT|INT||
|FR-01-03-FE|FE||
|FR-01-03-INT|INT||
|FR-01-04-FE|FE||
|FR-01-04-BE|BE||
|FR-01-05-FE|FE||
|FR-01-05-BE|BE||
|FR-01-06-BE|BE||
|FR-01-07-INT|INT||
|FR-01-07-BE|BE||
|FR-01-08-FE|FE||
|FR-01-08-INT|INT||
|FR-01-08-BE|BE||



|FR-01-09-FE|FE|
| - | - |
|FR-01-10-INT|INT|
|FR-01-11-INT|INT|
|FR-01-12-INT|INT|
|FR-01-13-BE|BE|
|FR-01-13-FE|FE|
|FR-02-01-BE|BE|
|FR-02-02-INT1|INT|
|FR-02-02-INT2|INT|
|FR-02-02-BE2|BE|
|FR-02-05-BE|BE|
|FR-02-03-BE|BE|
|#REF!|#REF!|
|FR-02-04-BE|BE|
|FR-02-06-BE|BE|
|FR-02-07-BE|BE|
|FR-02-07-INT|INT|
|FR-02-08-INT|INT|
|FR-02-08-BE|BE|
|FR-02-09-FE|FE|
|FR-02-09-BE|BE|
|FR-02-10-FE|FE|
|FR-02-10-BE|BE|
|FR-02-11-FE|FE|
|FR-02-11-BE|BE|
|FR-03-01-FE|FE|
|FR-03-02-BE|BE|
|FR-03-03-BE|BE|
|FR-03-04-FE|FE|
|FR-03-04-BE|BE|
|FR-03-05-FE|FE|
|FR-03-05-BE|BE|
|FR-03-06-FE|FE|
|FR-03-06-BE|BE|
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||


|||
| - | - |
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||
|||


|||
| - | - |
|||
|||
|||
|*Risk order puts integrations first: the unknowns must be touched early, not at the end of DBIZ3.*||
|||
|**4. Open questions still outstanding — paste back into the owning feature's spec.md**||
|**#**|**[NEEDS CLARIFICATION] question**|
|1|[NEEDS CLARIFICATION: which totals must the weekly summary contain?]|
|2|[NEEDS CLARIFICATION: is card-swipe data pulled as a daily batch or in real time?]|
|||
|||
|||
|||
|||
|||
|||
|||


||||
| - | - | - |
||||
||||
||||
||||
||||
||||
|**Cost (VND)**|**Role in the sensitivity analysis**||
|48,520,000|Lowest investment, lowest benefit||
|61,940,000|The client figure; base cash flow for NPV||
|84,360,000|If the Could pool is never consumed||
||||
||||
||||
|**Source**|||
|*ITviec 2025-26 salary report*|||
|*ITviec 2025-26 salary report*|||
|*ITviec 2025-26 salary report*|||
|*Class parameter, recalibrated each cohort*|||
|*Class parameter*|||
|*Class parameter — used only for the cross-check*|||
|5 people x 12 weeks x 5 h|||
|*Covers estimation error only. Do NOT add 20% on top of the Could pool — same buffer twice.*|||
||||
||||
|**Effective priority**|**Person-hours**|**Risk order**|
|Must|2\.0|3|
|Must|6\.8|2|
|Must|2\.0|3|
|Must|4\.6|1|
|Must|2\.0|3|
|Must|9\.2|1|
|Must|2\.0|3|
|Must|3\.4|2|
|Should|3\.0|3|
|Should|4\.4|2|
|Could|6\.8|2|
|Could|9\.2|1|
|Could|3\.4|2|
|Could|3\.0|3|
|Could|11\.2|1|
|Could|8\.8|2|



|Must|2\.0|3|
| - | -: | -: |
|Must|11\.2|1|
|Should|9\.2|1|
|Must|9\.2|1|
|Must|3\.4|2|
|Must|2\.0|3|
|Must|3\.4|2|
|Must|18\.4|1|
|Must|18\.4|1|
|Must|8\.8|2|
|Must|6\.8|2|
|Must|17\.6|2|
|#REF!|#REF!|#REF!|
|Should|3\.4|2|
|Must|13\.6|2|
|Could|3\.4|2|
|Could|9\.2|1|
|Could|9\.2|1|
|Could|6\.8|2|
|Must|2\.0|3|
|Must|3\.4|2|
|Should|4\.0|3|
|Should|3\.4|2|
|Could|2\.0|3|
|Could|3\.4|2|
|Must|4\.0|3|
|Should|3\.4|2|
|Must|6\.8|2|
|Must|2\.0|3|
|Must|3\.4|2|
|Should|2\.0|3|
|Should|6\.8|2|
|Should|2\.0|3|
|Should|6\.8|2|
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||


||||
| - | - | - |
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||
||||


||||
| - | - | - |
||||
||||
||||
||||
||||
||||
|**Blocking?**|**Owning feature**|**Default applied**|
|No|F-03|Assuming sessions per workstation and hours per workstation|
|Yes|F-02|Assuming a daily batch|
||||
||||
||||
||||
||||
||||
||||
||||

|**Capital Budgeting Model**|||
| - | :- | :- |
||||
|**1. Assumptions**|||
|Initial Investment / CAPEX (VND)|61,940,000||
|Max Revenue Capacity (VND)|357,600,000||
|WACC / Discount Rate|15\.0%||
|Corporate Income Tax|20\.0%||
|Useful Life (Years)|5||
||||
|**2. Revenue Ramp-up**|||
|Market Penetration Rate|||
||||
|**3. Cash Flow Projection**|||
|**Item / Year**|**0**||
|CAPEX|-61,940,000||
|Revenue|||
|OPEX:|||
|Server Cost|||
|API Cost|||
|Maintenance|||
|Marketing|||
|Cutomer Service|||
|Payment Gateway|||
|Total OPEX|||
|Depreciation|||
|EBT|||
|Tax (20%)|||
|Net Income|||
|Net Cash Flow (NCF)|-61,940,000||
|Cumulative NCF|-61,940,000||
||||
|**4. Key metrics**|||
|Net Present Value (NPV)|242,135,758||
|Internal Rate of Return (IRR)|87\.6%||
|Payback Period (Years)|1\.73||



||||||
| - | :- | :- | :- | :- |
||||||
||||||
||||||
||||||
||||||
||||||
||||||
||||||
||||||
|30\.0%|60\.0%|80\.0%|100\.0%|100\.0%|
||||||
||||||
|**1.00**|**2.00**|**3.00**|**4.00**|**5.00**|
|0|0|0|0|0|
|107,280,000|214,560,000|286,080,000|357,600,000|357,600,000|
||||||
|24,000,000|24,000,000|24,000,000|24,000,000|24,000,000|
|48,000|48,000|48,000|48,000|48,000|
|9,291,000|9,291,000|9,291,000|9,291,000|9,291,000|
|26,820,000|53,640,000|71,520,000|89,400,000|89,400,000|
|36,000,000|36,000,000|36,000,000|36,000,000|36,000,000|
|2,145,600|4,291,200|5,721,600|7,152,000|7,152,000|
|98,304,600|127,270,200|146,580,600|165,891,000|165,891,000|
|12,388,000|12,388,000|12,388,000|12,388,000|12,388,000|
|-3,412,600|74,901,800|127,111,400|179,321,000|179,321,000|
|0|14,980,360|25,422,280|35,864,200|35,864,200|
|-3,412,600|59,921,440|101,689,120|143,456,800|143,456,800|
|8,975,400|72,309,440|114,077,120|155,844,800|155,844,800|
|-52,964,600|19,344,840|133,421,960|289,266,760|445,111,560|
||||||
||||||
||||||
||||||
||||||


|||||||
| - | - | - | - | - | - |
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||


|||||||
| - | - | - | - | - | - |
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||


|||||||
| - | - | - | - | - | - |
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||
|||||||


||
| - |
||
||
||
||
||
||
||
||
||
||
||
||
||
||
||
||
||
||
||
||
||
||
||
||
||
||
||
||
||
||
||
||
||
||

|**No.**|**Member**|**Student ID**|
| :-: | :-: | :-: |
|1|Đinh Thu Thảo|2412560044|
|2|Nguyễn Hà Phương|2412560039|
|3|Vũ Hương Ly|2412560026|
|4|Nguyễn Hương Giang|2411560012|
|5|Nguyễn Danh Quang|2411560041|



|**Responsible Tasks**|
| :-: |
|<p>- Draft context diagram</p><p>- Sequence diagram for staff</p><p>- Function list: Feature Verified Vehicle Report (F-01)</p>|
|<p>- Context diagram: feature Verified Vehicle</p><p>Report (F-01), feature Sell Request & Quote (F-03).</p><p>- Review function list</p><p>- Cost Estimation</p>|
|<p>- Context diagram: feature market price (F-02)</p><p>- Use case diagram: feature market price (F-02).</p><p>- Sequence diagrams for three features (F-01,02,03).</p>|
|<p>- Draw CVCA before and after diagram.</p><p>- Function list: feature market price, sale request and preliminary cost. (F-02, F-03)</p><p>- Review context, use case, sequence diagrams.</p>|
|<p>- Use Case Diagram for three features</p><p>- Capital Budgeting</p>|


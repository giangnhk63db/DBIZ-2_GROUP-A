# Glossary: Verified Vehicle Report

Every abbreviation and project-specific term used in this repository, with what it means **here**.
Terms are grouped by topic and sorted by usage; use your editor's search to find one. Paths are
relative to `Docs/` unless they start with `README` or `GLOSSARY`.

**How to use this file**

- Look a term up here before guessing its meaning (README rule 8).
- Do not introduce a synonym for a defined term. If a term has two spellings in the repository, the
  one marked "use" is the one to write.
- When a new term or abbreviation enters any file, add one row here in the same change.
- Section 9 lists look-alikes that cause mistakes. Read it once.

**Contents:** The terms used in this project is clarified in this file and divided into 11 groups to explain<br>1 Course and project · 2 Organisations, partners, systems · 3 Vehicle and domain ·
4 Market price · 5 Invoices · 6 Analysis and design method · 7 Requirements, rules and IDs ·
8 Screens and UI · 9 Look-alikes · 10 Estimation and finance ·
11 Vietnamese interface terms

---

## 1. Course and project

| Term | Stands for / meaning | Where used |
|---|---|---|
| DBIZ2 | Digital Business Project II: the course in which this design package is written | README, Project Brief |
| DBIZ3 | The follow-up course in which the team builds the product from these files with an AI coding agent | README, Project Brief |
| FTU | Foreign Trade University | README |
| VJCBI College | Vietnam – Japan College of Business and Innovation, subsidiary school of Foreign Trade university | README |
| Group A | The five-member team that owns this repository | README, Project Brief |
| L1 to L8 | Lesson 1 to Lesson 8 of DBIZ2. Not the same as L2/L3 function levels (section 9) | README folder map, reading order |
| Senior Meeting | A client review: after Lesson 4 (scope confirmation) and after Lesson 8 (final package review) | Project Brief |
| Midterm | Review after Lesson 6: function list, cost and capital budgeting (W0 to W6) | Project Brief |
| W0 to W6 | The numbered worksheets of the Lesson 5-6 workbook, kept as files in `Function List/`<br>W0 scope, W1 function list, W2 cost, W4 priorities, W5 handover, W6 cash flow; there is no W3 file) | Function List |
| Design package | What this repository is: requirements, analysis and external design only, no code | README |
| Integrator | Team role that merges every file and keeps names consistent | Project Brief |
| AI Log | `Project/AI Log.md`: one row each time an AI tool proposed something the team did not accept | README rule 9 |
| AI coding agent | The AI tool that builds the product in DBIZ3 from these files | README |

## 2. Organisations, partners, systems

| Term | Stands for / meaning | Where used |
|---|---|---|
| TASCO / Tasco Corporations | The client, a corporate group. Written `TASCO` in system names and `Tasco` in prose; same company | Project Brief, CVCA |
| TASCO Mobility | The existing consumer app of Tasco, in which this feature is built (ADR-001). Also called "TASCO Mobility System" in diagrams. | ADR-001, Architecture |
| Verified Vehicle Report | The product and the feature: a verified record of one car, plus market price and a path to a preliminary quote. "Report" is the name; "Passport" was the earlier name. Verified Vehicle Report is aimed to be integrated in Tasco Mobility | Project Brief, W0 |
| VETC | Vietnam Electronic Toll Collection, a Tasco group toll-collection company. Read-only source of identification data and toll passage history | CVCA, W0 Table 4 |
| Carpla | Tasco's used-car business. Its staff review sale requests and give the preliminary quote | Project Brief |
| Carpla Service | Tasco group maintenance and repair business. Read-only source of the official maintenance and repair history | CVCA, SyR-04 |
| Carpla CRM | The existing Carpla system in which staff review the sale request and enter the quote. This system builds no staff screens of its own, only integrated staff screens in the CRM (ADR-003) | ADR-003, Screen List |
| CRM | Customer Relationship Management: software for managing customers available of Carpla | ADR-003 |
| Carpla staff | The actor who reviews a sale request and sends the preliminary quote (UC-3.07, UC-3.08) | W0, UC-3 |
| Car Owner | The primary actor who creates the report, views it, tracks the market price and submits a sale request | W0, UC-1 to UC-3 |
| Used Car Buyer | A stakeholder in the CVCA value chain who pays for the car; not an actor of the system | CVCA |
| Tasco Insurance, Tasco Land | Tasco group companies shown in the CVCA; the new system reads nothing from them | CVCA |
| Nextgen VN / Nextgen Việt Nam | Partner marketplace; source of used-car listing data for calculating market price for reference | CVCA, FR-02-02 |
| Chợ Tốt | Partner marketplace of Carousell Group; source of used-car listing data for calculating market price for reference | CVCA, FR-02-02 |
| Partner marketplaces | Nextgen VN and Chợ Tốt together | W0, W1 |
| Vehicle Inspection Portal | Government portal that returns the inspection status (W0 calls it the National Vehicle Inspection Portal) | SyR-10, FR-01-10 |
| Traffic Fine Lookup Portal | Government portal that returns the traffic fine status (W0: Traffic Police Department portal); queried by license plate | SyR-11, FR-01-11 |
| Secured Transaction Registry Portal | Government portal that returns the mortgage status; queried by VIN | SyR-12, FR-01-12 |
| Electronic Invoice Lookup Portal | Government portal that confirms whether an invoice is authentic (W0: General Department of Taxation electronic invoice portal) | SyR-08, FR-01-07 |
| Lookup portals / the three portals | The inspection, traffic fine and secured transaction portals | ADR-006, BR-REF-01 |
| EV | Electric vehicle | SyR-05 |
| EV charging stations | Partner and Tasco charging stations; read-only source of charging session data for the battery SoH | CVCA, W0 Table 4 |
| Weekly timer / Weekly Timer | The non-human actor that starts the weekly jobs; in the architecture it is the Scheduler (ADR-002) | W0, UC-2 |
| Scheduler | Business-layer element that starts the Market price service every week (ADR-002) | Architecture |
| External system | Anything outside the TASCO Mobility System that it reads: nine in total (ADR-004). All are read-only | ADR-004, W0 Table 4 |

## 3. Vehicle and domain terms

| Term | Stands for / meaning | Where used |
|---|---|---|
| VIN | Vehicle Identification Number: exactly 17 uppercase letters and digits, never I, O or Q. The only way a vehicle enters the system (BR-VFVR-01) | UC-1.01, FR-01-01 |
| Vehicle profile / vehicle record / Verified vehicle report| The digital record created from a VIN; the Vehicle Profile screen is S-F01-02. This consolidated record will also be sent to Carpla staff for preliminary quote | SyR-01, Screen List |
| License plate | Vehicle registration plate; the key for the traffic fine query and one key for the inspection query | FR-01-10, FR-01-11 |
| ODO | Odometer reading in km | FR-01-03 |
| Verified minimum mileage | The lowest mileage the system can prove, computed from VETC toll passage history (sum of distances between consecutive stations, BR-DIST-01). Shown as "verified minimum ODO" | UC-1.03, FR-01-03 |
| Toll passage history | The record of toll stations a car passed, from VETC | CVCA, S-F01-03 |
| Mileage unverified | Status shown when VETC has no passage record for the car | FR-01-03 |
| Declared mileage | The mileage the owner enters in a sale request. Blocked when more than 20% below the VETC minimum mileage until declared again (SyR-19) | SyR-19, S-F03-01 |
| Mileage plausibility | Check that minimum mileage divided by max(car age in years, 1) reaches 2,000 km per year; otherwise INSUFFICIENT_DATA (BR-MIL-01, UC-2.03) | FR-02-01 |
| SoH | State of Health of the battery, as a rounded percentage, for electric vehicles only. Charging time in the standard voltage window V1 to V2 of the current battery divided by that of a new battery; needs at least 3 valid sessions (BR-SOH-02). Grades: Tốt (Good) at 80% or more, Trung bình (Average) from 65% to below 80%, Thấp (Low) below 65% (BR-SOH-01) | UC-1.06, AD-UC-1.06 |
| Charging session data | Records of charging sessions from the charging stations; input to the SoH | SyR-05 |
| Legal status | Inspection status, unpaid traffic fines and mortgage status of the car together | UC-1.07, FR-01-13 |
| Inspection status | Validity period of the vehicle inspection | FR-01-10 |
| Unpaid traffic fines | Traffic violations with a penalty that is not yet paid | FR-01-11 |
| Mortgage status | Whether the car is mortgaged (credit institution, loan amount, active or released) | FR-01-12, CVCA |
| Refresh | The owner's action that re-queries the three portals in parallel (BR-REF-01) | UC-1.07 |
| Re-check legal status | The same three queries run by the system when a sale request is submitted | UC-3.02, FR-03-02 |
| "No data" | Legal status result when a portal has no record or does not answer; the request is still forwarded | FR-03-02, Activity Index |
| Sale request (use) / sell request | A request from the Car Owner to sell the car to Carpla, with current mileage, desired price and phone number. Both spellings occur; "sale request" is the form in UC and FR names | UC-3.01, SyR-21 |
| Preliminary quote | Price range that Carpla staff send back for a sale request; "more accurate" than before because it rests on the verified report | StR-04, UC-3.08 |
| Package | The verified report plus the latest market price, made available to Carpla staff (UC-3.06) | FR-03-03 |
| Pre-filled form / automatically filled selling form | The sale request form filled from the vehicle record; the owner only adds mileage, desired price and phone number | StR-03, SyR-20 |
| Vehicle Passport | Name of feature F-01 | W0 Table 2 |

## 4. Market price 

| Term | Stands for / meaning | Where used |
|---|---|---|
| Market price | The weekly price range the owner sees for a tracked car, shown for reference | UC-2.01, FR-02-10 |
| Weekly price record | The stored result of one weekly calculation: median, P25-P75 range, confidence level, or a failure state | FR-02-06, Activity Index |
| Tracked car | A car whose market price is calculated every week; tracking starts when the car model is first signed up and ends with "stop tracking" (UC-2.11) | FR-02-12 |
| 12-week trend | One price point per week for the latest 12 weeks, drawn as a line chart | UC-2.02, FR-02-11 |
| Listing | One used-car advertisement from a partner marketplace | FR-02-02 |
| Listing store | Where synced listings are kept (cached copy, W0 data group "Market listing"); shown as `LDB` in sequence diagrams | S-07, CVCA |
| Comparable listings / comparable vehicles | Listings with the same brand and model whose year and mileage are close to the car's | FR-02-03 |
| Comparison level, Level 0 / 1 / 2 | Steps of the widening rule. Level 0: same brand and model, year within ±1, mileage within ±20%. Level 1: year ±2. Level 2: mileage ±40%. Not the same as L2 (section 9) | AD-UC-2.08 |
| Widening rule | Move to the next level when fewer than 30 comparable listings remain | FR-02-03 |
| Outlier | A listing price removed before pricing, detected with the IQR | AD-UC-2.08 |
| IQR | Interquartile range: the spread between the 25th and 75th percentile, used to remove price outliers | AD-UC-2.08 |
| Median | Middle price of the comparable listings; the headline market price | FR-02-06 |
| P25-P75 range | The range between the 25th and 75th percentile of comparable prices; rounded to the nearest million VND | FR-02-06 |
| Confidence level | High, Medium or Low, assigned from the sample size and the comparison level used. Not assigned when data is insufficient | FR-02-05, AD-UC-2.09 |
| BR-COM-01 | Rule: a price is calculated only if at least 30 comparable listings remain after outlier removal and widening up to Level 2 | AD-UC-2.04 |
| INSUFFICIENT_DATA | Weekly price record state: mileage plausibility failed, or fewer than 30 comparable listings remain after Level 2. No price, no confidence level | FR-02-01, FR-02-06 |
| API_FAILED | Weekly price record state when both marketplace partners fail after the retries (written `API_failed` in W1) | FR-02-02 |
| Partial sync | A sync in which one partner failed; the system continues with the other and flags it | FR-02-02 |
| Retry rule | Marketplace sync: up to 3 retries, 6 hours apart, then log an ERROR with the HTTP status (BR-RET-01 in AD-UC-2.05; the same ID is used differently in AD-UC-1.03, see section 9) | AD-UC-2.05 |
| "Calculating" | Message shown when a car has no weekly price yet | FR-02-10 |
| Backfill | Building the 12 earlier weekly prices of a newly tracked car from the 180-day listing history and past VETC readings | UC-2.10, FR-02-08, FR-02-09 |
| 180-day listing history | Listing history fetched once per car model, the first time it is signed up (UC-2.06). `<<extend>>` of UC-2.10 | UC-2.06, AI Log #1 |
| First sign-up | A car model that is not yet in the listing store | FR-02-07 |

## 5. Invoices 

| Term | Stands for / meaning | Where used |
|---|---|---|
| External invoice | A maintenance or repair invoice from outside Carpla Service, uploaded by the owner (PDF, XML or image) | UC-1.05, SyR-06 |
| OCR | Optical Character Recognition: reading text from an invoice image or PDF | FR-01-06, BR-OCR-01 |
| Parsing | Reading the fields from an XML invoice | FR-01-06 |
| Invoice key fields | Seller tax code, invoice symbol, invoice number and total amount: what is sent to the portal to check authenticity | FR-01-06, CVCA |
| Invoice authenticity | Result from the Electronic Invoice Lookup Portal; only valid invoices join the maintenance history (BR-INV-01) | FR-01-07 |
| Manual verification | Owner checks and edits the scanned values when parsing fails or needs review | FR-01-06 |
| Maintenance timeline | The maintenance history shown to the owner: official Carpla Service records plus valid external invoices | FR-01-04, Activity Index |

## 6. Analysis and design method

| Term | Stands for / meaning | Where used |
|---|---|---|
| CVCA | Customer Value Chain Analysis: stakeholders and what flows between them (money, information, goods or services), drawn before the system is designed | Context/CVCA.md |
| Value flow edge labels | `$` money, `i:` information, `goods:` or `service:` goods or services | CVCA |
| Context Diagram | The system as one black box with the entities around it and every flow across the boundary | Context/Context Diagram.md |
| CTX: flow-nn | One flow of the context diagram | list-of-flow-context-diagram.md |
| Boundary | The line between the system and the outside; data groups "crossing the boundary" are listed in the context diagram file | Context Diagram |
| P (in the CVCA) | Problem number, P-1 to P-5. In the context diagram `P` is the process node, the system itself | CVCA, Context Diagram |
| StR | Stakeholder Requirement: "a stakeholder shall be able to ...", from a problem | CVCA section 4 |
| SyR | System Requirement: "the system shall ...", from an StR | Architecture section 1 |
| UC | Use case: one goal of an actor, `UC-f.nn` | W0, Use Case Diagram |
| UC-0.xx | The four overview use cases of the Overview diagram (create report, view identification and status, view market price, request sale). Not in W0 Table 1 | Use Case Diagram overview |
| Actor | A person or system that starts a use case or takes part in it (Car Owner, Carpla staff, weekly timer, external systems) | Use Case Diagram |
| `<<include>>` / `<<extend>>` | Use case relations: the included use case always runs; the extending one runs only under a condition. AI Log #1 explains why UC-2.06 extends UC-2.10 | Use Case Diagram |
| `usecase-beta` | Mermaid diagram type used for use case diagrams | README section 6 |
| SD | Sequence Diagram: how the parts talk for one use case; files `S-01` to `S-10`, messages `S-X.YY` | Sequence Diagram |
| SEQ | Prefix of "Source trace" cells in W1 and Screen Specs that cite sequence messages | W1 |
| Participant | One lifeline in a sequence diagram (see section 10 for its short names) | Sequence Diagram |
| AD | Activity Diagram: behaviour of one use case, with branches and objects; `AD-UC-f.nn` | Activity Diagram |
| Swimlane / lane | Part of an AD that belongs to one actor or system (for example Car Owner, TASCO Mobility System) | Activity Diagram |
| `swimlane-beta` | Mermaid diagram type used for activity diagrams | README section 6 |
| s / d / f nodes | In an AD table: `s` action, `d` decision, `f` fork (parallel branches) | AD files |
| Object state | State of a data object across an AD (for example Invoice File: Uploaded, Stored, Parsed, Verified); collected in Activity Index | Activity Index |
| Activity Index | One row per use case: has a diagram or why not | Activity Diagram |
| Usage flow | The order in which one actor uses the use cases, with the screens for each step | External Design |
| Architecture layers | Presentation layer, Business layer, Data access layer; plus external systems | Architecture section 3 |
| ADR | Architecture Decision Record: one decision with context, decision and consequences. Accepted ADRs are not rewritten | Architecture/adr |
| Traceability | The chain from problem to screen and the table that maps context flow to use case to sequence message | README section 4, traceability.md |
| Source trace | W1 column that lists the UC, CTX flow and sequence messages behind a function | W1 |
| Mermaid | Text-based diagram language used for all diagrams; renders in GitHub or on mermaid.live | README section 6 |
| `TB` / `LR` | Mermaid layout direction: top to bottom, left to right | diagram code |
| Won't-have | Features recorded as out of scope (Sale Alert, Maintenance Alert) and never built in this release | W0 Table 3, W4 |
| Open question | A row marked `[NEEDS CLARIFICATION: ...]`; never a decision | README rule 6 |

## 7. Requirements, rules and IDs

| Term | Stands for / meaning | Where used |
|---|---|---|
| F-nn | Feature: F-01 Vehicle Passport, F-02 Market Price, F-03 Sell Request & Preliminary Quote. `F-00` = shared platform and access | W0 Table 2 |
| FR | Function, read as functional requirement: `FR-<feature>-<nn>` at L2, with an EARS acceptance criterion | W1 |
| L2 / L3 | Function levels in W1. L2 = a business behaviour the client signs off. L3 = one work item in one layer | W1 |
| FE | Front end: screens and client-side validation | W1, Screen Spec |
| BE | Back end: server logic and storage | W1, Screen Spec |
| INT | Integration: connector to an external system | W1 |
| Work item | One L3 row: one function, one layer, sized S, M or L | W1 |
| EARS | Easy Approach to Requirements Syntax: a sentence pattern for acceptance criteria | W1 |
| WHEN / IF / WHERE / SHALL | EARS keywords. WHEN: an event. IF: an unwanted case or condition. WHERE: an optional feature (for example electric vehicles only). SHALL: the obligation | W1 |
| EARS precise? | W1 column: whether the criterion is precise enough to test; vague criteria cost more verification time | W1, W2 |
| Hidden work? | W1 column: technical work the client does not see (for example credentials, rate limits, aggregation) | W1 |
| BR | Business Rule: `BR-<TOPIC>-nn`, owned by an AD. Topics in use: COM, DIST, ERR, FILE, INV, MIL, OCR, PRI, REF, RET, SOH, VFVR | AD files |
| BR-VFVR-01 | VIN format: 17 uppercase alphanumeric characters, no I, O, Q | AD-UC-1.01 |
| BR-DIST-01 | Minimum travel distance: accumulate distances between consecutive toll stations | AD-UC-1.03 |
| BR-FILE-01 | Invoice file format and size | AD-UC-1.05 |
| BR-OCR-01 | OCR extraction and review | AD-UC-1.05 |
| BR-INV-01 | Invoice authenticity cross-check | AD-UC-1.05 |
| BR-SOH-01 to 05 | SoH rules: grade classification, minimum charging sessions, rounding, calculation frequency, success notification trigger | AD-UC-1.06 |
| BR-REF-01 | Query the three legal-status portals in parallel | AD-UC-1.07 |
| BR-ERR-01 | Partial data fault tolerance: show partial results when one query fails | AD-UC-1.07 |
| BR-MIL-01 | Mileage plausibility threshold, 2,000 km per year | AD-UC-2.04 |
| BR-PRI-01 | Marketplace requests contain only make and model; never VIN, plate or owner data | AD-UC-2.05 |
| BR-RET-01 / 02 | Retry and fallback rules. BR-RET-01 means a VETC retry rule in AD-UC-1.03 and a marketplace retry rule in AD-UC-2.05; BR-RET-02 is the charging data retry rule | AD-UC-1.03, 1.06, 2.05 |
| SR | Screen Rule: `SR-nnn`, numbered inside one Screen Spec; not unique across specs | Screen Spec |
| Retry | The system tries an external call again; after the maximum retries the owner sees a popup with "Thử lại" and "Đóng" | BR-RET, Ui-conventions |
| Timeout | An external system does not answer in time; for a portal it gives status "pending" or "No data" depending on the use case | FR-01-10, FR-03-02 |
| API | Application Programming Interface: how the system calls an external system or its own back end | W1 |
| PDF / XML / JPG / PNG | Invoice file formats accepted (PDF, XML, images); PNG and JPG also name the screen images | FR-01-05, Screen/img |
| HTTP status | Code returned by a partner API, written to the ERROR log on a failed sync | FR-02-02 |
| Database / Vehicle Report database | The single data store of the system; also holds invoice files (ADR-005); shown as `DB` | Architecture, ADR-005 |

## 8. Screens and UI

| Term | Stands for / meaning | Where used |
|---|---|---|
| Screen ID | `S-F<feature>-<nn>`; `S-F00-nn` for screens shared by all features | Screen List |
| Screen List | One row per screen: purpose, entry points, UCs and FRs served, priority | External Design |
| Screen Transition | Diagram of how the user moves between screens | External Design |
| Screen Spec | The specification of one screen: elements, states, interactions, rules, images | Screen/ |
| Element inventory | Table of every element of a screen with its number, type and behaviour. The element names are the component names | Screen Spec section 3 |
| Numbered image | A state image with numbers on the elements (`*-numbered.png`); numbers match the `#` column of the inventory | Screen/img/img-numbered |
| UI | User interface | Ui-conventions |
| UI conventions | Language, formats, components, messages and design tokens of one actor's screens | External Design |
| Design token | A named value (colour, font, spacing, corner) that screens use instead of raw values | Ui-conventions section 6 |
| HEX / `#RRGGBB` | Colour written in hexadecimal, for example `#E4E6EB` | Ui-conventions, Screen Spec |
| CSS px | Pixel unit of the interface; frame width and tap targets are given in px | Ui-conventions |
| Screen state | For example Default, Loading, Empty, Success, Error-API, Error-validate-FE, Error-validate-BE. Each state is named after its mockup frame | Screen Spec, AI Log #4 |
| FE validation / Validate FE | Check in the app before anything is sent; the error shows inline below the field | Ui-conventions section 4 |
| BE error / Validate BE / Error-API | The system or an external service failed after the maximum retries; the error shows as a popup | Ui-conventions section 4 |
| Inline error | Error text right below the field | Ui-conventions |
| Popup | Overlay for a system error or a success message, always with a close button | Ui-conventions |
| Modal | Overlay used for the Upload invoice sheet and for error and success notifications; never for navigation | Ui-conventions, Screen List |
| Banner | Non-blocking message inside the screen (unavailable data, waiting, mileage unverified) | Ui-conventions |
| Empty state | What a list or section shows when there is nothing yet: why, plus the next action | Ui-conventions |
| CTA | Call To Action: the primary button of a screen | Screen Spec |
| Top bar | Bar with back arrow and screen name at the top of every screen except Home | Ui-conventions |
| Section card | A block of the Vehicle Profile with a title and a link "Xem chi tiết" or "Xem tất cả" | Ui-conventions |
| Status badge | Icon plus text for legal status or request status; never colour alone | Ui-conventions |
| Figma frame | A drawn state of a screen in the team's design file; the images in `Screen/img/` are exports | Screen Spec |
| WCAG 2.2 AA | Web Content Accessibility Guidelines level AA: minimum contrast (4.5:1 for body text), non-colour cues and tap target sizes | Ui-conventions section 5 |
| Dates and times | `dd/MM/yyyy` and `HH:mm` (24-hour). `MM` is month, `mm` is minutes | Ui-conventions section 2 |
| VND / ₫ | Vietnamese dong; dots for thousands, symbol after the number: `850.000.000 ₫` | Ui-conventions section 2 |
| triệu | Million (dong); the unit on the 12-week chart axis, for example "850 triệu" | Ui-conventions section 2 |
| "—" (long dash) | Placeholder shown in place of a value that is loading, empty or unavailable | Ui-conventions |
| Price range format | `820.000.000 – 860.000.000 ₫`: en dash, unit once at the end | Ui-conventions section 2 |

## 9. Look-alikes

| Looks like | Is | And is different from |
|---|---|---|
| `StR-nn` | Stakeholder Requirement | `STR` in the context diagram and architecture drawings: Secured Transaction Registry Portal |
| `S-nn` | A sequence diagram file (`S-06`) | `S-X.YY` a message inside it (`S-6.16`); `S-Fxx-nn` a screen (`S-F02-01`) |
| `SD`, `SEQ` | Sequence diagram; the prefix of sequence references in a trace cell | `S-nn` files and `S-X.YY` messages themselves |
| `L2`, `L3` | Function levels in W1 | Lesson 2, Lesson 3 (`L1` to `L8`); Level 0/1/2 of the widening rule |
| `S`, `M`, `L` | Work item sizes (4, 8, 16 hours) | `S-` as a prefix of IDs; `L` for Lesson |
| `FR-02-05` | L2 function "Calculate the confidence" | `FR-02-05-BE`, the L3 work item that belongs to `FR-02-03` (see README section 7) |
| `FE`, `BE` | Front end, back end | "Validate FE" and "Validate BE", names of error states |
| `BR-RET-01` | A retry rule | It names two different rules (VETC in AD-UC-1.03, marketplaces in AD-UC-2.05) |
| `SR-nnn` | Screen Rule inside a Screen Spec | Not unique across specs; always write the screen ID with it |
| `P-n` | CVCA problem | `P25-P75` percentile range; `P` the system node in the context diagram |
| `INV` | Invoice service (Architecture) | Electronic Invoice Lookup Portal (CVCA diagram) |
| `CS` | Carpla Service (diagrams) | "Customer service" cost line in W6 |
| `CT` | Chợ Tốt (sequence diagram) | `CTX`, context flow; `CTA`, call to action |
| `MM` | Month in `dd/MM/yyyy` | `mm`, minutes (as in `HH:mm`) |
| Sale request / sell request | The same thing | Use "sale request" |
| TASCO / Tasco | The same company | Use `TASCO` in system names, `Tasco` in prose |
| Report / Passport | Verified Vehicle Report (product) and Vehicle Passport (feature F-01) | The title of `CVCA.md` still says "Verified Vehicle Passport" |

## 10. Estimation and finance 

| Term | Stands for / meaning | Where used |
|---|---|---|
| MoSCoW | Priority method: Must, Should, Could, Won't | W4 |
| Must / Should / Could / Won't | R1 the release is unusable without it; R2 important but a workaround exists; R3 desirable, low impact if dropped; R4 agreed out of scope (in W0 Table 3) | W4 |
| Initial priority | Priority set in W1 | W1 |
| Effective priority | Priority after W4 rules; the one that counts for scope and cost. Today equal to the initial priority | W4, W2 |
| FP | Function Points: a size measure used only as a cross-check of the hours | W0, W2 |
| ILF | Internal Logical File: data the system stores and maintains itself (10 FP in W0) | W0 Table 4 |
| EIF | External Interface File: data the system only reads from another system (7 FP in W0) | W0 Table 4 |
| EI / EO / EQ | External Input, External Output, External Inquiry: types of transactional function | W1 |
| NC | Not Countable: a function that scores zero FP but carries real work | W1 |
| Data functions / transactional functions | The 103 FP of ILF and EIF; the 109 FP of EI, EO and EQ | W2 |
| Base hours | Hours of a work item before AI and verification factors: S = 4, M = 8, L = 16 | W1, W2 |
| AI leverage | Factor on base hours when AI helps: FE 0.35, BE 0.7, INT 1.0 | W2 |
| Verification factor | Extra share of base hours to check the work: 0.15 when the EARS criterion is precise, 0.4 when vague | W2 |
| Person-hours (p-h) | Base hours × AI leverage + base hours × verification factor | W2 |
| Junior / Senior / Expert rate | 200.000, 300.000 and 400.000 VND per hour (FE junior; BE and INT senior) | W2, W5 |
| Capacity | 300 person-hours: 5 people × 12 weeks × 5 hours | W5 |
| Committed / Expected / Ceiling | The three scenarios: Must only, Must + Should, Must + Should + Could. W5 calls them Pessimistic, Base, Optimistic. Committed is the Minimum Usable SubseT | W2, W5 |
| Contingency | Money buffer of 10% on the Must set, for estimation error only | W5 |
| tasks.md seed | The Must work items ordered by risk (INT first, then BE, then FE) for DBIZ3 | W5 |
| CAPEX | Capital expenditure: the one-off investment in year 0 | W6 |
| OPEX | Operating expenditure: yearly running cost | W6 |
| EBT | Earnings Before Tax | W6 |
| NCF | Net Cash Flow: net income plus depreciation (and CAPEX in year 0) | W6 |
| NPV | Net Present Value of the cash flows at the discount rate | W6 |
| IRR | Internal Rate of Return: the rate at which NPV is zero | W6 |
| WACC | Weighted Average Cost of Capital: the discount rate used (15% per year) | W6 |
| Payback period | Time until cumulative net cash flow turns positive | W6 |
| Market penetration rate | Share of maximum revenue capacity reached each year | W6 |

## 11. Vietnamese interface terms

The Car Owner interface is Vietnamese only (`Ui-conventions-car-owner.md`). Terms that appear in
the design documents:

| Vietnamese | English meaning |
|---|---|
| Tạo báo cáo xe | Create vehicle report: the primary button of S-F01-01 |
| Gửi yêu cầu bán xe | Submit sale request |
| Xem chi tiết | View details: link on a Section card |
| Xem tất cả | View all: link on a Section card |
| Thử lại | Retry: button in a system error popup |
| Đóng | Close |
| Hủy | Cancel |
| Mẫu xe | Vehicle model |
| Chưa xác minh | Not verified: badge |
| Đã xác thực | Verified: badge |
| Tốt / Trung bình / Thấp | Good / Average / Low: the three SoH grades (80% or more; 65% to below 80%; below 65%) |
| Thế chấp / Đang thế chấp | Mortgage / Currently mortgaged |
| Phạt nguội | Traffic fine issued after the fact from camera or record evidence, not on the spot; the label of the unpaid-fines status |
| Không có | None: badge value for no fine or no mortgage |
| Tuần 01 | Week 01: label on the x-axis of the 12-week chart |
| bạn | "you" (informal): how the interface addresses the Car Owner |
| Làm mới | Refresh: secondary button that triggers a VETC sync (S-F01-03) or parallel legal status query (S-F01-02) |
| Số km đã đi | Verified minimum mileage: section card header on S-F01-02 and S-F01-03 |
| Lịch sử qua trạm VETC | VETC toll passage history: screen title of S-F01-03 |
| Lượt ra / Lượt vào | Exit pass / Entry pass: toll station direction badges on S-F01-03 |
| Đồng bộ gần nhất | Last synced: prefix of the VETC synchronization timestamp on S-F01-03 |
| Tải hóa đơn lên | Upload invoice: secondary button on S-F01-02 that opens the S-F01-04 upload sheet |
| Đăng kiểm | Vehicle inspection: legal status label on S-F01-02 |
| Còn hạn / Hết hạn | Valid / Expired: vehicle inspection status badges on S-F01-02 |
| Màu sơn | Paint color: vehicle identification field on S-F01-02 |

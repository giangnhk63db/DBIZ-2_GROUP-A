

| Field | Value |
| ----- | ----- |
| Version | V3 Reviewed |
| Inputs | [CVCA Before](https://github.com/giangnhk63db/DBIZ-2_GROUP-A/blob/459cd34ceac5aa22e3f0283c2a89749fac32c8b6/Context/CVCA%20Before.md) [CVCA After](https://github.com/giangnhk63db/DBIZ-2_GROUP-A/blob/459cd34ceac5aa22e3f0283c2a89749fac32c8b6/Context/CVCA%20After.md) [Context Diagram](https://github.com/giangnhk63db/DBIZ-2_GROUP-A/blob/459cd34ceac5aa22e3f0283c2a89749fac32c8b6/Context/Context%20Diagram.md) [Use Case Diagram](https://github.com/giangnhk63db/DBIZ-2_GROUP-A/tree/459cd34ceac5aa22e3f0283c2a89749fac32c8b6/Use%20Case%20Diagram) |

**1\. System requirements**

| ID | Stakeholder | Stakeholder requirement | CVCA flow |
| ----- | ----- | ----- | ----- |
| StR-01 | Car Owner | The Car Owner wants to create a verified vehicle report for their car and send a sell request. | “Create verified vehicle report & Sell request” |
| StR-02 | Car Owner | The Car Owner wants the report to contain trustworthy data about the car: identification, toll passage history, maintenance and repair history, battery condition and legal status. | VETC, Carpla Service, charging stations, portals → TASCO MOBILITY |
| StR-03 | Car Owner | The Car Owner wants to add maintenance and repair invoices from outside the Tasco system to the vehicle record. | “External invoice”  |
| StR-04 | Car Owner | The Car Owner wants to know a market price reference for their car. | Nextgen Việt Nam, Chợ Tốt → TASCO MOBILITY; “market price” → Car Owner |
| StR-05 | Car Owner | The Car Owner wants the selling form to be filled in automatically. | “Automatically filled selling form” |
| StR-06 | Car Owner | The Car Owner wants a more accurate preliminary quote. | “More accurate preliminary quote” |
| StR-07 | TASCO MOBILITY | TASCO MOBILITY wants the mileage declared by the Car Owner to be consistent with the toll passage data from VETC. | VETC → TASCO MOBILITY |

| ID | System requirement | From |
| ----- | ----- | ----- |
| SyR-01 | The system shall let a Car Owner create a vehicle record by entering the 17-character VIN. | StR-01 |
| SyR-02 | The system shall retrieve the identification data of the vehicle from VETC and show it to the Car Owner. | StR-02 |
| SyR-03 | The system shall retrieve the toll passage history from VETC and show the verified minimum mileage. | StR-02 |
| SyR-04 | The system shall retrieve the maintenance and repair history from Carpla Service and show it to the Car Owner. | StR-02 |
| SyR-05 | The system shall retrieve charging session data and show the battery condition, for electric vehicles only. | StR-02 |
| SyR-06 | The system shall let a Car Owner upload external maintenance and repair invoices. | StR-03 |
| SyR-07 | The system shall extract the key data from each uploaded invoice. | StR-03 |
| SyR-08 | The system shall check the authenticity of each invoice through the Electronic Invoice Lookup Portal and add only valid invoices to the maintenance history. | StR-02, StR-03 |
| SyR-09 | The system shall let a Car Owner refresh the legal status of the vehicle. | StR-02 |
| SyR-10 | The system shall retrieve the inspection status from the Vehicle Inspection Portal. | StR-02 |
| SyR-11 | The system shall retrieve the traffic fine status from the Traffic Fine Lookup Portal. | StR-02 |
| SyR-12 | The system shall retrieve the mortgage status from the Secured Transaction Registry Portal. | StR-02 |
| SyR-13 | The system shall synchronize used car price listings from Nextgen Việt Nam and Chợ Tốt every week. | StR-04 |
| SyR-14 | The system shall select comparable listings and remove invalid listings before pricing. | StR-04 |
| SyR-15 | The system shall calculate the weekly market price range and a confidence level for each tracked car. | StR-04 |
| SyR-16 | The system shall show the Car Owner the latest market price, its confidence level and the 12-week price trend. | StR-04 |
| SyR-17 | The system shall let a Car Owner stop tracking the market price of a car. | StR-04 |
| SyR-18 | The system shall build the 12-week price history when a car model is tracked for the first time. | StR-04 |
| SyR-19 | The system shall block a sell request when the declared mileage is more than 20% below the minimum mileage from VETC, until the owner declares the mileage again. | StR-07 |
| SyR-20 | The system shall fill the selling form automatically from the vehicle record. | StR-05 |
| SyR-21 | The system shall let a Car Owner submit a sell request, re-check the legal status, and make the verified report and the latest market price available to Carpla staff. | StR-01, StR-05 |
| SyR-22 | The system shall let Carpla staff review the sell request with its verified report and market price. | StR-06 |
| SyR-23 | The system shall let Carpla staff send a preliminary quote for a sell request. | StR-06 |
| SyR-24 | The system shall show the preliminary quote to the Car Owner. | StR-06 |

**2\. Functional design**

| Function | Serves |
| ----- | ----- |
| Create the vehicle record from a VIN | SyR-01 |
| Get identification data, toll passage history and verified minimum mileage from VETC | SyR-02, SyR-03 |
| Get maintenance and repair history from Carpla Service | SyR-04 |
| Calculate the battery condition from charging data | SyR-05 |
| Upload and keep external invoice files | SyR-06 |
| Extract data from invoices | SyR-07 |
| Verify invoice authenticity and merge valid invoices into the history | SyR-08 |
| Refresh legal status from the three portals | SyR-09, SyR-10, SyR-11, SyR-12 |
| Synchronize listings every week | SyR-13 |
| Select comparable listings and clean invalid listings | SyR-14 |
| Calculate the weekly market price and confidence level | SyR-15 |
| Show market price, confidence level and trend | SyR-16 |
| Stop price tracking | SyR-17 |
| Build the 12-week price history | SyR-18 |
| Check the declared mileage against VETC | SyR-19 |
| Fill the selling form automatically | SyR-20 |
| Submit a sell request, re-check legal status, package the report and price | SyR-21 |
| Show the sell request to Carpla staff | SyR-22 |
| Store the preliminary quote and show it to the Car Owner | SyR-23, SyR-24 |
| Start weekly jobs on time | SyR-13, SyR-15 |
| Send notifications | SyR-21, SyR-23 |
| Call external data sources | SyR-02, 03, 04, 05, 08, 10, 11, 12, 13 |

**3\. Physical design**

flowchart TB

 subgraph P\["Presentation layer"\]

   direction LR

   APP\["TASCO Mobility app **(**Verified Vehicle Report**)**: Car Owner screens"\]

   STAFF\["Carpla CRM **(**Carpla staff screens entering preliminary price**)**: Carpla staff screens"\]

 end

 subgraph B\["Business layer"\]

   direction LR

   VR\["Vehicle report service"\]

   INV\["Invoice service"\]

   LEG\["Legal status update service"\]

   MP\["Market price service"\]

   SELL\["Sell request service"\]

   SCH\["Scheduler"\]

   NOT\["Notification service"\]

 end

 subgraph D\["Data access layer"\]

   direction LR

   DB\[**(**"Vehicle Report database"**)**\]

 end

 subgraph X\["External systems and Tasco group systems"\]

   direction LR

   VETC\["VETC"\]

   CS\["Carpla Service"\]

   EVC\["EV charging stations"\]

   INSP\["Vehicle Inspection Portal"\]

   FINE\["Traffic Fine Lookup Portal"\]

   REG\["Secured Transaction Registry Portal"\]

   EINV\["Electronic Invoice Lookup Portal"\]

   NEXT\["Nextgen Việt Nam"\]

   CHO\["Chợ Tốt"\]

 end

 P \--\> B

 B \--\> D

 SCH \--\> MP

 INV \--\> VR

 VR \--\> LEG

 SELL \--\> LEG

 SELL \--\> NOT

 SELL \--\> VR

 SELL \--\> MP

 VR \-.\-\> VETC

 VR \-.\-\> CS

 VR \-.\-\> EVC

 INV \-.\-\> EINV

 LEG \-.\-\> INSP

 LEG \-.\-\> FINE

 LEG \-.\-\> REG

 MP \-.\-\> NEXT

 MP \-.\-\> CHO

Solid arrows run from layer to layer; dotted arrows go to external systems. Inside the business layer, the Scheduler starts the Market price service every week, the Invoice service merges valid invoices into the vehicle record, and the Sell request service asks the Legal status service to re-check and sends messages through the Notification service. Each service calls its own external systems directly.

| Function | Element | Layer |
| ----- | ----- | ----- |
| Create the vehicle record from a VIN | Vehicle report service | Business |
| Get identification data, toll passage history and verified minimum mileage from VETC | Vehicle report service, calls VETC | Business, external |
| Get maintenance and repair history from Carpla Service | Vehicle report service, calls Carpla Service | Business, external |
| Calculate the battery condition from charging data | Vehicle report service, calls EV charging stations | Business, external |
| Upload and keep external invoice files | Invoice service, Vehicle Report database | Business, Data access |
| Extract data from invoices | Invoice service | Business |
| Verify invoice authenticity and merge valid invoices into the history | Invoice service, calls Electronic Invoice Lookup Portal, merges through Vehicle report service | Business, external |
| Refresh legal status from the three portals | Vehicle report service starts the refresh, Legal status update service calls Vehicle Inspection Portal, Traffic Fine Lookup Portal and Secured Transaction Registry Portal | Business, external |
| Synchronize listings every week | Market price service, calls Nextgen Việt Nam and Chợ Tốt | Business, external |
| Select comparable listings and clean invalid listings | Market price service | Business |
| Calculate the weekly market price and confidence level | Market price service | Business |
| Show market price, confidence level and trend | Market price service | Business |
| Stop price tracking | Market price service | Business |
| Build the 12-week price history | Market price service | Business |
| Check the declared mileage against VETC | Sell request service, reads from Vehicle report service | Business |
| Fill the selling form automatically | Sell request service, reads from Vehicle report service | Business |
| Submit a sell request, re-check legal status, package the report and price | Sell request service, calls Legal status update service, reads from Vehicle report service and Market price service | Business |
| Show the sell request to Carpla staff | Carpla CRM screens (ADR-003) | Presentation |
| Store the preliminary quote and show it to the Car Owner | Sell request service, Carpla CRM screens (ADR-003) | Business, Presentation |
| Start weekly jobs on time | Scheduler starts Market price service (ADR-002) | Business |
| Send notifications | Notification service | Business |
| Keep vehicle records, invoices and invoice files, legal status, listings, price records, sell requests, quotes | Vehicle Report database | Data access |

**4\. Decisions**

| ADR | Decision | Status |
| ----- | ----- | ----- |
| ADR-001 | Build the feature inside the existing TASCO Mobility app, no separate app | Accepted |
| ADR-002 | Add a scheduler for the weekly listing synchronization and price calculation | Accepted |
| ADR-003 | Carpla staff review the sell request and enter the preliminary quote in Carpla’s existing CRM (Customer Relationship Management), no new staff website in this system | Accepted |
| ADR-004 | Each service calls its own external systems directly, with no shared connector | Proposed |
| ADR-005 | Keep invoice files in the Vehicle Report database | Accepted |
| ADR-006 | Put the legal status check in its own service, used by both the Vehicle report service and the Sell request service | Accepted |


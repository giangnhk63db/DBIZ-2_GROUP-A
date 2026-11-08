# 

| Field | Value |
| ----- | ----- |
| Version | v2 |
| Inputs | CVCA (customer value chain analysis, stakeholder requirements), Context Diagram, Use Case Diagram |

Abbreviations: StR \= stakeholder requirement; SyR \= system requirement; VIN \= vehicle identification number; VETC \= Tasco's electronic toll collection company; CRM \= customer relationship management; API \= application programming interface; EV \= electric vehicle; SoH \= state of health (of the battery).

## **1\. System requirements**

| ID | System requirement | From |
| ----- | ----- | ----- |
| SyR01 | The system shall let a car owner create a vehicle record by entering the vehicle's VIN. | Goal 1 |
| SyR02 | The system shall collect the vehicle's data (mileage from VETC toll passage history, maintenance and repair history from Carpla Service, battery data from EV charging stations, and inspection, unpaid fine and mortgage status from government portals) and show it to the owner as one verified vehicle report. | Goal 1, Goal 3 |
| SyR03 | The system shall let a car owner upload external repair or maintenance invoices to add to the report. | Goal 1 |
| SyR04 | The system shall show the owner a market price with a clear basis and its trend. | Goal 2 |
| SyR05 | The system shall keep the listing and price history for market prices up to date, including synchronizing 180-day listing history for a car model not yet stored and backfilling 12-week price history. | Goal 2 |
| SyR06 | The system shall pre-fill the basic fields of the Carpla sale request form from the report, so the owner only enters the phone number and the desired price. | Goal 4 |
| SyR07 | The system shall send the sale request together with the verified vehicle report to Carpla, retrying up to 3 times with a 5-second wait if sending fails. | Goal 3, Goal 5 |
| SyR08 | The system shall let Carpla staff view the verified vehicle report with the sale request and return the preliminary quote. | Goal 3, Goal 5 |
| SyR09 | When a government portal is unavailable, the system shall still send the report and label the legal data as not updated, with the date it was last retrieved. | Goal 3 |
| SyR10 | The system shall let Data Infrastructure Staff monitor data source connection status, receive failure alerts, view data source issues and update their resolution status. | Goal 3 |
| SyR11 | The system shall let a car owner stop tracking a vehicle. | Goal 1 |
| SyR12 | The system shall allow only logged-in users to use the feature. | all |

The detailed, testable version of each requirement is an EARS row in W1. In the "From" column, replace "Goal n" with the StR ID of the CVCA.

## **2\. Functional design**

The system requirements are divided into subordinate functions.

| Function | Serves |
| ----- | ----- |
| Identify the user and the role | SyR12 |
| Create and list vehicle records from a VIN | SyR01, SyR11 |
| Collect vehicle data from VETC, Carpla Service, EV charging stations and government portals | SyR02, SyR09 |
| Save and merge owner-uploaded invoices | SyR03 |
| Build the verified vehicle report | SyR02, SyR03, SyR09 |
| Compute and show market price and trend | SyR04 |
| Synchronize listing history and backfill price history on schedule | SyR05 |
| Pre-fill the sale request form | SyR06 |
| Package the report and send the sale request to Carpla, with retry | SyR07, SyR08, SyR09 |
| Monitor data sources and record data issues | SyR10 |
| Send notifications (failure alerts, request status) | SyR07, SyR10 |

## **3\. Physical design**
```mermaid
flowchart TB
  subgraph PRES["Presentation layer"]
    APP["TASCO Mobility app: Verified Vehicle Report screens"]
    STAFFWEB["Staff web: data source monitoring screens"]
  end
  subgraph BUS["Business layer"]
    VEH["Vehicle service"]
    CONN["Data collection service"]
    RPT["Report service"]
    MKT["Market price service"]
    SALE["Sale request service"]
    MON["Data source monitoring service"]
    SCH["Scheduler"]
    NOTI["Notification adapter"]
  end
  subgraph DATA["Data access layer"]
    DB[("Verified Vehicle Report database")]
  end

  APP --> VEH
  APP --> RPT
  APP --> MKT
  APP --> SALE
  STAFFWEB --> MON
  VEH --> DB
  CONN --> DB
  RPT --> DB
  MKT --> DB
  SALE --> DB
  MON --> DB
  SCH --> CONN
  SCH --> MKT
  MON --> NOTI
  SALE --> NOTI

  ACC["TASCO Mobility account service"]
  VETC["VETC"]
  CSV["Carpla Service"]
  EVS["EV charging stations"]
  GOV["Government portals"]
  LIST["Used car marketplace data APIs"]
  CRM["Carpla CRM"]
  MAIL["Notification channel"]

  APP -.-> ACC
  CONN -.-> VETC
  CONN -.-> CSV
  CONN -.-> EVS
  CONN -.-> GOV
  MKT -.-> LIST
  SALE -.-> CRM
  NOTI -.-> MAIL
  ```

| Function | Element | Layer |
| ----- | ----- | ----- |
| Identify the user and the role | TASCO Mobility app with TASCO Mobility account service (ADR-002) | Presentation, external |
| Create and list vehicle records from a VIN | Vehicle service | Business |
| Collect vehicle data from VETC, Carpla Service, EV charging stations and government portals | Data collection service with the external data sources (ADR-005) | Business, external |
| Save and merge owner-uploaded invoices | Report service | Business |
| Build the verified vehicle report | Report service | Business |
| Compute and show market price and trend | Market price service | Business |
| Synchronize listing history and backfill price history on schedule | Scheduler starts Market price service jobs (ADR-006) | Business |
| Pre-fill the sale request form | Sale request service | Business |
| Package the report and send the sale request to Carpla, with retry | Sale request service with Carpla CRM (ADR-003) | Business, external |
| Monitor data sources and record data issues | Data source monitoring service, Staff web | Presentation, Business |
| Send notifications | Notification adapter (ADR-007) | Business, external |
| Keep vehicle records, reports, invoices, price and listing history, sale requests, data issues | Verified Vehicle Report database | Data access |

## **4\. Decisions**

| ADR | Decision | Status |
| ----- | ----- | ----- |
| ADR-001 | Deliver Verified Vehicle Report as a feature inside the TASCO Mobility app, not as a separate app | Accepted |
| ADR-002 | Sign in with the existing TASCO Mobility account; the feature keeps no passwords of its own | Proposed |
| ADR-003 | Send the sale request and report to Carpla's existing CRM, with no new screens for Carpla staff, if Tasco confirms the CRM can receive the package and return the quote | Proposed |
| ADR-004 | Do not calculate the car's price in the report; the report supports Carpla staff, who give the preliminary quote | Accepted |
| ADR-005 | Obtain external vehicle and legal data through each source's own channel; when a government portal fails, still send the report with the legal data labelled as not updated | Accepted |
| ADR-006 | Add a scheduler for timed jobs (listing synchronization, price history backfill, data refresh) | Proposed |
| ADR-007 | Send all notifications through one Notification adapter | Proposed |
| ADR-008 | Buy market price and listing data through the APIs of online used car marketplaces; no web crawler | Accepted |

## **Change log**

| Version | When | Change | Why |
| ----- | ----- | ----- | ----- |
| v1 | Lesson 4 | TASCO Mobility app, Staff web, Vehicle service, Data collection service, Report service, Market price service, Sale request service, Data source monitoring service, Notification adapter, Scheduler, database | First version from the CVCA, Context Diagram and Use Case Diagram |


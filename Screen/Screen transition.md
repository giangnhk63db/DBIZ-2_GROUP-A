**Screen Transition**

1. **Car owner**  
```mermaid
---
config:
  layout: elk
  elk:
    mergeEdges: false
    nodePlacementStrategy: NETWORK_SIMPLEX
  flowchart:
    nodeSpacing: 120
    rankSpacing: 260
    padding: 50
---
flowchart LR
  entry(["Entry: Car owner signed in<br>to Tasco Mobility"])
  entry_quote(["Entry: Carpla staff<br>sent quote"])

  S00_01["S-F00-01<br>Home"]

  S01_01["S-F01-01<br>Create Vehicle Report<br>(Enter VIN)"]
  S01_02["S-F01-02<br>Vehicle Profile"]
  S01_03["S-F01-03<br>Verified Minimum Mileage"]
  S01_05["S-F01-05<br>Maintenance History"]
  S01_06["S-F01-06<br>Battery Health (SoH)"]

  S02_01["S-F02-01<br>Market Price &<br>12-Week Trend"]

  S03_01["S-F03-01<br>Submit Sale Request"]
  S03_03["S-F03-03<br>Sale Request &<br>Preliminary Quote"]

  entry --> S00_01

  S00_01 -->|"clicks Create Verified<br>Vehicle Report"| S01_01
  S00_01 -->|"selects an<br>existing vehicle"| S01_02
  S01_01 -->|"[VIN accepted,<br>profile created]"| S01_02
  S01_01 -.->|"back"| S00_01
  S01_02 -.->|"back"| S00_01

  S01_02 -->|"clicks View details<br>(Minimum mileage)"| S01_03
  S01_03 -.->|"back"| S01_02

  S01_02 -->|"clicks View all<br>(Maintenance and repair history)"| S01_05
  S01_05 -->|"uploads invoice<br>[timeline updated]"| S01_05
  S01_05 -.->|"back"| S01_02

  S01_02 -->|"clicks View all<br>(Battery health)"| S01_06
  S01_06 -.->|"back"| S01_02

  S01_02 -->|"clicks Refresh<br>[legal status updated]"| S01_02

  S01_02 -->|"clicks View trend"| S02_01
  S02_01 -.->|"back"| S01_02

  S01_02 -->|"clicks Submit<br>sale request"| S03_01
  S03_01 -.->|"back"| S01_02
  S03_01 -->|"enters current mileage, desired price,<br>phone number, clicks Submit<br>[sale request saved]"| S03_03

  entry_quote -->|"[quote sent]"| S03_03
  S03_03 -.->|"back<br>(after Submit)"| S01_02
  S03_03 -.->|"back<br>(from notification)"| S00_01

  classDef screen fill:#f2c9c9,stroke:#8b1a10,stroke-width:2px,color:#000;
  classDef entryNode fill:#fdf0b8,stroke:#c9a400,stroke-width:2px,color:#000;
  class S00_01,S01_01,S01_02,S01_03,S01_05,S01_06,S02_01,S03_01,S03_03 screen;
  class entry,entry_quote entryNode;
 ```    
2. **Carpla staff**  
```mermaid  
---
config:
  layout: elk
  flowchart:
    nodeSpacing: 110
    rankSpacing: 220
    padding: 40
---
flowchart LR
  entry_staff(["Entry: Carpla CRM receives sale request<br>with the verified vehicle report"])
  S03_02["S-F03-02<br>Sale Request Review & Quote"]
  exit_staff(["Exit: Carpla CRM"])

  entry_staff -->|"Opens the sale request<br>(owner-declared info + verified vehicle report)"| S03_02
  S03_02 -->|"Enters suggested quote range<br>and sends it to car owner"| exit_staff
  S03_02 -.->|"back"| exit_staff

  classDef screen fill:#f2c9c9,stroke:#8b1a10,stroke-width:2px,color:#000;
  classDef entryNode fill:#fdf0b8,stroke:#c9a400,stroke-width:2px,color:#000;
  class S03_02 screen;
  class entry_staff,exit_staff entryNode;
  
```   

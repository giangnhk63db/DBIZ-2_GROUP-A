**AD-UC-2.05 Sync Listings from Partner Marketplaces**

|Field|Value|
|-|-|
|AD ID|`AD-UC-2.05`|
|Use case|`UC-2.05 Sync Listings from Partner Marketplaces`|
|Feature|`F-02 Market Price`|
|Primary actor|none: started by time (Weekly timer, through UC-2.04)|
|Lanes|TASCO Mobility System · Nextgen Việt Nam · Chợ Tốt - Carousell Group|
|Inputs (L3-L5)|Context Diagram (flow-15, flow-16) · Use Case Diagram UC - 02· Sequence diagram 06, S-6.02 to S-6.10 · W1 rows FR-02-02 (INT1, INT2, BE2)|
|Trigger|UC-2.04 starts the weekly run and calls the synchronization|
|Preconditions|TASCO Mobility has access credentials for both marketplace APIs; the weekly run has started|
|Success end state|Listings of both marketplaces are stored with source and sync time; the valid source set of the week contains both|
|Failure end state(s)|Only one marketplace synced: its listings are kept and the sync is flagged partial. No marketplace synced: API\_FAILED result is returned to UC-2.04|
|FRs covered|`FR-02-02-INT1`, `FR-02-02-INT2`, `FR-02-02-BE2`|
|Business rules applied|BR-RET-01, BR-PRI-01|
|Version / status|v0.3 · Review|
|Author|Vũ Hương Ly|

## **1. Diagram**

```mermaid 

swimlane-beta TB
  subgraph SYS["TASCO Mobility"]
    start@{ shape: sm-circ }
    f1@{ shape: fork }
    mN{" "}
    rN("Request incremental sync from Nextgen Việt Nam, make and model only (FR-02-02-INT1)")
    dN{"Nextgen Việt Nam synced?"}
    kN("Store Nextgen Việt Nam listings with source and sync time (FR-02-02-BE2)")
    qN{"Retry limit reached?"}
    wN@{ shape: hourglass }
    eN("Log ERROR with HTTP status for Nextgen Việt Nam (FR-02-02-INT1)")
    mjN{" "}
    mC{" "}
    rC("Request incremental sync from Chợ Tốt, make and model only (FR-02-02-INT2)")
    dC{"Chợ Tốt synced?"}
    kC("Store Chợ Tốt listings with source and sync time (FR-02-02-BE2)")
    qC{"Retry limit reached?"}
    wC@{ shape: hourglass }
    eC("Log ERROR with HTTP status for Chợ Tốt (FR-02-02-INT2)")
    mjC{" "}
    j1@{ shape: join }
    d1{"Which marketplaces synced?"}
    s6("Return API_FAILED result (FR-02-02-INT2)")
    s7("Flag partial sync status (FR-02-02-INT1, FR-02-02-INT2)")
    stop1@{ shape: fr-circ }
  end
  subgraph NGX["Nextgen Việt Nam"]
    lN("Return new, price-updated and removed listings")
  end
  subgraph CT["Chợ Tốt - Carousell Group"]
    lC("Return new, price-updated and removed listings")
  end
  start --> f1
  f1 --> mN --> rN --> dN
  dN -->|"[yes]"| lN --> kN --> mjN
  dN -->|"[no]"| qN
  qN -->|"[no, fewer than 3 retries]"| wN
  wN -->|"6 hours after the failed attempt"| mN
  qN -->|"[yes, 3 retries used]"| eN --> mjN
  mjN --> j1
  f1 --> mC --> rC --> dC
  dC -->|"[yes]"| lC --> kC --> mjC
  dC -->|"[no]"| qC
  qC -->|"[no, fewer than 3 retries]"| wC
  wC -->|"6 hours after the failed attempt"| mC
  qC -->|"[yes, 3 retries used]"| eC --> mjC
  mjC --> j1
  j1 --> d1
  d1 -->|"[none]"| s6 --> stop1
  d1 -->|"[only one]"| s7 --> stop1
  d1 -->|"[both]"| stop1


```

Reading notes:

* The two marketplaces are synced independently and in parallel (fork/join). One marketplace giving up after 3 retries does not stop the other.
* "Retry limit reached?" counts retries after the first attempt: 1 first try + up to 3 retries, 6 hours apart.
* UC-2.05 only returns the outcome. When no marketplace succeeded it returns API\_FAILED to UC-2.04, which records the status and sends the alert (see AD-UC-2.04). When only one succeeded, the weekly run goes on with that marketplace's listings and the sync is flagged partial.
* The merges before the join (`mjN`, `mjC`) make each branch deliver exactly one token to the join.
* Only the system lane cites FR IDs. Nextgen Việt Nam and Chợ Tốt - Carousell Group are external systems.

## **2. Node trace**

|Node|Kind|Text|Traces to|
|-|-|-|-|
|rN|Action|Request incremental sync from Nextgen Việt Nam (make and model only, no VIN, plate or owner)|FR-02-02-INT1 / SEQ S-6.02 / CTX flow-15|
|dN|Decision|Nextgen Việt Nam synced?|FR-02-02-INT1 / SEQ S-6.02, S-6.04|
|kN|Action|Store Nextgen Việt Nam listings with source and sync time|FR-02-02-BE2 / SEQ S-6.08|
|qN|Decision|Retry limit (3 retries) reached?|FR-02-02-INT1, FR-02-02-BE2 / SEQ S-6.03|
|wN|Time event|Wait 6 hours before the next retry|FR-02-02-INT1, FR-02-02-BE2 / SEQ S-6.03|
|eN|Action|Log ERROR with HTTP status for Nextgen Việt Nam|FR-02-02-INT1|
|rC|Action|Request incremental sync from Chợ Tốt (make and model only)|FR-02-02-INT2 / SEQ S-6.05 / CTX flow-16|
|dC|Decision|Chợ Tốt synced?|FR-02-02-INT2 / SEQ S-6.05, S-6.07|
|kC|Action|Store Chợ Tốt listings with source and sync time|FR-02-02-BE2 / SEQ S-6.08|
|qC|Decision|Retry limit (3 retries) reached?|FR-02-02-INT2, FR-02-02-BE2 / SEQ S-6.06|
|wC|Time event|Wait 6 hours before the next retry|FR-02-02-INT2, FR-02-02-BE2 / SEQ S-6.06|
|eC|Action|Log ERROR with HTTP status for Chợ Tốt|FR-02-02-INT2|
|d1|Decision|Which marketplaces synced? (valid source set of the week)|FR-02-02-INT2 / SEQ S-6.09|
|s6|Action|Return API\_FAILED result (no valid marketplace)|FR-02-02-INT2 / SEQ S-6.10|
|s7|Action|Flag partial sync status (only one marketplace valid)|FR-02-02-INT1, FR-02-02-INT2|

### Note
N: Nextgen Việt Nam

C: Chợ Tốt - Carousell Group

### Business rule description

| Rule ID | Description | Applied Node |
|---|---|---|
| BR-RET-01 | When a marketplace sync fails, the system retries up to 3 times, 6 hours apart. After the 3rd retry fails, it stops retrying that marketplace and logs an ERROR with the HTTP status. | qN, wN, eN, qC, wC, eC |
| BR-PRI-01 | Requests to the marketplaces contain only the vehicle make and model. VIN, license plate and owner information are never sent. | rN, rC |

## **3. Branches and acceptance tests**

|Branch|Given|When|Then|
|-|-|-|-|
|dN \[yes]|Nextgen Việt Nam API is reachable|The system requests the incremental sync with make "Toyota" and model "Vios" only|Nextgen Việt Nam returns its new, price-updated and removed listings; the system stores them with source and sync time|
|dN \[no]|Nextgen Việt Nam API times out on the first attempt|The system checks the result|The flow goes to "Retry limit reached?"|
|qN \[no, fewer than 3 retries]|First attempt failed at 08:00 (0 retries used)|The system checks the retry count|The system waits 6 hours and sends the request again at 14:00|
|qN \[yes, 3 retries used]|Attempts at 08:00, 14:00, 20:00 and 02:00 next day all failed (HTTP 503)|The system checks the retry count|No more retry; the system logs ERROR "503" for Nextgen Việt Nam; this branch ends|
|dC \[yes]|Chợ Tốt API is reachable|The system requests the incremental sync with make and model only|Chợ Tốt returns its listings; the system stores them with source and sync time|
|dC \[no]|Chợ Tốt API times out on the first attempt|The system checks the result|The flow goes to "Retry limit reached?"|
|qC \[no, fewer than 3 retries]|First attempt failed at 08:00 (0 retries used)|The system checks the retry count|The system waits 6 hours and sends the request again at 14:00|
|qC \[yes, 3 retries used]|Attempts at 08:00, 14:00, 20:00 and 02:00 next day all failed (HTTP 503)|The system checks the retry count|No more retry; the system logs ERROR "503" for Chợ Tốt; this branch ends|
|d1 \[none]|Both branches ended with ERROR logs|Both branches reach the join|The system returns the API\_FAILED result to UC-2.04 and the activity ends|
|d1 \[only one]|Nextgen Việt Nam stored its listings; Chợ Tốt ended with an ERROR log|Both branches reach the join|The system flags partial sync status; the weekly run goes on with the Nextgen Việt Nam listings|
|d1 \[both]|Both marketplaces stored their listings|Both branches reach the join|The activity ends; the valid source set has both marketplaces|

## **4. Object states used**

|Object|States in this diagram|Full state list (all diagrams)|
|-|-|-|
|No object node in this diagram (API\_FAILED is returned here and recorded in AD-UC-2.04)|||

## **5. Screens touched**

|Actor action|Screen ID|
|-|-|
|||

## **6. Open questions**

|#|Question|Blocking?|Default applied|
|-|-|-|-|
|1|\[NEEDS CLARIFICATION: who uses the "partial sync" flag? UC-2.04 only checks "at least one synced", and no SEQ message shows the flag]|No|Drawn as an action (s7) because FR-02-02 requires it|
|2|\[NEEDS CLARIFICATION: SEQ diagram 06 names the system "Market Price Service" and the partners "Nextgen VN" and "Cho Tot"; the Context Diagram uses "TASCO Mobility", "Nextgen Việt Nam", "Chợ Tốt - Carousell Group"]|No|Lanes use the Context Diagram names; SEQ labels to be aligned|
|3|\[NEEDS CLARIFICATION: no BR IDs exist for the 3 retries / 6 hours rule]|No|Rule written in the text only|

## **7. Verification log**

|Check|Result|By|Date|
|-|-|-|-|
|Renders on mermaid.live|PASS|Vũ Hương Ly||
|Every decision has guards on all outgoing flows, no overlap, no gap|PASS (dN, qN, dC, qC, d1)|Vũ Hương Ly|2026-11-04|
|No action has two incoming flows (merge used instead)|PASS (mN, mC, mjN, mjC used)|Vũ Hương Ly|2026-11-04|
|Every fork has a matching join|PASS (f1 / j1)|Vũ Hương Ly|2026-11-04|
|Every system action cites an FR|PASS|Vũ Hương Ly|2026-11-04|
|Every FR of this use case appears in the diagram|3 of 3|Vũ Hương Ly|2026-11-04|
|Lane names match the Context Diagram and the use case actors; no Web App, API or database lanes|PASS|Vũ Hương Ly|2026-11-04|
|Related sequence diagram uses the same names and FR IDs; its alt fragments appear here as decisions|PASS except names (open question 2)|Vũ Hương Ly|2026-11-04|
|AI critic prompt run, findings resolved or logged as open questions|Pending|||




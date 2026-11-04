# Activity Diagram: AD-UC-1.07 Refresh Legal Status

| Field | Value |
|---|---|
| AD ID | `AD-UC-1.07` |
| Use case | `UC-1.07 Refresh Legal Status` |
| Feature | `F-01 Vehicle Identification & Status` |
| Primary actor | Car Owner |
| Lanes | Car Owner · Verified Vehicle Report · Vehicle Inspection Portal · Traffic Fine Lookup Portal · Secured Transaction Registry Portal |
| Inputs (L3-L5) | Context Diagram v1 · Use Case Diagram v1 · Sequence: SD-UC-1.07 · W1 rows (FR-01-09-FE, FR-01-10-INT, FR-01-11-INT, FR-01-12-INT, FR-01-13-BE, FR-01-13-FE) |
| Trigger | Car owner clicks "Refresh Legal Status" to update the vehicle's consolidated information |
| Preconditions | User is authenticated and viewing an existing vehicle profile on the system |
| Success end state | Legal status data is successfully refreshed from all three external portals, aggregated, stored, and the consolidated card is displayed to the user |
| Failure end state(s) | One or more external portals timeout or fail; the system logs the errors, aggregates the available partial data, and displays it with specific error indicators |
| FRs covered | `FR-01-09-FE`, `FR-01-10-INT`, `FR-01-11-INT`, `FR-01-12-INT`, `FR-01-13-BE`, `FR-01-13-FE` |
| Business rules applied | `BR-REF-01`, `BR-ERR-01` |
| Version / status | v0.1 - Draft |
| Author | Nguyễn Danh Quang |

## Business rule description

| Rule ID | Description | Applied Node |
|---|---|---|
| `BR-REF-01` | Concurrent Multi-Portal Query Rule: When refreshing the legal status, the system must simultaneously query the three external government portals (Inspection, Traffic Fine, Secured Transaction) in parallel via a fork node to minimize overall latency. | `f1` |
| `BR-ERR-01` | Partial Data Fault Tolerance Rule: During the parallel legal status check, if any individual external portal query fails or times out, the system must independently log the error, bypass that failed component without crashing, and proceed to aggregate whatever data is successfully retrieved. | `d2`, `d3`, `d4` |

## 1. Diagram

```mermaid
swimlane-beta TB
  subgraph CAR["Car Owner"]
    start@{ shape: sm-circ }
    a1("Click Refresh Legal Status")
    a2("View consolidated legal status")
    stop1@{ shape: fr-circ }
    stop2@{ shape: fr-circ }
  end
  
  subgraph SYS["Verified Vehicle Report"]
    d1{"Request already in progress?"}
    c1("Disable duplicate click")
    f1@{ shape: fork }   
    s1("Query Inspection Portal (FR-01-10-INT)")
    s2("Query Traffic Fine Portal (FR-01-11-INT)")
    s3("Query Secured Transaction (FR-01-12-INT)")
    d2{"Inspection response OK?"}
    c2("Log Inspection error")
    d3{"Traffic Fine response OK?"}
    c3("Log Traffic Fine error")
    d4{"Mortgage response OK?"}
    c4("Log Mortgage error")
    j1@{ shape: join }
    s4("Aggregate & store results (FR-01-13-BE)")
    s5("Display consolidated card (FR-01-13-FE)")
  end
  
  subgraph PORTAL1["Vehicle Inspection Portal"]
    e1("Process and return response")
  end
  subgraph PORTAL2["Traffic Fine Lookup Portal"]
    e2("Process and return response")
  end
  subgraph PORTAL3["Secured Transaction Registry Portal"]
    e3("Process and return response")
  end
  
  start --> a1 --> d1
  d1 -->|"Yes"| c1 --> stop2
  d1 -->|"No"| f1
  f1 --> s1 --> e1 --> d2
  f1 --> s2 --> e2 --> d3
  f1 --> s3 --> e3 --> d4
  d2 -->|"Yes"| j1
  d2 -->|"No"| c2 --> j1
  d3 -->|"Yes"| j1
  d3 -->|"No"| c3 --> j1
  d4 -->|"Yes"| j1
  d4 -->|"No"| c4 --> j1
  j1 --> s4 --> s5 --> a2 --> stop1
```

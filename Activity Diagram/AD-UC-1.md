# Activity Diagram: AD-UC-1.01 Enter VIN to initialize Report

| Field | Value |
|---|---|
| AD ID | `AD-UC-1.01` |
| Use case | `UC-1.01 Enter VIN to initialize Report` |
| Feature | `F-01 Vehicle Identification & Status` |
| Primary actor | Car Owner |
| Lanes | Car Owner · Verified Vehicle Report |
| Inputs (L3-L5) | Context Diagram v1 · Use Case Diagram v1 · Sequence: SD-UC-1.01 (Figure 1) · W1 rows (FR-01-01-FE, FR-01-01-BE) |
| Trigger | Car owner selects Create Verified Vehicle Report and enters a 17-character VIN |
| Preconditions | User is on the system home screen and is authenticated |
| Success end state | 17-character VIN is successfully validated, database existence is confirmed, and a digital vehicle profile is initialized and displayed |
| Failure end state(s) | VIN initialization is rejected due to invalid character format/length, or non-existent VIN in the database, returning the user to the input state |
| FRs covered | `FR-01-01-FE`, `FR-01-01-BE` |
| Business rules applied | `BR-VFVR-01` |
| Version / status | v0.1 - Draft |
| Author | Nguyễn Danh Quang |

## 1. Diagram

```mermaid
swimlane-beta TB
  subgraph ACT["Car Owner"]
    start@{ shape: sm-circ }
    a1("Select Create Verified Vehicle Report")
    a2("Enter 17-character VIN")
    a3("View validation error")
    a4("Receive profile created confirmation")
    stop1@{ shape: fr-circ }
    stop2@{ shape: fr-circ }
  end
  
  subgraph SYS["Verified Vehicle Report"]
    s1("Client-side format validation (FR-01-01-FE)")
    d1{"Is VIN format valid?"}
    s2("Display format error feedback (FR-01-01-FE)")
    
    s3("Validate checksum logic & check existence (FR-01-01-BE)")
    d2{"Does VIN exist?"}
    s4("Display error 'Số VIN không tồn tại' (FR-01-01-FE)")
    
    s5("Return vehicle profile created state (FR-01-01-FE)")
    m1{" "}
  end

  start --> a1 --> a2 --> s1 --> d1
  
  d1 -->|"[No]"| s2 --> m1
  d1 -->|"[Yes]"| s3 --> d2
  
  d2 -->|"[No]"| s4 --> m1
  d2 -->|"[Yes]"| s5 --> a4 --> stop2
  
  m1 --> a3 --> stop1
```

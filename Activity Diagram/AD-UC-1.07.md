**Activity Diagram: AD-UC-1.07 Refresh Legal Status**

*1\. Diagram*
```mermaid
swimlane-beta TB
  subgraph CAR["Car Owner"]
    start@{ shape: sm-circ }
    a1("Click Refresh Legal Status")
    a2("View consolidated legal status")
    stop1@{ shape: fr-circ }
    stop2@{ shape: fr-circ }
  end
  subgraph SYS["TASCO Mobility System"]
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

**Reading notes:**

- The workflow strictky prevents duplicate user actions at the UI level (FR-01-09-FE) before initiating any external queries.
- A fork node (f1) is utilized to dispatch three parallel queries to independent government portals to optimize data retrieval time.
- External portals act as black boxes (Process and return response). The system independently evaluates each response (d2, d3, d4) to determine success or failure.
- Fault tolerance is integrated by logging individual API failures (c2, c3, c4) without interrupting the overall process.
- All paths synchronize at the join node (j1), allowing the system to aggregate and store the consolidated results (FR-01-13-BE) and display partial results even if some queries fail.

**2\. Node trace**

| Node | Kind | Text | Traces to |
| ----- | ----- | ----- | ----- |
| a1 | Action | Click Refresh Legal Status | FR-01-09-FE |
| d1 | Decision | Request already in progress? | FR-01-09-FE |
| c1 | Action | Disable duplicate click | FR-01-09-FE |
| s1 | Action | Query Inspection Portal | FR-01-10-INT |
| s2 | Action | Query Traffic Fine Portal | FR-01-11-INT |
| s3 | Action | Query Secured Transaction | FR-01-12-INT |
| e1 | Action | Process and return response | N/A |
| e2 | Action | Process and return response | N/A |
| e3 | Action | Process and return response | N/A |
| d2 | Decision | Inspection response OK? | FR-01-10-INT |
| c2 | Action | Log Inspection error | FR-01-10-INT |
| d3 | Decision | Traffic Fine response OK? | FR-01-11-INT |
| c3 | Action | Log Traffic Fine error | FR-01-11-INT |
| d4 | Decision | Mortgage response OK? | FR-01-12-INT |
| c4 | Action | Log Mortgage error | FR-01-12-INT |
| s4 | Action | Aggregate & store results | FR-01-13-BE |
| s5 | Action | Display consolidated card | FR-01-13-FE |
| a2 | Action | View consolidated legal status | N/A |

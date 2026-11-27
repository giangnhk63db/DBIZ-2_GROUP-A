# Usage Flow: Carpla CRM

| Field | Value |
| ----- | ----- |
| Product | Carpla CRM |
| Version | v1|
| Date | 2026-11-18 |
| Sources | W0 Table 1 (use case inventory); Use Case Diagram; Activity Diagram Index |

## 1. Carpla Staff: Process sale request and send quote

``` mermaid 
flowchart LR
  A(["Receives a new vehicle sale request"]) --> U307["UC-3.07: Review Verified Report"]
  U307 --> U308["UC-3.08: Send Preliminary Quote to Car Owner"]
  U308 --> Z(["Quote sent to Car Owner"])
```

| Step | Use case | Activity diagram | Screens (fill in after Block 2) |
| ----- | ----- | ----- | ----- |
| 1 | UC-3.07: Review Verified Report | not needed | S-F03-02, S-F03-03 |
| 2 | UC-3.08: Send Preliminary Quote to Car Owner | not needed | S-F03-03  |

## Use cases without a flow
N/A


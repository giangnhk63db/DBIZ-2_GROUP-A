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
**Reading notes:**\
\
* Validation Priority: The workflow strictly prioritizes client-side format validation (FR-01-01-FE) as the primary gatekeeper to filter out invalid inputs before initiating any backend database interactions.\
* Backend Verification: A secondary backend validation (FR-01-01-BE) verifies the checksum logic and actual database existence. If the VIN does not exist, the system actively triggers a specific error message "VIN does not exist".\
* Control Flow Integrity: To adhere to UML structural integrity, both validation failure paths (format error and non-existent VIN) safely converge into a single control merge node (`m1`) before returning the validation error to the user.\
\
## **2\. Node trace**\
\
| Node | Kind | Text | Traces to |\
| :---- | :---- | :---- | :---- |\
| s1 | Action | Client-side format validation | FR-01-01-FE |\
| d1 | Decision | Is VIN format valid? | FR-01-01-FE / BR-VFVR-01 |\
| s2 | Action | Display format error feedback | FR-01-01-FE |\
| s3 | Action | Validate checksum logic & check existence | FR-01-01-BE |\
| d2 | Decision | Does VIN exist? | FR-01-01-BE |\
| s4 | Action | Display error 'VIN does not exist' | FR-01-01-FE |\
| s5 | Action | Return vehicle profile created state | FR-01-01-FE |\
| m1 | Merge | Control node merging format validation failure and VIN existence failure paths | N/A (Control Node) |\
\
## \
\
**Business rule description**\
\
| Rule ID | Description | Applied Node |\
| :--- | :--- | :--- |\
| BR-VFVR-01 | VIN Format Validation Rule: The Vehicle Identification Number (VIN) submitted by the user must contain exactly 17 alphanumeric characters, strictly excluding the letters I, O, and Q (to prevent visual confusion with numbers 1 and 0). If the input violates this mandatory format length or character restriction, the system must immediately reject the submission and display a format error feedback. | d1 |\
\
## **3\. Branches and acceptance tests**\
\
| Branch | Given | When | Then |\
| :---- | :---- | :---- | :---- |\
| d1 [No] | User is on the Create Report screen | User enters a VIN that is not exactly 17 characters (e.g., "ABC1234") or contains invalid letters (I, O, Q) | System blocks the request, prevents backend call, and displays a format error message |\
| d1 [Yes] | User is on the Create Report screen | User enters a correctly formatted 17-character VIN (e.g., "VF123456789012345") | System accepts the input and proceeds to backend database verification |\
| d2 [No] | A valid 17-character VIN has been submitted to the backend | The system queries the database and finds no existing vehicle record matching the VIN | System aborts initialization and displays the specific error "VIN does not exist" |\
| d2 [Yes] | A valid 17-character VIN has been submitted to the backend | The system queries the database and successfully retrieves the matching vehicle record | System initializes the digital vehicle profile and displays the created state confirmation |\
\
## \
\
## **4\. Object states used**\
\
| Object | States in this diagram | Full state list (all diagrams) |\
| :---- | :---- | :---- |\
| Verified Vehicle Report | Initialized | Initialized, Validating, Completed, Expired |\
\
## \
\
## **5\. Screens touched**\
\
| Actor action | Screen ID |\
| :---- | :---- |\
|  |  |\
\
## \
\
## **6\. Open questions**\
\
| # | Question | Blocking? | Default applied |\
| :---- | :---- | :---- | :---- |\
| 1 | What is the expected system behavior if the backend database connection times out or fails during the VIN existence check? | No | Display a generic system timeout error and return the user to the input state without crashing. |\
| 2 | Should there be a rate limit or a maximum number of retries if the user repeatedly enters invalid or non-existent VINs to prevent brute-force attacks? | No | No rate limit is applied at the application level; assuming infrastructure/WAF handles basic rate limiting. |\
\
## \
\
## **7\. Verification log**\
\
| Check | Result | By | Date |\
| :---- | :---- | :---- | :---- |\
| Renders on mermaid.live | Pass | Nguyễn Danh Quang | 2026-11-04 |\
| Every decision has guards on all outgoing flows, no overlap, no gap | Pass | Nguyễn Danh Quang | 2026-11-04 |\
| No action has two incoming flows (merge used instead) | Pass | Nguyễn Danh Quang | 2026-11-04 |\
| Every fork has a matching join | N/A (no forks) | Nguyễn Danh Quang | 2026-11-04 |\
| Every system action cites an FR | Pass | Nguyễn Danh Quang | 2026-11-04 |\
| Every FR of this use case appears in the diagram | 2 of 2 | Nguyễn Danh Quang | 2026-11-04 |\
| Lane names match the Context Diagram and the use case actors; no Web App, API or database lanes | Pass | Nguyễn Danh Quang | 2026-11-04 |\
| Related sequence diagram uses the same names and FR IDs; its alt fragments appear here as decisions | Pass | Nguyễn Danh Quang | 2026-11-04 |\
| AI critic prompt run, findings resolved or logged as open questions | Pass | Nguyễn Danh Quang | 2026-11-04 |\
\
#

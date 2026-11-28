# Activity Diagram: AD-UC-1.01 Enter VIN to initialize Report

| Field | Value |
|---|---|
| AD ID | `AD-UC-1.01` |
| Use case | `UC-1.01 Enter VIN to initialize Report` |
| Feature | `F-01 Vehicle Identification & Status` |
| Primary actor | Car Owner |
| Lanes | Car Owner · TASCO Mobility System |
| Inputs (L3-L5) | [Context Diagram](../context/context-diagram.md) · [Use Case Diagram](../use-case/use-case-diagram.md) · Sequence: [S-01](../sequence/S-01.md) · [W1 Function List](../function-list/w1-function-list.md) (`FR-01-01-FE`, `FR-01-01-BE`) |
| Trigger | Car Owner selects `"Tạo báo cáo xe"` and enters a 17-character VIN |
| Preconditions | User is on the system home screen and is authenticated |
| Success end state | 17-character VIN is successfully validated, backend existence is confirmed, a new `Verified vehicle report` is initialized, and confirmation popup `"Đã tạo báo cáo xe"` is displayed before proceeding to `S-F01-02` |
| Failure end state(s) | VIN initialization is rejected due to invalid character format/length (`Error validate FE`), non-existent VIN (`"Không tìm thấy thông tin xe"`), or database/system connection failure (`"Lỗi kết nối hệ thống"`), returning the user to the input state |
| FRs covered | `FR-01-01-FE`, `FR-01-01-BE` |
| Business rules applied | `BR-VFVR-01` |
| Version / status | v1.0 · Reviewed |
| Author | Nguyễn Danh Quang |

## 1. Diagram

```mermaid
swimlane-beta TB
  subgraph ACT["Car Owner"]
    start@{ shape: sm-circ }
    a1("Select 'Tạo báo cáo xe'")
    a2("Enter 17-character VIN and submit")
    a3("View validation or connection error")
    a4("Dismiss 'Đã tạo báo cáo xe' confirmation popup")
    stop1@{ shape: fr-circ }
    stop2@{ shape: fr-circ }
  end

  subgraph SYS["TASCO Mobility System"]
    s1("Client-side format validation (FR-01-01-FE)")
    d1{"Is VIN format valid (BR-VFVR-01)?"}
    s2("Display inline error: 'VIN cần đủ 17 ký tự gồm chữ và số (không dùng I, O, Q). Vui lòng kiểm tra lại.' (FR-01-01-FE)")
    s3("Validate backend rules & check VIN existence (FR-01-01-BE)")
    d2{"Is database connection available?"}
    s4("Display error popup: 'Lỗi kết nối hệ thống' (FR-01-01-FE)")
    d3{"Does VIN exist?"}
    s5("Display error popup: 'Không tìm thấy thông tin xe' (FR-01-01-FE)")
    s6("Initialize and save Verified vehicle report (FR-01-01-BE)")
    s7("Display confirmation popup: 'Đã tạo báo cáo xe' (FR-01-01-FE)")
    m1{" "}
  end

  start --> a1 --> a2 --> s1 --> d1

  d1 -->|"[No]"| s2 --> m1
  d1 -->|"[Yes]"| s3 --> d2

  d2 -->|"[No]"| s4 --> m1
  d2 -->|"[Yes]"| d3

  d3 -->|"[No]"| s5 --> m1
  d3 -->|"[Yes]"| s6 --> s7 --> a4 --> stop2

  m1 --> a3 --> stop1
```

Reading notes:

- **Black-Box System Boundary:** Following the course MBSE template, `TASCO Mobility System` is modeled as a single black-box lane (no separate Web App, API, or Database swimlanes); internal database queries and persistence steps (`s3`, `s6`) are detailed in Sequence Diagram `S-01`.
- **Validation Priority (`d1`):** Client-side format validation (`FR-01-01-FE`, `BR-VFVR-01`) acts as the primary gatekeeper to filter out invalid inputs before executing backend verification (`FR-01-01-BE`).
- **Control Flow & UI Consistency:** All failure paths (`s2`, `s4`, `s5`) converge through merge node `m1` before returning feedback (`a3`) to the user, and all Vietnamese UI messages match the `S-F01-01` Screen Spec verbatim.

## 2. Node trace

| Node | Kind | Text | Traces to |
|---|---|---|---|
| s1 | Action | Client-side format validation | `FR-01-01-FE` |
| d1 | Decision | Is VIN format valid (`BR-VFVR-01`: exact 17 uppercase alphanumeric characters excluding `I`, `O`, `Q`)? | `FR-01-01-FE` / `BR-VFVR-01` |
| s2 | Action | Display inline error: `"VIN cần đủ 17 ký tự gồm chữ và số (không dùng I, O, Q). Vui lòng kiểm tra lại."` | `FR-01-01-FE` |
| s3 | Action | Validate backend rules & check VIN existence | `FR-01-01-BE` |
| d2 | Decision | Is database connection available? | `FR-01-01-BE` |
| s4 | Action | Display error popup: title `"Lỗi kết nối hệ thống"`, body `"Hệ thống chưa khởi tạo được báo cáo xe từ số VIN này. Vui lòng bấm Thử lại, hoặc đóng và quay lại sau."` | `FR-01-01-FE` |
| d3 | Decision | Does VIN exist? | `FR-01-01-BE` |
| s5 | Action | Display error popup: title `"Không tìm thấy thông tin xe"`, body `"Hệ thống không tìm thấy dữ liệu phương tiện cho số VIN này. Vui lòng kiểm tra lại số VIN và bấm Thử lại, hoặc đóng để quay lại."` | `FR-01-01-FE` |
| s6 | Action | Initialize and save `Verified vehicle report` | `FR-01-01-BE` |
| s7 | Action | Display confirmation popup: title `"Đã tạo báo cáo xe"`, body `"Đã khởi tạo báo cáo xe cho số VIN <vehicle.vin>. Hồ sơ phương tiện đã sẵn sàng."` | `FR-01-01-FE` |
| m1 | Merge | Control node merging format validation failure (`s2`), connection error (`s4`), and VIN existence failure (`s5`) paths | N/A (Control Node) |

## 3. Branches and acceptance tests

| Branch | Given | When | Then |
|---|---|---|---|
| d1 [No] | User is on the Create Report screen (`S-F01-01`) | User enters a VIN that is not exactly 17 characters (e.g., `"RLHJC6101IOQ12"`) or contains invalid letters (`I`, `O`, `Q`) | System blocks the request, prevents backend call, and displays inline error `"VIN cần đủ 17 ký tự gồm chữ và số (không dùng I, O, Q). Vui lòng kiểm tra lại."` |
| d1 [Yes] | User is on the Create Report screen (`S-F01-01`) | User enters a correctly formatted 17-character VIN (e.g., `"RLHJC6101PY000123"`) | System displays loading state `"Đang kiểm tra số VIN và khởi tạo hồ sơ xe. Vui lòng chờ..."` and proceeds to backend verification (`FR-01-01-BE`) |
| d2 [No] | A valid 17-character VIN has been submitted | The database connection times out or fails during backend verification | System aborts initialization and displays error popup `"Lỗi kết nối hệ thống"` (`"Hệ thống chưa khởi tạo được báo cáo xe từ số VIN này. Vui lòng bấm Thử lại, hoặc đóng và quay lại sau."`) |
| d2 [Yes] | A valid 17-character VIN has been submitted | The database connection succeeds | System checks whether a matching vehicle record exists (`d3`) |
| d3 [No] | A valid 17-character VIN has been submitted to the backend (e.g., `"RLHJC6101PY999999"`) | The system finds no existing vehicle record matching the VIN | System aborts initialization and displays error popup `"Không tìm thấy thông tin xe"` (`"Hệ thống không tìm thấy dữ liệu phương tiện cho số VIN này. Vui lòng kiểm tra lại số VIN và bấm Thử lại, hoặc đóng để quay lại."`) |
| d3 [Yes] | A valid 17-character VIN has been submitted to the backend (e.g., `"RLHJC6101PY000123"`) | The system retrieves and confirms the matching vehicle record | System initializes the `Verified vehicle report`, displays confirmation popup `"Đã tạo báo cáo xe"` (`"Đã khởi tạo báo cáo xe cho số VIN RLHJC6101PY000123. Hồ sơ phương tiện đã sẵn sàng."`), and dismissing it opens `S-F01-02` |

## 4. Object states used

| Object | States in this diagram | Full state list (all diagrams) |
|---|---|---|
| Verified Vehicle Report | Initialized | Initialized, Validating, Completed, Expired |

## 5. Screens touched

| Actor action | Screen ID |
|---|---|
| `a1` Select `"Tạo báo cáo xe"` | `S-F00-01` |
| `a2` Enter 17-character VIN and submit | `S-F01-01` |
| `a3` View validation or connection error | `S-F01-01` |
| `a4` Dismiss `"Đã tạo báo cáo xe"` confirmation popup | `S-F01-01` (navigates to `S-F01-02`) |

## 6. Open questions

| # | Question | Blocking? | Default applied |
|---|---|---|---|
| 1 | [NEEDS CLARIFICATION: Should there be a rate limit or a maximum number of retries if the user repeatedly enters invalid or non-existent VINs to prevent brute-force attacks?] | No | No rate limit is applied at the application level; assuming infrastructure/WAF handles basic rate limiting. |
| 2 | [NEEDS CLARIFICATION: What should happen if the Car Owner submits a valid VIN that already has an active `Verified vehicle report`?] | Yes | Redirect directly to the existing `S-F01-02` Vehicle Profile screen instead of creating a duplicate report. |

## 7. Verification log

| Check | Result | By | Date |
|---|---|---|---|
| Renders on mermaid.live | Pass | Nguyễn Danh Quang | 2026-11-28 |
| Every decision has guards on all outgoing flows, no overlap, no gap | Pass | Nguyễn Danh Quang | 2026-11-28 |
| No action has two incoming flows (merge used instead) | Pass | Nguyễn Danh Quang | 2026-11-28 |
| Every fork has a matching join | N/A (no forks) | Nguyễn Danh Quang | 2026-11-28 |
| Every system action cites an FR | Pass | Nguyễn Danh Quang | 2026-11-28 |
| Every FR of this use case appears in the diagram | 2 of 2 (`FR-01-01-FE`, `FR-01-01-BE`) | Nguyễn Danh Quang | 2026-11-28 |
| Lane names match the Context Diagram and the use case actors; no Web App, API or database lanes | Pass — strictly follows Prof. Seki's black-box lane rule (DB details remain in `S-01`) | Nguyễn Danh Quang | 2026-11-28 |
| Related sequence diagram uses the same names and FR IDs; its alt fragments appear here as decisions | Pass | Nguyễn Danh Quang | 2026-11-28 |
| AI critic prompt run, findings resolved or logged as open questions | Pass | Nguyễn Danh Quang | 2026-11-28 |

---

Template source: DBIZ2 Lesson 7, VJCBI College - FTU. Notation: OMG UML 2.5.1, Clause 15.
Position: the black-box activity diagram of a use case (MBSE Grid cell "Use Cases", item 2), the cell
left open in Prof. Seki's Lesson 4. The inside of the system stays in the Lesson 4 sequence diagrams.
Structure aligned with the DBIZ3 Spec Document, section 4 (Flows), so the file can be linked from a
module spec without rewriting.

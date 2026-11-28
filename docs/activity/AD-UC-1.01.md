# Activity Diagram: AD-UC-1.01 Enter VIN to initialize Report

| Field | Value |
|---|---|
| AD ID | `AD-UC-1.01` |
| Use case | `UC-1.01 Enter VIN to initialize Report` |
| Feature | `F-01 Vehicle Passport` |
| Primary actor | Car Owner |
| Lanes | Car Owner · TASCO Mobility System · Database · VETC |
| Inputs (L3-L5) | [Context Diagram](../Context/Context%20Diagram.md) · [Use Case Diagram](../Use%20Case%20Diagram/ucd-vehicle-identification-status.md) · Sequence: [S-01](../Sequence%20Diagram/S-01.md) · Screen Spec: [S-F01-01](../Screen/screen-spec-S-F01-01.md) · [W1 Function List](../Function%20List/w1-function-list.md) (`FR-01-01-FE`, `FR-01-01-BE`) |
| Trigger | Car Owner taps `"Tạo báo cáo xe"` on `S-F00-01` and submits a 17-character VIN on `S-F01-01` |
| Preconditions | Car Owner is signed in on the Home screen (`S-F00-01`) |
| Success end state | 17-character VIN passes client-side validation (`BR-VFVR-01`), either opens an existing `Verified vehicle report` in `Database` or verifies vehicle existence via `VETC`, persists a new `Verified vehicle report` in `Database`, and displays confirmation popup `"Đã tạo báo cáo xe"` before opening `S-F01-02` |
| Failure end state(s) | VIN initialization is rejected due to invalid format/length (`Error validate FE`), non-existent VIN in VETC (`Error validate BE`: `"Không tìm thấy thông tin xe"`), or API/database connection failure (`Error API`: `"Lỗi kết nối hệ thống"`), returning the Car Owner to `S-F01-01` |
| FRs covered | `FR-01-01-FE`, `FR-01-01-BE` |
| Business rules applied | `BR-VFVR-01` |
| Version / status | v1.0 · Reviewed |
| Author | Nguyễn Danh Quang |

## 1. Diagram

```mermaid
swimlane-beta TB
  subgraph ACT["Car Owner"]
    start@{ shape: sm-circ }
    a1("Select 'Tạo báo cáo xe' on Home screen")
    a2("Enter 17-character VIN and tap 'Tạo báo cáo xe'")
    a3("View inline or popup error feedback")
    a4("Dismiss 'Đã tạo báo cáo xe' popup via 'Đóng'")
    stop1@{ shape: fr-circ }
    stop2@{ shape: fr-circ }
  end

  subgraph SYS["TASCO Mobility System"]
    s1("Validate VIN format and length (FR-01-01-FE)")
    d1{"Is VIN format valid (BR-VFVR-01)?"}
    s2("Display Error validate FE inline message (FR-01-01-FE)")
    s3("Show loading banner and send VIN verification request (FR-01-01-BE)")
    s7("Redirect to existing Vehicle Profile S-F01-02 (FR-01-01-FE)")
    d3{"Is VETC/DB connection available?"}
    s4("Display Error API popup: 'Lỗi kết nối hệ thống' (FR-01-01-FE)")
    d4{"Does VIN exist in VETC vehicle data?"}
    s5("Display Error validate BE popup: 'Không tìm thấy thông tin xe' (FR-01-01-FE)")
    s6("Display Success popup: 'Đã tạo báo cáo xe' (FR-01-01-FE)")
    m1{" "}
    m2{" "}
  end

  subgraph DB["Database"]
    db1("Check if active Verified vehicle report exists for VIN (FR-01-01-BE)")
    d2{"Does active report already exist?"}
    db2("Initialize and save Verified vehicle report record (FR-01-01-BE)")
  end

  subgraph VETC["VETC"]
    vetc1("Query vehicle identification data by VIN (FR-01-01-BE)")
  end

  start --> a1 --> a2 --> s1 --> d1

  d1 -->|"[No]"| s2 --> m1
  d1 -->|"[Yes]"| s3 --> db1 --> d2

  d2 -->|"[Yes]"| s7 --> m2
  d2 -->|"[No]"| vetc1 --> d3

  d3 -->|"[No]"| s4 --> m1
  d3 -->|"[Yes]"| d4

  d4 -->|"[No]"| s5 --> m1
  d4 -->|"[Yes]"| db2 --> s6 --> a4 --> m2

  m1 --> a3 --> stop1
  m2 --> stop2
```

Reading notes:

- **Client-Side Gatekeeping (`d1`):** `TASCO Mobility System` validates the 17-character uppercase alphanumeric rule excluding `I`, `O`, and `Q` (`BR-VFVR-01`) before calling backend services, blocking invalid inputs immediately with inline red text on `S-F01-01`.
- **Database Lane (`DB`) & External Source (`VETC`):** `Database` is added per Prof. Lân's review feedback to check whether an active `Verified vehicle report` already exists for the VIN (`db1`, `d2`) and to persist newly initialized reports (`db2`). For new vehicles (`d2 [No]`), `TASCO Mobility System` queries `VETC` (`vetc1`) as the read-only source of vehicle identification data (`Mẫu xe`, `Màu sơn`, `Biển số xe`).
- **UI Consistency & Retry:** All Vietnamese texts in `s2`, `s4`, `s5`, and `s6` match [screen-spec-S-F01-01.md](../Screen/screen-spec-S-F01-01.md) verbatim. Tapping `"Thử lại"` on the `Error validate BE` or `Error API` popup re-triggers `s3` $\rightarrow$ `db1`, while tapping `"Đóng"` returns focus to the editable form (`stop1`).

## 2. Node trace

| Node | Kind | Text | Traces to |
|---|---|---|---|
| s1 | Action | Validate VIN format and length | `FR-01-01-FE` |
| d1 | Decision | Is VIN format valid (`BR-VFVR-01`: 17 uppercase alphanumeric chars, excluding `I`, `O`, `Q`)? | `FR-01-01-FE` / `BR-VFVR-01` |
| s2 | Action | Display `Error validate FE` inline message: `"VIN cần đủ 17 ký tự gồm chữ và số (không dùng I, O, Q). Vui lòng kiểm tra lại."` | `FR-01-01-FE` |
| s3 | Action | Show loading banner (`"Đang kiểm tra số VIN và khởi tạo hồ sơ xe. Vui lòng chờ..."`) and send VIN verification request | `FR-01-01-BE` |
| db1 | Action | Check if active `Verified vehicle report` exists for VIN in `Database` | `FR-01-01-BE` |
| d2 | Decision | Does active `Verified vehicle report` already exist in `Database`? | `FR-01-01-BE` |
| s7 | Action | Redirect directly to existing Vehicle Profile screen (`S-F01-02`) | `FR-01-01-FE` |
| vetc1 | Action | Query vehicle identification data (`Mẫu xe`, `Màu sơn`, `Biển số xe`) by VIN from `VETC` | `FR-01-01-BE` |
| d3 | Decision | Is VETC / database connection available? | `FR-01-01-BE` |
| s4 | Action | Display `Error API` popup: title `"Lỗi kết nối hệ thống"`, body `"Hệ thống chưa khởi tạo được báo cáo xe từ số VIN này. Vui lòng bấm Thử lại, hoặc đóng và quay lại sau."` | `FR-01-01-FE` |
| d4 | Decision | Does VIN exist in `VETC vehicle data`? | `FR-01-01-BE` |
| s5 | Action | Display `Error validate BE` popup: title `"Không tìm thấy thông tin xe"`, body `"Hệ thống không tìm thấy dữ liệu phương tiện cho số VIN này. Vui lòng kiểm tra lại số VIN và bấm Thử lại, hoặc đóng để quay lại."` | `FR-01-01-FE` |
| db2 | Action | Initialize and save `Verified vehicle report` record in `Database` | `FR-01-01-BE` |
| s6 | Action | Display `Success` popup: title `"Đã tạo báo cáo xe"`, body `"Đã khởi tạo báo cáo xe cho số VIN <vehicle.vin>. Hồ sơ phương tiện đã sẵn sàng."` | `FR-01-01-FE` |
| m1 | Merge | Control node merging `Error validate FE` (`s2`), `Error API` (`s4`), and `Error validate BE` (`s5`) failure paths | N/A (Control Node) |
| m2 | Merge | Control node merging existing report redirect (`s7`) and new report confirmation (`a4`) before opening `S-F01-02` | N/A (Control Node) |

## 3. Branches and acceptance tests

| Branch | Given | When | Then |
|---|---|---|---|
| d1 [No] | Car Owner is on `S-F01-01` | Car Owner enters a VIN whose length is not 17 characters or contains `I`, `O`, or `Q` (e.g., `"RLHJC6101IOQ12"`) and taps `"Tạo báo cáo xe"` | System blocks backend calls, turns input border `#B42318`, disables `"Tạo báo cáo xe"`, and displays inline error `"VIN cần đủ 17 ký tự gồm chữ và số (không dùng I, O, Q). Vui lòng kiểm tra lại."` |
| d1 [Yes] | Car Owner is on `S-F01-01` | Car Owner enters a valid 17-character VIN (e.g., `"RLHJC6101PY000123"`) and taps `"Tạo báo cáo xe"` | System displays top loading banner `"Đang kiểm tra số VIN và khởi tạo hồ sơ xe. Vui lòng chờ..."`, disables `"Tạo báo cáo xe"` (`"Đang tạo báo cáo..."`), and checks `Database` (`db1`) |
| d2 [Yes] | A valid 17-character VIN is submitted | `Database` finds an active `Verified vehicle report` already registered for this VIN | System skips duplicate creation and navigates directly to the existing `S-F01-02` Vehicle Profile screen |
| d2 [No] | A valid 17-character VIN is submitted | `Database` confirms no active `Verified vehicle report` exists for this VIN yet | System queries external `VETC` (`vetc1`) to verify vehicle existence and retrieve identification fields |
| d3 [No] | `TASCO Mobility System` queries `VETC` or `Database` | The external `VETC` API or database connection fails or times out | System displays `Error API` popup with title `"Lỗi kết nối hệ thống"` and message `"Hệ thống chưa khởi tạo được báo cáo xe từ số VIN này. Vui lòng bấm Thử lại, hoặc đóng và quay lại sau."` with `"Đóng"` and `"Thử lại"` buttons |
| d3 [Yes] | `TASCO Mobility System` queries `VETC` | `VETC` returns a valid HTTP response | System checks whether a matching vehicle record exists in the `VETC` response (`d4`) |
| d4 [No] | `VETC` responds successfully for a valid 17-character VIN (e.g., `"RLHJC6101PY999999"`) | `VETC` returns no vehicle record matching the submitted VIN | System aborts initialization and displays `Error validate BE` popup with title `"Không tìm thấy thông tin xe"` and message `"Hệ thống không tìm thấy dữ liệu phương tiện cho số VIN này. Vui lòng kiểm tra lại số VIN và bấm Thử lại, hoặc đóng để quay lại."` |
| d4 [Yes] | `VETC` responds successfully for a valid 17-character VIN (e.g., `"RLHJC6101PY000123"`) | `VETC` returns valid vehicle identification data (`Mẫu xe`, `Màu sơn`, `Biển số xe`) | `Database` saves the initialized `Verified vehicle report`, system displays `Success` popup `"Đã tạo báo cáo xe"` (`"Đã khởi tạo báo cáo xe cho số VIN RLHJC6101PY000123. Hồ sơ phương tiện đã sẵn sàng."`), and tapping `"Đóng"` opens `S-F01-02` |

## 4. Object states used

| Object | States in this diagram | Full state list (all diagrams) |
|---|---|---|
| Verified vehicle report | Initialized | Initialized, Validating, Completed, Expired |
| VETC vehicle data | Queried | Queried, Unavailable |

## 5. Screens touched

| Actor action | Screen ID |
|---|---|
| `a1` Select `"Tạo báo cáo xe"` on Home screen | `S-F00-01` |
| `a2` Enter 17-character VIN and tap `"Tạo báo cáo xe"` | `S-F01-01` |
| `a3` View inline or popup error feedback (`Error validate FE`, `Error validate BE`, `Error API`) | `S-F01-01` |
| `a4` Dismiss `"Đã tạo báo cáo xe"` popup via `"Đóng"` | `S-F01-01` (navigates to `S-F01-02`) |

## 6. Open questions

| # | Question | Blocking? | Default applied |
|---|---|---|---|
| 1 | [NEEDS CLARIFICATION: Should there be an application-level rate limit or maximum retry cap if the Car Owner repeatedly submits non-existent VINs on `S-F01-01`?] | No | No rate limit is enforced at the application layer; infrastructure/WAF handles request throttling. |
| 2 | [NEEDS CLARIFICATION: What should happen if the Car Owner submits a valid VIN that already has an active `Verified vehicle report` in `Database`?] | Yes | Redirect directly to the existing `S-F01-02` Vehicle Profile screen instead of creating a duplicate report (`d2 [Yes] -> s7`, aligned with `S-F01-01` Open Question #2). |

## 7. Verification log

| Check | Result | By | Date |
|---|---|---|---|
| Renders on mermaid.live | Pass | Nguyễn Danh Quang | 2026-11-28 |
| Every decision has guards on all outgoing flows, no overlap, no gap | Pass | Nguyễn Danh Quang | 2026-11-28 |
| No action has two incoming flows (merge used instead) | Pass | Nguyễn Danh Quang | 2026-11-28 |
| Every fork has a matching join | N/A (no forks) | Nguyễn Danh Quang | 2026-11-28 |
| Every system action cites an FR | Pass | Nguyễn Danh Quang | 2026-11-28 |
| Every FR of this use case appears in the diagram | 2 of 2 (`FR-01-01-FE`, `FR-01-01-BE`) | Nguyễn Danh Quang | 2026-11-28 |
| Lane names match the Context Diagram and the use case actors; no Web App, API or database lanes | Pass — `TASCO Mobility System` and `VETC` match Context Diagram; `Database` lane added per Prof. Lân's review feedback for BE existence and business-rule validation | Nguyễn Danh Quang | 2026-11-28 |
| Related sequence diagram uses the same names and FR IDs; its alt fragments appear here as decisions | Pass | Nguyễn Danh Quang | 2026-11-28 |
| AI critic prompt run, findings resolved or logged as open questions | Pass | Nguyễn Danh Quang | 2026-11-28 |

---

Template source: DBIZ2 Lesson 7, VJCBI College - FTU. Notation: OMG UML 2.5.1, Clause 15.
Position: the black-box activity diagram of a use case (MBSE Grid cell "Use Cases", item 2), the cell
left open in Prof. Seki's Lesson 4. The inside of the system stays in the Lesson 4 sequence diagrams.
Structure aligned with the DBIZ3 Spec Document, section 4 (Flows), so the file can be linked from a
module spec without rewriting.

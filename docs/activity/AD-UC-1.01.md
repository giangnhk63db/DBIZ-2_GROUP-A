# Activity Diagram: AD-UC-1.01 Enter VIN to initialize Report

| Field | Value |
|---|---|
| AD ID | `AD-UC-1.01` |
| Use case | `UC-1.01 Enter VIN to initialize Report` |
| Feature | `F-01 Vehicle Passport` |
| Primary actor | Car Owner |
| Lanes | Car Owner · TASCO Mobility System · Database |
| Inputs (L3-L5) | [Context Diagram](../context/context-diagram.md) · [Use Case Diagram](../use-case/use-case-diagram.md) · Sequence: [S-01](../sequence/S-01.md) · Screen Spec: [S-F01-01](../external-design/screens/screen-spec-S-F01-01.md) · [W1 Function List](../function-list/w1-function-list.md) (`FR-01-01-FE`, `FR-01-01-BE`) |
| Trigger | Car Owner selects `"Tạo báo cáo xe"` on `S-F00-01` and submits a 17-character VIN on `S-F01-01` |
| Preconditions | Car Owner is signed in on the Home screen (`S-F00-01`) |
| Success end state | 17-character VIN passes client-side format validation (`BR-VFVR-01`), backend existence and checksum rules are confirmed in `Database`, a new `Verified vehicle report` is initialized, and confirmation popup `"Đã tạo báo cáo xe"` is displayed before navigating to `S-F01-02` |
| Failure end state(s) | VIN initialization is rejected due to invalid format/length (`Error validate FE`), non-existent VIN in `Database` (`Error validate BE`: `"Không tìm thấy thông tin xe"`), or database/API connection failure (`Error API`: `"Lỗi kết nối hệ thống"`), returning the Car Owner to `S-F01-01` |
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
    s1("Client-side format validation (FR-01-01-FE)")
    d1{"Is VIN format valid (BR-VFVR-01)?"}
    s2("Display Error validate FE inline message (FR-01-01-FE)")
    s3("Show loading banner and send verification request (FR-01-01-BE)")
    s4("Display Error API popup: 'Lỗi kết nối hệ thống' (FR-01-01-FE)")
    s5("Display Error validate BE popup: 'Không tìm thấy thông tin xe' (FR-01-01-FE)")
    s6("Display Success popup: 'Đã tạo báo cáo xe' (FR-01-01-FE)")
    m1{" "}
  end

  subgraph DB["Database"]
    db1("Validate checksum logic and query VIN existence (FR-01-01-BE)")
    d2{"Is database connection available?"}
    d3{"Does VIN exist?"}
    db2("Initialize and save Verified vehicle report (FR-01-01-BE)")
  end

  start --> a1 --> a2 --> s1 --> d1

  d1 -->|"[No]"| s2 --> m1
  d1 -->|"[Yes]"| s3 --> db1 --> d2

  d2 -->|"[No]"| s4 --> m1
  d2 -->|"[Yes]"| d3

  d3 -->|"[No]"| s5 --> m1
  d3 -->|"[Yes]"| db2 --> s6 --> a4 --> stop2

  m1 --> a3 --> stop1
```

Reading notes:

- **Validation Priority (`d1`):** `TASCO Mobility System` enforces `BR-VFVR-01` (exact 17 uppercase alphanumeric characters excluding `I`, `O`, and `Q`) on the client side (`FR-01-01-FE`) as the primary gatekeeper before initiating any `Database` query, displaying inline red feedback on `S-F01-01` when violated.
- **Database Lane (`DB`):** Added per Prof. Lân's review feedback to isolate backend business-rule/checksum verification and VIN existence checks (`db1`, `d2`, `d3`) as well as `Verified vehicle report` persistence (`db2`) from UI presentation.
- **Control Flow & UI Consistency:** All failure branches (`s2`, `s4`, `s5`) converge through merge node `m1` before returning feedback to `Car Owner` (`a3`). Every Vietnamese message in `s2`, `s4`, `s5`, and `s6` matches [screen-spec-S-F01-01.md](../external-design/screens/screen-spec-S-F01-01.md) verbatim; tapping `"Thử lại"` on `Error validate BE` or `Error API` re-triggers `s3` $\rightarrow$ `db1`, while tapping `"Đóng"` returns to the editable form (`stop1`).

## 2. Node trace

| Node | Kind | Text | Traces to |
|---|---|---|---|
| s1 | Action | Client-side format validation | `FR-01-01-FE` |
| d1 | Decision | Is VIN format valid (`BR-VFVR-01`: exact 17 uppercase alphanumeric characters excluding `I`, `O`, `Q`)? | `FR-01-01-FE` / `BR-VFVR-01` |
| s2 | Action | Display `Error validate FE` inline message: `"VIN cần đủ 17 ký tự gồm chữ và số (không dùng I, O, Q). Vui lòng kiểm tra lại."` | `FR-01-01-FE` |
| s3 | Action | Show loading banner (`"Đang kiểm tra số VIN và khởi tạo hồ sơ xe. Vui lòng chờ..."`) and send verification request | `FR-01-01-BE` |
| db1 | Action | Validate checksum logic and query VIN existence in `Database` | `FR-01-01-BE` |
| d2 | Decision | Is database connection available? | `FR-01-01-BE` |
| s4 | Action | Display `Error API` popup: title `"Lỗi kết nối hệ thống"`, body `"Hệ thống chưa khởi tạo được báo cáo xe từ số VIN này. Vui lòng bấm Thử lại, hoặc đóng và quay lại sau."` | `FR-01-01-FE` |
| d3 | Decision | Does VIN exist? | `FR-01-01-BE` |
| s5 | Action | Display `Error validate BE` popup: title `"Không tìm thấy thông tin xe"`, body `"Hệ thống không tìm thấy dữ liệu phương tiện cho số VIN này. Vui lòng kiểm tra lại số VIN và bấm Thử lại, hoặc đóng để quay lại."` | `FR-01-01-FE` |
| db2 | Action | Initialize and save `Verified vehicle report` in `Database` | `FR-01-01-BE` |
| s6 | Action | Display `Success` popup: title `"Đã tạo báo cáo xe"`, body `"Đã khởi tạo báo cáo xe cho số VIN <vehicle.vin>. Hồ sơ phương tiện đã sẵn sàng."` | `FR-01-01-FE` |
| m1 | Merge | Control node merging `Error validate FE` (`s2`), `Error API` (`s4`), and `Error validate BE` (`s5`) failure paths | N/A (Control Node) |

## 3. Branches and acceptance tests

| Branch | Given | When | Then |
|---|---|---|---|
| d1 [No] | Car Owner is on `S-F01-01` | Car Owner enters a VIN whose length is not 17 characters or contains `I`, `O`, or `Q` (e.g., `"RLHJC6101IOQ12"`) and taps `"Tạo báo cáo xe"` | System blocks the backend call, turns input border `#B42318`, disables `"Tạo báo cáo xe"`, and displays inline error `"VIN cần đủ 17 ký tự gồm chữ và số (không dùng I, O, Q). Vui lòng kiểm tra lại."` |
| d1 [Yes] | Car Owner is on `S-F01-01` | Car Owner enters a valid 17-character VIN (e.g., `"RLHJC6101PY000123"`) and taps `"Tạo báo cáo xe"` | System displays top loading banner `"Đang kiểm tra số VIN và khởi tạo hồ sơ xe. Vui lòng chờ..."`, disables `"Tạo báo cáo xe"` (`"Đang tạo báo cáo..."`), and forwards the verification request to `Database` |
| d2 [No] | A valid 17-character VIN is submitted (`"RLHJC6101PY000123"`) | The `Database` connection times out or fails during the existence check | System displays `Error API` popup with title `"Lỗi kết nối hệ thống"` and message `"Hệ thống chưa khởi tạo được báo cáo xe từ số VIN này. Vui lòng bấm Thử lại, hoặc đóng và quay lại sau."` with `"Đóng"` and `"Thử lại"` buttons |
| d2 [Yes] | A valid 17-character VIN is submitted | The `Database` connection succeeds | System evaluates whether a matching vehicle record exists (`d3`) |
| d3 [No] | `Database` connection is active and a valid 17-character VIN is queried (e.g., `"RLHJC6101PY999999"`) | `Database` finds no vehicle record matching the submitted VIN | System aborts initialization and displays `Error validate BE` popup with title `"Không tìm thấy thông tin xe"` and message `"Hệ thống không tìm thấy dữ liệu phương tiện cho số VIN này. Vui lòng kiểm tra lại số VIN và bấm Thử lại, hoặc đóng để quay lại."` |
| d3 [Yes] | `Database` connection is active and a valid 17-character VIN is queried (e.g., `"RLHJC6101PY000123"`) | `Database` confirms the vehicle record exists | `Database` saves the initialized `Verified vehicle report`, system displays `Success` popup `"Đã tạo báo cáo xe"` (`"Đã khởi tạo báo cáo xe cho số VIN RLHJC6101PY000123. Hồ sơ phương tiện đã sẵn sàng."`), and tapping `"Đóng"` opens `S-F01-02` |

## 4. Object states used

| Object | States in this diagram | Full state list (all diagrams) |
|---|---|---|
| Verified vehicle report | Initialized | Initialized, Validating, Completed, Expired |

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
| 1 | [NEEDS CLARIFICATION: Should there be an application-level rate limit or maximum retry cap if the Car Owner repeatedly submits invalid or non-existent VINs on `S-F01-01`?] | No | No rate limit is enforced at the application layer; infrastructure/WAF handles basic request throttling. |
| 2 | [NEEDS CLARIFICATION: What should happen if the Car Owner submits a valid VIN that already has an active `Verified vehicle report` in `Database`?] | Yes | Redirect directly to the existing `S-F01-02` Vehicle Profile screen instead of creating a duplicate report (aligned with `S-F01-01` Open Question #2). |

## 7. Verification log

| Check | Result | By | Date |
|---|---|---|---|
| Renders on mermaid.live | Pass | Nguyễn Danh Quang | 2026-11-28 |
| Every decision has guards on all outgoing flows, no overlap, no gap | Pass | Nguyễn Danh Quang | 2026-11-28 |
| No action has two incoming flows (merge used instead) | Pass | Nguyễn Danh Quang | 2026-11-28 |
| Every fork has a matching join | N/A (no forks) | Nguyễn Danh Quang | 2026-11-28 |
| Every system action cites an FR | Pass | Nguyễn Danh Quang | 2026-11-28 |
| Every FR of this use case appears in the diagram | 2 of 2 (`FR-01-01-FE`, `FR-01-01-BE`) | Nguyễn Danh Quang | 2026-11-28 |
| Lane names match the Context Diagram and the use case actors; no Web App, API or database lanes | Pass — `TASCO Mobility System` matches Context Diagram; `Database` lane added per Prof. Lân's review feedback for BE existence and checksum validation | Nguyễn Danh Quang | 2026-11-28 |
| Related sequence diagram uses the same names and FR IDs; its alt fragments appear here as decisions | Pass | Nguyễn Danh Quang | 2026-11-28 |
| AI critic prompt run, findings resolved or logged as open questions | Pass | Nguyễn Danh Quang | 2026-11-28 |

---

Template source: DBIZ2 Lesson 7, VJCBI College - FTU. Notation: OMG UML 2.5.1, Clause 15.
Position: the black-box activity diagram of a use case (MBSE Grid cell "Use Cases", item 2), the cell
left open in Prof. Seki's Lesson 4. The inside of the system stays in the Lesson 4 sequence diagrams.
Structure aligned with the DBIZ3 Spec Document, section 4 (Flows), so the file can be linked from a
module spec without rewriting.

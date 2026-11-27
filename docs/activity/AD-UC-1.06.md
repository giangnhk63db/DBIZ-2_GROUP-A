# Activity Diagram: AD-UC-1.06 View Battery State of Health (SoH)

| Field | Value |
| :---- | :---- |
| AD ID | AD-UC-1.06 |
| Use case | UC-1.06 Assess Battery Health Condition (SoH Calculation) |
| Feature | F-01 Vehicle Identification & Status |
| Primary actor | Car Owner |
| Lanes | Car Owner · TASCO Mobility System · Electronic Vehicle Charging Stations |
| Inputs (L3-L5) | Context Diagram v1 · Use Case Diagram v1 · Sequence: SD-UC-1.06 · W1 rows (FR-01-08-FE, FR-01-08-BE, FR-01-08-INT) |
| Trigger | Car owner selects view EV battery health status |
| Preconditions | Vehicle profile exists in the system and charging session telemetry data is available |
| Success end state | `stop1`: SoH is newly calculated and displayed with the success popup. `stop3`: SoH is displayed from the cached result (Default state, no popup) |
| Alternative end state | `stop4`: fewer than 3 valid charging sessions, SoH is not calculated and the Empty state is displayed |
| Failure end state(s) | `stop2`: charging station data retrieval fails and the maximum of 3 retries is exceeded, triggering an error notification |
| FRs covered | FR-01-08-FE, FR-01-08-BE, FR-01-08-INT |
| Business rules applied | BR-RET-02 (Telemetry Data Retry & Fallback Rule) · BR-SOH-01 (SoH Grade Classification Rule) · BR-SOH-02 (Minimum Charging Sessions Rule) · BR-SOH-03 (SoH Rounding Rule) · BR-SOH-04 (SoH Calculation Frequency & Update Rule) · BR-SOH-05 (SoH Success Notification Trigger Rule) |
| Version / status | v0.4 · Reviewed |
| Author | Đinh Thu Thảo |

## 

1. ## **Diagram**

```mermaid

swimlane-beta TB
 subgraph CAR["Car Owner"]
   start@{ shape: sm-circ }
   a1("Select view EV battery health status (FR-01-08-FE)")
   a2("View SoH metrics & reports with success popup 'Đã tính xong sức khỏe pin' (FR-01-08-FE)")
   a3("View SoH metrics & reports in Default state, no popup (FR-01-08-FE)")
   a4("View Empty state: insufficient data banner and placeholders (FR-01-08-FE)")
   error_view("View error notification popup: 'Không kết nối được trạm sạc' (FR-01-08-FE)")
   stop1@{ shape: fr-circ }
   stop2@{ shape: fr-circ }
   stop3@{ shape: fr-circ }
   stop4@{ shape: fr-circ }
 end
 subgraph SYS["TASCO Mobility System"]
   d0{"Recalculation required? (new valid session since last calculation / no result yet / 30-day re-evaluation due)"}
   m1{" "}
   s1("Request charging session telemetry and battery metrics (FR-01-08-BE)")
   d1{"API responded without timeout & charging session data and battery metrics valid?"}
   s4("Wait 2 seconds, then retry (FR-01-08-BE)")
   d2{"Valid sessions in V1 to V2 window >= 3?"}
   s3("Calculate SoH using charging time ratio from V1 to V2 (FR-01-08-BE)")
   s2("Max 3 retries exceeded: log error & prepare notification (FR-01-08-BE)")
 end
 subgraph STA["Electronic Vehicle Charging Stations"]
   e1("Retrieve charging logs and data (FR-01-08-INT)")
 end

 start --> a1 --> d0
 d0 -->|"[no recalculation needed, use cached SoH]"| a3 --> stop3
 d0 -->|"[recalculation required]"| m1
 m1 --> s1 --> e1 --> d1
 d1 -->|"[fail/timeout & retries done < 3]"| s4 --> m1
 d1 -->|"[data valid]"| d2
 d1 -->|"[fail/timeout & retries done = 3]"| s2 --> error_view --> stop2
 d2 -->|"[valid sessions >= 3]"| s3 --> a2 --> stop1
 d2 -->|"[valid sessions < 3]"| a4 --> stop4

```

**Reading notes:**

* Cache check: After the Car Owner selects the battery health view, the system checks whether a recalculation is required (BR-SOH-04). If no new valid charging session exists since the last calculation, the cached SoH is shown in the Default state without the success popup (BR-SOH-05) and the flow ends at `stop3`.
* Telemetry request: If recalculation is required, the system requests charging session telemetry and battery metrics, and the request is forwarded to the Electronic Vehicle Charging Stations, which retrieve the charging logs and data (`s1 → e1`).
* Response validation (`d1`): The response from the charging station is passed back to the TASCO Mobility System, which checks (in the SYS lane) that the API responded without timeout and that the charging session data and battery metrics are valid. `d1` is evaluated after every attempt.
* Retry loop: `retries done` counts the retries already made, not the attempts. After attempt 1, 2 or 3 fails, `retries done` is 0, 1 or 2 (< 3), so the system waits 2 seconds (`s4`) and loops back via merge node `m1`. After the 4th attempt fails, `retries done = 3` and the loop stops. This gives 1 initial request + 3 retries = 4 attempts in total (BR-RET-02).
* Minimum sessions check (`d2`): When the data is valid, the system counts valid charging sessions in the standard voltage window V1 to V2. If there are at least 3, SoH is computed (BR-SOH-02, BR-SOH-03) and shown with the success popup (BR-SOH-05), ending at `stop1`. If fewer than 3, SoH is not calculated and the Empty state is displayed, ending at `stop4`.
* Error management: If all 4 attempts fail, a persistent error is logged and the connection failure popup is shown, terminating at dedicated end node `stop2`.

2. ## **Node trace**

| Node | Kind | Text | Traces to |
| :---- | :---- | :---- | :---- |
| a1 | Action | Select view EV battery health status | FR-01-08-FE |
| d0 | Decision | Recalculation required? (new valid session since last calculation / no result yet / 30-day re-evaluation due) | FR-01-08-BE / BR-SOH-04 |
| m1 | Merge | Control node merging initial request path and retry loop-back path | N/A (Control Node) |
| s1 | Action | Request charging session telemetry and battery metrics | FR-01-08-BE |
| e1 | Action | Retrieve charging logs and data | FR-01-08-INT |
| d1 | Decision | API responded without timeout & charging session data and battery metrics valid? | FR-01-08-BE / BR-RET-02 |
| s4 | Action | Wait 2 seconds, then retry | FR-01-08-BE / BR-RET-02 |
| d2 | Decision | Valid sessions in V1 to V2 window >= 3? | FR-01-08-BE / BR-SOH-02 |
| s3 | Action | Calculate SoH using charging time ratio from V1 to V2: <br>$\text{SoH} = \frac{\text{Charging time from } V_1 \to V_2 \text{ (current)}}{\text{Charging time from } V_1 \to V_2 \text{ (new battery)}} \times 100\%$ | FR-01-08-BE / BR-SOH-01, BR-SOH-03, BR-SOH-04 |
| a2 | Action | View SoH metrics & reports with success popup 'Đã tính xong sức khỏe pin' | FR-01-08-FE / BR-SOH-01, BR-SOH-05 |
| a3 | Action | View SoH metrics & reports in Default state, no popup (cached result) | FR-01-08-FE / BR-SOH-01, BR-SOH-04, BR-SOH-05 |
| a4 | Action | View Empty state: insufficient data banner and `—` placeholders | FR-01-08-FE / BR-SOH-02 |
| s2 | Action | Max 3 retries exceeded: log error & prepare notification | FR-01-08-BE / BR-RET-02 |
| error_view | Action | View error notification popup: 'Không kết nối được trạm sạc' | FR-01-08-FE |
| stop1 | Final | End: SoH newly calculated and displayed | N/A (Control Node) |
| stop2 | Final | End: error after max retries | N/A (Control Node) |
| stop3 | Final | End: cached SoH displayed | N/A (Control Node) |
| stop4 | Final | End: Empty state (insufficient valid sessions) | N/A (Control Node) |

## 

**Business rule description**

| Rule ID | Description | Applied Node |
| :---- | :---- | :---- |
| BR-RET-02 | Telemetry Data Retry & Fallback Rule: When requesting charging session data and battery metrics from the charging station, if the data is inaccessible, invalid, or connection timeouts occur, the system must automatically execute a retry mechanism up to a maximum of 3 retries (maximum 4 attempts in total), with a 2-second delay between consecutive attempts. If all 4 attempts fail, the system halts the retry loop, logs an error, and notifies the user. | d1, s4, s2 |
| BR-SOH-01 | SoH Grade Classification Rule: Based on the rounded SoH percentage, the system classifies battery health into three levels: **Tốt (Good)** when $\text{SoH} \ge 80\%$ (green `#1B7A43` with check icon); **Trung bình (Average)** when $65\% \le \text{SoH} < 80\%$ (amber/warning `#8A5300` with warning triangle icon); and **Thấp (Low)** when $\text{SoH} < 65\%$ (red `#B42318` with alert icon). | s3, a2, a3 |
| BR-SOH-02 | Minimum Charging Sessions Rule: A minimum of **3 valid charging sessions** within the standard voltage window ($V_1 \to V_2$) at TASCO charging stations is required to compute SoH. If fewer than 3 valid sessions exist, SoH is not calculated and the UI enters the Empty state showing an insufficient data banner and `—` placeholders. | d2, a4 |
| BR-SOH-03 | SoH Rounding Rule: The computed SoH percentage is rounded to the nearest whole integer using the **Round Half Up** rule (e.g., `91.5%` rounds to `92%`, `91.4%` rounds to `91%`) and formatted without a space before `%`. | s3 |
| BR-SOH-04 | SoH Calculation Frequency & Update Rule: SoH is recalculated on an event-driven/periodic basis only when **at least one new valid charging session** is recorded since the last calculation (at most once per day upon session sync, or re-evaluated every 30 days). Otherwise, the system returns the latest cached SoH result. The "Cập nhật" (Updated) date reflects the date of the latest calculation (`dd/MM/yyyy`). | d0, s3, a3 |
| BR-SOH-05 | SoH Success Notification Trigger Rule: The success popup ("Đã tính xong sức khỏe pin") is displayed **only when SoH is newly calculated** (either on the first successful calculation for the vehicle or when recalculated after new charging sessions). On subsequent routine views with no new calculation, the screen opens directly in the Default state without the popup. | a2, a3 |

3. ## **Branches and acceptance tests**

| Branch | Given | When | Then |
| :---- | :---- | :---- | :---- |
| d0 [no recalculation needed, use cached SoH] | A cached SoH result exists, no new valid charging session since the last calculation, and the 30-day re-evaluation is not due | Car owner selects view EV battery health status | System skips the station request, displays the cached SoH in Default state without the success popup, and ends at stop3 |
| d0 [recalculation required] | No SoH result exists yet, or at least one new valid charging session was recorded, or the 30-day re-evaluation is due | Car owner selects view EV battery health status | System enters merge node m1 and requests charging session telemetry and battery metrics |
| d1 [fail/timeout & retries done < 3] | Charging data request sent to charging station (attempt 1, 2 or 3) | Station returns timeout, connection error, or inaccessible/invalid data | System waits 2 seconds, then loops back via merge node m1 to request data again |
| d1 [data valid] | Charging data request sent to charging station (any attempt 1 to 4) | Station returns accessible and valid charging logs and battery metrics | System proceeds to check the number of valid charging sessions at d2 |
| d1 [fail/timeout & retries done = 3] | Charging data request failed on attempts 1, 2 and 3, and 3 retries have been made | Station continues to return connection errors or invalid data on the 4th attempt | System stops retrying, logs error, shows the error popup 'Không kết nối được trạm sạc', and ends at stop2 |
| d2 [valid sessions >= 3] | Valid charging data was retrieved | At least 3 valid charging sessions exist in the V1 to V2 window | System calculates SoH (rounded Round Half Up), classifies the grade, displays the report with the success popup, and ends at stop1 |
| d2 [valid sessions < 3] | Valid charging data was retrieved | Fewer than 3 valid charging sessions exist in the V1 to V2 window | System does not calculate SoH, displays the Empty state with the insufficient data banner and `—` placeholders (no success popup), and ends at stop4 |

## 

4. ## **Object states used** ##

| Object | States in this diagram | Full state list (all diagrams) |
| :---- | :---- | :---- |
| Charging Data | Requested -> Retrieved -> Validated -> Processed (success path); Requested -> Failed (max retries exceeded); Requested -> Retrieved -> Validated (insufficient sessions, not processed) | Initial, Requested, Retrieved, Validated, Processed, Failed |
| Battery Health Report | Initial -> Computed -> Displayed (new calculation); Initial -> Displayed (cached result); Initial -> Empty (fewer than 3 valid sessions); Initial -> Error (max retries exceeded) | Initial, Computed, Displayed, Empty, Error |

## 

5. ## **Screens touched** ##

| Actor action | Screen ID |
| :---- | :---- |
| Select view EV battery health status (a1) | S-F01-02 → S-F01-06 |
| View SoH metrics & reports with success popup (a2) | S-F01-06 (Default state with success popup) |
| View SoH metrics & reports in Default state, no popup (a3) | S-F01-06 (Default state) |
| View Empty state: insufficient data banner (a4) | S-F01-06 (Empty state) |
| View error notification popup: 'Không kết nối được trạm sạc' (error_view) | S-F01-06 (Error notification (BE) popup) |

## 

6. ## **Open questions** ##

| # | Question | Blocking? | Default applied |
| :---- | :---- | :---- | :---- |
| 1 | Standard time interval delay between automated retry requests? | No | 2-second delay between consecutive retries, now shown in the diagram as action s4 and in BR-RET-02 |
| 2 | Does the cache and recalculation check (d0, BR-SOH-04) belong in this use case, or in the session sync process of another use case? | No | Kept in this diagram as decision d0 so that BR-SOH-04 and BR-SOH-05 are visible in the flow |
| 3 | Should the Empty state (stop4) be treated as a success variant or a failure variant in the use case end states? | No | Treated as an alternative end state, separate from the success and failure end states |

## 

7. ## **Verification log** ##

The diagram changed from v0.4 to v0.5 (added d0, d2, s4, a3, a4, stop3, stop4; moved d1 to the TASCO Mobility System lane; rewrote guards). All checks below were re-run on 2026-11-28.

| Check | Result | By | Date |
| :---- | :---- | :---- | :---- |
| Renders on mermaid.live | Pass | Đinh Thu Thảo | 2026-11-28 |
| Every decision has guards on all outgoing flows, no overlap, no gap (d0, d1, d2) | Pass | Đinh Thu Thảo | 2026-11-28 |
| No action has two incoming flows (merge used instead) | Pass | Đinh Thu Thảo | 2026-11-28 |
| Every fork has a matching join | Pass | Đinh Thu Thảo | 2026-11-28 |
| Every system action cites an FR | Pass | Đinh Thu Thảo | 2026-11-28 |
| Every FR of this use case appears in the diagram | Pass | Đinh Thu Thảo | 2026-11-28 |
| Lane names match the Context Diagram and the use case actors; no Web App, API or database lanes | Pass | Đinh Thu Thảo | 2026-11-28 |
| Related sequence diagram SD-UC-1.06 uses the same names and FR IDs; its alt fragments appear here as decisions (including the new d0 and d2) | Pass | Đinh Thu Thảo | 2026-11-28 |
| Every business rule in the header is applied to at least one node | Pass | Đinh Thu Thảo | 2026-11-28 |
| AI critic prompt run, findings resolved or logged as open questions | Pass | Đinh Thu Thảo | 2026-11-28 |

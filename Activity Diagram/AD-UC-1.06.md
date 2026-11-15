# **AD-UC-1.06** 

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
| Success end state | Battery State of Health (SoH) metrics and reports are successfully calculated and displayed |
| Failure end state(s) | Charging station data retrieval fails or exceeds max 3 retries, triggering an error notification |
| FRs covered | FR-01-08-FE, FR-01-08-BE, FR-01-08-INT |
| Business rules applied | BR-RET-02 (Telemetry Data Retry & Fallback Rule) · BR-SOH-01 (SoH Grade Classification Rule) · BR-SOH-02 (Minimum Charging Sessions Rule) · BR-SOH-03 (SoH Rounding Rule) · BR-SOH-04 (SoH Calculation Frequency & Update Rule) · BR-SOH-05 (SoH Success Notification Trigger Rule) |
| Version / status | v0.4 · Reviewed |
| Author | Đinh Thu Thảo |

## 

## **1\. Diagram**

*Code:*  
```mermaid

swimlane-beta TB
 subgraph CAR["Car Owner"]
   start@{ shape: sm-circ }
   a1("Select view EV battery health status (FR-01-08-FE)")
   a2("View Battery State of Health (SoH) metrics & reports (FR-01-08-FE)")
   error_view("View error notification popup: 'Cannot connect to station' (FR-01-08-FE)")
   stop1@{ shape: fr-circ }
   stop2@{ shape: fr-circ }
 end
 subgraph SYS["TASCO Mobility System"]
   m1{" "}
   s1("Request charging session telemetry and battery metrics (FR-01-08-BE)")
   s3("Calculate SoH using charging time ratio from V1 to V2 (FR-01-08-BE)")
   s2("Max 3 retries exceeded: log error & prepare notification (FR-01-08-BE)")
 end
 subgraph STA["Electronic Vehicle Charging Stations"]
   e1("Retrieve charging logs and data (FR-01-08-INT)")
   d1{"Charging station data available & API connection?"}


 end




 start --> a1 --> m1
 m1 --> s1 --> e1 --> d1
 d1 -->|"[retry < 3]"| m1
 d1 -->|"[success]"| s3 --> a2 --> stop1
 d1 -->|"[max retries exceeded]"| s2 --> error_view --> stop2

```

**Reading notes:**

* Telemetry Request: The operator requests charging session telemetry to check battery status.  
* Station Data Retrieval & Validation: The charging station returns logs; the system evaluates data validity and accessibility with a maximum of 3 retries using merge node m1. This gives 1 initial request + 3 retries = 4 attempts in total. 
* SoH Calculation & Display: If valid, State of Health is computed using the charging time ratio and displayed on the UI.  
* Error Management: If all retries fail, a persistent error is logged and a connection failure popup is shown, terminating at dedicated end node stop2.

## **2\. Node trace**

| Node | Kind | Text | Traces to |
| :---- | :---- | :---- | :---- |
| a1 | Action | Select view EV battery health status | FR-01-08-FE |
| m1 | Merge | Control node merging initial request path and retry loop-back path | N/A (Control Node) |
| s1 | Action | Request charging session telemetry and battery metrics | FR-01-08-BE |
| e1 | Action | Retrieve charging logs and data | FR-01-08-INT |
| d1 | Decision | Charging station data available & API connection? | FR-01-08-BE / BR-RET-02 |
| s3 | Action | Calculate SoH using charging time ratio from V1 to V2: <br>$\text{SoH} = \frac{\text{Charging time from } V_1 \to V_2 \text{ (current)}}{\text{Charging time from } V_1 \to V_2 \text{ (new battery)}} \times 100\%$ | FR-01-08-BE / BR-SOH-01, BR-SOH-02, BR-SOH-03, BR-SOH-04 |
| a2 | Action | View Battery State of Health (SoH) metrics & reports | FR-01-08-FE |
| s2 | Action | Max 3 retries exceeded: log error & prepare notification | FR-01-08-BE |
| error\_view | Action | View error notification popup: 'Cannot connect to station' | FR-01-08-FE |

## 

**Business rule description**

| Rule ID | Description | Applied Node |
| :---- | :---- | :---- |
| BR-RET-02 | Telemetry Data Retry & Fallback Rule: When requesting charging session data and battery metrics from the charging station, if the data is inaccessible, invalid, or connection timeouts occur, the system must automatically execute a retry mechanism up to a maximum of 3 retries (maximum 4 attempts in total). If all 4 attempts fail, the system halts the retry loop, logs an error, and notifies the user. | d1 |
| BR-SOH-01 | SoH Grade Classification Rule: Based on the rounded SoH percentage, the system classifies battery health into three levels: **Tốt (Good)** when $\text{SoH} \ge 80\%$ (green `#1B7A43` with check icon); **Trung bình (Average)** when $65\% \le \text{SoH} < 80\%$ (amber/warning `#8A5300` with warning triangle icon); and **Thấp (Low)** when $\text{SoH} < 65\%$ (red `#B42318` with alert icon). | s3, a2 |
| BR-SOH-02 | Minimum Charging Sessions Rule: A minimum of **3 valid charging sessions** within the standard voltage window ($V_1 \to V_2$) at TASCO charging stations is required to compute SoH. If fewer than 3 valid sessions exist, SoH is not calculated and the UI enters the Empty state showing an insufficient data banner and `—` placeholders. | s3 |
| BR-SOH-03 | SoH Rounding Rule: The computed SoH percentage is rounded to the nearest whole integer using the **Round Half Up** rule (e.g., `91.5%` rounds to `92%`, `91.4%` rounds to `91%`) and formatted without a space before `%`. | s3 |
| BR-SOH-04 | SoH Calculation Frequency & Update Rule: SoH is recalculated on an event-driven/periodic basis only when **at least one new valid charging session** is recorded since the last calculation (at most once per day upon session sync, or re-evaluated every 30 days). Otherwise, the system returns the latest cached SoH result. The "Cập nhật" (Updated) date reflects the date of the latest calculation (`dd/MM/yyyy`). | s3 |
| BR-SOH-05 | SoH Success Notification Trigger Rule: The success popup ("Đã tính xong sức khỏe pin") is displayed **only when SoH is newly calculated** (either on the first successful calculation for the vehicle or when recalculated after new charging sessions). On subsequent routine views with no new calculation, the screen opens directly in the Default state without the popup. | a2 |

## **3\. Branches and acceptance tests**

| Branch | Given | When | Then |
| :---- | :---- | :---- | :---- |
| d1 \[retry \< 3\] | Charging data request sent to charging station | Station returns timeout or inaccessible data on attempt 1, 2 | System automatically loops back via merge node m1 to request data again |
| d1 \[success\] | Charging data request sent to charging station | Station returns accessible and valid charging logs successfully | System calculates SoH metrics and renders the battery health |
| d1 \[max retries exceeded\] | Charging data request failed 3 times previously | Station continues to return connection errors on the 4th attempt | System stops retrying, logs error, triggers error popup, and stops at stop2 |

## 

## **4\. Object states used**

| Object | States in this diagram | Full state list (all diagrams) |
| :---- | :---- | :---- |
| Charging Data | Requested \-\> Retrieved \-\> Validated \-\> Processed | Initial, Requested, Retrieved, Validated, Processed, Failed |
| Battery Health Report | Initial \-\> Computed \-\> Displayed | Initial, Computed, Displayed, Error |

## 

## **5\. Screens touched**

| Actor action | Screen ID |
| :---- | :---- |
| Select view EV battery health status (a1) | S-F01-02 → S-F01-06 |
| View Battery State of Health (SoH) metrics & reports (a2) | S-F01-06 |
| View error notification popup: 'Cannot connect to station' (error_view) | S-F01-06 (Error notification (BE) popup) |

## 

## **6\. Open questions**

| \# | Question | Blocking? | Default applied |
| :---- | :---- | :---- | :---- |
| 1 | Standard time interval delay between automated retry requests? | No | Set a 2-second delay between consecutive request retries |

## 

## **7\. Verification log**

| Check | Result | By | Date |
| :---- | :---- | :---- | :---- |
| Renders on mermaid.live | Pass | Đinh Thu Thảo | 2026-10-26 |
| Every decision has guards on all outgoing flows, no overlap, no gap | Pass | Đinh Thu Thảo | 2026-10-26 |
| No action has two incoming flows (merge used instead) | Pass | Đinh Thu Thảo | 2026-10-26 |
| Every fork has a matching join | Pass | Đinh Thu Thảo | 2026-10-26 |
| Every system action cites an FR | Pass | Đinh Thu Thảo | 2026-10-26 |
| Every FR of this use case appears in the diagram | 3 of 3 | Đinh Thu Thảo | 2026-10-26 |
| Lane names match the Context Diagram and the use case actors; no Web App, API or database lanes | Pass | Đinh Thu Thảo | 2026-10-26 |
| Related sequence diagram uses the same names and FR IDs; its alt fragments appear here as decisions | Pass | Đinh Thu Thảo | 2026-10-26 |
| AI critic prompt run, findings resolved or logged as open questions | Pass | Đinh Thu Thảo | 2026-10-26 |


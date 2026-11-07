# **AD-UC-1.03**

| Field | Value |
| :---- | :---- |
| AD ID | AD-UC-1.03 |
| Use case | UC-1.03 View Toll Station History & Calculate Minimum Travel Distance |
| Feature | F-01 Vehicle Identification & Status |
| Primary actor | Car Owner |
| Lanes | Car Owner · TASCO Mobility System · VETC Toll System |
| Inputs (L3-L5) | Context Diagram v1 · Use Case Diagram v1 · Sequence: SD-UC-1.03 · W1 rows (FR-01-03-FE, FR-01-03-BE, FR-01-03-INT) |
| Trigger | Car owner selects view toll history and route distance |
| Preconditions | Vehicle profile exists in the system and car owner is authenticated |
| Success end state | Toll station history is successfully retrieved, parsed, distance calculated, and displayed on UI |
| Failure end state(s) | Connection timeout or API failure exceeds max 3 retries, leading to an error notification |
| FRs covered | FR-01-03-FE, FR-01-03-BE, FR-01-03-INT |
| Business rules applied | BR-RET-01 (VETC Integration Retry Rule) |
| Version / status | v0.3 · Reviewed |
| Author | Đinh Thu Thảo |

## 

## **1\. Diagram**

*Code:* 

```mermaid
swimlane-beta TB
 subgraph CAR["Car Owner"]
   start@{ shape: sm-circ }
   a1("Select view toll history and route distance (FR-01-03-FE)")
   a2("See toll history timeline and minimum calculated distance (FR-01-03-FE)")
   error_view("View error notification popup: 'Connection timeout, please try again' (FR-01-03-FE)")
   stop1@{ shape: fr-circ }
   stop2@{ shape: fr-circ }
 end
 subgraph SYS["TASCO Mobility System"]
   s1("Send toll data request to VETC System (FR-01-03-BE)")
   s3("Parse toll station logs and timestamp sequence (FR-01-03-BE)")
   s4("Apply route algorithm to calculate minimum travel distance (FR-01-03-BE)")
   s2("Max 3 retries exceeded: log connection timeout & notify user (FR-01-03-BE)")
   m4{" "}
 end
 subgraph VETC["VETC Toll System"]
   v1("Retrieve station passing logs and toll transaction history (FR-01-03-INT)")
   d1{" VETC data available & API connection?"}


 end


 start --> a1 --> m4
 m4 --> s1 --> v1 --> d1
 d1 -->|"[retry < 3]"| m4 
 d1 -->|"[success]"| s3 --> s4 --> a2 --> stop1
 d1 -->|"[max retries exceeded]"| s2 --> error_view --> stop2

```
**Reading notes:**

* Request & VETC Integration: The car owner selects to view toll history, triggering the TASCO system to request station passing logs from the VETC system.  
* Validation & Retry Loop: The system checks if the connection and data are valid; if an API failure occurs with fewer than 3 attempts, it retries via merge node m4.  
* Success & Distance Calculation: Valid logs are parsed and processed through a routing algorithm to compute the minimum travel distance for display on the UI.   
* Error Handling: If maximum retries are exceeded, a timeout error notification is displayed on UI and terminates at a dedicated end node stop2.

## **2\. Node trace**

| Node | Kind | Text | Traces to |
| :---- | :---- | ----- | :---- |
| a1 | Action | Select view toll history and route distance | FR-01-03-FE |
| m4 | Merge | Control node merging initial request path and retry loop-back path | N/A (Control Node) |
| s1 | Action | Send toll data request to VETC System | FR-01-03-BE |
| v1 | Action | Retrieve station passing logs and toll transaction history | FR-01-03-INT |
| d1 | Decision | VETC data available & API connection? | FR-01-03-BE / BR-RET-01 |
| s3 | Action | Parse toll station logs and timestamp sequence | FR-01-03-BE |
| s4 | Action | Apply route algorithm to accumulate standard physical distances between consecutive stations ($S_i \to S_{i+1}$):<br>$D_{total} = \sum_{i=1}^{n-1} d(S_i, S_{i+1})$ | FR-01-03-BE / BR-DIST-01 |
| a2 | Action | See toll history timeline and minimum calculated distance | FR-01-03-FE |
| s2 | Action | Max 3 retries exceeded: log connection timeout & notify user | FR-01-03-BE |
| error\_view | Action | View error notification popup: “Connection timeout, please try again” | FR-01-03-FE |

## 

**Business rule description**

| Rule ID | Description | Applied Node |
| :--- | :--- | :--- |
| BR-RET-01 | VETC Integration Retry Rule: When requesting toll transaction logs from the external VETC Toll System, if a connection timeout or API failure occurs, the TASCO backend must automatically retry, up to 3 retries after the initial request (maximum 4 attempts in total). If the 3rd retry (4th attempt) also fails, the system stops retrying, logs the timeout error and notifies the user. | d1 |
| BR-DIST-01 | Minimum Travel Distance Accumulation Rule: The system sorts station passing logs chronologically ($S_1 \rightarrow S_2 \rightarrow \dots \rightarrow S_n$) and computes the total distance with: $$D_{\text{total}} = \sum_{i=1}^{n-1} d(S_i, S_{i+1})$$. Notations: $S_i, S_{i+1}$ are two consecutive toll stations passed by the vehicle, $d(S_i, S_{i+1})$ is the standard physical distance between consecutive stations, provided directly by the VETC system. Turnaround and duplicate rule: A turnaround $A \rightarrow B \rightarrow A$ still counts both segments ($A$ to $B$ and $B$ to $A$); only consecutive records at the same station ($S_i = S_{i+1}$, e.g. duplicate card scan) have an increment of $0$. The final result is rendered on the UI. | s4 |

## **3\. Branches and acceptance tests**

| Branch | Given | When | Then |
| :---- | :---- | :---- | :---- |
| d1 \[retry \< 3\] | Toll history request is sent to VETC | VETC times out or returns an API error and the number of retries done is 1, 2 | System automatically loops back via merge node m4 to retry the request |
| d1 \[success\] | Toll history request is sent to VETC | VETC API returns valid station passing logs successfully | System parses logs, gets distances from the Map Service, accumulates the minimum travel distance and displays the timeline |
| d1 \[max retries exceeded\] | Toll history request failed 3 times previously | VETC API fails or times out on the 4th attempt (3rd retry) | System logs connection timeout, stops retries, displays error popup, and stops at stop2 |

## 

## **4\. Object states used**

| Object | States in this diagram | Full state list (all diagrams) |
| :---- | :---- | :---- |
| Toll Transaction Data | Requested \-\> Retrieved \-\> Parsed \-\> Calculated | Initial, Requested, Retrieved, Parsed, Calculated, Failed |
| History View | Initial \-\> Displayed | Initial, Loading, Displayed, Error |

## 

## **5\. Screens touched**

| Actor action | Screen ID |
| :---- | :---- |
| Select view toll history and route distance (a1) | S-F01-02 → S-F01-03 |
| See toll history timeline and minimum calculated distance (a2) | S-F01-03 |
| View error notification popup: “Connection timeout, please try again” (error_view) | S-F01-03 (Error notification (BE) popup) || Actor action | Screen ID |
| :---- | :---- |
|  |  |

## 

## **6\. Open questions**

| \# | Question | Blocking? | Default applied |
| :---- | :---- | :---- | :---- |
| 1 | Exact timeout threshold in seconds before triggering an automatic retry? | No | Set connection timeout limit to 5 seconds per attempt |

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

# 


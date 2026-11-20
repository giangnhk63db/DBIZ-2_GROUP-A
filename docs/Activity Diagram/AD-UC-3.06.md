# **Activity Diagram: AD-UC-3.06**

| Field | Value |
| ----- | ----- |
| AD ID | AD-UC-3.06 |
| Use case | UC-3.06 Package and forward verified report and market price to Carpla staff |
| Feature | F-03 Sell Request & Preliminary Quote |
| Primary actor | none: started by the system; the Car Owner is only notified on failure |
| Lanes | Tasco Mobility System (see OQ 5\) · Carpla CRM (see OQ 4\) · Car Owner |
| Inputs (L3-L5) | Context Diagram · Use Case Diagram (Sell Request & Preliminary Quote) · Sequence: Submit Sale Request Sequence Diagram (steps S-9.11 to S-9.13) · W1 rows: FR-03-03-BE (FR-03-03-FE; FR-03-04, FR-03-05, FR-03-06); weekly market price comes from F-02 (FR-02-10-BE) · Referenced activity diagrams: AD-UC-3.01, AD-UC-3.03, AD-UC-3.04, AD-UC-3.05 |
| Trigger | The legal re-check of the sale request is finished (end of UC-3.02, gathering sufficient data from the Re-check process) |
| Preconditions | A sale request exists; the verified report exists for the VIN; the car owner declared mileage, desired price and phone number are stored; the latest weekly market price may exist (UC-2.09) |
| Success end state | Carpla receives the data package (stop1) |
| Failure end state(s) | After 3 retries the sale request is kept as not forwarded and the owner is notified (stop2) |
| FRs covered | FR-03-03 (L2, Must, EARS: package the verified report and the latest market price and make the package available to Carpla staff; IF no market price exists, forward with the price marked as unavailable).  |
| Business rules applied | none |
| Version / status | v0.3 · Reviewed |
| Author | Nguyễn Hương Giang  |

1. ## **Code**

```mermaid
swimlane-beta TB
subgraph CO["Car Owner"]
start@{ shape: sm-circ }
e1("Submit sale request (AD-UC-3.01)")
e2("Receive and view car price (FR-03-06)")
stop1@{ shape: fr-circ }
stop2@{ shape: fr-circ }
end
subgraph TMS["Tasco Mobility System"]
s00("Update legal status data (AD-UC-3.03, AD-UC-3.04, AD-UC-3.05)")
s1("Collect verified inspection & status, latest market price and car owner declared mileage, desired price and phone number (FR-03-03-BE)")
d1{"Market price available?(FR-03-03-BE)"}
s2("Package verified inspection & status, latest market price and car owner declared mileage, desired price and phone number (FR-03-03-BE)")
s3("Package verified inspection & status and car owner declared information with price marked as unavailable (FR-03-03-BE)")
m1{" "}
d2{"Forward package to Carpla CRM (FR-03-03-BE)"}
s4("Wait 5 seconds and add 1 to retry count (FR-03-03-BE)")
s5("Keep sale request as not forwarded for later resending (FR-03-03-BE)")
s6("Notify car owner that the sale request could not be forwarded (FR-03-06-FE)")
end
subgraph CRM["Carpla CRM"]
c1("Receive packaged verified inspection & status, market price and car owner declared information (FR-03-04)")
c2("Review, decide price and send to car owner (FR-03-04, FR-03-05)")
end
start --> e1 --> s00 --> s1 --> d1
d1 -->|"Yes"| s2 --> m1
d1 -->|"No"| s3 --> m1
m1 --> d2
d2 -->|"Forwarded"| c1 --> c2 --> e2 --> stop1
d2 -->|"Forward failed and fewer than 3 retries made"| s4 --> m1
d2 -->|"Forward failed and 3 retries made"| s5 --> s6 --> stop2
```


2. ## **Node trace** 

| Node | Kind | Text | Traces to |
| ----- | ----- | ----- | ----- |
| e1 | Action  | Submit sale request | AD-UC-3.01 |
| s00 | Action  | Update legal status data | AD-UC-3.03, AD-UC-3.04, AD-UC-3.05 |
| s1 | Action | Collect verified inspection & status, latest market price and car owner declared mileage, desired price and phone number | FR-03-03-BE |
| d1 | Decision | “Market price available?” Guards: Yes / No | (none) |
| s2 | Action | Package verified inspection & status, latest market price and car owner declared mileage, desired price and phone number | FR-03-03-BE |
| s3 | Action | Package verified inspection & status and car owner declared information with price marked as unavailable | FR-03-03-BE |
| m1 | Merge | (none) | (none) |
| d2 | Decision | “Forward package to Carpla CRM”. Guards: Forwarded / Forward failed and fewer than 3 retries made / Forward failed and 3 retries made | FR-03-03-BE |
| s4 | Action | Wait 5 seconds and add 1 to retry count | FR-03-03-BE |
| s5 | Action | Keep sale request as not forwarded for later resending | FR-03-03-BE |
| s6 | Action | Notify car owner that the sale request could not be forwarded | FR-03-06-FE |
| c1 | Action (Carpla CRM) | Receive packaged verified inspection & status, market price and car owner declared information | FR-03-04 |
| c2 | Action (Carpla CRM) | Review, decide price and send to car owner | FR-03-04, FR-03-05 |
| e2 | Action (Car Owner) | Receive and view car price | FR-03-06 |

3. ## **Branches and Acceptance tests**

| Branch | Given | When | Then |
| ----- | ----- | ----- | ----- |
| d1 \[Yes\] | The car owner has submitted a sale request and the legal status data is updated; a latest weekly market price exists | The system collects the data | The package contains the verified inspection & status, the market price and the owner-declared mileage, desired price and phone number, and goes on to the forward step |
| d1 \[No\] | Same as above, but no market price exists | The system collects the data | The package is built with the price marked as unavailable and is still forwarded |
| d2 \[Forwarded\] | A package is forwarded successfully to Carpla CRM system | The system forwards the package | Carpla staff receive it and decide the price; the car owner receives and views the price; flow ends at stop1 |
| d2 \[Forward failed, fewer than 3 retries\] | The forward attempt fails and the retry count is below 3 | The failure is detected | The system waits 5 seconds, adds 1 to the retry count (s4) and forwards again (back through m1) |
| d2 \[Forward failed, 3 retries made\] | The forward attempt fails and the retry count is 3 | The failure is detected | The sale request is kept as not forwarded (s5), the owner is notified (s6), and flow ends at stop2 |

4. ## **Object states used**

| Object | States in this diagram | Full state list (all diagrams) |
| ----- | ----- | ----- |
| Package forwarded | Forwarding, Forwarded,  Unable to  forward | Forwarding, Forwarded,  Unable to  forward |
| Market price  | No data, Have data | No data, Have data |

5. ## **Screen touched**
N/A

6. ## **Open question**

| \# | Question | Blocking? | Default applied |
| ----- | ----- | ----- | ----- |
| 1 | \[NEEDS CLARIFICATION: s00 refers to three diagrams (AD-UC-3.03, 3.04, 3.05). Do they run in sequence or in parallel, and what happens if the legal status update fails? This diagram has no failure branch after s00.\] | No | Treated as one step that always succeeds; failure handled in previous activity diagram |
| 2 | \[NEEDS CLARIFICATION: The step “Set retry count to 0” was removed, so the diagram no longer shows where the retry count starts. Is the initial value defined elsewhere (sequence diagram, FR-03-03-BE)?\] | No | The retry count starts at 0 when the request is created; no step in this diagram |

7. ## **Verification**

| Check | Result | By | Date |
| ----- | ----- | ----- | ----- |
| Renders on mermaid.live | Pass | Nguyễn Hương Giang | 2026-11-04 |
| Every decision has guards on all outgoing flows, no overlap, no gap | Pass by reading: d1 has two guards that cover each other | Nguyễn Hương Giang | 2026-11-04 |
| No action has two incoming flows (merge used instead) | Pass by reading | Nguyễn Hương Giang | 2026-11-04 |
| Every fork has a join | Pass: no fork in this diagram | Claude draft | 2026-11-04 |
| Every system action cites an FR | Pass | Nguyễn Hương Giang | 2026-11-04 |
| Every FR of this use case appears in the diagram | Pass | Nguyễn Hương Giang | 2026-11-04 |
| Related sequence diagram uses the same names and FR IDs; its alt fragments appear here as decisions | Pass | Nguyễn Hương Giang | 2026-11-04 |
| AI critic prompt run, findings resolved or logged as open questions | Pass | Nguyễn Hương Giang | 2026-11-04 |


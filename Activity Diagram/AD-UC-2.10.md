#### **AD-UC-2.10**

| Field | Value |
| :---- | :---- |
| AD ID | AD-UC-2.10 |
| Use case | UC-2.10 Back-calculate 12-Week Market Price History |
| Feature | F-02 Market Price |
| Primary actor | Car Owner |
| Lanes | Car Owner · TASCO Mobility System |
| Inputs (L3-L5) | Context Diagram (Figure 3\) · Use Case Diagram (Figure 6\) · Sequence: Figure 14, first-sign-up back-calculation (S-7.01 → S-7.21) · W1 rows (FR-02-07-INT, FR-02-08-INT, FR-02-08-BE, FR-02-06-BE, FR-02-04-BE, FR-02-09-FE, FR-02-10-FE, FR-02-11-FE) · UC-2.03, UC-2.06, UC-2.07, UC-2.08 and UC-2.09 |
| Trigger | A car owner adds a vehicle for the first time and the system needs to generate its 12-week market-price history |
| Preconditions | The car owner can add a vehicle; the vehicle's model can be identified; the system can access the historical marketplace listings and the relevant weekly checkpoints |
| Success end state | The system displays the vehicle's market price and 12-week trend chart. Each checkpoint with sufficient data has an estimated/back-calculated price record; checkpoints without sufficient data are recorded as INSUFFICIENT\_DATA |
| Failure end state(s) | For a checkpoint, mileage is missing or implausible, or fewer than 30 comparable listings remain; the system records INSUFFICIENT\_DATA with the back-calculation flag and continues to the next checkpoint. The supplied flow does not define a separate terminal failure that prevents the chart from being displayed |
| FRs covered | FR-02-07, FR-02-08, FR-02-06, FR-02-04, FR-02-09, FR-02-10, FR-02-11 |
| Business rules applied | UC-2.10 / Figure 14: a first-time vehicle model triggers a 180-day listing-history sync; each of the 12 weekly checkpoints uses verified mileage, listing cleaning, comparable-listing selection and price/confidence calculation; each generated price record is flagged as back-calculated; insufficient checkpoints are recorded as INSUFFICIENT\_DATA with the back-calculation flag |
| Version / status | v0.1 · Draft |
| Author | Nguyễn Hà Phương |

## 

1. **Diagram:**

\---  
config:  
 flowchart:  
   nodeSpacing: 80  
   rankSpacing: 90  
   curve: linear  
\---  
flowchart TB  
subgraph OWNER\["Car Owner"\]  
  start@{ shape: f-circ }  
  a1**(**"Add a new vehicle"**)**  
  a2**(**"View market price and 12-week trend chart"**)**  
  stop1@{ shape: fr-circ }  
end  
subgraph SYS\["TASCO Mobility System"\]  
  direction TB  
  d1{"Car model already in the listing store?"}  
  s1**(**"Display Calculating Price (FR-02-09-FE**)**")  
  s2**(**"Sync 180-day listing history (UC-2.06**,** FR-02-07-INT**)**")  
  m2{" "}  
  s3**(**"Check verified mileage at the checkpoint (UC-2.03**,** FR-02-08-INT**)**")  
  d2{"Mileage available and plausible?"}  
  s4**(**"Clean listings active at the checkpoint (FR-02-03-BE,FR-02-08-BE**)**")  
  s5**(**"Select comparable vehicles (FR-02-05-BE, FR-02-08-BE**)**")  
  d3{"Sufficient sample size?"}  
  s6**(**"Calculate price and confidence (FR-02-06-BE, FR-02-04-BE, FR-02-08-BE**)**")  
  s7**(**"Save price record with back-calculation flag (FR-02-08-BE,FR-02-06-BE,FR-02-04-BE**)**")  
  m4{" "}  
  s8**(**"Record INSUFFICIENT\_DATA with back-calculation flag (FR-02-08-INT**,**  FR-02-08-BE, FR-02-06-BE, FR-02-04-BE**)")**  
  m3{" "}  
  d4{"Another of the 12 weekly checkpoints left?"}  
  s9**(**"Display market price and 12-week trend chart (FR-02-09-FE**,** FR-02-10-FE**)**")  
end  
start \--\> a1 \--\> d1  
d1 \--\>\|"\[no\]"\| s1 \--\> s2 \--\> m2  
d1 \--\>\|"\[yes\]"\| m2  
m2 \--\> s3 \--\> d2  
d2 \--\>\|"\[yes\]"\| s4 \--\> s5 \--\> d3  
d2 \--\>\|"\[no\]"\| m4  
d3 \--\>\|"\[yes\]"\| s6 \--\> s7 \--\> m3  
d3 \--\>\|"\[no\]"\| m4  
m4 \--\> s8 \--\> m3  
m3 \--\> d4  
d4 \--\>\|"\[yes\]"\| m2  
d4 \--\>\|"\[no\]"\| s9 \--\> a2 \--\> stop1

**Reading notes**

- If the vehicle model is not yet in the listing store, the system displays “Calculating Price” and synchronizes 180 days of listing history. If the model is already present, it skips that first-time setup and proceeds to the checkpoint calculations.  
- For each of the 12 weekly checkpoints, the system checks verified mileage, cleans listings active at that checkpoint, selects comparable vehicles and checks whether at least 30 comparable listings remain.  
- A successful checkpoint produces a price record with the back-calculation flag. A checkpoint that fails the mileage or sample-size check produces INSUFFICIENT\_DATA with the same flag. Both paths rejoin before the system checks whether another checkpoint remains.  
- After all 12 checkpoints have been processed, the system displays the market price and 12-week trend chart.  
- The diagram shows one vehicle added by the owner. Any iteration over multiple vehicles is outside this use case unless the UC-2.10 description explicitly requires it.

2. **Node Trace:** 

| Node | Kind | Text | Traces to |
| ----- | ----- | ----- | ----- |
| a1 | Action (Car Owner) | Add a new vehicle | UC-2.10 trigger; S-7.01 |
| d1 | Decision | Car model already in the listing store? | FR-02-07-BE (detect first sign-up); S-7.01 to S-7.03 |
| s1 | Action | Display Calculating Price | FR-02-09-FE (message “Calculating” when no weekly price exists); S-7.04 to S-7.05 |
| s2 | Action | Sync 180-day listing history (UC-2.06) | FR-02-07-INT (fetch 180-day listing history); S-7.06 to S-7.10; extend relation |
| s3 | Action | Check verified mileage at the checkpoint (UC-2.03) | FR-02-08-INT; S-7.11 to S-7.12; include relation |
| d2 | Decision | Mileage available and plausible? | FR-02-08-INT; FR-02-01-BE (mileage plausibility check, UC-2.03); S-7.13 (alt: mileage missing or implausible) |
| s4 | Action | Clean listings active at the checkpoint (UC-2.07) | FR-02-03-BE, FR-02-08-BE; S-7.14 to S-7.15 |
| s5 | Action | Select comparable vehicles (UC-2.08) | FR-02-05-BE, FR-02-08-BE; S-7.16 |
| d3 | Decision | Sufficient sample size? | FR-02-05-BE, FR-02-04-BE, FR-02-06-BE, FR-02-08-BE; S-7.17 |
| s6 | Action | Calculate price and confidence (UC-2.09) | FR-02-06-BE, FR-02-04-BE, FR-02-08-BE; S-7.18 |
| s7 | Action | Save price record with back-calculation flag | FR-02-08-BE, FR-02-06-BE, FR-02-04-BE; S-7.19 |
| s8 | Action | Record INSUFFICIENT\_DATA with back-calculation flag | FR-02-08-BE, FR-02-06-BE, FR-02-04-BE; S-7.13 and S-7.17 |
| d4 | Decision | Another of the 12 weekly checkpoints left? | FR-02-08-BE; Figure 14 loop; S-7.20 |
| s9 | Action | Display market price and 12-week trend chart | FR-02-09-FE, FR-02-10-FE; S-7.21 |
| a2 | Action (Car Owner) | View market price and 12-week trend chart | FR-02-10-FE, FR-02-11-FE; S-7.21 |
| m2 | Merge | Joins d1 \[yes\], s2 and d4 \[yes\] (loop back) | N/A |
| m4 | Merge | Joins d2 \[no\] and d3 \[no\] | N/A |
| m3 | Merge | Joins s7 and s8 | N/A |

3. ##  **Branches and acceptance tests**

| Branch | Given | When | Then |
| :---- | :---- | :---- | :---- |
| d1 \[no\] | The owner adds a vehicle whose model is not yet in the listing store | The system checks whether the model already exists | The system displays “Calculating Price”, synchronizes 180 days of listing history, and then begins checkpoint processing |
| d1 \[yes\] | The owner adds a vehicle whose model is already in the listing store | The system checks whether the model already exists | The system skips the first-time history sync and proceeds to checkpoint processing |
| d2 \[yes\] | Verified mileage is available and passes the plausibility check for the current checkpoint | The system checks mileage | The system cleans listings active at that checkpoint and continues to comparable-listing selection |
| d2 \[no\] | Verified mileage is missing or fails the plausibility check for the current checkpoint | The system checks mileage | The system records INSUFFICIENT\_DATA with the back-calculation flag for that checkpoint and proceeds to the checkpoint-loop decision |
| d3 \[yes\] | At least 30 comparable listings remain after selection and outlier removal for the current checkpoint | The system checks the sample size | The system calculates price and confidence, saves the price record with the back-calculation flag, and proceeds to the checkpoint-loop decision |
| d3 \[no\] | Fewer than 30 comparable listings remain for the current checkpoint | The system checks the sample size | The system records INSUFFICIENT\_DATA with the back-calculation flag and proceeds to the checkpoint-loop decision |
| d4 \[yes\] | The current checkpoint is not the final one of the 12 weekly checkpoints | The current checkpoint's success or insufficient-data result has been recorded | The system proceeds to the next checkpoint and repeats the mileage, cleaning, selection and calculation steps |
| d4 \[no\] | The final weekly checkpoint has been processed | The system checks whether another checkpoint remains | The system displays the market price and 12-week trend chart and ends the use case |

## **4\. Object states used**

| Object | States in this diagram | Full state list (all diagrams) |
| :---- | :---- | :---- |
| Vehicle Model in Listing Store | Already present; not present (first-time setup required) | Not specified in the supplied material |
| Weekly Price Record | Saved with back-calculation flag; INSUFFICIENT\_DATA with back-calculation flag | Saved; INSUFFICIENT\_DATA; API\_FAILED (UC-2.05 / Figure 13); Estimated (backfill flag, UC-2.10) — terminology requires confirmation |
| Verified Mileage at Checkpoint | Available and plausible; missing or implausible | Not specified in the supplied material |
| Comparison Level | Level 0; Level 1; Level 2 (set by UC-2.08 and used during back-calculation) | Level 0, Level 1, Level 2 |
| 12-Week Back-calculation | Checkpoint pending; checkpoint processed; all 12 checkpoints processed | Not specified in the supplied material |

## **5\. Screens touched**

## **6\. Open questions**

| \# | Question | Blocking? | Default applied |
| :---- | :---- | :---- | :---- |
| 1 | Does the 180-day listing-history sync happen only when the model is absent from the listing store, or whenever a user adds a vehicle? | No | Follow the supplied diagram: sync only when the model is absent |
| 2 | Is the 12-week back-calculation based on 12 historical weekly checkpoints in addition to the current week, or does the 12-week period include the current week? | Yes | Diagram says 12 weekly checkpoints but does not define the date boundaries; confirm before implementation |
| 3 | What is the exact rule for deriving verified mileage at each historical checkpoint from UC-2.03? | No | Reuse UC-2.03's verified-mileage result; do not invent an interpolation or estimation method |
| 4 | If mileage is missing/implausible or the comparable sample is insufficient at one checkpoint, should the system continue with the remaining checkpoints? | No | Yes, following Figure 14: record INSUFFICIENT\_DATA, merge and continue the loop |
| 5 | Does the chart display gaps for INSUFFICIENT\_DATA weeks, or does it apply a separate display/interpolation rule? | No | No interpolation is assumed; the chart displays only what the price records support until the display rule is confirmed |
| 6 | The node trace cites FR-02-08-INT for the back-calculation flag and also FR-02-08-BE for record persistence. Which FR owns the flag and which owns saving the record? | No | Retain both references on s7/s8 pending W1 ownership confirmation |
| 7 | The diagram's s9 action displays the chart, while the original node trace lists a2 as the owner viewing it. Should the actor action be represented separately after the system display? | No | The revised diagram treats system display as s9 and the owner viewing the result as the final outcome; add a separate owner action only if the sequence diagram distinguishes it |
| 8 | Does the price record's back-calculation flag imply the Estimated state, or is Estimated a separate status from Saved? | No | Keep “saved with back-calculation flag” as the action outcome; do not equate the flag with a status until the data model confirms |
| 9 | The supplied diagram references FR-02-09-FE, FR-02-10-FE and FR-02-11-FE, while the node trace attributes s9 to FR-02-09-FE and FR-02-10-FE and a2 to FR-02-10-FE and FR-02-11-FE. Which FRs own display versus viewing? | No | Keep FR-02-09 and FR-02-10 on the system display; record FR-02-11 as an actor-viewing trace to confirm |
| 10 | The source uses suffixes such as \-BE, \-INT and \-FE; the completed UC-2.09 example shortens FR IDs. | No | Use shortened IDs in the diagram for readability, but retain the full suffixes in the Inputs field and this open question until the canonical W1 format is confirmed |

## **7\. Verification log**

| Check | Result | By | Date |
| :---- | :---- | :---- | :---- |
| Renders on mermaid.live | Mermaid code supplied; external rendering not independently verified | Nguyễn Hà Phương | 2026-11-04 |
| Every decision has guards on all outgoing flows, no overlap, no gap | Pass: d1-d4 each have yes/no guards | Nguyễn Hà Phương | 2026-11-04 |
| No action has two incoming flows (merge used instead) | Pass: m1, m2 and m3 handle converging paths | Nguyễn Hà Phương | 2026-11-04 |
| Every fork has a matching join | Pass: no fork is used | Nguyễn Hà Phương | 2026-11-04 |
| Every system action cites an FR | Pass: s1-s9 cite one or more FRs; actor action a1 is traced to UC-2.10 trigger | Nguyễn Hà Phương | 2026-11-04 |
| Every FR of this use case appears in the diagram | 7 FR groups represented: FR-02-03, FR-02-04, FR-02-05, FR-02-06, FR-02-07, FR-02-08, FR-02-09 and FR-02-10; FR-02-11 remains an actor-viewing trace/open question | Nguyễn Hà Phương | 2026-11-04 |
| Lane names match the Context Diagram and the use case actors; no Web App, API or database lanes | Car Owner and TASCO Mobility System follow the supplied diagram; Context Diagram naming still requires confirmation | Nguyễn Hà Phương | 2026-11-04 |
| Related sequence diagram uses the same names and FR IDs; its alt fragments appear here as decisions | Figure 14 S-7.01-S-7.21 is represented; confirm exact alt labels and whether the chart-viewing actor step is separate | Nguyễn Hà Phương | 2026-11-04 |
| AI critic prompt run, findings resolved or logged as open questions | Draft self-check completed; independent critic review remains outstanding | Nguyễn Hà Phương | 2026-11-04 |


#### **AD-UC-2.08**

| Field | Value |
| :---- | :---- |
| AD ID | AD-UC-2.08 |
| Use case | UC-2.08 Select Comparable Listings |
| Feature | F-02 Market Price |
| Primary actor | Weekly Timer |
| Lanes | Weekly Timer · TASCO Mobility System |
| Inputs (L3-L5) | Context Diagram (Figure 3\) · Use Case Diagram (Figure 6\) · Sequence: Figure 13, comparable-listing selection (S-6.20 → S-6.24) · W1 rows (FR-02-05-BE, FR-02-04-BE, FR-02-06-BE) · UC-2.08 description |
| Trigger | Weekly timer fires and the weekly pricing job reaches comparable-listing selection for a vehicle |
| Preconditions | The vehicle's verified mileage is available and plausible (UC-2.03); cleaned marketplace listings are available from UC-2.07; the vehicle's brand, model, manufacturing year and mileage are known |
| Success end state | At least 30 comparable listings remain after outlier removal, and the comparable listings plus the comparison level are passed to UC-2.09 |
| Failure end state(s) | Fewer than 30 comparable listings remain after Level 2; the system records INSUFFICIENT\_DATA and stops without passing a comparable sample for price calculation |
| FRs covered | FR-02-05 (selection and comparison level); FR-02-04 and FR-02-06 are referenced for the insufficient-data outcome in the supplied trace |
| Business rules applied | UC-2.08 rules: Level 0 uses the same brand/model, manufacturing year within ±1 and mileage within ±20%; remove price outliers using IQR; if fewer than 30 remain, expand to Level 1 (year ±2); if still fewer than 30, expand to Level 2 (mileage ±40%); if fewer than 30 remain after Level 2, record INSUFFICIENT\_DATA |
| Version / status | v0.1 · Draft |
| Author | Nguyễn Hà Phương |

## 

1. **Diagram:**
```mermaid
---  
config:  
 flowchart:  
   nodeSpacing: 80  
   rankSpacing: 90  
   curve: linear  
---  
flowchart TB  
subgraph TIMER\["Weekly Timer"\]  
  start@{ shape: f-circ }  
  t1@{ shape: hourglass, label: "Every week" }  
end  
subgraph SYS\["TASCO Mobility System"\]  
  direction TB  
  s1**(**"Select Level 0 listings: year \+-1**,** mileage \+-20% (FR-02-05-BE**)**")  
  s2**(**"Remove price outliers by IQR (FR-02-05-BE**)**")  
  d1{"At least 30 listings?"}  
  s3**(**"Expand to Level 1 (year \+-2**)**, select and remove outliers **(**FR-02-05-BE**)**")  
  d2{"At least 30 listings?"}  
  s4**(**"Expand to Level 2 (mileage \+-40%**)**, select and remove outliers **(**FR-02-05-BE**)**")  
  d3{"At least 30 listings?"}  
  m1{" "}  
  s5**(**"Pass comparable listings and level to price calculation (FR-02-05-BE**)**")  
  s6**(**"Record INSUFFICIENT\_DATA and stop (FR-02-04-BE**,** FR-02-05-BE,FR-02-06-BE**)**")  
  stop1@{ shape: fr-circ }  
  stop2@{ shape: fr-circ }  
end  
start \--\> t1 \--\> s1 \--\> s2 \--\> d1  
d1 \--\>\|"\[yes\]"\| m1  
d1 \--\>\|"\[no\]"\| s3 \--\> d2  
d2 \--\>\|"\[yes\]"\| m1  
d2 \--\>\|"\[no\]"\| s4 \--\> d3  
d3 \--\>\|"\[yes\]"\| m1  
d3 \--\>\|"\[no\]"\| s6 \--\> stop2  
m1 \--\> s5 \--\> stop1
```
**Reading notes**

- The system applies the expansion levels in order and stops at the first level that leaves at least 30 listings after IQR outlier removal.  
- Level 0 is the narrowest comparison set; Level 1 widens the manufacturing-year tolerance; Level 2 widens the mileage tolerance.  
- The system passes both the selected listings and the level reached to UC-2.09 because the comparison level affects the confidence assignment.  
- This diagram covers one target vehicle. Iteration over vehicle models and vehicles belongs to the parent weekly job (UC-2.04), not this use case.

2. **Node Trace:**   
   

| Node | Kind | Text | Traces to |
| ----- | ----- | ----- | ----- |
| t1 | Time event | Every week (weekly timer fires) | FR-02-01-BE2; S-6.01 |
| s1 | Action | Select Level 0 listings: year \+-1, mileage \+-20% | FR-02-05-BE (same brand, model, year within ±1, mileage within 20%); S-6.20 (minimum tolerance ±10,000 km from the UC-2.08 description) |
| s2 | Action | Remove price outliers by IQR | FR-02-05-BE; S-6.21 (the IQR rule is from the UC-2.08 description, not in the FR text) |
| d1 | Decision | At least 30 listings? (after Level 0\) | FR-02-05-BE; S-6.22 (alt: fewer than 30 and expansion available) |
| s3 | Action | Expand to Level 1 (year \+-2), select and remove outliers | FR-02-05-BE; S-6.22 |
| d2 | Decision | At least 30 listings? (after Level 1\) | FR-02-05-BE; S-6.22 (loop until 30 listings or Level 2\) |
| s4 | Action | Expand to Level 2 (mileage \+-40%), select and remove outliers | FR-02-05-BE; S-6.23  |
| d3 | Decision | At least 30 listings? (after Level 2\) | FR-02-05-BE, FR-02-04-BE, FR-02-06-BE; S-6.24 |
| s5 | Action | Pass comparable listings and level to price calculation | FR-02-05-BE; Figure 13 alt; S-6.24 |
| s6 | Action | Record INSUFFICIENT\_DATA and stop | FR-02-05-BE, FR-02-04-BE, FR-02-06-BE; S-6.24 |
| m1 | Merge | Joins d1 \[yes\], d2 \[yes\] and d3 \[yes\] | N/A |

## **3\. Branches and acceptance tests**

| Branch | Given | When | Then |
| :---- | :---- | :---- | :---- |
| d1 \[yes\] | Level 0 selection and IQR removal leave exactly 30 comparable listings | UC-2.08 checks the Level 0 sample size | The system passes the listings and Level 0 to price calculation; it does not expand further |
| d1 \[no\] | Level 0 selection and IQR removal leave 29 listings | UC-2.08 checks the Level 0 sample size | The system expands the year tolerance to Level 1 (±2), selects listings and removes outliers again |
| d2 \[yes\] | Level 1 selection and IQR removal leave at least 30 listings | UC-2.08 checks the Level 1 sample size | The system passes the listings and Level 1 to price calculation; it does not expand to Level 2 |
| d2 \[no\] | Level 1 selection and IQR removal leave 29 listings | UC-2.08 checks the Level 1 sample size | The system expands the mileage tolerance to Level 2 (±40%), selects listings and removes outliers again |
| d3 \[yes\] | Level 2 selection and IQR removal leave at least 30 listings | UC-2.08 checks the Level 2 sample size | The system passes the listings and Level 2 to price calculation |
| d3 \[no\] | Level 2 selection and IQR removal leave 29 listings | UC-2.08 checks the Level 2 sample size | The system records INSUFFICIENT\_DATA and stops; UC-2.09 must not calculate a price from this sample |

## **4\. Object states used**

| Object | States in this diagram | Full state list (all diagrams) |
| :---- | :---- | :---- |
| Comparable Listing Set | Level 0 selected; Level 1 selected; Level 2 selected; passed to price calculation; insufficient data | The supplied material defines the comparison levels and INSUFFICIENT\_DATA; it does not provide a complete object-state catalogue |
| Comparison Level | Level 0; Level 1; Level 2 | Level 0, Level 1, Level 2 |
| Comparable Sample | At least 30 listings; fewer than 30 listings | Minimum threshold is 30; a complete sample-status catalogue is not supplied |

## **5\. Screens touched**

## **6\. Open questions**

| \# | Question | Blocking? | Default applied |
| :---- | :---- | :---- | :---- |
| 1 | S-6.20 mentions a minimum tolerance of ±10,000 km, while the Level 0 description says mileage ±20%. Is the actual mileage window the greater of 20% and 10,000 km, or a different rule? | No | Diagram retains the stated ±20% label; the ±10,000 km note is flagged for confirmation |
| 2 | Does IQR outlier removal run independently after each expansion level, using the price distribution of that level's selected listings? | No | Yes, as implied by the supplied actions for Level 0, Level 1 and Level 2 |
| 3 | Is the 30-listing threshold evaluated after outlier removal at every level? | No | Yes, following the diagram's order: select → remove outliers → check sample size |
| 4 | The supplied node trace says “loop until 30 listings or Level 2”, but the diagram uses a sequential expansion without a loop-back edge. Is the sequential representation intended? | No | Keep the three explicit checks and expansions; no loop is needed because each level is attempted once |
| 5 | The source trace for the insufficient-data action includes FR-02-04 and FR-02-06 as well as FR-02-05. Which FR owns recording the status versus consuming it downstream? | No | Keep all three references on s6 pending W1 ownership confirmation |
| 6 | The supplied trace uses FR-02-05-BE and related suffixes, while the completed UC-2.09 example uses shortened FR IDs. | No | Use shortened IDs consistently in this draft; confirm canonical W1 identifiers |

## **7\. Verification log**

| Check | Result | By | Date |
| :---- | :---- | :---- | :---- |
| Renders on mermaid.live | Mermaid code supplied; external rendering not independently verified | Nguyễn Hà Phương | 2026-11-04 |
| Every decision has guards on all outgoing flows, no overlap, no gap | Pass: d1, d2 and d3 each have yes/no guards | Nguyễn Hà Phương | 2026-11-04 |
| No action has two incoming flows (merge used instead) | Pass: m1 joins the successful branches before s5 | Nguyễn Hà Phương | 2026-11-04 |
| Every fork has a matching join | Pass: no fork is used | Nguyễn Hà Phương | 2026-11-04 |
| Every system action cites an FR | Pass: s1-s6 cite one or more FRs | Nguyễn Hà Phương | 2026-11-04 |
| Every FR of this use case appears in the diagram | 3 referenced FRs: FR-02-04, FR-02-05, FR-02-06; ownership of the failure status remains open | Nguyễn Hà Phương | 2026-11-04 |
| Lane names match the Context Diagram and the use case actors; no Web App, API or database lanes | Weekly Timer and TASCO Mobility System follow the supplied diagram; Context Diagram naming still requires confirmation | Nguyễn Hà Phương | 2026-11-04 |
| Related sequence diagram uses the same names and FR IDs; its alt fragments appear here as decisions | Figure 13 S-6.20-S-6.24 is reflected in s1-s6 and d1-d3; confirm the sequence participant and exact alt-fragment labels | Nguyễn Hà Phương | 2026-11-04 |
| AI critic prompt run, findings resolved or logged as open questions | Draft self-check completed; independent critic review remains outstanding | Nguyễn Hà Phương | 2026-11-04 |

## 

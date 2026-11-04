**AD-UC-2.04 Calculate Weekly Market Price**

|Field|Value|
|-|-|
|AD ID|`AD-UC-2.04`|
|Use case|`UC-2.04 Calculate Weekly Market Price`|
|Feature|`F-02 Market Price`|
|Primary actor|none: started by time (Weekly timer)|
|Lanes|TASCO Mobility System (one system lane).|
|Inputs (L3-L5)|Context Diagram · Use Case Diagram (UC-02) · Sequence diagram 06, S-6.01 to S-6.27 · W1 rows FR-02-01 to FR-02-06|
|Trigger|The weekly timer fires at the fixed time|
|Preconditions|At least one car is tracked; the weekly job is scheduled; UC-2.05, UC-2.07, UC-2.03, UC-2.08, UC-2.09 are available|
|Success end state|Every tracked car has a weekly price record for this week: a price with a confidence level, or INSUFFICIENT\_DATA, or its previous price kept when the calculation failed|
|Failure end state(s)|No marketplace synced: API\_FAILED recorded, failure alert sent, no car priced this week|
|FRs covered|`FR-02-01-BE`. Cited from included UCs: `FR-02-02-INT1`, `FR-02-02-INT2`, `FR-02-03-BE`, `FR-02-04-BE`, `FR-02-05-BE`, `FR-02-06-BE`|
|Business rules applied|Rules used: 2,000 km/year threshold (configurable); at least 30 comparable listings|
|Version / status|v0.3 · Review|
|Author|Vũ Hương Ly|

## **1. Diagram**

```mermaid
swimlane-beta TB
  subgraph SYS["TASCO Mobility"]
    t1@{ shape: hourglass }
    s1("Sync listings from partner marketplaces, UC-2.05 (FR-02-02-INT1, FR-02-02-INT2)")
    d1{"At least one marketplace synced?"}
    s2("Record API_FAILED status (FR-02-02-INT2)")
    o1["Weekly price record [API_FAILED]"]
    s8("Send failure alert (FR-02-01-BE)")
    stop1@{ shape: fr-circ }
    m1{" "}
    d2{"Another tracked car to price?"}
    s3("Normalize and clean listings of the car's model, UC-2.07 (FR-02-03-BE)")
    s4("Check verified mileage plausibility, UC-2.03 (FR-02-01-BE)")
    d3{"Mileage plausible?"}
    s5("Select comparable vehicles, UC-2.08 (FR-02-05-BE)")
    d4{"At least 30 comparable listings?"}
    m2{" "}
    s6("Record INSUFFICIENT_DATA status (FR-02-01-BE, FR-02-06-BE)")
    o2["Weekly price record [INSUFFICIENT_DATA]"]
    s7("Calculate market price and confidence level, UC-2.09 (FR-02-06-BE, FR-02-04-BE)")
    d5{"Calculation succeeded?"}
    s9("Save weekly price record (FR-02-06-BE)")
    s10("Keep previous weekly price and log failure (FR-02-01-BE)")
    m3{" "}
    stop2@{ shape: fr-circ }
  end
  t1 -->|"every week at a fixed time"| s1 --> d1
  d1 -->|"[no]"| s2 --> o1 --> s8 --> stop1
  d1 -->|"[yes]"| m1
  m1 --> d2
  d2 -->|"[no]"| stop2
  d2 -->|"[yes]"| s3 --> s4 --> d3
  d3 -->|"[below threshold]"| m2
  d3 -->|"[plausible]"| s5 --> d4
  d4 -->|"[no, after widening]"| m2
  d4 -->|"[yes]"| s7 --> d5
  m2 --> s6 --> o2 --> m3
  d5 -->|"[yes]"| s9 --> m3
  d5 -->|"[no]"| s10 --> m3
  m3 --> m1
```

Reading notes:

* UC-2.04 only reads the result of UC-2.05. Calling the marketplace APIs and retrying (3 retries, 6 hours apart) happen in UC-2.05 (see AD-UC-2.05). UC-2.04 is where API\_FAILED is recorded and alerted.
* The weekly run stops as a whole only when every marketplace failed (API\_FAILED, end `stop1`). INSUFFICIENT\_DATA or a failed calculation affects one car only; the loop goes on with the next car (end `stop2` when no car is left).
* UC-2.05, 2.07, 2.03, 2.08 and 2.09 are one action each. Level 0/1/2 widening and outlier removal are inside UC-2.08. The VETC call for mileage is inside UC-2.03.
* Order inside the loop follows SEQ S-6.11 to S-6.27: clean listings, check mileage, select comparable, calculate, save.

## **2. Node trace**

|Node|Kind|Text|Traces to|
|-|-|-|-|
|t1|Time event|Weekly timer fires at a fixed time|FR-02-01-BE / SEQ S-6.01|
|s1|Action|Sync listings from partner marketplaces (UC-2.05)|FR-02-02-INT1, FR-02-02-INT2  / SEQ S-6.02 to S-6.09|
|d1|Decision|At least one marketplace synced? (reads valid source set from UC-2.05)|FR-02-02-INT2 / SEQ S-6.09, S-6.10, S-6.11|
|s2|Action|Record API\_FAILED status|FR-02-02-INT2 / SEQ S-6.10|
|o1|Object|Weekly price record in state API\_FAILED|FR-02-02-INT2 / SEQ S-6.10|
|s8|Action|Send failure alert|FR-02-01-BE|
|d2|Decision|Another tracked car to price?|FR-02-01-BE ( "for every tracked car")|
|s3|Action|Normalize and clean listings of the car's model (UC-2.07)|FR-02-03-BE / SEQ S-6.11 to S-6.15|
|s4|Action|Check verified mileage plausibility (UC-2.03)|FR-02-01-BE / SEQ S-6.16, S-6.17, S-6.18|
|d3|Decision|Mileage plausible? (mileage / max(age, 1) vs 2,000 km/year)|FR-02-01-BE / SEQ S-6.18|
|s5|Action|Select comparable vehicles (UC-2.08), widening Level 0, 1, 2|FR-02-05-BE / SEQ S-6.20 to S-6.23|
|d4|Decision|At least 30 comparable listings after widening?|FR-02-06-BE , FR-02-04-BE / SEQ S-6.24|
|s6|Action|Record INSUFFICIENT\_DATA status|FR-02-01-BE (L2 FR-02-01, mileage below threshold), FR-02-06-BE (L2 FR-02-06, fewer than 30) / SEQ S-6.19, S-6.24|
|o2|Object|Weekly price record in state INSUFFICIENT\_DATA|FR-02-01-BE, FR-02-06-BE / SEQ S-6.19, S-6.24|
|s7|Action|Calculate median, P25-P75 and confidence level (UC-2.09)|FR-02-06-BE , FR-02-04-BE / SEQ S-6.25, S-6.26|
|d5|Decision|Calculation succeeded?|FR-02-01-BE (failure clause)|
|s9|Action|Save weekly price record|FR-02-06-BE / SEQ S-6.27|
|s10|Action|Keep previous weekly price and log failure|FR-02-01-BE|

## **3. Branches and acceptance tests**

|Branch|Given|When|Then|
|-|-|-|-|
|d1 \[no]|The weekly run has started; Nextgen VN and Chợ Tốt both failed all attempts, so UC-2.05 returns API\_FAILED|UC-2.04 reads the result of UC-2.05|The system records API\_FAILED, sends a failure alert, and the run ends (`stop1`); no car is priced this week|
|d1 \[yes]|Nextgen VN synced, Chợ Tốt failed (partial sync)|UC-2.04 reads the valid source set|The run continues with the Nextgen VN listings only; the car loop starts|
|d2 \[yes]|3 tracked cars; car 1 has just finished|The system checks for the next car|Car 2 is taken and goes to normalize and clean listings (s3)|
|d2 \[no]|3 tracked cars; car 3 has just finished|The system checks for the next car|The weekly run ends (`stop2`)|
|d3 \[below threshold]|Car manufactured in 2026 (age counted as 1 year), minimum verified mileage 1,500 km (1,500 km/year < 2,000)|The system compares mileage per year with the threshold|INSUFFICIENT\_DATA is recorded for this car; no comparable selection; loop goes on with the next car|
|d3 \[plausible]|Car manufactured in 2021 (age 5), minimum verified mileage 25,000 km (5,000 km/year >= 2,000)|The system compares mileage per year with the threshold|The car goes to comparable selection (s5)|
|d4 \[no, after widening]|After Level 0, 1 and 2 and outlier removal, only 22 comparable listings remain|The system counts comparable listings|INSUFFICIENT\_DATA is recorded; no price and no confidence level are calculated; loop goes on|
|d4 \[yes]|34 comparable listings remain after outlier removal|The system counts comparable listings|The system calculates median, P25-P75 and confidence level (s7)|
|d5 \[yes]|Calculation for the car finishes without error|The system checks the calculation result|The weekly price record (median and P25-P75 rounded to the nearest million VND, confidence level) is saved; loop goes on|
|d5 \[no]|Calculation fails for one car; its previous weekly price is 650 million VND|The system checks the calculation result|The previous price (650 million VND) is kept, the failure is logged, and the loop goes on with the remaining cars|

## **4. Object states used**

|Object|States in this diagram|Full state list (all diagrams)|
|-|-|-|
|Weekly price record|API\_FAILED, INSUFFICIENT\_DATA|API\_FAILED, INSUFFICIENT\_DATA, \[success state: not named in W0/W1/SEQ, see open question 3]|

## **5. Screens touched**

|Actor action|Screen ID|
|-|-|
|||

## **6. Open questions**

|#|Question|Blocking?|Default applied|
|-|-|-|-|
|1|\[NEEDS CLARIFICATION: what is the status name of a successfully saved weekly price record? Only API\_FAILED and INSUFFICIENT\_DATA are named]|No|Success object not drawn; s9 has no object node|
|2|\[NEEDS CLARIFICATION: UC-2.04 text says "for every vehicle model"; FR-02-01 says "for every tracked car"]|No|Loop is per tracked car (FR wording); listings are cleaned per the car's model|
|3|\[NEEDS CLARIFICATION: SEQ diagram 06 names the system "Market Price Service"; the Context Diagram names it "TASCO Mobility"]|No|Lane uses "TASCO Mobility"; SEQ labels to be aligned|
|4|\[NEEDS CLARIFICATION: no BR IDs exist for the 2,000 km/year threshold and the 30-listing minimum]|No|Rules written in the text only|

## **7. Verification log**

|Check|Result|By|Date|
|-|-|-|-|
|Renders on mermaid.live|OK|Vũ Hương Ly||
|Every decision has guards on all outgoing flows, no overlap, no gap|OK (d1 to d5)|Vũ Hương Ly|2026-11-04|
|No action has two incoming flows (merge used instead)|OK (m1, m2, m3 used)|Vũ Hương Ly|2026-11-04|
|Every fork has a matching join|OK (no fork in this diagram)|Vũ Hương Ly|2026-11-04|
|Every system action cites an FR|OK|Vũ Hương Ly|2026-11-04|
|Every FR of this use case appears in the diagram|1 of 1|Vũ Hương Ly|2026-11-04|
|Lane names match the Context Diagram and the use case actors; no Web App, API or database lanes|OK (TASCO Mobility System)|Vũ Hương Ly|2026-11-04|
|Related sequence diagram uses the same names and FR IDs; its alt fragments appear here as decisions|OK except lane name (open question 6)|Vũ Hương Ly|2026-11-04|
|AI critic prompt run, findings resolved or logged as open questions|Pending|||




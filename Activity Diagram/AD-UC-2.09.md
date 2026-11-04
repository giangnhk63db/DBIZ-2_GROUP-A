**AD-UC-2.09:**

# 

| Field | Value |
| :---- | :---- |
| AD ID | AD-UC-2.09 |
| Use case | UC-2.09 Calculate Market Price and Confidence |
| Feature | F-02 Market Price |
| Primary actor | Weekly Timer |
| Lanes | Weekly Timer · TASCO Mobility System |
| Inputs (L3-L5) | Context Diagram (Figure 3\) · Use Case Diagram (Figure 6\) · Sequence: Figure 13 Calculate Weekly Market Price (S-6.25 → S-6.27; also Figure 14 steps 19-20 for backfill reuse) · W1 rows (FR-02-06-BE, FR-02-04-BE) |
| Trigger | Weekly timer fires and the weekly price job (UC-2.04) reaches the price calculation of a vehicle |
| Preconditions | Verified mileage of the vehicle is plausible (UC-2.03); comparable listings have been selected (UC-2.08) with their comparison level (Level 0, 1 or 2); and the set of valid marketplaces for the week is known (UC-2.05) |
| Success end state | Weekly price record is saved with the median price, the P25-P75 range (rounded to the nearest million VND) and a confidence level of High, Medium or Low |
| Failure end state(s) | INSUFFICIENT\_DATA is recorded because fewer than 30 comparable listings remain; no price is calculated and no confidence level is assigned |
| FRs covered | FR-02-06, FR-02-04 |
| Business rules applied | BR-2.09-1 (at least 30 comparable listings are needed to calculate a price) · BR-2.09-2 (price values are rounded to the nearest million VND) · BR-2.09-3 (High \= Level 0 and all valid sources available; Medium \= Level 1 or one marketplace missing that week; Low \= Level 2\) |
| Version / status | v0.1 · Draft |
| Author | Nguyễn Hà Phương |

## 

1. **Diagram**

\---

config:

 flowchart:

   nodeSpacing: 80

   rankSpacing: 90

   curve: linear

\---

flowchart TB

subgraph TIMER\["Weekly Timer"\]

  start@{ shape: f-circ }

  t1@{ shape: hourglass, label: "Every week" }

end

subgraph SYS\["TASCO Mobility System"\]

  direction TB

  d1{"At least 30 listings?"}

  s1**(**"Calculate median and P25-P75 (FR-02-06-BE**)**")

  s2**(**"Round to nearest million VND (FR-02-06-BE**)**")

  d2{"Level 2 criteria used? **(**mileage \+-40%**)**"}

  d3{"Level 1 criteria used **(**year \+-2**)** or one marketplace missing?"}

  s5**(**"Assign Low (FR-02-04-BE**)**")

  s4**(**"Assign Medium (FR-02-04-BE**)**")

  s3**(**"Assign High (FR-02-04-BE**)**")

  m1{" "}

  s6**(**"Save weekly price record (FR-02-06-BE**,** FR-02-04-BE**)**")

  s7**(**"Record INSUFFICIENT\_DATA**,** no price (FR-02-06-BE**,** FR-02-04-BE**)**")

  stop1@{ shape: fr-circ }

  stop2@{ shape: fr-circ }

end

start \--\> t1 \--\> d1

d1 \--\>\|"\[yes\]"\| s1 \--\> s2 \--\> d2

d1 \--\>\|"\[no\]"\| s7 \--\> stop2

d2 \--\>\|"\[yes\]"\| s5 \--\> m1

d2 \--\>\|"\[no\]"\| d3

d3 \--\>\|"\[yes\]"\| s4 \--\> m1

d3 \--\>\|"\[no\]"\| s3 \--\> m1

m1 \--\> s6 \--\> stop1

**Reading notes**

- The workflow starts when the weekly timer fires and ends in one of two final nodes: the weekly price record is saved, or INSUFFICIENT\_DATA is recorded.  
- Decision d1 checks the minimum sample size. Exactly 30 comparable listings is sufficient; fewer than 30 ends the use case without calculating a price or assigning confidence.  
- Decisions d2 and d3 apply BR-2.09-3 in order: Low first (Level 2), then Medium (Level 1 or one marketplace missing), otherwise High (Level 0 and all valid sources).  
- Therefore, if Level 2 was used and one marketplace is missing, the result is Low because the Level 2 condition is evaluated first (Open Question 2).  
- The diagram covers one vehicle. The loops over each vehicle model and each vehicle in the model group (Figure 13\) belong to UC-2.04.

## **2\. Node trace**

| Node | Kind | Text | Traces to |
| ----- | ----- | ----- | ----- |
| t1 | Time event | Every week (weekly timer fires) | FR-02-01-BE2; S-6.01 |
| d1 | Decision | At least 30 listings? | FR-02-06-BE, FR-02-04-BE, BR-2.09-1; S-6.24 to S-6.25 |
| s1 | Action | Calculate median and P25-P75 | FR-02-06-BE; S-6.25 |
| s2 | Action | Round to nearest million VND | FR-02-06-BE; S-6.25 |
| d2 | Decision | Level 2 criteria used? (mileage \+-40%) | FR-02-04-BE, BR-2.09-3; UC-2.09 description (Low when Level 2). “Criteria” means the comparison criteria chosen in UC-2.08; Level 2 widens mileage to \+-40% (S-6.23); S-6.26 |
| d3 | Decision | Level 1 criteria used (year \+-2) or one marketplace missing? | FR-02-04-BE, BR-2.09-3; UC-2.09 description (Medium when Level 1 or one marketplace missing). Level 1 widens year to \+-2 (S-6.22); the marketplace condition uses the valid data sources determined at S-6.09 (FR-02-02-INT1, FR-02-02-INT2); S-6.26 |
| s5 | Action | Assign Low | FR-02-04-BE; S-6.26 |
| s4 | Action | Assign Medium | FR-02-04-BE; S-6.26 |
| s3 | Action | Assign High | FR-02-04-BE; S-6.26 (High when Level 0 is used and all valid sources are available; valid sources are determined at S-6.09) |
| m1 | Merge | Joins s5, s4 and s3 | N/A |
| s6 | Action | Save weekly price record | FR-02-06-BE, FR-02-04-BE; S-6.27 |
| s7 | Action | Record INSUFFICIENT\_DATA, no price | FR-02-06-BE, FR-02-04-BE (IF fewer than 30 after Level 2); S-6.24 |

## **3\. Branches and acceptance tests**

| Branch | Given | When | Then |
| :---- | :---- | :---- | :---- |
| d1 \[yes\] | The weekly job runs UC-2.09 for a vehicle with exactly 30 comparable listings remaining after outliers are removed | UC-2.09 checks the sample size | The system calculates the median and P25-P75 range, rounds them, assigns a confidence level and saves the weekly price record |
| d1 \[no\] | A vehicle has 29 comparable listings remaining after Level 2 | UC-2.09 checks the sample size | The system records INSUFFICIENT\_DATA, calculates no price, assigns no confidence level and ends |
| d2 \[yes\] | Listings were selected at Level 2 (mileage ±40%); marketplace availability may include one or more valid sources | Rounded price values are available and d2 is evaluated | The system assigns Low and saves the record. If one marketplace is missing, the result remains Low because Level 2 is checked first (Open Question 2\) |
| d2 \[no\] | Listings were selected at Level 0 or Level 1 | Rounded price values are available and d2 is evaluated | The system proceeds to d3 to check whether Level 1 was used or one marketplace was missing |
| d3 \[yes\] | Either Level 1 was used with both marketplaces available, or Level 0 was used and one marketplace is missing (for example, Nextgen VN synced and Chợ Tốt failed after three retries) | The workflow reaches d3 | The system assigns Medium and saves the record |
| d3 \[no\] | Level 0 was used and all valid marketplaces are available (for example, both Nextgen VN and Chợ Tốt synced successfully) | The workflow reaches d3 | The system assigns High and saves the record; an illustrative median of 652,400,000 VND is rounded to 652,000,000 VND |

## **4\. Object states used**

| Object | States in this diagram | Full state list (all diagrams) |
| :---- | :---- | :---- |
| Weekly Price Record | Saved; INSUFFICIENT\_DATA | Saved; INSUFFICIENT\_DATA; API\_FAILED (UC-2.05, Figure 13); Estimated (backfill flag, UC-2.10) |
| Confidence Level | High; Medium; Low; not assigned when INSUFFICIENT\_DATA | High; Medium; Low |
| Comparison Level | Level 0; Level 1; Level 2 (set by UC-2.08 and read here) | Level 0; Level 1; Level 2 |

## **5\. Screens touched**

## **6\. Open questions**

| \# | Question | Blocking? | Default applied |
| :---- | :---- | :---- | :---- |
| 1 | W1 gives FR-02-04 the source trace UC-2.08 and S-6.20 → S-6.24, but in Figure 13 steps 20-24 belong to UC-2.08 and confidence is assigned at step 26 inside UC-2.09. Which use case and steps own FR-02-04? | No | FR-02-04 is covered here at S-6.26. The integrator updates the W1 source trace to UC-2.09 |
| 2 | UC-2.09 says Medium applies when Level 1 is used or one marketplace is missing, and Low applies when Level 2 is used. Which level applies to Level 2 with one marketplace missing? | No | Low. Level 2 is tested first, so Low wins when two rules match |
| 3 | FR-02-04 says confidence depends on the size of the comparable sample and the comparison level, but the UC-2.09 rule uses comparison level and marketplace availability. Is a sample-size rule needed? | No | Use the UC-2.09 rule. Sample size matters only through the minimum of 30 listings |
| 4 | In Figure 13, the alternative \[Fewer than 30 listings after Level 2\] (step 24, INSUFFICIENT\_DATA) sits inside the UC-2.08 band, and the UC-2.09 band starts only in the alternative \[Sufficient sample size\]. FR-02-06 repeats the same guard. Which use case records the status? | No | d1 stays in this diagram as the guard for FR-02-06 and mirrors the Figure 13 alternative. The outcome is the same |
| 5 | BR-2.09-1 to BR-2.09-3 are local labels because the W1 business-rule IDs were not available. Weekly Timer is an actor in Figure 6 but is not among the five blocks of the Context Diagram description, although Figure 13 shows it as a participant (step 1, weekly-job trigger). Confirm both names | No | Use the local BR labels and Weekly Timer lane as shown in Figure 6 until confirmed |
| 6 | UC-2.09 says the weekly record also holds vehicle ID, week reference date, sample size, mileage used, expansion level, number of contributing marketplaces, source set, status and backfill flag, but no FR lists these fields. UC-2.09 is also reused per checkpoint by UC-2.10 (Figure 14, steps 19-20) with the back-calculation flag. Which FR owns the record fields? | No | Keep s6 generic (“Save weekly price record”). The backfill path is handled in AD-UC-2.10 |

## 7\. Verification log

| Check | Result | By | Date |
| :---- | :---- | :---- | :---- |
| Renders on mermaid.live | Mermaid code supplied; external rendering not independently verified | Nguyễn Hà Phương | 2026-11-05 |
| Every decision has guards on all outgoing flows, no overlap, no gap | Pass: d1, d2 and d3 each have two mutually exclusive guards | Nguyễn Hà Phương | 2026-11-05 |
| No action has two incoming flows (merge used instead) | Pass: m1 joins the three confidence branches before s6 | Nguyễn Hà Phương | 2026-11-05 |
| Every fork has a matching join | Pass: no fork is used | Nguyễn Hà Phương | 2026-11-05 |
| Every system action cites an FR | Pass: s1-s7 cite FR-02-06 and/or FR-02-04 | Nguyễn Hà Phương | 2026-11-05 |
| Every FR of this use case appears in the diagram | 2 of 2: FR-02-06 and FR-02-04 | Nguyễn Hà Phương | 2026-11-05 |
| Lane names match the Context Diagram and the use case actors; no Web App, API or database lanes | Weekly Timer and TASCO Mobility System follow Figure 6 and the supplied diagram; Context Diagram naming remains open under Question 5 | Nguyễn Hà Phương | 2026-11-05 |
| Related sequence diagram uses the same names and FR IDs; its alt fragments appear here as decisions | Figure 13: steps 25-27 map to s1-s6; \[Sufficient sample size\] is represented by d1 \[yes\], and \[Fewer than 30 listings after Level 2\] by d1 \[no\]/s7. Confidence branches d2 and d3 come from the UC-2.09 description; Figure 14 steps 19-20 are covered by AD-UC-2.10 | Nguyễn Hà Phương | 2026-11-05 |
| AI critic prompt run, findings resolved or logged as open questions | Draft self-check completed; the six questions above are retained; independent critic review remains outstanding | Nguyễn Hà Phương | 2026-11-05 |


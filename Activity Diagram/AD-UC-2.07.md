#### **AD-UC-2.07** 

| Field | Value |
| :---- | :---- |
| AD ID | AD-UC-2.07 |
| Use case | UC-2.07 Clean Marketplace Listings |
| Feature | F-02 Market Price |
| Primary actor | Weekly Timer |
| Lanes | Weekly Timer · TASCO Mobility System |
| Inputs (L3-L5) | Context Diagram (Figure 3\) · Use Case Diagram (Figure 6\) · Sequence: weekly listing-cleaning flow (sequence figure/step reference to be confirmed) · W1 rows (FR-02-03-BE) · UC-2.07 description |
| Trigger | Weekly timer fires and the weekly job reaches the marketplace-listing cleaning step |
| Preconditions | Marketplace listings have been collected and are available for cleaning; brand and model mappings to the TASCO catalog are available |
| Success end state | Eligible, normalized and de-duplicated listings are passed to the pricing process |
| Failure end state(s) | A listing that is too old, removed, incomplete, has an invalid price, or is marked “Negotiable” is excluded from pricing |
| FRs covered | FR-02-03 |
| Business rules applied | UC-2.07 rule: retain listings published within the previous 90 days and not removed; mandatory fields must be present; price must be above zero and must not be “Negotiable”; cross-marketplace duplicates are identified by the same brand, model and manufacturing year, with mileage difference of at most 1,000 km; retain the most recently published duplicate |
| Version / status | v0.1 · Draft |
| Author | Nguyễn Hà Phương |

## 
```
1. **Diagram:**
---
config:
swimlane-beta:
  nodeSpacing: 80
  rankSpacing: 90
  curve: linear
---
swimlane-beta TB
subgraph TIMER["Weekly Timer"]
  start@{ shape: f-circ }
  t1@{ shape: hourglass, label: "Every week" }
end
subgraph SYS["TASCO Mobility System"]
  direction TB
  s1("Map brand and model to TASCO catalog (FR-02-03-BE)")
  s2("Convert prices to VND and mileage to km (FR-02-03-BE)")
  d1{"Published within 90 days and not removed?"}
  d2{"Mandatory fields present, price above zero, not marked 'Negotiable'?"}
  d3{"Duplicate of a listing on another marketplace?"}
  s3("Keep only the most recently published listing (FR-02-03-BE)")
  m2{" "}
  s4("Pass cleaned listing to pricing (FR-02-03-BE)")
  m1{" "}
  s5("Exclude listing from pricing (FR-02-03-BE)")
  stop1@{ shape: fr-circ }
  stop2@{ shape: fr-circ }
end
start --> t1 --> s1 --> s2 --> d1
d1 -->|"[yes]"| d2
d1 -->|"[no]"| m1
d2 -->|"[yes]"| d3
d2 -->|"[no]"| m1
d3 -->|"[yes]"| s3 --> m2
d3 -->|"[no]"| m2
m2 --> s4 --> stop1
m1 --> s5 --> stop2
```

**Reading notes**

- The checks are applied in sequence: recency/removal first, required-field and price validity second, and cross-marketplace duplicate handling last.  
- A duplicate group contributes only its most recently published listing to pricing. A listing that is not a duplicate proceeds directly to pricing.  
- The duplicate rule is based on the UC-2.07 description: same brand, model and manufacturing year, with mileage differing by no more than 1,000 km.  
- This diagram covers the cleaning of listings for the weekly pricing process. It does not show marketplace collection or the later comparable-listing selection in UC-2.08.

2. **Node Trace:** 

| Node | Kind | Text | Traces to |
| ----- | ----- | ----- | ----- |
| t1 | Time event | Every week (weekly timer fires) | FR-02-01-BE2; S-6.01 |
| s1 | Action | Map brand and model to TASCO catalog | FR-02-03-BE; S-6.12 |
| s2 | Action | Convert prices to VND and mileage to km | FR-02-03-BE; S-6.12 |
| d1 | Decision | Published within 90 days and not removed? | FR-02-03-BE; the 90-day rule is from the UC-2.07 description; S-6.13 |
| d2 | Decision | Mandatory fields present, price above zero, not marked ‘Negotiable’? | FR-02-03-BE; S-6.14 |
| d3 | Decision | Duplicate of a listing on another marketplace? | FR-02-03-BE; the rule (same brand, model, manufacturing year, mileage difference of at most 1,000 km) is from the UC-2.07 description; S-6.15 |
| s3 | Action | Keep only the most recently published listing | FR-02-03-BE; S-6.15 |
| s4 | Action | Pass cleaned listing to pricing | FR-02-03-BE |
| s5 | Action | Exclude listing from pricing | FR-02-03-BE |
| m1 | Merge | Joins d1 \[no\] and d2 \[no\] | N/A |
| m2 | Merge | Joins d3 \[yes\] (via s3) and d3 \[no\] | N/A |

## **3\. Branches and acceptance tests**

| Branch | Given | When | Then |
| :---- | :---- | :---- | :---- |
| d1 \[yes\] | A listing was published 30 days ago and has not been removed | The weekly cleaning process checks the listing | The system proceeds to validate its mandatory fields and price |
| d1 \[no\] | A listing was published 91 days ago, or the source marks it as removed | The weekly cleaning process checks the listing | The system excludes the listing from pricing |
| d2 \[yes\] | A recent, active listing has all mandatory fields, a price greater than zero, and a numeric price rather than “Negotiable” | The listing passes the recency check | The system checks whether it duplicates a listing on another marketplace |
| d2 \[no\] | A recent, active listing is missing a mandatory field, has a zero/negative price, or is marked “Negotiable” | The listing reaches the validity check | The system excludes the listing from pricing |
| d3 \[yes\] | Two marketplace listings have the same mapped brand, model and manufacturing year, and their mileage differs by no more than 1,000 km | The system compares the listings across marketplaces | The system retains only the most recently published listing and passes that retained listing to pricing |
| d3 \[no\] | No listing on another marketplace matches the duplicate rule | The system completes the duplicate check | The system passes the listing to pricing without duplicate removal |

## **4\. Object states used**

| Object | States in this diagram | Full state list (all diagrams) |
| :---- | :---- | :---- |
| Marketplace Listing | Normalized; eligible for pricing; excluded from pricing; duplicate/ superseded | Not specified in the supplied material; confirm the canonical listing-state vocabulary with the data model owner |
| Listing Price / Mileage | Original marketplace value; normalized to VND / km | Not specified in the supplied material |

## **5\. Screens touched**

## **6\. Open questions**

| \# | Question | Blocking? | Default applied |
| :---- | :---- | :---- | :---- |
| 1 | Does “published within 90 days” include the exact boundary date (published exactly 90 days before the run)? | No | Treat the 90-day boundary as inclusive until confirmed |
| 2 | If two duplicate listings have the same publication timestamp, how should the system choose the retained listing? | No | Tie-break rule is not defined; retain this as an implementation clarification rather than inventing a rule |
| 3 | Does “removed” mean explicitly removed by the marketplace, unavailable at retrieval time, or both? | No | Use the source's explicit removed indicator; confirm whether retrieval unavailability also counts |
| 4 | The duplicate rule is described as same brand, model and manufacturing year with mileage difference ≤1,000 km. Is mileage compared pairwise between listings, or against a representative listing in a duplicate group? | No | Use pairwise comparison for the draft; confirm the grouping algorithm |
| 5 | The supplied trace cites FR-02-03-BE, while the completed UC-2.09 example uses the shortened FR format. | No | Use FR-02-03 in this document for consistency; confirm the canonical W1 identifier |

## **7\. Verification log**

| Check | Result | By | Date |
| :---- | :---- | :---- | :---- |
| Renders on mermaid.live | Mermaid code supplied; external rendering not independently verified | Nguyễn Hà Phương | 2026-11-04 |
| Every decision has guards on all outgoing flows, no overlap, no gap | Pass: d1, d2 and d3 each have yes/no guards | Nguyễn Hà Phương | 2026-11-04 |
| No action has two incoming flows (merge used instead) | Pass: s4 receives the two d3 outcomes; m1 joins the exclusion paths | Nguyễn Hà Phương | 2026-11-04 |
| Every fork has a matching join | Pass: no fork is used | Nguyễn Hà Phương | 2026-11-04 |
| Every system action cites an FR | Pass: s1-s5 cite FR-02-03 | Nguyễn Hà Phương | 2026-11-04 |
| Every FR of this use case appears in the diagram | 1 of 1: FR-02-03 | Nguyễn Hà Phương | 2026-11-04 |
| Lane names match the Context Diagram and the use case actors; no Web App, API or database lanes | Weekly Timer and TASCO Mobility System follow the supplied diagram; Context Diagram naming still requires confirmation | Nguyễn Hà Phương | 2026-11-04 |
| Related sequence diagram uses the same names and FR IDs; its alt fragments appear here as decisions | Not fully verifiable: the corresponding sequence-diagram identifier and step range were not supplied | Nguyễn Hà Phương | 2026-11-04 |
| AI critic prompt run, findings resolved or logged as open questions | Draft self-check completed; independent critic review remains outstanding | Nguyễn Hà Phương | 2026-11-04 |


One row per use case in the Use Case inventory. Every use case appears, even when it needs no diagram, so a reader can see that the decision was made on purpose.

1. **Input checks**

| \# | Finding | Source to fix | Action | Owner | Status |
| ----- | ----- | ----- | ----- | ----- | ----- |
| 1 | Check verified mileage plausibility & Mileage plausible are missing in the feature list | W1 | Add features in feature list | Nguyễn Hương Giang | Done |
| 2 | The \[No data\] guard has no counterpart in sequence Figure 16, which only has the alt “Any API gateway fails / All checks succeed”. Add a branch to the sequence?\] | Sequence Diagram | Add an alt for “portal answers without a record” to the sequence | Vũ Hương Ly | Done |

2. **Use cases**

Rule used in this course: draw an activity diagram for every Must use case that has at least one decision, more than one lane, or a time trigger. A use case that is a single display step gets “not needed” and a one-line reason. A use case has at most one diagram, and one diagram never covers two use cases.

| Use case | Name | Feature | Priority | Activity diagram | Status | Reason if not needed |
| ----- | ----- | ----- | ----- | ----- | ----- | ----- |
| UC-1.01 | Enter VIN to initialize Report  | F-01  | Must | AD-UC-1.01.md  | Review |  |
| UC-1.02 | View Vehicle Identification | F-01 | Must |  | Review | Pure read-only data retrieval flow. The system fetches data via VETC API and displays information directly on the screen without branching logic or complex exception handling.  |
| UC-1.03 | View verified Minimum Mileage | F-01 | Must | AD-UC-1.03.md | Review |  |
| UC-1.04 | View Maintenance & Repair History in Carpla Service | F-01 | Must |  | Review | The function focuses purely on fetching maintenance history data from the Carpla Service system and displaying it in a timeline format, without triggering decision branches or complex business errors  |
| UC-1.05 | Upload External Maintenance & Repair Invoices  | F-01 | Must | AD-UC-1.05 | Review |  |
| UC-1.06 | View Battery State of Health (SoH)  | F-01 | Must | AD-UC-1.06 | Review |  |
| UC-1.07 | Refresh Legal Status  | F-01 | Must | AD-UC-1.07 | Review |  |
| UC-1.08 | View Inspection Status  | F-01 | Must |  | Review | Only involves a single display step; the complex retrieval logic is already covered by UC-1.07.  |
| UC-1.09 | View Unpaid Traffic Fine Status  | F-01 | Must |  | Review | Only involves a single display step; the complex retrieval logic is already covered by UC-1.07.  |
| UC-1.10 | View Mortgage Status  | F-01 | Must |  | Review | Only involves a single display step; the complex retrieval logic is already covered by UC-1.07.  |
| UC-2.01 | View Market Price | F-02 | Must |  | Review | Only one actor is involved, with no parallel flows. |
| UC-2.02 | View 12-Week Price Trend Chart | F-02 | Must |  | Review | A simple timeline diagram. |
| UC-2.03 | Check Verified Mileage Plausibility | F-02 | Must |  | Review | There is only one condition.  |
| UC-2.04 | Calculate Weekly Market Price | F-02 | Must | AD-UC-2.04 | Review |  |
| UC-2.05 | Sync Listings from Partner Marketplaces | F-02 | Must | AD-UC-2.05 | Review |  |
| UC-2.06 | Sync 180-Day Listing History | F-02 | Must |  | Review | A straightforward flow, no branching conditions. |
| UC-2.07 | Normalize and Clean Listings | F-02 | Must | AD-UC-2.07 | Review |  |
| UC-2.08 | Select Comparable Vehicles | F-02 | Must  | AD-UC-2.08 | Review |  |
| UC-2.09 | Calculate Market Price and Confidence | F-02 | Must | AD-UC-2.09 | Review |  |
| UC-2.10 | Backfill 12-Week Price History | F-02 | Must | AD-UC-2.10 | Review |  |
| UC- 2.11 | Stop Market Price Tracking | F-02 | Should  |  | Review | A straightforward flow, no branching conditions. |
| UC-3.01 | Submit Sale Request | F-03 | Must | AD-UC-3.01 | Review |  |
| UC-3.02 | Re-check Legal Status | F-03 | Should |  | Review | Only invokes the re-check of each legal-status type; the real logic is covered by the activity diagrams of each legal status below. |
| UC-3.03 | Check Inspection Status | F-03 | Must | AD-UC-3.03 | Review |  |
| UC-3.04 | Check Unpaid Traffic Fine Status | F-03 | Must | AD-UC-3.04 | Review |  |
| UC-3.05 | Check Mortgage Status | F-03 | Must | AD-UC-3.05 | Review |  |
| UC-3.06 | Package & Forward Verified Report and Market Price to Carpla Staff | F-03 | Must | AD-UC-3.06 | Review |  |
| UC-3.07 | Review Verified Report | F-03 | Should |  | Review | Carpla staff receive the valuation request with the attached report and carry out valuation. |
| UC-3.08 | Send Preliminary Quote to Car Owner | F-03 | Should |  | Review | After valuation, staff press a button to send the quote to the car owner. No decision or loop. |
| UC-3.09 | View Preliminary Quote | F-03 | Should |  | Review | This use case only needs passive display, single step, no decision. The car owner views the quote sent after valuation.  |

Object state lists collected from all diagrams (copy into the data model in DBIZ3):

| Object | States |
| ----- | ----- |
| Toll Transaction Data | Initial, Requested, Retrieved, Parsed, Calculated, Failed |
| History View | Initial, Loading, Displayed, Error |
| Invoice File | Uploaded, Stored, Parsed, Verified, Flagged, Archived, Rejected |
| Maintenance Timeline | Initial, Pending Update, Updated, Synced |
| Charging Data | Initial, Requested, Retrieved, Validated, Processed, Failed |
| Battery Health Report | Initial, Computed, Displayed, Error |
| Weekly Price Record | API\_FAILED, INSUFFICIENT\_DATA, \[success state: name to be fixed in the data model\]  |
| Vehicle report creating | Created, Not created |
| Sale request submission | Created, Sending, Sent |
| Inspection status | Have data, No data |
| Unpaid fines | Have data, No data |
| Mortgage status | No mortgage, Mortgaged, No data |
| Report  | Red flag attached, No outstanding mortgage indicated |
| Package forwarded | Forwarding, Forwarded,  Unable to  forward |
| Market price  | No data, Have data |
| Verified Vehicle Report | Initialized, Validating, Completed, Expired  |
| Legal Status Updated | Empty, Outdated, Updating, Updated  |
| External Portal Query  | Pending, Sent, Responded, Timeout, Error  |


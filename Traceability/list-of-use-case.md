# LIST OF USE CASES TRONG USE CASE DIAGRAM

| UC-ID | Diagram | Use Case Name | Primary Actor | Secondary Actor | include/extend | Note |
|---|---|---|---|---|---|---|
| UC-1.01 | 01 - Vehicle Identification and Status | Enter VIN to initialize Report | Car Owner | - |  |  |
| UC-1.02 | 01 - Vehicle Identification and Status | View Vehicle Identification | Car Owner | VETC |  |  |
| UC-1.03 | 01 - Vehicle Identification and Status | View Verified Minimum Mileage | Car Owner | VETC |  |  |
| UC-1.04 | 01 - Vehicle Identification and Status | View Maintenance & Repair History in Carpla Service | Car Owner | Carpla Service | extend UC-1.05 |  |
| UC-1.05 | 01 - Vehicle Identification and Status | Upload External Maintenance & Repair Invoices | Car Owner | Electronic Invoice Portal |  |  |
| UC-1.06 | 01 - Vehicle Identification and Status | View Battery State of Health (SoH) | Car Owner | Electronic Vehicle Charging Station |  |  |
| UC-1.07 | 01 - Vehicle Identification and Status | Refresh Legal Status | Car Owner | Vehicle Inspection Portal, Traffic Fine Lookup Portal, Secured Transaction Registry Portal |  |  |
| UC-1.08 | 01 - Vehicle Identification and Status | View Inspection Status | Car Owner | - | include UC-1.07 |  |
| UC-1.09 | 01 - Vehicle Identification and Status | View Unpaid Traffic Fine Status | Car Owner | - | include UC-1.08 |  |
| UC-1.10 | 01 - Vehicle Identification and Status | View Mortgage Status | Car Owner | - | include UC-1.09 |  |
| UC-2.01 | 02 - Market Price | View Market Price | Car Owner | - | include UC-2.02 |  |
| UC-2.02 | 02 - Market Price | View 12-Week Price Trend Chart | - | - |  |  |
| UC-2.03 | 02 - Market Price | Check Verified Mileage Plausibility | - | VETC |  |  |
| UC-2.04 | 02 - Market Price | Calculate Weekly Market Price | Weekly timer | - | include UC-2.03,05,07,08,09 |  |
| UC-2.05 | 02 - Market Price | Sync Listings from Partner Marketplaces | - | Nextgen VN, Chợ Tốt |  |  |
| UC-2.06 | 02 - Market Price | Sync 180-Day Listing History | - | Nextgen VN, Chợ Tốt |  |  |
| UC-2.07 | 02 - Market Price | Normalize and Clean Listings | - | - |  |  |
| UC-2.08 | 02 - Market Price | Select Comparable Vehicles | - | - |  |  |
| UC-2.09 | 02 - Market Price | Calculate Market Price and Confidence | - | - |  |  |
| UC-2.10 | 02 - Market Price | Backfill 12-Week Price History | - | VETC | extend UC-2.06<br>include UC-2.07,08,09 |  |
| UC-2.11 | 02 - Market Price | Stop Market Price Tracking | Car Owner | - |  |  |
| UC-3.01 | 03 - Sell Request & Preliminary Quote | Submit Sale Request | Car Owner | - | include UC-3.02,06 |  |
| UC-3.02 | 03 - Sell Request & Preliminary Quote | Re-check Legal Status | - | - | include UC-3.03,04,05 |  |
| UC-3.03 | 03 - Sell Request & Preliminary Quote | Check Inspection Status | - | Vehicle Inspection Portal |  |  |
| UC-3.04 | 03 - Sell Request & Preliminary Quote | Check Unpaid Traffic Fine Status | - | Traffic Fine Lookup Portal |  |  |
| UC-3.05 | 03 - Sell Request & Preliminary Quote | Check Mortgage Status | - | Secured Transaction Registry Portal |  |  |
| UC-3.06 | 03 - Sell Request & Preliminary Quote | Package & Forward Verified Report and Market Price to Carpla Staff | - | - |  |  |
| UC-3.07 | 03 - Sell Request & Preliminary Quote | Review Verified Report | Carpla Staff | - |  |  |
| UC-3.08 | 03 - Sell Request & Preliminary Quote | Send Preliminary Quote to Car Owner | Carpla Staff | - |  |  |
| UC-3.09 | 03 - Sell Request & Preliminary Quote | View Preliminary Quote | Car Owner | - |  |  |

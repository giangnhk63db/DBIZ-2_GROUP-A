# LIST OF MESSAGES TRONG SEQUENCE DIAGRAM

| SEQ: S-X.Y | No. - Name | Message |From | To | Note |
|---|---|---|---|---|---|
| SEQ: S-1.01 | 01 - Vehicle Report | Select Create Verified Vehicle Report | Car Owner | TASCO Mobility System |  |
| SEQ: S-1.02 | 01 - Vehicle Report | Enter 17-character VIN | Car Owner | TASCO Mobility System |  |
| SEQ: S-1.03 | 01 - Vehicle Report | Initialize digital vehicle profile | TASCO Mobility System | TASCO Mobility System |  |
| SEQ: S-1.04 | 01 - Vehicle Report | Vehicle profile created | TASCO Mobility System | Car Owner |  |
| SEQ: S-1.05 | 01 - Vehicle Report | Open Vehicle Identification | Car Owner | TASCO Mobility System |  |
| SEQ: S-1.06 | 01 - Vehicle Report | Query core vehicle identity data | TASCO Mobility System | VETC |  |
| SEQ: S-1.07 | 01 - Vehicle Report | Return identity data | VETC | TASCO Mobility System |  |
| SEQ: S-1.08 | 01 - Vehicle Report | Display verified identification | TASCO Mobility System | Car Owner |  |
| SEQ: S-1.09 | 01 - Vehicle Report | Request vehicle mileage | Car Owner | TASCO Mobility System |  |
| SEQ: S-1.10 | 01 - Vehicle Report | Retrieve logged mileage records | TASCO Mobility System | VETC |  |
| SEQ: S-1.11 | 01 - Vehicle Report | Return mileage records | VETC | TASCO Mobility System |  |
| SEQ: S-1.12 | 01 - Vehicle Report | Display verified minimum ODO | TASCO Mobility System | Car Owner |  |
| SEQ: S-2.01 | 02 - Maintenance & Repair History and Upload External Invoice | Open maintenance history | Car Owner | TASCO Mobility System |  |
| SEQ: S-2.02 | 02 - Maintenance & Repair History and Upload External Invoice | Fetch official service and repair records | TASCO Mobility System | Carpla Service |  |
| SEQ: S-2.03 | 02 - Maintenance & Repair History and Upload External Invoice | Return records | Carpla Service | TASCO Mobility System |  |
| SEQ: S-2.04 | 02 - Maintenance & Repair History and Upload External Invoice | Display consolidated maintenance timeline | TASCO Mobility System | Car Owner |  |
| SEQ: S-2.05 | 02 - Maintenance & Repair History and Upload External Invoice | Upload invoice file (PDF/XML/Image) | Car Owner | TASCO Mobility System |  |
| SEQ: S-2.06 | 02 - Maintenance & Repair History and Upload External Invoice | Extract data via OCR or file parsing | TASCO Mobility System | TASCO Mobility System |  |
| SEQ: S-2.07 | 02 - Maintenance & Repair History and Upload External Invoice | Cross-check tax code, invoice number, total amount | TASCO Mobility System | Electronic Invoice Portal |  |
| SEQ: S-2.08 | 02 - Maintenance & Repair History and Upload External Invoice | Authenticity result | Electronic Invoice Portal | TASCO Mobility System |  |
| SEQ: S-2.09 | 02 - Maintenance & Repair History and Upload External Invoice | [Invoice valid] Update maintenance history | TASCO Mobility System | TASCO Mobility System |  |
| SEQ: S-2.10 | 02 - Maintenance & Repair History and Upload External Invoice | [Invoice valid] Show updated timeline | TASCO Mobility System | Car Owner |  |
| SEQ: S-2.11 | 02 - Maintenance & Repair History and Upload External Invoice | [Invoice invalid] Reject invoice and show error | TASCO Mobility System | Car Owner |  |
| SEQ: S-3.01 | 03 - View Battery State of Health | Open battery health (SoH) | Car Owner (EV) | TASCO Mobility System |  |
| SEQ: S-3.02 | 03 - View Battery State of Health | Retrieve charging session data | TASCO Mobility System | EV Charging Stations |  |
| SEQ: S-3.03 | 03 - View Battery State of Health | Return charging sessions | EV Charging Stations | TASCO Mobility System |  |
| SEQ: S-3.04 | 03 - View Battery State of Health | Calculate SoH (current vs original charging time) | TASCO Mobility System | TASCO Mobility System |  |
| SEQ: S-3.05 | 03 - View Battery State of Health | Display SoH percentage | TASCO Mobility System | Car Owner (EV) |  |
| SEQ: S-4.01 | 04 - Refresh Legal Status | Click Refresh Legal Status | Car Owner | TASCO Mobility System |  |
| SEQ: S-4.02 | 04 - Refresh Legal Status | [par: UC-1.08] Send plate and VIN | TASCO Mobility System | Vehicle Inspection Portal |  |
| SEQ: S-4.03 | 04 - Refresh Legal Status | [par: UC-1.08] Registration and inspection validity | Vehicle Inspection Portal | TASCO Mobility System |  |
| SEQ: S-4.04 | 04 - Refresh Legal Status | [par: UC-1.09] Send plate | TASCO Mobility System | Traffic Fine Lookup Portal |  |
| SEQ: S-4.05 | 04 - Refresh Legal Status | [par: UC-1.09] Outstanding unpaid fines | Traffic Fine Lookup Portal | TASCO Mobility System |  |
| SEQ: S-4.06 | 04 - Refresh Legal Status | [par: UC-1.10] Send VIN | TASCO Mobility System | Secured Transaction Registry Portal |  |
| SEQ: S-4.07 | 04 - Refresh Legal Status | [par: UC-1.10] Mortgage status | Secured Transaction Registry Portal | TASCO Mobility System |  |
| SEQ: S-4.08 | 04 - Refresh Legal Status | Update Verified Vehicle Report | TASCO Mobility System | TASCO Mobility System |  |
| SEQ: S-4.09 | 04 - Refresh Legal Status | Display latest legal status | TASCO Mobility System | Car Owner |  |
| SEQ: S-5.01 | 05 - View Market Price and 12 Week Chart | Open the Market Price section | Car Owner | Verified Vehicle Report |  |
| SEQ: S-5.02 | 05 - View Market Price and 12 Week Chart | Request the vehicle’s market price | Verified Vehicle Report | Market Price Service |  |
| SEQ: S-5.03 | 05 - View Market Price and 12 Week Chart | Retrieve the latest weekly price record | Market Price Service | Weekly Price DB |  |
| SEQ: S-5.04 | 05 - View Market Price and 12 Week Chart | Return the weekly price record | Weekly Price DB | Market Price Service |  |
| SEQ: S-5.05 | 05 - View Market Price and 12 Week Chart | [Status OK] Median price, P25-P75, trend, confidence level, listing count, update date, minimum verified mileage | Market Price Service | Verified Vehicle Report |  |
| SEQ: S-5.06 | 05 - View Market Price and 12 Week Chart | [Status OK] Display the price, the label “Asking Prices for Similar Vehicles”, and a trend arrow with the percentage change | Verified Vehicle Report | Car Owner |  |
| SEQ: S-5.07 | 05 - View Market Price and 12 Week Chart | [Status OK] Display a note that actual mileage may be higher, which could result in a lower price | Verified Vehicle Report | Car Owner |  |
| SEQ: S-5.08 | 05 - View Market Price and 12 Week Chart | [Status OK] Display a disclaimer that the reference price is based on asking prices and is not Carpla’s purchase price | Verified Vehicle Report | Car Owner |  |
| SEQ: S-5.09 | 05 - View Market Price and 12 Week Chart | [Status OK – Electric Vehicle] Display a note that the price has not been adjusted for battery SoH | Verified Vehicle Report | Car Owner |  |
| SEQ: S-5.10 | 05 - View Market Price and 12 Week Chart | [INSUFFICIENT_DATA] Return status INSUFFICIENT_DATA | Market Price Service | Verified Vehicle Report |  |
| SEQ: S-5.11 | 05 - View Market Price and 12 Week Chart | [INSUFFICIENT_DATA] Display “Insufficient data to calculate the market price” (or insufficient verified mileage notice) | Verified Vehicle Report | Car Owner |  |
| SEQ: S-5.12 | 05 - View Market Price and 12 Week Chart | [New model, no listings] Return status “Calculating” | Market Price Service | Verified Vehicle Report |  |
| SEQ: S-5.13 | 05 - View Market Price and 12 Week Chart | [New model, no listings] Display “Calculating Price” | Verified Vehicle Report | Car Owner |  |
| SEQ: S-5.14 | 05 - View Market Price and 12 Week Chart | [Latest sync failed on all marketplaces] Return the previous price record | Market Price Service | Verified Vehicle Report |  |
| SEQ: S-5.15 | 05 - View Market Price and 12 Week Chart | [Latest sync failed on all marketplaces] Display the previous record with the label “Last Updated: dd/mm” | Verified Vehicle Report | Car Owner |  |
| SEQ: S-5.16 | 05 - View Market Price and 12 Week Chart | Request median prices for the latest 12 weeks | Verified Vehicle Report | Market Price Service |  |
| SEQ: S-5.17 | 05 - View Market Price and 12 Week Chart | Query 12 weekly records | Market Price Service | Weekly Price DB |  |
| SEQ: S-5.18 | 05 - View Market Price and 12 Week Chart | Return the list of records | Weekly Price DB | Market Price Service |  |
| SEQ: S-5.19 | 05 - View Market Price and 12 Week Chart | [Sufficient history] Return 12-week data | Market Price Service | Verified Vehicle Report |  |
| SEQ: S-5.20 | 05 - View Market Price and 12 Week Chart | [Sufficient history] Render the chart, leave INSUFFICIENT_DATA weeks blank, label back-calculated data as “Estimated” | Verified Vehicle Report | Car Owner |  |
| SEQ: S-5.21 | 05 - View Market Price and 12 Week Chart | [Sufficient history] Display a chart note explaining market fluctuations and the impact of additional mileage driven | Verified Vehicle Report | Car Owner |  |
| SEQ: S-5.22 | 05 - View Market Price and 12 Week Chart | [Insufficient history] Display “No Historical Data Available” | Verified Vehicle Report | Car Owner |  |
| SEQ: S-6.01 | 06 - Calculate Weekly Market Price | Trigger weekly job at a fixed time | Weekly Timer | Market Price Service |  |
| SEQ: S-6.02 | 06 - Calculate Weekly Market Price | [par: Nextgen VN] Request incremental sync (make and model only) | Market Price Service | Nextgen VN |  |
| SEQ: S-6.03 | 06 - Calculate Weekly Market Price | [par: Nextgen VN - sync failed] Retry synchronization (up to 3 retries, 6 hours apart) | Market Price Service | Nextgen VN |  |
| SEQ: S-6.04 | 06 - Calculate Weekly Market Price | [par: Nextgen VN] Return new, price-updated, and removed listings | Nextgen VN | Market Price Service |  |
| SEQ: S-6.05 | 06 - Calculate Weekly Market Price | [par: Cho Tot] Request incremental sync (make and model only) | Market Price Service | Cho Tot |  |
| SEQ: S-6.06 | 06 - Calculate Weekly Market Price | [par: Cho Tot – sync failed] Retry synchronization (up to 3 retries, 6 hours apart) | Market Price Service | Cho Tot |  |
| SEQ: S-6.07 | 06 - Calculate Weekly Market Price | [par: Cho Tot] Return new, price-updated, and removed listings | Cho Tot | Market Price Service |  |
| SEQ: S-6.08 | 06 - Calculate Weekly Market Price | Store listings with source marketplace, original listing ID, posting date, removal date, and anonymized seller ID | Market Price Service | Listing Store |  |
| SEQ: S-6.09 | 06 - Calculate Weekly Market Price | Determine valid data sources for the current week | Market Price Service | Market Price Service |  |
| SEQ: S-6.10 | 06 - Calculate Weekly Market Price | [All syncs failed] Record status API_FAILED | Market Price Service | Weekly Price DB |  |
| SEQ: S-6.11 | 06 - Calculate Weekly Market Price | [≥1 sync succeeded] Retrieve listings for the vehicle model | Market Price Service | Listing Store |  |
| SEQ: S-6.12 | 06 - Calculate Weekly Market Price | Map make and model to the TASCO catalog, normalize prices to VND and mileage to km | Market Price Service | Market Price Service |  |
| SEQ: S-6.13 | 06 - Calculate Weekly Market Price | Retain listings active within the last 90 days | Market Price Service | Market Price Service |  |
| SEQ: S-6.14 | 06 - Calculate Weekly Market Price | Exclude listings with missing fields, zero prices, or negotiable prices | Market Price Service | Market Price Service |  |
| SEQ: S-6.15 | 06 - Calculate Weekly Market Price | Remove cross-marketplace duplicates and retain the most recent listing | Market Price Service | Market Price Service |  |
| SEQ: S-6.16 | 06 - Calculate Weekly Market Price | Retrieve minimum verified mileage and manufacturing year | Market Price Service | VETC |  |
| SEQ: S-6.17 | 06 - Calculate Weekly Market Price | Return minimum verified mileage and manufacturing year | VETC | Market Price Service |  |
| SEQ: S-6.18 | 06 - Calculate Weekly Market Price | Compare minimum verified mileage divided by vehicle age (minimum 1 year) against the configured threshold | Market Price Service | Market Price Service |  |
| SEQ: S-6.19 | 06 - Calculate Weekly Market Price | [Mileage below threshold] Record status INSUFFICIENT_DATA | Market Price Service | Weekly Price DB |  |
| SEQ: S-6.20 | 06 - Calculate Weekly Market Price | [Mileage plausible] Select Level 0 listings (year ±1, mileage ±20%, minimum mileage ±10,000 km) | Market Price Service | Market Price Service |  |
| SEQ: S-6.21 | 06 - Calculate Weekly Market Price | Remove price outliers outside Q1 - 1.5 x IQR to Q3 + 1.5 x IQR | Market Price Service | Market Price Service |  |
| SEQ: S-6.22 | 06 - Calculate Weekly Market Price | [<30 listings] Expand to Level 1 (year ±2) | Market Price Service | Market Price Service |  |
| SEQ: S-6.23 | 06 - Calculate Weekly Market Price | [<30 listings] Expand to Level 2 (mileage ±40%, minimum mileage ±20,000 km) | Market Price Service | Market Price Service |  |
| SEQ: S-6.24 | 06 - Calculate Weekly Market Price | [<30 listings after Level 2] Record status INSUFFICIENT_DATA | Market Price Service | Weekly Price DB |  |
| SEQ: S-6.25 | 06 - Calculate Weekly Market Price | [Sufficient sample] Calculate median and P25-P75, round to the nearest million VND | Market Price Service | Market Price Service |  |
| SEQ: S-6.26 | 06 - Calculate Weekly Market Price | [Sufficient sample] Assign confidence level: High, Medium, or Low | Market Price Service | Market Price Service |  |
| SEQ: S-6.27 | 06 - Calculate Weekly Market Price | [Sufficient sample] Save weekly price record | Market Price Service | Weekly Price DB |  |
| SEQ: S-7.01 | 07 - 12-week Backfill and 180-day Sync | Add a new vehicle | Car Owner | Verified Vehicle Report |  |
| SEQ: S-7.02 | 07 - 12-week Backfill and 180-day Sync | Trigger 12-week historical backfill | Verified Vehicle Report | Market Price Service |  |
| SEQ: S-7.03 | 07 - 12-week Backfill and 180-day Sync | Check whether the vehicle model already exists in the listing store | Market Price Service | Listing Store |  |
| SEQ: S-7.04 | 07 - 12-week Backfill and 180-day Sync | [Model not in store] Return status “Calculating Price” | Market Price Service | Verified Vehicle Report |  |
| SEQ: S-7.05 | 07 - 12-week Backfill and 180-day Sync | [Model not in store] Display “Calculating Price” | Verified Vehicle Report | Car Owner |  |
| SEQ: S-7.06 | 07 - 12-week Backfill and 180-day Sync | [par: Nextgen VN] Request 180-day listing history for the vehicle model | Market Price Service | Nextgen VN |  |
| SEQ: S-7.07 | 07 - 12-week Backfill and 180-day Sync | [par: Nextgen VN] Return historical listings | Nextgen VN | Market Price Service |  |
| SEQ: S-7.08 | 07 - 12-week Backfill and 180-day Sync | [par: Cho Tot] Request 180-day listing history for the vehicle model | Market Price Service | Cho Tot |  |
| SEQ: S-7.09 | 07 - 12-week Backfill and 180-day Sync | [par: Cho Tot] Return historical listings | Cho Tot | Market Price Service |  |
| SEQ: S-7.10 | 07 - 12-week Backfill and 180-day Sync | Store retrieved listings | Market Price Service | Listing Store |  |
| SEQ: S-7.11 | 07 - 12-week Backfill and 180-day Sync | Retrieve minimum verified mileage as of the checkpoint (historical toll passage records) | Market Price Service | VETC |  |
| SEQ: S-7.12 | 07 - 12-week Backfill and 180-day Sync | Return mileage as of the checkpoint | VETC | Market Price Service |  |
| SEQ: S-7.13 | 07 - 12-week Backfill and 180-day Sync | [Mileage missing/implausible] Record INSUFFICIENT_DATA with the back-calculation flag | Market Price Service | Weekly Price DB |  |
| SEQ: S-7.14 | 07 - 12-week Backfill and 180-day Sync | [Valid mileage] Retrieve listings active at the checkpoint | Market Price Service | Listing Store |  |
| SEQ: S-7.15 | 07 - 12-week Backfill and 180-day Sync | Clean and standardize listings | Market Price Service | Market Price Service |  |
| SEQ: S-7.16 | 07 - 12-week Backfill and 180-day Sync | Select comparable vehicles | Market Price Service | Market Price Service |  |
| SEQ: S-7.17 | 07 - 12-week Backfill and 180-day Sync | [Insufficient sample] Record INSUFFICIENT_DATA with the back-calculation flag | Market Price Service | Weekly Price DB |  |
| SEQ: S-7.18 | 07 - 12-week Backfill and 180-day Sync | [Sufficient sample] Calculate price and confidence level | Market Price Service | Market Price Service |  |
| SEQ: S-7.19 | 07 - 12-week Backfill and 180-day Sync | [Sufficient sample] Save the price record with the back-calculation flag (displayed as “Estimated”) | Market Price Service | Weekly Price DB |  |
| SEQ: S-7.20 | 07 - 12-week Backfill and 180-day Sync | Backfill completed | Market Price Service | Verified Vehicle Report |  |
| SEQ: S-7.21 | 07 - 12-week Backfill and 180-day Sync | Display the market price and 12-week price trend chart | Verified Vehicle Report | Car Owner |  |
| SEQ: S-8.01 | 08 - Stop Tracking | Delete the vehicle or mark it as sold | Car Owner | Verified Vehicle Report |  |
| SEQ: S-8.02 | 08 - Stop Tracking | Request to stop price tracking | Verified Vehicle Report | Market Price Service |  |
| SEQ: S-8.03 | 08 - Stop Tracking | Remove the vehicle from the weekly job | Market Price Service | Market Price Service |  |
| SEQ: S-8.04 | 08 - Stop Tracking | Confirm request | Market Price Service | Verified Vehicle Report |  |
| SEQ: S-8.05 | 08 - Stop Tracking | Market price tracking has been stopped | Verified Vehicle Report | Car Owner |  |
| SEQ: S-9.01 | 09 - Submit Sale Request | Enter desired price, current ODO and phone number | Car Owner | TASCO Mobility System |  |
| SEQ: S-9.02 | 09 - Submit Sale Request | Click Submit sale request | Car Owner | TASCO Mobility System |  |
| SEQ: S-9.03 | 09 - Submit Sale Request | [par: UC-3.03] Query plate/VIN | TASCO Mobility System | Vehicle Inspection Portal |  |
| SEQ: S-9.04 | 09 - Submit Sale Request | [par: UC-3.03] Inspection validity | Vehicle Inspection Portal | TASCO Mobility System |  |
| SEQ: S-9.05 | 09 - Submit Sale Request | [par: UC-3.04] Query unpaid fines | TASCO Mobility System | Traffic Fine Lookup Portal |  |
| SEQ: S-9.06 | 09 - Submit Sale Request | [par: UC-3.04] Fine status | Traffic Fine Lookup Portal | TASCO Mobility System |  |
| SEQ: S-9.07 | 09 - Submit Sale Request | [par: UC-3.05] Query mortgage status | TASCO Mobility System | Secured Transaction Registry Portal |  |
| SEQ: S-9.08 | 09 - Submit Sale Request | [par: UC-3.05] Mortgage status | Secured Transaction Registry Portal | TASCO Mobility System |  |
| SEQ: S-9.09 | 09 - Submit Sale Request | [Any API gateway fails] Block submit and show error | TASCO Mobility System | Car Owner |  |
| SEQ: S-9.10 | 09 - Submit Sale Request | [All checks succeed – vehicle mortgaged] Attach Red Flag to report | TASCO Mobility System | TASCO Mobility System |  |
| SEQ: S-9.11 | 09 - Submit Sale Request | [All checks succeed] Bundle desired price, current ODO, phone number and verified vehicle report (identification & status) | TASCO Mobility System | TASCO Mobility System |  |
| SEQ: S-9.12 | 09 - Submit Sale Request | [All checks succeed] Forward data package | TASCO Mobility System | Carpla CRM |  |
| SEQ: S-9.13 | 09 - Submit Sale Request | [All checks succeed] Request submitted successfully | TASCO Mobility System | Car Owner |  |
| SEQ: S-10.01 | 10 - Staff Review and Car Owner Preliminary Quote | Open internal system | Carpla Staff | Carpla CRM |  |
| SEQ: S-10.02 | 10 - Staff Review and Car Owner Preliminary Quote | Show Verified Report (Identification & Legal status), desired price, market price, current ODO and phone number | Carpla CRM | Carpla Staff |  |
| SEQ: S-10.03 | 10 - Staff Review and Car Owner Preliminary Quote | Evaluate maintenance, fines, mortgage, price | Carpla Staff | Carpla Staff |  |
| SEQ: S-10.04 | 10 - Staff Review and Car Owner Preliminary Quote | Input suggested quote range | Carpla Staff | Carpla CRM |  |
| SEQ: S-10.05 | 10 - Staff Review and Car Owner Preliminary Quote | Submit preliminary quote | Carpla CRM | TASCO Mobility System |  |
| SEQ: S-10.06 | 10 - Staff Review and Car Owner Preliminary Quote | Push notification | TASCO Mobility System | Car Owner |  |
| SEQ: S-10.07 | 10 - Staff Review and Car Owner Preliminary Quote | Open app and view quote | Car Owner | TASCO Mobility System |  |
| SEQ: S-10.08 | 10 - Staff Review and Car Owner Preliminary Quote | Display preliminary quote | TASCO Mobility System | Car Owner |  |
| SEQ: S-10.09 | 10 - Staff Review and Car Owner Preliminary Quote | Decide to accept and book Physical Inspection at showroom | Car Owner | Car Owner |  |

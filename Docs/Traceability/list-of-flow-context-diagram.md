# List of flow in context diagram:

| CTX: flow-No. | Direction | From | To | Flow label |
|---|---|---|---|---|
| CTX: flow-01 | Owner → System | Car Owner | TASCO Mobility | Sell request (mileage, phone number, desired price) |
| CTX: flow-02 | Owner → System | Car Owner | TASCO Mobility | VIN |
| CTX: flow-03 | Owner → System | Car Owner | TASCO Mobility | External invoice (optional) |
| CTX: flow-04 | Owner → System | Car Owner | TASCO Mobility | Refresh request |
| CTX: flow-05 | System → Owner | TASCO Mobility | Car Owner | Verified vehicle report |
| CTX: flow-06 | System → Owner | TASCO Mobility | Car Owner | Pre-filled Carpla quote form |
| CTX: flow-07 | System → Owner | TASCO Mobility | Car Owner | Preliminary quote |
| CTX: flow-08 | System → Owner | TASCO Mobility | Car Owner | Market price & 12-week price chart |
| CTX: flow-09 | System → Staff | TASCO Mobility | Carpla Staff | Sell request (mileage, phone number, desired price) |
| CTX: flow-10 | System → Staff | TASCO Mobility | Carpla Staff | Verified vehicle report & market price (identification & vehicle status) |
| CTX: flow-11 | Staff → System | Carpla Staff | TASCO Mobility | Preliminary quote |
| CTX: flow-12 | TASCO → System | VETC | TASCO Mobility | Identification data & toll passage history |
| CTX: flow-13 | TASCO → System | Carpla Service | TASCO Mobility | Maintenance & repair history |
| CTX: flow-14 | Partner → System | Electric Vehicle Charging Stations | TASCO Mobility | Charging session data |
| CTX: flow-15 | Partner → System | Nextgen Việt Nam | TASCO Mobility | Used-car listing data (brand, model, manufacturing year, mileage, price; posting date, listing status, removal date) |
| CTX: flow-16 | Partner → System | Chợ Tốt - Carousell Group | TASCO Mobility | Used-car listing data (brand, model, manufacturing year, mileage, price; posting date, listing status, removal date) |
| CTX: flow-17 | System → Gov | TASCO Mobility | Electronic Invoice Portal | Seller tax code, invoice symbol, invoice number, total amount |
| CTX: flow-18 | Gov → System | Electronic Invoice Portal | TASCO Mobility | Invoice authenticity result |
| CTX: flow-19 | System → Gov | TASCO Mobility | Traffic Fine Lookup Portal | License plate |
| CTX: flow-20 | Gov → System | Traffic Fine Lookup Portal | TASCO Mobility | Traffic fine status |
| CTX: flow-21 | System → Gov | TASCO Mobility | Secured Transaction Registry Portal | VIN |
| CTX: flow-22 | Gov → System | Secured Transaction Registry Portal | TASCO Mobility | Mortgage status: credit institution, loan amount, active / released |
| CTX: flow-23 | System → Gov | TASCO Mobility | Vehicle Inspection Portal | License plate, VIN |
| CTX: flow-24 | Gov → System | Vehicle Inspection Portal | TASCO Mobility | Inspection status |

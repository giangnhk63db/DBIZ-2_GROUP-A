```mermaid
flowchart TB
   subgraph TASCO["TASCO CORPORATION"]
       direction LR
       VETC["VETC"]
       CARPLA_SV["Carpla Service"]
   end

   subgraph PARTNER["PARTNER"]
       direction LR
       EVCS["Electric Vehicle<br/>Charging Stations"]
       NEXTGEN["Nextgen Việt Nam"]
       CHOTOT["Chợ Tốt - Carousell Group"]
   end

   P(["<b>Verified Vehicle Report</b>"])
   OWNER["Car Owner"]
   STAFF["Carpla Staff"]

   subgraph GOV["GOVERNMENT"]
       direction LR
       EINV["Electronic Invoice<br/>Portal"]
       FINE["Traffic Fine Lookup<br/>Portal"]
       STR["Secured Transaction<br/>Registry Portal"]
       INSP["Vehicle Inspection<br/>Portal"]
   end

   VETC -->|"Identification data &<br/>toll passage history"| P
   CARPLA_SV -->|"Maintenance &<br/>repair history"| P

   EVCS -->|"Charging session data"| P
   NEXTGEN -->|"Used-car listing data<br/>(Brand, model; Manufacturing year, mileage, price;<br/>Posting date, listing status, removal date)"| P
   CHOTOT -->|"Used-car listing data<br/>(Brand, model; Manufacturing year, mileage, price;<br/>Posting date, listing status, removal date)"| P

   OWNER -->|"Sell request<br/>(mileage, phone number, desired price)"| P
   OWNER -->|"VIN"| P
   OWNER -->|"External invoice (optional)"| P
   OWNER -->|"Refresh request"| P

   P -->|"Verified vehicle report"| OWNER
   P -->|"Pre-filled Carpla quote form"| OWNER
   P -->|"Preliminary quote"| OWNER
   P -->|"Market price & 12-week price chart"| OWNER

   P -->|"Sell request<br/>(mileage, phone number, desired price)"| STAFF
   P -->|"Verified vehicle report & market price<br/>(identification & vehicle status)"| STAFF
   STAFF -->|"Preliminary quote"| P

   P -->|"Seller tax code, invoice symbol,<br/>invoice number, total amount"| EINV
   EINV -->|"Invoice authenticity result"| P

   P -->|"License plate"| FINE
   FINE -->|"Traffic fine status"| P

   P -->|"VIN"| STR
   STR -->|"Mortgage status: credit institution,<br/>loan amount, active / released"| P

   P -->|"License plate, VIN"| INSP
   INSP -->|"Inspection status"| P

   classDef process fill:#fcefc7,stroke:#b8862b,stroke-width:2px,color:#000
   classDef tasco fill:#fdf1cf,stroke:#c9a227,color:#000
   classDef partner fill:#d9f7fb,stroke:#1a1aa6,color:#000
   classDef owner fill:#9fc5e8,stroke:#1a1aa6,color:#000
   classDef pink fill:#f4cccc,stroke:#cc4a6a,color:#000

   class P process
   class VETC,CARPLA_SV tasco
   class EVCS,NEXTGEN,CHOTOT partner
   class OWNER owner
   class STAFF,EINV,FINE,STR,INSP pink

   style TASCO fill:none,stroke:#555,stroke-dasharray:5 5
   style PARTNER fill:none,stroke:#555,stroke-dasharray:5 5
   ```

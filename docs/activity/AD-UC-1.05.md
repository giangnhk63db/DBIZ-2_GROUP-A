# Activity Diagram: AD-UC-1.05 Upload External Maintenance & Repair Invoices

| Field | Value |
| :---- | :---- |
| AD ID | AD-UC-1.05 |
| Use case | UC-1.05 Upload External Maintenance & Repair Invoices |
| Feature | F-01 Vehicle Identification & Status |
| Primary actor | Car Owner |
| Lanes | Car Owner · TASCO Mobility System · Electronic Invoice Portal |
| Inputs (L3-L5) | Context Diagram v1 · Use Case Diagram v1 · Sequence: SD-UC-1.05 (Figure 9\) · W1 rows (FR-01-05-FE, FR-01-05-BE, FR-01-06-BE, FR-01-07-INT, FR-01-07-BE) |
| Trigger | Car owner selects upload external maintenance and repair invoice |
| Preconditions | Vehicle profile exists in the system and car owner is authenticated |
| Success end state | External invoice file is successfully validated, parsed (or manually completed by the car owner), cross-checked, and merged into the vehicle maintenance history timeline |
| Failure end state(s) | Invoice file is rejected due to invalid format/size or authentication mismatch with the Electronic Invoice Portal |
| FRs covered | FR-01-05-FE, FR-01-05-BE, FR-01-06-BE, FR-01-07-INT, FR-01-07-BE |
| Business rules applied | BR-FILE-01, BR-OCR-01, BR-INV-01 |
| Version / status | v0.4 · Reviewed |
| Author | Đinh Thu Thảo |

## 

1. ## **Diagram** 

```mermaid

swimlane-beta TB
  subgraph CAR["Car Owner"]
    start@{ shape: sm-circ }
    a1("Open maintenance history and select upload invoice (FR-01-05-FE)")
    m1{" "}
    a2("Upload invoice file PDF, XML, or image (FR-01-05-FE)")
    a_edit("Review and edit extracted invoice info if needed, then confirm (FR-01-05-FE)")
    d_choice{"Enter manually or choose another file?"}
    a_manual("Fill in missing fields or enter all invoice info manually, then confirm (FR-01-05-FE)")
    m4{" "}
    a3("See updated maintenance timeline or error message (FR-01-05-FE)")
    stop1@{ shape: fr-circ }
  end
  subgraph SYS["TASCO Mobility System"]
    d1{"File format or size valid?"}
    s1("Reject invalid file and show error (FR-01-05-FE)")
    s2("Store uploaded file (FR-01-05-BE)")
    s3a("Detect real file type: XML, PDF, or image (FR-01-06-BE)")
    d_xml{"XML file?"}
    s3b("Parse XML tags directly, no OCR (FR-01-06-BE)")
    s3c("Extract PDF text or preprocess image and run OCR (FR-01-06-BE)")
    m2{" "}
    s3d("Extract and normalize tax code, invoice number, total amount, invoice date (FR-01-06-BE)")
    d2{"All 4 fields extracted and valid?"}
    s_ocr("Notify OCR failure or missing fields and open manual entry screen, keep any extracted values (FR-01-06-BE)")
    s4("Display extracted data on edit/confirmation screen, highlight low-confidence fields (FR-01-06-BE)")
    m3{" "}
    s5("Send invoice info via API to verify with Electronic Invoice Portal (FR-01-07-INT)")
    d3{"Invoice valid?"}
    s6("Merge validated external invoice into vehicle history (FR-01-07-BE)")
    s7("Reject invoice and show error message (FR-01-07-BE)")
  end
  subgraph PORTAL["Electronic Invoice Portal"]
    e1("Verify invoice and return authenticity result (FR-01-07-INT)")
  end

  start --> a1 --> m1 --> a2 --> d1
  d1 -->|"[no]"| s1 --> m4
  d1 -->|"[yes]"| s2 --> s3a --> d_xml
  d_xml -->|"[yes]"| s3b --> m2
  d_xml -->|"[no]"| s3c --> m2
  m2 --> s3d --> d2
  d2 -->|"[yes]"| s4 --> a_edit --> m3
  d2 -->|"[no]"| s_ocr --> d_choice
  d_choice -->|"[enter manually]"| a_manual --> m3
  d_choice -->|"[choose another file]"| m1
  m3 --> s5 --> e1 --> d3
  d3 -->|"[yes]"| s6 --> m4
  d3 -->|"[no]"| s7 --> m4
  m4 --> a3 --> stop1
```

**Reading notes:**

- File Upload & Validation: The car owner uploads an external invoice file (PDF, XML, or image), and the system validates its format and size before storing it; if invalid, the file is rejected with an error.
- Extraction (XML vs OCR): The system detects the real file type. XML files are parsed directly from their tags (no OCR needed). PDF and image files go through text extraction or image preprocessing and OCR. In both cases the system extracts and normalizes four key fields: seller tax code, invoice number, total amount, and invoice date.
- Extraction Result: If all four fields are extracted in a valid format, the data is displayed on the edit/confirmation screen for the car owner to review, edit if needed, and confirm (low-confidence fields are highlighted). If the file is unreadable or any field is missing or invalid, the system notifies the car owner and opens the manual entry screen, keeping any values it did extract.
- Manual Entry or Re-upload: On the manual entry screen, the car owner chooses either to fill in the missing fields or enter all invoice info manually and confirm, or to choose another file, which returns to the upload step.
- Authentication Check: After the car owner confirms (from review or manual entry), the system sends the invoice information via API to the Electronic Invoice Portal to verify authenticity.
- Result & Timeline Update: If the invoice is valid, it is merged into the vehicle's maintenance history and the updated timeline is displayed; if invalid (or the file was rejected at validation), an error message is shown.

2. ## **Node trace**

| Node | Kind | Text | Traces to |
| :---- | :---- | :---- | :---- |
| a1 | Action | Open maintenance history and select upload invoice | FR-01-05-FE |
| m1 | Merge | Merge path for initial upload and re-upload attempts | N/A (Control Node) |
| a2 | Action | Upload invoice file PDF, XML, or image | FR-01-05-FE |
| d1 | Decision | File format or size valid? | FR-01-05-FE / BR-FILE-01 |
| s1 | Action | Reject invalid file and show error | FR-01-05-FE / BR-FILE-01 |
| s2 | Action | Store uploaded file | FR-01-05-BE |
| s3a | Action | Detect real file type: XML, PDF, or image | FR-01-06-BE / BR-OCR-01 |
| d_xml | Decision | XML file? | FR-01-06-BE / BR-OCR-01 |
| s3b | Action | Parse XML tags directly, no OCR | FR-01-06-BE / BR-OCR-01 |
| s3c | Action | Extract PDF text or preprocess image and run OCR | FR-01-06-BE / BR-OCR-01 |
| m2 | Merge | Merge XML parsing path and PDF/image OCR path | N/A (Control Node) |
| s3d | Action | Extract and normalize tax code, invoice number, total amount, invoice date | FR-01-06-BE / BR-OCR-01 |
| d2 | Decision | All 4 fields extracted and valid? | FR-01-06-BE / BR-OCR-01 |
| s_ocr | Action | Notify OCR failure or missing fields and open manual entry screen, keep any extracted values | FR-01-06-BE / BR-OCR-01 |
| s4 | Action | Display extracted data on edit/confirmation screen, highlight low-confidence fields | FR-01-06-BE / BR-OCR-01 |
| a_edit | Action | Review and edit extracted invoice info if needed, then confirm | FR-01-05-FE / BR-OCR-01 |
| d_choice | Decision | Enter manually or choose another file? | FR-01-05-FE / BR-OCR-01 |
| a_manual | Action | Fill in missing fields or enter all invoice info manually, then confirm | FR-01-05-FE / BR-OCR-01 |
| m3 | Merge | Merge confirmed invoice info from review and manual entry | N/A (Control Node) |
| s5 | Action | Send invoice info via API to verify with Electronic Invoice Portal | FR-01-07-INT |
| e1 | Action | Verify invoice and return authenticity result | FR-01-07-INT |
| d3 | Decision | Invoice valid? | FR-01-07-BE / BR-INV-01 |
| s6 | Action | Merge validated external invoice into vehicle history | FR-01-07-BE / BR-INV-01 |
| s7 | Action | Reject invoice and show error message | FR-01-07-BE / BR-INV-01 |
| m4 | Merge | Merge all final success and error notification flows | N/A (Control Node) |
| a3 | Action | See updated maintenance timeline or error message | FR-01-05-FE |
## 

**Business rule description**

| Rule ID | Description | Applied Node |
| ----- | ----- | :---: |
| BR-FILE-01 | File Format and Size Rule: The system only accepts uploaded files in .pdf, .xml, or image formats (.jpg, .png), with a file size adhering to the system limit (e.g., <= 10MB). Invalid files are rejected immediately at the validation step. | d1 |
| BR-OCR-01 | Automated OCR Data Extraction & Review Rule: The system extracts four key invoice fields: seller tax code, invoice number, total amount, and invoice date. (1) File type is detected from the actual file content, not only the extension. (2) XML files are parsed directly from their tags and do not go through OCR. (3) PDF files with a text layer have their text extracted directly; scanned PDFs and images are preprocessed (rotation, deskew, noise removal, contrast enhancement) and then run through OCR, which returns text, position, and confidence per token. (4) Each field is located by keyword and position and then normalized and format-checked: **Tax code** is the seller's (not the buyer's), 10 digits or 13 digits (10 + 3-digit branch code), spaces and hyphens removed; **Invoice number** is digits only; **Total amount** is the final payable total (including VAT, not line-item or pre-tax amounts), thousand separators removed, converted to a positive integer in VND, and cross-checked against the amount in words when available; **Invoice date** is a valid date (dd/mm/yyyy), not in the future, and stored in ISO format. (5) If all four fields are extracted in a valid format, the data is shown on an edit/confirmation screen, so the car owner can review and edit if needed before confirming. (6) If the file is unreadable, or any field is missing or invalid, the file is not rejected: the system notifies the car owner, opens the manual entry screen with any extracted values kept, and the car owner either fills in the missing fields or enters all invoice info manually, or chooses another file to upload. (7) The car owner must provide all four fields before confirming. User edits and manual entries do not bypass BR-INV-01: the Portal still decides whether the invoice is valid. | d_xml, s3b, s3c, s3d, d2, s_ocr, s4, a_edit, d_choice, a_manual |
| BR-INV-01 | Invoice Authenticity Cross-Check Rule: After confirmation, invoice data is sent via API to the Electronic Invoice Portal for verification. An invoice is considered valid only if the tax code exists, details match, and the invoice status is not cancelled or replaced on the tax system. | d3 |

3. ## **Branches and acceptance tests**

| Branch | Given | When | Then |
| :---- | :---- | :---- | :---- |
| d1 [no] | Vehicle maintenance screen is open | Car owner uploads an unsupported file format (.exe, .txt) or oversized file | System rejects the file immediately and displays an error message |
| d1 [yes] | Vehicle maintenance screen is open | Car owner uploads a valid PDF, XML, or image file under size limit | System accepts and stores the uploaded file, then detects the file type and starts extraction |
| d_xml [yes] | File is stored successfully | The uploaded file is an XML invoice | System parses the XML tags directly (no OCR) and extracts tax code, invoice number, total amount, and invoice date |
| d_xml [no] | File is stored successfully | The uploaded file is a PDF or image | System extracts PDF text or preprocesses the image and runs OCR, then extracts and normalizes the four fields |
| d2 [yes] | File is stored successfully | All four fields (tax code, invoice number, total amount, invoice date) are extracted in a valid format | System displays extracted data on the edit/confirmation screen (low-confidence fields highlighted) for the car owner to review, edit if needed, and confirm |
| d2 [no] | File is stored successfully | OCR or XML parsing fails (blurred scan, corrupt structure), or one or more fields are missing or invalid | System notifies the car owner, opens the manual entry screen with any extracted values kept, and offers the choice to enter manually or choose another file |
| d_choice [enter manually] | Manual entry screen is open after extraction failure or missing fields | Car owner fills in the missing fields or enters all invoice info manually and confirms | System accepts the confirmed info and sends it to the Electronic Invoice Portal for verification |
| d_choice [choose another file] | Manual entry screen is open after extraction failure or missing fields | Car owner chooses to select another file | System returns to the upload step so the car owner can upload a new file |
| d3 [yes] | Car owner confirms invoice data and API request is sent | Electronic Invoice Portal confirms invoice authenticity and returns valid status | System merges the validated external invoice into the vehicle maintenance history and displays the updated timeline |
| d3 [no] | Car owner confirms invoice data and API request is sent | Electronic Invoice Portal returns invalid, fake, or unmatched status | System rejects the invoice, prevents merging, and displays an error message |

4. ## **Object states used** ## 

| Object | States in this diagram | Full state list (all diagrams) |
| :---- | :---- | :---- |
| Invoice File | Uploaded -> Stored -> Parsed -> Verified / Rejected | Uploaded, Stored, Parsed, Verified, Archived, Rejected |
| Maintenance Timeline | Consolidated -> Pending Update -> Updated | Initial, Pending Update, Updated, Synced |

5. ## **Screens touched** ## 

| Actor action | Screen ID |
| :---- | :---- |
| Open maintenance history and select upload invoice (a1) | S-F01-02 -> S-F01-05 (Upload invoice modal) |
| Upload invoice file PDF, XML, or image (a2) | S-F01-05 (Upload invoice modal) |
| Review and edit extracted invoice info if needed, then confirm (a_edit) | S-F01-05 (Invoice review & confirmation view) |
| Choose to enter manually or choose another file (d_choice) | S-F01-05 (Invoice review & confirmation view) / S-F01-05 (Upload invoice modal) |
| Fill in missing fields or enter all invoice info manually, then confirm (a_manual) | S-F01-05 (Invoice review & confirmation view) |
| See updated maintenance timeline or error message (a3) | S-F01-05 / S-F01-02 |
## 

6. ## **Open questions** ## 

| \# | Question | Blocking? | Default applied |
| :---- | :---- | :---- | :---- |
| 1 | Maximum allowable upload file size for images and PDFs? | No | Set limit to 10 MB per invoice file |
| 2 | Confidence threshold below which an extracted field is highlighted for review? | No | Set threshold to 80% |
| 3 | Does the Electronic Invoice Portal API require additional fields beyond tax code, invoice number, total amount, and invoice date (e.g., invoice series/symbol)? | Yes (for FR-01-07-INT) | Send the four extracted fields only; confirm with the integration team |
| 4 | Which OCR engine is used (self-hosted or external service)? This affects invoice data security. | No | Not specified; the diagram stays engine-agnostic |

## 

7. ## **Verification log** ## 

| Check | Result | By | Date |
| :---- | :---- | :---- | :---- |
| Renders on mermaid.live | Pass | Đinh Thu Thảo | 2026-10-26 |
| Every decision has guards on all outgoing flows, no overlap, no gap | Pass | Đinh Thu Thảo | 2026-10-26 |
| No action has two incoming flows (merge used instead) | Pass | Đinh Thu Thảo | 2026-10-26 |
| Every fork has a matching join | Pass | Đinh Thu Thảo | 2026-10-26 |
| Every system action cites an FR | Pass | Đinh Thu Thảo | 2026-10-26 |
| Every FR of this use case appears in the diagram | Pass | Đinh Thu Thảo | 2026-10-26 |
| Lane names match the Context Diagram and the use case actors; no Web App, API or database lanes | Pass | Đinh Thu Thảo | 2026-10-26 |
| Related sequence diagram uses the same names and FR IDs; its alt fragments appear here as decisions | Pass | Đinh Thu Thảo | 2026-10-26 |
| AI critic prompt run, findings resolved or logged as open questions | Pass | Đinh Thu Thảo | 2026-10-26 |

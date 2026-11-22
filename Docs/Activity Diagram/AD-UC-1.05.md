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
| Success end state | External invoice file is successfully validated, parsed, cross-checked, and merged into the vehicle maintenance history timeline |
| Failure end state(s) | Invoice file is rejected due to invalid format/size or authentication mismatch with the Electronic Invoice Portal |
| FRs covered | FR-01-05-FE, FR-01-05-BE, FR-01-06-BE, FR-01-07-INT, FR-01-07-BE |
| Business rules applied | BR-FILE-01, BR-OCR-01, BR-INV-01 |
| Version / status | v0.4 · Reviewed |
| Author | Đinh Thu Thảo |

## 

## **1\. Diagram**

*Code:*  

```mermaid

swimlane-beta TB
  subgraph CAR["Car Owner"]
    start@{ shape: sm-circ }
    a1("Open maintenance history and select upload invoice (FR-01-05-FE)")
    m1{" "}
    a2("Upload invoice file PDF, XML, or image (FR-01-05-FE)")
    a_edit("Review and edit extracted invoice info if needed, then confirm (FR-01-05-FE)")
    m4{" "}
    a3("See updated maintenance timeline or error message (FR-01-05-FE)")
    stop1@{ shape: fr-circ }
  end
  subgraph SYS["TASCO Mobility System"]
    d1{"File format or size valid?"}
    s1("Reject invalid file and show error (FR-01-05-FE)")
    s2("Store uploaded file (FR-01-05-BE)")
    s3("Extract invoice data via OCR or parsing (FR-01-06-BE)")
    d2{"Parsing successful?"}
    s_ocr("Reject unreadable file and show OCR error (FR-01-06-BE)")
    s4("Display extracted data on edit/confirmation screen (FR-01-06-BE)")
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
  d1 -->|"[yes]"| s2 --> s3 --> d2
  d2 -->|"[no]"| s_ocr --> m4
  d2 -->|"[yes]"| s4 --> a_edit --> s5
  s5 --> e1 --> d3
  d3 -->|"[yes]"| s6 --> m4
  d3 -->|"[no]"| s7 --> m4
  m4 --> a3 --> stop1
```

**Reading notes:**

- File Upload & Validation: The car owner uploads an external invoice file (PDF, XML, or image), and the system validates its format and size before storing it; if invalid, the file is rejected with an error.
- OCR Extraction & User Review: The system extracts key invoice details via OCR or parsing. If parsing fails, the unreadable file is rejected with an OCR error; otherwise, the extracted data is displayed for the car owner to review, edit if needed, and confirm.
- Authentication Check: After user confirmation, the system sends the invoice information via API to the Electronic Invoice Portal to verify authenticity.
- Result & Timeline Update: If the invoice is valid, it is merged into the vehicle's maintenance history and the updated timeline is displayed; if invalid (or rejected earlier), an error message is shown.

## 2. Node trace

| Node | Kind | Text | Traces to |
| :---- | :---- | :---- | :---- |
| a1 | Action | Open maintenance history and select upload invoice | FR-01-05-FE |
| m1 | Merge | Merge path for initial upload and re-upload attempts | N/A (Control Node) |
| a2 | Action | Upload invoice file PDF, XML, or image | FR-01-05-FE |
| d1 | Decision | File format or size valid? | FR-01-05-FE / BR-FILE-01 |
| s1 | Action | Reject invalid file and show error | FR-01-05-FE / BR-FILE-01 |
| s2 | Action | Store uploaded file | FR-01-05-BE |
| s3 | Action | Extract invoice data via OCR or parsing | FR-01-06-BE / BR-OCR-01 |
| d2 | Decision | Parsing successful? | FR-01-06-BE / BR-OCR-01 |
| s_ocr | Action | Reject unreadable file and show OCR error | FR-01-06-BE / BR-OCR-01 |
| s4 | Action | Display extracted data on edit/confirmation screen | FR-01-06-BE / BR-OCR-01 |
| a_edit | Action | Review and edit extracted invoice info if needed, then confirm | FR-01-05-FE / BR-OCR-01 |
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
| BR-OCR-01 | Automated OCR Data Extraction & Review Rule: The OCR/Parsing engine extracts key invoice data (seller tax code, invoice number, total amount, date). If parsing fails or the file is unreadable, the system rejects the file and displays an OCR error. Otherwise, extracted data is shown on an edit/confirmation screen so the car owner can review and edit if needed before confirming. User edits do not bypass BR-INV-01: the Portal still decides whether the invoice is valid. | d2, s_ocr, s4, a_edit |
| BR-INV-01 | Invoice Authenticity Cross-Check Rule: After confirmation, invoice data is sent via API to the Electronic Invoice Portal for verification. An invoice is considered valid only if the tax code exists, details match, and the invoice status is not cancelled or replaced on the tax system. | d3 |

## 3. Branches and acceptance tests

| Branch | Given | When | Then |
| :---- | :---- | :---- | :---- |
| d1 [no] | Vehicle maintenance screen is open | Car owner uploads an unsupported file format (.exe, .txt) or oversized file | System rejects the file immediately and displays an error message |
| d1 [yes] | Vehicle maintenance screen is open | Car owner uploads a valid PDF, XML, or image file under size limit | System accepts and stores the uploaded file, then runs OCR/parsing |
| d2 [no] | File is stored successfully | OCR processing or XML parsing fails due to blurred scan or corrupt structure | System rejects the unreadable file and displays an OCR error message |
| d2 [yes] | File is stored successfully | OCR or XML parsing succeeds | System displays extracted data on the edit/confirmation screen for the car owner to review, edit if needed, and confirm |
| d3 [yes] | Car owner confirms invoice data and API request is sent | Electronic Invoice Portal confirms invoice authenticity and returns valid status | System merges the validated external invoice into the vehicle maintenance history and displays the updated timeline |
| d3 [no] | Car owner confirms invoice data and API request is sent | Electronic Invoice Portal returns invalid, fake, or unmatched status | System rejects the invoice, prevents merging, and displays an error message |

## 4. Object states used

| Object | States in this diagram | Full state list (all diagrams) |
| :---- | :---- | :---- |
| Invoice File | Uploaded -> Stored -> Parsed -> Verified / Rejected | Uploaded, Stored, Parsed, Verified, Archived, Rejected |
| Maintenance Timeline | Consolidated -> Pending Update -> Updated | Initial, Pending Update, Updated, Synced |

## 5. Screens touched

| Actor action | Screen ID |
| :---- | :---- |
| Open maintenance history and select upload invoice (a1) | S-F01-02 -> S-F01-04 (Upload invoice modal) |
| Upload invoice file PDF, XML, or image (a2) | S-F01-04 (Upload invoice modal) |
| Review and edit extracted invoice info if needed, then confirm (a_edit) | S-F01-04 (Invoice review & confirmation view) |
| See updated maintenance timeline or error message (a3) | S-F01-04 / S-F01-02 |
## 

## **6\. Open questions**

| \# | Question | Blocking? | Default applied |
| :---- | :---- | :---- | :---- |
| 1 | Maximum allowable upload file size for images and PDFs? | No | Set limit to 10 MB per invoice file |

## 

## **7\. Verification log**

| Check | Result | By | Date |
| :---- | :---- | :---- | :---- |
| Renders on mermaid.live | Pass | Đinh Thu Thảo | 2026-10-26 |
| Every decision has guards on all outgoing flows, no overlap, no gap | Pass | Đinh Thu Thảo | 2026-10-26 |
| No action has two incoming flows (merge used instead) | Pass | Đinh Thu Thảo | 2026-10-26 |
| Every fork has a matching join | Pass | Đinh Thu Thảo | 2026-10-26 |
| Every system action cites an FR | Pass | Đinh Thu Thảo | 2026-10-26 |
| Every FR of this use case appears in the diagram | 5 of 5 | Đinh Thu Thảo | 2026-10-26 |
| Lane names match the Context Diagram and the use case actors; no Web App, API or database lanes | Pass | Đinh Thu Thảo | 2026-10-26 |
| Related sequence diagram uses the same names and FR IDs; its alt fragments appear here as decisions | Pass | Đinh Thu Thảo | 2026-10-26 |
| AI critic prompt run, findings resolved or logged as open questions | Pass | Đinh Thu Thảo | 2026-10-26 |


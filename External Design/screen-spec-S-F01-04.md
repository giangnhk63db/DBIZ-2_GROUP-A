# Screen Spec: S-F01-04 Maintenance History

<!--
DBIZ2 Lesson 8 template. One file per screen: docs/external-design/screens/screen-spec-<Screen ID>.md
The same template is used in DBIZ3 (Session 4), so what you write now is what the coding agent
reads next semester. The mockup image stays an image; everything around it is text.
-->

| Field | Value |
|---|---|
| Screen ID | `S-F01-04` |
| Screen name | Maintenance History |
| Actor | Car Owner |
| Priority | Must |
| Feature | `F-01 Vehicle Identification & Status` |
| Activity diagrams | `AD-UC-1.05` |
| Mockup image |  |
| Status | Draft |

## 1. Purpose

**Shown when:** the car owner taps the "Maintenance/repair" card in S-F01-02 to see the full maintenance and repair timeline of the vehicle.

**The user leaves this screen when:** the car owner taps the back button to return to S-F01-02. The Upload invoice modal opens on top of this screen and closes back into it, so it is not an exit.

## 2. Mockup

![S-F01-04](img/S-F01-04.png)

Upload invoice modal (bottom sheet opened from this screen):

![S-F01-04 Upload invoice modal](img/S-F01-04-upload-modal.png)

<!-- The image is the visual contract: spacing, grouping, hierarchy. The tables below are the
     behavioural contract. Crop one screen per file. Do not paste a sheet of twelve screens. -->

## 3. Element inventory

<!-- Field names in "Content / data source" are proposed names; align them with the API contract.
     Data groups come from W0 Table 4: "Carpla Service records" (EIF), "Uploaded maintenance invoice" (ILF),
     "Electronic invoice portal data" (EIF). -->

### 3.1 Screen body

| # | Element | Type | Content / data source | Required | Validation |
|---|---|---|---|---|---|
| 1 | AppHeader | Header | Static title "Bảo dưỡng / Sửa chữa" | Yes | none |
| 2 | BackButton | Button | Chevron icon, static | Yes | none |
| 3 | MaintenanceTimeline | List | Records of the vehicle sorted by date, newest first. Carpla Service records (FR-01-04-BE) plus verified external invoices (FR-01-07-BE) | Yes | none (read-only) |
| 4 | RecordDate | Text | `serviceDate`, format dd/mm/yyyy | Yes | none |
| 5 | RecordTitle | Text | `serviceTitle` (for example "Bảo dưỡng định kỳ 20.000 km") | Yes | none |
| 6 | RecordDetail | Text | `serviceDetail` (work done, or cost) | No | none |
| 7 | SourceBadge | Badge | Static text by source: "Carpla Service" or "Hóa đơn ngoài · Đã xác thực" | Yes | none |
| 8 | NewRecordBadge | Badge | Static text "Mới"; shown only on the record just added from an upload | No | none |
| 9 | EmptyHistoryPrompt | Image + Text | Icon, title "Chưa có lịch sử bảo dưỡng", hint "Tải hóa đơn ngoài để bổ sung lịch sử." Shown only in the Empty state | Yes (Empty only) | none |
| 10 | SkeletonList | List | Grey placeholder cards, no data. Shown only in the Loading state | Yes (Loading only) | none |
| 11 | UploadInvoiceButton | Button | Static text "Tải hóa đơn ngoài", fixed at the bottom of the screen | Yes | none |
| 12 | ErrorPopup | Modal | Title "Không tải được lịch sử", message "Không kết nối được Carpla Service. Vui lòng thử lại.", buttons CloseButton "Đóng" and RetryButton "Thử lại" (Error notification BE) | Yes (Error only) | none |
| 13 | SuccessToast | Toast | Static text "Đã thêm hóa đơn vào lịch sử" | Yes (Success only) | none |

### 3.2 Upload invoice modal (`UploadInvoiceModal`)

| # | Element | Type | Content / data source | Required | Validation |
|---|---|---|---|---|---|
| M1 | UploadInvoiceModal | Modal (bottom sheet) | Static title "Tải hóa đơn ngoài" | Yes | none |
| M2 | ModalCloseButton | Button | "✕" icon | Yes | none |
| M3 | FileDropzone | Input (file) | Static text "Chọn tệp hoặc chụp ảnh hóa đơn"; opens the file picker or camera | Yes | BR-FILE-01: PDF, XML, JPG or PNG, size at or below the system limit (10 MB in the mockup) |
| M4 | FormatHint | Text | Static text "Hỗ trợ PDF, XML, JPG, PNG · tối đa 10 MB" | Yes | none |
| M5 | SelectedFileRow | Text | `fileName`, `fileSize`, `fileType` of the selected file, with a file-type icon | Yes (when a file is selected) | none |
| M6 | RemoveFileButton | Button | "✕" icon inside SelectedFileRow | Yes (when a file is selected) | none |
| M7 | UploadProgress | Progress | Progress bar and text "Đang tải lên và đọc hóa đơn… {n}%" | Yes (Loading only) | none |
| M8 | UploadErrorMessage | Text | Red text "Tệp không hợp lệ: chỉ nhận PDF, XML, JPG, PNG, tối đa 10 MB." | Yes (Error only) | none |
| M9 | UploadSubmitButton | Button | Static text "Tải lên"; label "Đang xử lý…" while Loading | Yes | none |
| M10 | ChooseAnotherFileButton | Button | Static text "Chọn tệp khác". Shown only in the Error state | Yes (Error only) | none |
| M11 | InvoiceResultIcon | Image | Green check icon | Yes (Success only) | none |
| M12 | InvoiceResultTitle | Text | Static text "Hóa đơn đã được xác thực" | Yes (Success only) | none |
| M13 | InvoiceResultMessage | Text | Static text "Lịch sử bảo dưỡng đã được cập nhật." | Yes (Success only) | none |
| M14 | InvoiceSummaryCard | Text | Three rows: "Garage" (`sellerName`), "Ngày" (`invoiceDate`), "Số tiền" (`totalAmount`, format 1.850.000 đ) | Yes (Success only) | none |
| M15 | ViewHistoryButton | Button | Static text "Xem lịch sử" | Yes (Success only) | none |

## 4. States

<!-- The mockup usually shows only the happy state. The other four are where products break. -->

### 4.1 Screen states

| State | What the user sees | Trigger |
|---|---|---|
| Default | Timeline of record cards (date, title, detail, SourceBadge) and the UploadInvoiceButton | Carpla Service returns at least one record, or at least one verified external invoice exists |
| Empty (no data) | EmptyHistoryPrompt in place of the list; UploadInvoiceButton stays active | FR-01-04: no official record is found and no verified external invoice exists |
| Loading | SkeletonList; UploadInvoiceButton disabled | Screen opens, or RetryButton is tapped, while the request to Carpla Service is running |
| Error | ErrorPopup over a dimmed screen with skeleton cards behind it | The request to Carpla Service fails or times out |
| Success / confirmation | New record at the top of the timeline with a green border and NewRecordBadge, plus SuccessToast | The owner taps ViewHistoryButton after an invoice was verified and merged (FR-01-07-BE) |

### 4.2 Upload invoice modal states

| State | What the user sees | Trigger |
|---|---|---|
| Default | SelectedFileRow with the file name, size and type; FormatHint; UploadSubmitButton "Tải lên" enabled | A valid file was picked (the modal opens in Empty; Default follows file selection) |
| Empty (no data) | FileDropzone with dashed border; UploadSubmitButton "Tải lên" disabled | The modal opens, or RemoveFileButton was tapped |
| Loading | SelectedFileRow with UploadProgress; UploadSubmitButton "Đang xử lý…" disabled | UploadSubmitButton tapped: store file, extract data (OCR or parsing), cross-check with the Electronic Invoice Portal |
| Error | SelectedFileRow with red border, UploadErrorMessage, ChooseAnotherFileButton | The file fails BR-FILE-01 (format or size). Other failures: see section 9, questions 2 and 5 |
| Success / confirmation | InvoiceResultIcon, InvoiceResultTitle, InvoiceResultMessage, InvoiceSummaryCard, ViewHistoryButton | The portal confirms the invoice is valid and it is merged into the timeline |

## 5. Interactions and navigation

| # | Element | User action | System response | Goes to screen |
|---|---|---|---|---|
| 1 | BackButton | tap | Leaves the screen | `S-F01-02` |
| 2 | UploadInvoiceButton | tap | Opens UploadInvoiceModal in the Empty state (AD-UC-1.05 a1) | stays |
| 3 | FileDropzone | tap | Opens the file picker or camera (AD-UC-1.05 a2) | stays |
| 4 | FileDropzone | select a file | Shows SelectedFileRow; enables UploadSubmitButton | stays |
| 5 | RemoveFileButton | tap | Clears the selection; modal returns to Empty | stays |
| 6 | UploadSubmitButton | tap | Checks format and size (BR-FILE-01). Valid: stores the file (FR-01-05-BE), extracts data (FR-01-06-BE), verifies with the portal (FR-01-07-INT) and shows Loading. Invalid: rejects at once and shows modal Error | stays |
| 7 | ChooseAnotherFileButton | tap | Opens the file picker again; clears the error | stays |
| 8 | ViewHistoryButton | tap | Closes the modal; the timeline shows the new record (AD-UC-1.05 a3) | stays |
| 9 | ModalCloseButton | tap | Closes UploadInvoiceModal without saving (while processing: see section 9, question 6) | stays |
| 10 | RetryButton (ErrorPopup) | tap | Closes the popup and requests the history again (Loading) | stays |
| 11 | CloseButton (ErrorPopup) | tap | Closes the popup | stays |

## 6. Screen-level rules

| Rule ID | Rule | Source |
|---|---|---|
| SR-001 | The screen is read-only for Carpla Service records. The only way to add a record is the invoice upload. | W0 Table 4 (EIF); FR-01-04 |
| SR-002 | Records from Carpla Service and verified external invoices appear in one timeline, newest first, each with a SourceBadge. | FR-01-04, FR-01-07; mockup |
| SR-003 | EmptyHistoryPrompt appears only when there is no official record and no verified external invoice. | FR-01-04 |
| SR-004 | Only PDF, XML and image files (JPG, PNG) within the size limit are accepted. An invalid file is rejected immediately and nothing is stored. | FR-01-05; BR-FILE-01 |
| SR-005 | UploadSubmitButton is disabled until a file is selected, and while the upload is processing. | AD-UC-1.05 d1; mockup |
| SR-006 | An invoice rejected by the Electronic Invoice Portal (tax code not found, details mismatch, cancelled or replaced) is never merged into the timeline. | FR-01-07; BR-INV-01 |
| SR-007 | If parsing fails, the file is flagged for manual verification; manual correction does not bypass the portal check. | FR-01-06; BR-OCR-01 |
| SR-008 | SourceBadge always carries text. The green "Đã xác thực" wording must not rely on colour alone. | Accessibility |

## 7. Linked requirements

| FR ID | What this screen does for it |
|---|---|
| FR-01-04-FE | Shows the maintenance timeline, the Empty prompt and the error popup |
| FR-01-04-BE | Data source of the timeline: official records fetched from Carpla Service |
| FR-01-05-FE | Shows the file uploader (FileDropzone, UploadProgress) and the format and size errors |
| FR-01-05-BE | Stores the uploaded file after BR-FILE-01 passes |
| FR-01-06-BE | Extracts tax code, invoice number and total amount; flags the file when parsing fails |
| FR-01-07-INT | Cross-checks the invoice with the Electronic Invoice Portal during Loading |
| FR-01-07-BE | Merges the valid invoice into the timeline (Success) or rejects it |

## 8. Responsive and accessibility notes

- Smallest supported width: 360px
- What collapses or stacks on a narrow screen: record cards are full width and stack vertically; UploadInvoiceButton stays fixed at the bottom; the bottom sheet keeps full width and scrolls inside if content is taller than the viewport.
- Information never carried by colour alone: the error state has red text, a red border and the message wording; the verified source has the badge text "Đã xác thực"; the new record has the "Mới" badge, not only the green border.
- Text contrast and minimum size: body text is 13 px; hints and badges are 11 px, which is small and should be raised to 12 px where space allows. Blue `#1E3A8A` on white and red `#B42318` on white pass 4.5:1. The green badge text on light green is about 3.7:1 and fails for small text; use a darker green.

## 9. Open questions

| # | Question | Blocking? | Status |
|---|---|---|---|
| 1 | [NEEDS CLARIFICATION: the invoice summary shows Garage name and invoice date, but FR-01-06 only lists seller tax code, invoice number and total amount. Which fields should the summary show?] | No | Open |
| 2 | [NEEDS CLARIFICATION: the mockup covers only the invalid format or size error. There is no state or wording for a portal rejection (BR-INV-01) and none for "flagged for manual verification" (BR-OCR-01 requires the owner to confirm or correct the mandatory fields by hand).] | Yes | Open |
| 3 | [NEEDS CLARIFICATION: BR-FILE-01 gives "≤ 10 MB" only as an example. Confirm the final size limit, and whether "image" means only JPG and PNG.] | No | Open |
| 4 | [NEEDS CLARIFICATION: the Screen List says the Success notification is a popup; the mockup uses an in-sheet summary plus a toast. Which one is final?] | No | Open |
| 5 | [NEEDS CLARIFICATION: the Screen List says the BE error appears "after the maximum retries", but BR-RET-02 defines retries only for charging stations. What is the retry rule for Carpla Service?] | No | Open |
| 6 | [NEEDS CLARIFICATION: what happens when the owner closes the modal while the invoice is processing: cancel, or continue in the background?] | No | Open |
| 7 | [NEEDS CLARIFICATION: the screen is Must, but FR-01-05 is Should and FR-01-06 and FR-01-07 are Could. If the upload is cut, should the screen ship read-only without UploadInvoiceButton?] | No | Open |
| 8 | [NEEDS CLARIFICATION: the Screen List and Screen Transition say S-F01-02 opens this screen from "View all". The S-F01-02 design now uses a chevron on the "Bảo dưỡng/sửa chữa" card. Update both documents.] | No | Open |

---

## Completion checklist

- [ ] The mockup is a separate cropped image file, named with the Screen ID. (Export `S-F01-04.png` and `S-F01-04-upload-modal.png` from Figma.)
- [x] Every visible element in the mockup appears in the element inventory.
- [x] Every input element has a validation rule or an explicit "none".
- [x] All five states are filled in, or marked "not applicable" with a reason.
- [x] Every navigation target is a Screen ID that exists in the Screen List.
- [x] Every element that displays data names the field it displays.
- [x] Every linked FR exists in the Function List, and every action of this screen in an activity
      diagram is covered by an interaction row.

---

Template source: DBIZ3 Session 4 Screen Spec template (VJCBI College - FTU, 2026), adopted
unchanged in structure for DBIZ2 Lesson 8, with Feature and Activity diagram fields added.

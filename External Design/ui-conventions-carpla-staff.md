# UI Conventions: TASCO Verified Vehicle Report System (Carpla Staff, web)

Scope: the Carpla Staff screen S-F03-02 (Sale Request Review & Quote), which runs in a desktop web browser inside Carpla CRM. The Car Owner screens (mobile) have their own file: ui-conventions-car-owner.md.

## 1. Language and tone

- Interface language: Vietnamese only (no English version). All text the user sees on screen is written in Vietnamese. Proper names stay as they are: VIN, VETC, Carpla Service, SoH.
- Address the user as: "bạn".
- Tone: professional. Polite, neutral and concise; no slang, no emoji, no exclamation marks, no softening particles such as "nhé".
- Button labels are verbs that say what happens: verb + object, never a generic "OK" or "Xác nhận".
  - Buttons: "Gửi báo giá", "Quay lại", "Thử lại", "Đóng".

## 2. Formats

| Item | Format | Example |
|---|---|---|
| Date | dd/MM/yyyy | 16/10/2026 |
| Time | 24-hour, HH:mm | 09:00 |
| Currency | Dot as thousands separator, no decimals; one space between the number and the suffix "₫" | 850.000.000 ₫ |
| Price range (quote) | Low – high; one space before and after the en dash; unit written once at the end, with one space before it | 820.000.000 – 860.000.000 ₫ |
| Mileage | Dot as thousands separator; one space between the number and "km" | 45.230 km |
| Percent (SoH, confidence level) | Whole number, no space, % sign | 92% |
| Phone | Displayed in groups 4-3-3 | 0901 234 567 |
| VIN | 17 uppercase letters and digits, no I, O, Q; displayed without spaces | RLHJC6101PY000123 |
| Names | Family name + middle name + given name, first letter of each word capitalised | Nguyễn Văn An |

## 3. Components (use these names in every Screen Spec)

| Component | When to use | Notes |
|---|---|---|
| Page header | Top of the screen | Link "Quay lại" on the left (returns to Carpla CRM), screen title "Xem xét yêu cầu bán xe và báo giá" on the left below it. Screen title is in Vietnamese, sentence case: capitalise only the first letter of the first word, plus abbreviations and proper names (VIN, SoH, VETC, Carpla); all other words lowercase |
| Info panel | Read-only report: owner-declared information (current mileage, desired price, phone number) and the verified vehicle report (identification, mileage, battery health, legal status, market price) | Label on the left, value on the right; a value that is not verified shows "Chưa xác minh" with a Status badge (icon + text) |
| Data table | Maintenance and repair history, including external invoices uploaded by the car owner | Columns: Ngày, Nội dung, Nguồn (Carpla Service / Hóa đơn bên ngoài); newest on top; the invoice file opens in a new tab through a link in the row |
| Status badge | Verification status of a value, legal status (inspection, unpaid traffic fines, mortgage) | Always icon + text, never colour alone |
| Text input | Quote range: "Giá thấp nhất" and "Giá cao nhất" | Label above, FE error message below (inline); the inline error is only for errors caught by FE validation, BE errors use a popup (section 4); digits auto-formatted with dots while typing; suffix "₫" shown inside the field |
| Quote form | Staff enters the suggested quote range | Two Text inputs side by side: "Giá thấp nhất" and "Giá cao nhất" |
| Primary button | Send the quote | "Gửi báo giá"; one per screen; spans the width of its container, filled with the brand colour; placed at the bottom right of the Quote form |
| Secondary button | Leave without sending; alternative actions | "Quay lại"; outline style: border and text only, transparent background (no fill) |
| Banner | Non-blocking information: a value in the report is not verified | Sits inside the screen, never covers content; info style with icon + text; no buttons |
| Modal | System error notification; success notification | Never for navigation; always has a close button |

## 4. Messages

All message text shown to the user is in Vietnamese.

| Situation | Pattern | Example |
|---|---|---|
| Validation error (FE) | **Inline**, right below the field. What is wrong + how to fix, ending with "Vui lòng ...". No request is sent to the server. | "Vui lòng nhập giá thấp nhất." / "Giá thấp nhất không được lớn hơn giá cao nhất. Vui lòng nhập lại." |
| System error (BE) | **Popup** after the maximum retries. What happened + what the user can do now. Buttons "Thử lại" and "Đóng". | "Hệ thống chưa gửi được báo giá. Vui lòng bấm Thử lại, hoặc đóng và quay lại sau." |
| Empty state | **In the Data table area**, in the place where the rows would be. Why it is empty + what it means. | "Chủ xe chưa tải hóa đơn bảo dưỡng bên ngoài." |
| Success | **Popup** with a "Đóng" button. What was done. | "Đã gửi báo giá sơ bộ cho chủ xe." |
| Unavailable data (not an error) | **Banner**, not a popup. The screen opens normally; the banner sits at the top of the Info panel, and the affected value shows "Chưa xác minh". The banner has no button and stays until the data exists. Reason for no popup: nothing failed and the user has nothing to retry. | "Số km chưa được xác minh vì không có bản ghi qua trạm thu phí." |

## 5. Accessibility minimums

- Body text contrast at least 4.5:1 (WCAG 2.2, success criterion 1.4.3).
- Information is never carried by colour alone (WCAG 2.2, 1.4.1): verification status, legal status and errors always carry an icon or text as well.
- Clickable targets at least 24 by 24 CSS pixels (WCAG 2.2, 2.5.8).
- Smallest supported screen width: 1280px; content may scroll vertically only.
- Everything works with the keyboard alone (Tab order follows reading order, Enter sends the focused form) and the focused element always has a visible outline (WCAG 2.2, 2.1.1 and 2.4.7).

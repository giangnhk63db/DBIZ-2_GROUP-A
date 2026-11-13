# UI Conventions: TASCO Verified Vehicle Report System (Car Owner, mobile)

Scope: the Car Owner screens S-F00-01 to S-F03-03, designed for mobile phones. The Carpla Staff screen (web, Carpla CRM) has its own file: ui-conventions-carpla-staff.md.

## 1. Language and tone

- Interface language: Vietnamese only (no English version). All text the user sees on screen is written in Vietnamese. Proper names stay as they are: VIN, VETC, Carpla Service, SoH.
- Address the user as: "bạn".
- Tone: professional. Polite, neutral and concise; no slang, no emoji, no exclamation marks, no softening particles such as "nhé".
- Button labels are verbs that say what happens: verb + object, never a generic "OK" or "Xác nhận".
  - Buttons: "Tạo báo cáo xe", "Tải hóa đơn lên", "Gửi yêu cầu bán xe", "Làm mới", "Thử lại", "Đóng".
  - Section links: "Xem chi tiết", "Xem tất cả", "Xem xu hướng".

## 2. Formats

| Item | Format | Example |
|---|---|---|
| Date | dd/MM/yyyy | 16/10/2026 |
| Time | 24-hour, HH:mm | 09:00 |
| Currency | Dot as thousands separator, no decimals; one space between the number and the suffix "₫" | 850.000.000 ₫ |
| Price range (preliminary quote) | Low – high; one space before and after the en dash; unit written once at the end, with one space before it | 820.000.000 – 860.000.000 ₫ |
| Mileage | Dot as thousands separator; one space between the number and "km" | 45.230 km |
| Percent (SoH, confidence level) | Whole number, no space, % sign | 92% |
| Phone | Entered as 10 digits starting with 0; displayed in groups 4-3-3 | 0901 234 567 |
| VIN | 17 uppercase letters and digits, no I, O, Q; displayed without spaces | RLHJC6101PY000123 |
| Names | Family name + middle name + given name, first letter of each word capitalised | Nguyễn Văn An |
| Chart axis (12-week trend) | Week label on the x-axis; price in millions of dong on the y-axis | Tuần 01 / 850 triệu |

## 3. Components (use these names in every Screen Spec)

| Component | When to use | Notes |
|---|---|---|
| Top bar | Top of every screen except Home | Back arrow on the left, screen name in the centre. Screen name is in Vietnamese, sentence case: capitalise only the first letter of the first word, plus abbreviations and proper names (VIN, SoH, VETC, Carpla); all other words lowercase. One line; truncated with "…" if too long. Examples: "Tạo báo cáo xe", "Hồ sơ xe", "Lịch sử bảo dưỡng", "Sức khỏe pin (SoH)" |
| Primary button | The one main action of a screen | One per screen; spans the full width of the screen (edge to edge inside the side margins), filled with the brand colour; "Gửi yêu cầu bán xe" is fixed at the bottom of S-F01-02 |
| Secondary button | Alternative actions (for example "Đóng", "Hủy") | Outline style: border and text only, transparent background (no fill) |
| Text input | VIN, current mileage, desired price, phone number | Label above, FE error message below (inline); the inline error is only for errors caught by FE validation, BE errors use a popup (section 4); matching keyboard (numeric for mileage, price, phone) |
| Section card | Each block of the Vehicle Profile (identification, mileage, maintenance, battery, legal status, market data) | Block title plus a link on the right: "Xem chi tiết" / "Xem tất cả" / "Xem xu hướng" |
| List item | One row of information (a vehicle on Home, a vehicle spec) | The whole row is tappable if it opens another screen |
| Timeline item | Toll history, maintenance and repair history | Newest on top; date dd/MM/yyyy at the start of the row |
| Status badge | Legal status (inspection, unpaid traffic fines, mortgage), sale request status | Always icon + text, never colour alone |
| Line chart | 12-week price trend | Axis labels shown; latest week's value also written as text |
| Banner | Non-blocking information: mileage unverified, price being calculated, waiting for Carpla | Sits inside the screen, never covers content; info style with icon + text; no buttons |
| Modal | Upload invoice; system error notification; success notification | Never for navigation; always has a close button |

## 4. Messages

All message text shown to the user is in Vietnamese.

| Situation | Pattern | Example |
|---|---|---|
| Validation error (FE) | **Inline**, right below the field. What is wrong + how to fix, ending with "Vui lòng ...". No request is sent to the server. | "VIN cần đủ 17 ký tự gồm chữ và số (không dùng I, O, Q). Vui lòng kiểm tra lại." / "Số điện thoại cần đủ 10 chữ số, bắt đầu bằng 0. Vui lòng nhập lại." |
| System error (BE) | **Popup** after the maximum retries. What happened + what the user can do now. Buttons "Thử lại" and "Đóng". | "Hệ thống chưa lấy được dữ liệu từ VETC. Vui lòng bấm Thử lại, hoặc đóng và quay lại sau." |
| Empty state | **On the screen**, in the place where the list would be. Why it is empty + the next action. | Home: "Bạn chưa có xe nào. Bấm 'Tạo báo cáo xe' để thêm xe bằng số VIN." / Maintenance: "Chưa có lịch sử bảo dưỡng tại Carpla Service. Bạn có thể tải hóa đơn bảo dưỡng ở nơi khác lên." |
| Success | **Popup** with a "Đóng" button. What was done. | "Đã gửi yêu cầu bán xe. Carpla sẽ báo giá sơ bộ cho bạn." / "Đã tải hóa đơn lên. Lịch sử bảo dưỡng đã được cập nhật." |
| Unavailable data (not an error) | **Banner**, not a popup. The screen opens normally; the banner sits at the top of the affected section or screen content. The missing value shows "—" in place of the data. The banner has no button and stays until the data exists. Reason for no popup: nothing failed and the user has nothing to retry. | S-F01-03: "Chưa xác minh được số km vì không có bản ghi qua trạm thu phí." / S-F01-06: "Chưa đủ dữ liệu sạc để tính sức khỏe pin." |
| Waiting / loading | **Banner**, not a popup. Shown in the area that will hold the result, with a loading icon; the result area shows "—" until ready. The banner disappears once the data is available. | S-F02-01: "Đang tính giá thị trường cho xe của bạn. Vui lòng quay lại sau." / S-F03-03: "Yêu cầu đã gửi. Đang chờ Carpla báo giá sơ bộ." (together with the Status badge "Chờ Carpla báo giá") |

## 5. Accessibility minimums

- Body text contrast at least 4.5:1 (WCAG 2.2, success criterion 1.4.3).
- Information is never carried by colour alone (WCAG 2.2, 1.4.1): legal status, errors and chart lines always carry an icon or text as well.
- Tap targets at least 24 by 24 CSS pixels (WCAG 2.2, 2.5.8); primary buttons and the "Xem chi tiết" / "Xem tất cả" links should be at least 44 px high for finger use.
- Smallest supported screen width: 360px, single column, vertical scroll only; no horizontal scroll.
- Body text at least 14 px; the layout must not break when the user enlarges the system font size.

# **Screen Spec: S-F03-01 Submit sale request** 

# **1\. Purpose**

| Field | Value |
| ----- | ----- |
| Screen ID | S-F03-01 |
| Screen name | Sale Request (Vietnamese UI title in Top bar: "Yêu cầu bán xe") |
| Actor | Car Owner |
| Priority | Must |
| Feature | F-03 Sale Request |
| Activity diagrams | AD-UC-3.01 (submit sale request), AD-UC-3.03, AD-UC-3.04, AD-UC-3.05 (legal re-check: inspection, unpaid fines, mortgage), AD-UC-3.06 (package and forward to Carpla CRM). UC-3.02 has no activity diagram yet; its behaviour is the EARS criterion of FR-03-02 |
| Linked requirements | FR-03-01 Submit sale request (Must), FR-03-02 Re-check legal status before forwarding (Should), see section 7 |
| Mockup images | img/S-F03-01-default.png, img/S-F03-01-default-numbered.png, img/S-F03-01-error-validate-fe-numbered.png, five state images (section 2\) |
| Frame width | 360px |
| Figma frame | [Verified Vehicle Report \- S-F03-01 (5 states)](https://www.figma.com/design/C0hM92cku1UhbITMOXxvgD/Verified-Vehicle-Report---S-F03-01--S-F03-03--5-states-?node-id=28-118&t=aMNUfKIg1lxemCVq-0), node-id 28-118 |
| Status | Reviewed |

**Shown when:** the car owner taps the fixed bottom button "Submit sale request" of S-F01-02 to sell the car.  The screen shows the verified vehicle data and the legal status automatically updated, and collects the current mileage, the desired price and the phone number to create the sale request.

**The user leaves this screen when:** the car owner goes back to S-F01-02 with the Back arrow of the Top bar, or closes the success popup after the sale request has been sent.

## **2\. Mockup**

Each number is the \# of the element inventory in section 3\. Elements 23 (banner) appear only in the loading state; elements 24 to 27 (popup) appear only in the Backend and API error and success states. All colours, fonts and corners come from the design tokens in [ui-conventions-car-owner](https://github.com/giangnhk63db/DBIZ-2_GROUP-A/blob/856166f4d066a07c84b66f96b604380801b82088/External%20Design/ui-conventions-car-owner.md)

| Default screen numbered | Empty screen numbered |
| ----- | ----- |
|  |  |

## 

| Loading screen numbered | Error validate FE screen numbered |
| ----- | ----- |
|  |  |

## 

| Error API screen numbered | Success screen numbered |
| ----- | ----- |
|  |  |

| State | Image |
| ----- | ----- |
| Default | `img/S-F03-01-default.png` |
| Loading | `img/S-F03-01-loading.png` |
| Error (validateFE) | `img/S-F03-01-error-validate-FE.png` |
| Error API | `img/S-F03-01-error-API.png` |
| Success | `img/S-F03-01-success.png` |

## **3\. Element inventory**

The sections "Thông tin xe đã xác minh" and "Tình trạng pháp lý" are read-only. The section "Thông tin bạn cần nhập" has 3 textboxes. The screen has no Upload, Date time or Security element. 

| \# | Element | Category | Type | Content / data source | Required | Validation |
| ----- | ----- | ----- | ----- | ----- | ----- | ----- |
| 1 | Back | Selection | Top bar, back arrow (button) | static | Yes | none |
| 2 | Title | Display | Read-only text (Top bar screen name) | static: "Yêu cầu bán xe" | Yes | none |
| 3 | Vehicle card title | Display | Read-only text (Section card title) | static: "Thông tin xe đã xác minh" | Yes | none |
| 4 | VIN | Display | Label \+ read-only text | label "VIN"; value vehicle.vin(for example "RLHJC6101PY000123") | Yes | none |
| 5 | Vehicle name | Display | Label \+ read-only text | label "Xe";  value vehicle.make, vehicle.model, vehicle.variant (for example "Toyota Vios 1.5G") | Yes | none |
| 6 | Manufacturing year | Display | Label \+ read-only text | label "Năm sản xuất";  value vehicle.manufacturing\_year (for example "2022") | Yes | none |
| 7 | Verified minimum mileage | Display | Label \+ read-only text | label "Số km tối thiểu"; value vehicle.verified\_min\_mileage (FR-01-03); format 45.230 km | Yes | none |
| 8 | Legal card title | Display | Read-only text (Section card title) | static: "Tình trạng pháp lý" | Yes | none |
| 9 | Inspection status | Display | Label \+ Status badge (icon \+ text) | label "Đăng kiểm"; legal\_status.inspection\_status (FR-01-10); The mockup displays one in cases following:   "Còn hạn" with a check icon  “Hết hạn” with a warning icon shows "—" in the loading state | Yes | none |
| 10 | Unpaid fines status | Display | Label \+ Status badge (icon \+ text) | label "Phạt nguội chưa nộp"; legal\_status.fine\_status (FR-01-11);  The mockup displays one in cases following:  "Không có" with a check icon "Có" with a warning icon "—" in the loading state | Yes | none |
| 11 | Mortgage status | Display | Label \+ Status badge (icon \+ text) | label "Thế chấp"; legal\_status.mortgage\_status (FR-01-12);  The mockup displays one in cases following:  "Không có" with a check icon "—" in the loading state "Đang thế chấp" with a warning icon | Yes | none |
| 12 | Input card title | Display | Read-only text (Section card title) | static: "Thông tin bạn cần nhập" | Yes | none |
| 13 | Current mileage label | Display | Read-only text | static: "Số km hiện tại" | Yes | none |
| 14 | Current mileage | Input | Textbox (numeric) | sale\_request.current\_mileage; placeholder "Nhập số km hiện tại"; example value 45.500 km | Yes | Rule: Not empty; digits only.  FE validate current mileage entered according to this above rule: Pass: Enable button “Gửi yêu cầu bán xe” (Element 22), Not Pass/ Not entered: Display notification inline (S-F03-01-Error Validate FE; element 27). Disable button “Gửi yêu cầu bán xe”  |
| 15 | Current mileage hint | Display | Helper text | Hint: "Chỉ nhập chữ số, ví dụ 45.500".  | Yes | none |
| 16 | Desired price label | Display | Read-only text | static: "Giá mong muốn" | Yes | none |
| 17 | Desired price | Input | Textbox (numeric) | sale\_request.desired\_price; placeholder "Nhập giá mong muốn"; example value 850.000.000 ₫ | Yes | Digits only, greater than 0\. FE validate desired price entered according to this above rule: Pass: Enable button “Gửi yêu cầu bán xe” (Element 22), Not Pass/ Not entered: Display notification inline (S-F03-01-Error Validate FE; element 27). Disable button “Gửi yêu cầu bán xe”  |
| 18 | Desired price hint | Display | Helper text | Hint: "Chỉ nhập chữ số, ví dụ 850.000.000". Error text replaces the hint when element 17 is invalid | Yes | none |
| 19 | Phone number label | Display | Read-only text | static: "Số điện thoại" | Yes | none |
| 20 | Phone number | Input | Number input  | sale\_request.phone\_number; placeholder "Nhập số điện thoại"; example value 0901 234 567 | Yes | Digits only, greater than 0, exactly 10 digits, no space between. FE validate phone number entered according to this above rule: Pass: Enable button “Gửi yêu cầu bán xe” (Element 22), Not Pass/ Not entered: Display notification inline (S-F03-01-Error Validate FE; element 27). Disable button “Gửi yêu cầu bán xe”  |
| 21 | Phone number hint | Display | Helper tex | Hint: "10 chữ số, bắt đầu bằng 0".  | Yes | none |
| 22 | Submit | Selection | Primary button | static: "Gửi yêu cầu bán xe";  In the loading state the label is "Đang gửi yêu cầu…"  The button is inactive when having errors validate FE. | Yes | none |
| 23 | Re-check banner | Feedback | Banner (info, icon \+ read-only text) | static: "Đang kiểm tra lại tình trạng pháp lý. Vui lòng chờ trong giây lát." (FR-03-02); sits at the top of the content; visible only in the loading state | No | none |
| 24 | Notification message | Dialogue | Modal (popup), title \+ read-only text | Success: title "Đã gửi yêu cầu", text "Đã gửi yêu cầu bán xe. Tasco sẽ gửi báo giá sơ bộ đến bạn trong thời gian sớm nhất" with check icon. Error: title "Chưa gửi được yêu cầu", text "Hệ thống chưa gửi được yêu cầu bán xe. Vui lòng bấm Thử lại, hoặc đóng và quay lại sau." with warning icon | No | none |
| 25 | Error | Feedback | Inline error text | Error text red: “Vui lòng nhập số km hiện tại” when user did not entered current mileage. Error text red: “Số km chưa hợp lệ” when user entered current mileage in incorrect forma. | No | None |
| 26 | Error | Feedback | Inline error text | Error text red: “Vui lòng nhập giá mong muốn” when user did not entered  desired price. Error text red: “Thông tin chưa hợp lệ” when user entered desired price in incorrect format. | No | None |
| 27 | Error | Feedback | Inline error text | Error text red: “Vui lòng nhập số điện thoại” when user did not entered  phone number. Error text red: “Số điện thoại chưa hợp lệ” when user entered phone number in incorrect format. | No | None |
| 28 | Close popup | Selection | Icon button (X) in the Modal | static | No | none |
| 29 | Retry | Selection | Primary button in the Modal | static: "Thử lại"; error popup only | No | none |
| 30 | Close | Selection | Button in the Modal | static: "Đóng";  primary style in the success popup,  secondary (outlined) style in the error popup | No | none |

**Note**: `vehicle.`, `legal_status.`, `sale_request.` are the fields sourced from tables in the data source. The field names are proposed and will be finalized with the data dictionary.

## **4\. States**

This screen is a form, so there is no Empty state. 

| State | What the user sees | Trigger |
| ----- | ----- | ----- |
| Default | Vehicle card (3 to 7\) and legal card (8 to 11\) filled; input card (12 to 21\) with the three textboxes and their hints; button "Gửi yêu cầu bán xe" (22) active. No banner, no popup, no inline error. | When the car owner taps the fixed bottom button "Gửi yêu cầu bán xe" of S-F01-02 to sell the car. Screen opened and the verified vehicle data and the most recently updated legal status in vehicle profile are loaded. (FR-03-01). |
| Loading | Banner (23) at the top of the content. The legal badges (9, 10, 11\) show "—". The button (22) shows "Đang gửi yêu cầu…" and cannot be tapped again. The banner disappears when the request is sent. | The car owner taps "Gửi yêu cầu bán xe", FE validation passes and re-checks the legal status (FR-03-02).  |
| Error validate FE | Inline error text (25, 26 or 27, red) under each invalid field, replacing its hint. The button (22) is inactive. Nothing is sent to the server. | An input does not pass the rule of element 14, 17 or 20 (empty or wrong format) |
| Error API | Error popup (24 with warning icon, 28, 29, 30\) with title "Chưa gửi được yêu cầu", buttons "Thử lại" and "Đóng" (and X).  | The system could not create or send the sale request after the maximum retries to send to Tasco staff CRM web. |
| Success  | Success popup (24 with check icon, 28, 30\) with title "Đã gửi yêu cầu", the message and button "Đóng" (and X). The content behind it cannot be used. | The sale request is created and forwarded to Carpla staff  (FR-03-03). A data field that has no record or API does not respond is attached as "No data" and does not cause an error (FR-03-02) |

## **5\. Interactions and navigation**

| \# | Element | User action | System response | Goes to screen |
| ----- | ----- | ----- | ----- | ----- |
| 1 | Back (1) | tap | Returns without sending the request | S-F01-02 |
| 2 | Current mileage (14) | type digits | Accepts digits only; validates FE rule; shows inline error (25) if not valid | stays |
| 3 | Desired price (17) | type digits | Accepts digits only; validates FE rule; shows inline error (26) if not valid | stays |
| 4 | Phone number (20) | type digits | Accepts digits only; validates FE rule; shows inline error (27) if not valid | stays |
| 5 | Submit (22) | tap | FE pass: shows the loading state, sends the request, then shows the success popup or the error popup. FE not pass: shows the inline errors and keeps the button inactive | stays (loading, then success or error) |
| 6 | Retry (29) | tap (error popup) | Closes the popup, shows the loading state and sends the request again | stays (loading, then success or error) |
| 7 | Close (30) | tap (error or success popup) | Closes the popup. After an error the entered values are kept. After a success the screen closes. \[see section 9, question 3\] | stays after error; after success: S-F01-02 (to confirm) |
| 8 | Close popup (28) | tap (error or success popup) | Same as Close (30) | same as Close (30) |

## **6\. Screen-level rules**

| Rule ID | Rule | Source |
| :---- | :---- | :---- |
| SR-001 | The vehicle card and the legal card are read-only. Only the three inputs (14, 17, 20\) can be edited. | FR-03-01 |
| SR-002 | The three inputs are validated on FE only. If FE does not pass, nothing is sent to the server and the button (22) is inactive. There is no separate BE validation error on this screen: every failure after the request is sent is shown as the system error popup. | FR-03-01 |
| SR-003 | Mileage and desired price accept digits only. Phone number accepts digits only, exactly 10 digits, no space between. Desired price must be greater than 0\. | FR-03-01 |
| SR-004 | An inline error is red text that only states that the field is empty or not valid (for example “Số km chưa hợp lệ”). It does not explain the expected format; the hint of the field does. Colour is never the only signal: the error is written as text. | ui-conventions §4 |
| SR-005 | Each legal status badge is an icon \+ text (check icon for a good status, warning icon for a risk). Information is never carried by colour alone. | FR-01-10, FR-01-11, FR-01-12 |
| SR-006 | The legal re-check runs when the request is submitted. If a portal has no record or does not respond, that check is attached as "No data" and the request is still forwarded: this is not an error and does not open the error popup. | FR-03-02 |
| SR-007 | Loading (re-check) is shown as a Banner, not a popup. The banner has no button and disappears when the request is sent. | FR-03-02, ui-conventions §4 |
| SR-008 | A system error is shown as a popup only after the maximum retries, with the buttons "Thử lại" and "Đóng". A success popup has one button "Đóng". The popup buttons can trigger an action (retry sending the request) or navigation (closing the success popup leaves the screen). | ui-conventions §3, §4 |
| SR-009 | While a popup is open, the controls behind it cannot be used. | ui-conventions §3 (Modal) |
| SR-010 | Formats: mileage 45.230 km; price 850.000.000 ₫; phone number 10 digits, for example 0901234567. | ui-conventions §2 |
| SR-011 | The latest market price is not shown or sent from this screen. The system packages it for Carpla staff by itself; if no market price exists, the package is forwarded with the price marked as unavailable. | FR-03-03 |

## **7\. Linked requirements**

| FR ID (L2) | What this screen does for it | L3 work items (IDs as written in the Function List) |
| ----- | ----- | ----- |
| FR-03-01 Submit sale request | Shows the form (current mileage, desired price, phone number), validates it, sends the request and shows the processing notification (loading, success, error) | FR-03-01-FE: display the form and the sending notification |
| FR-03-02 Re-check legal status before forwarding | Shows the re-check banner and the legal status badges ("—" while re-checking) | FR-03-02-BE refresh the legal status |
| FR-03-03 Package and forward report and market price | Background, no UI on this screen. Runs after the request is created | FR-03-03-BE: not part of this screen |
| FR-01-03 View Verified Minimum Mileage | Source of the value of element 7 | FR-01-03-FE, FR-01-03-INT  (not part of this screen) |
| FR-01-10, FR-01-11, FR-01-12 (legal checks) | Source of the status values of elements 9, 10, 11 | FR-01-10-INT, FR-01-11-INT, FR-01-12-INT  (not part of this screen) |

## **8\. Responsive and accessibility notes**

* Smallest supported width: 360px, single column, vertical scroll only; no horizontal scroll.  
* On a narrow screen the cards and textboxes take the full card width. The button "Gửi yêu cầu bán xe" is full width.  
* Information is never carried by colour alone: the legal badges have an icon and text; inline errors have text; popups have an icon and text.  
* Text contrast at least 4.5:1; body text at least 14 px; layout must not break when the system font is enlarged.  
* Tap targets at least 44 x 44 px: Back arrow (1), the three textboxes (14, 17, 20), "Gửi yêu cầu bán xe" (22), popup X (28), "Thử lại" (29), "Đóng" (30).  
* The numeric textboxes open the numeric keyboard on a phone.  
* Each textbox has its label (13, 16, 19\) linked to it for the screen reader; the inline error (25, 26, 27\) is announced when it appears.  
* The loading banner and the popups must be readable by a screen reader (loading icon, check icon and warning icon have a text alternative).

  ## **9\. Open questions**

| \# | Question | Blocking? | Status |
| ----- | ----- | ----- | ----- |
| 1 | \[NEEDS CLARIFICATION: FR-03-01 has only the L3 item FR-03-01-FE. The BE work item that creates the sale request is missing from the Function List.\] | No | Open. Default. no new BE item. FR-03-01 has only FR-03-01-FE because opening the form from S-F01-02 needs no BE of its own, and submitting is handled by the chain FR-03-02-BE and FR-03-03-BE. Confirm that storing the `sale_request` (current mileage, desired price, phone number) is part of FR-03-03-BE. |
| 2 | \[NEEDS CLARIFICATION: after the success popup is closed, which screen does the car owner see? The Figma file also contains S-F03-03: is it the next screen?\] | No | Open. Default: return to S-F01-02 |
| 3 | \[NEEDS CLARIFICATION: the rule says "digits only", but the hints and examples use dots (45.500, 850.000.000) and the phone example has spaces (0901 234 567\) while the rule says "no space between". Does the textbox add the dots while typing, and is the phone example meant to be 0901234567?\] | No | Open. Default: the user types digits only, the textbox shows the dots for mileage and price |
| 4 | \[NEEDS CLARIFICATION: when is FE validation triggered (on leaving the field, on typing, or on tap of Submit)? The button is inactive while errors are shown, so empty fields need to be checked before the first tap.\] | No | Open. Default: on leaving the field  |
| 5 | \[NEEDS CLARIFICATION: what do the legal badges (9, 10, 11\) show when a portal returns "No data" (FR-03-02)? The mockup only defines "—" for loading.\] | No | Open |

  ## **Completion checklist**

* \[ \] The mockup is a separate cropped image file, named with the Screen ID, at the frame width. (Export the five frames from Figma, node-id 28-118.)  
* \[ \] The numbered image exists, and its numbers match the \# column of the element inventory.  
* \[ \] Every visible element in the mockup appears in the element inventory. (Check against Figma.)  
* \[ \] Every colour in the mockup is a design token from ui-conventions-car-owner.md.  
* \[x\] Every input element has a validation rule or an explicit "none".  
* \[x\] All five states are filled in (Empty is not applicable for a form).  
* \[ \] Every navigation target is a Screen ID that exists in the Screen List. (Pending section 9, question 3.)  
* \[x\] Every element that displays data names the field it displays.  
* \[x\] Every linked FR exists in the Function List (FR-03-01, FR-03-02, FR-03-03, FR-01-03, FR-01-10, FR-01-11, FR-01-12).  
* \[ \] Every action of this screen in AD-UC-3.01 and AD-UC-3.06 is covered. (Check the action IDs against the diagrams.)


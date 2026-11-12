# UI Conventions: TASCO Verified Vehicle Report System

## 1\. Language and tone

* Interface language: Vietnamese only (no English version). All text the user sees on screen is written in Vietnamese. Proper names stay as they are: VIN, VETC, Carpla Service, SoH.
* Address the user as: "bạn".
* Button labels are verbs that say what happens: verb + object, never a generic "OK" or "Xác nhận".

  * Buttons: "Tạo báo cáo xe", "Tải hóa đơn lên", "Gửi yêu cầu bán xe", "Làm mới", "Thử lại", "Đóng".
  * Section links: "Xem chi tiết", "Xem tất cả", "Xem xu hướng".

## 2\. Formats

|Item|Format|Example|
|-|-|-|
|Date|dd/MM/yyyy|16/10/2026|
|Time|24-hour, HH:mm|09:00|
|Currency|Dot as thousands separator, suffix " ₫", no decimals|850.000.000 ₫|
|Price range (preliminary quote)|Low - high, unit written once at the end|820.000.000 – 860.000.000 ₫|
|Mileage|Dot as thousands separator, suffix " km"|45.230 km|
|Percent (SoH, confidence level)|Whole number, no space, % sign|92%|
|Phone|Entered as 10 digits starting with 0; displayed in groups 4-3-3|0901 234 567|
|VIN|17 uppercase letters and digits, no I, O, Q; displayed without spaces|RLHJC6101PY000123|
|Names|Family name + middle name + given name, first letter of each word capitalised|Nguyễn Văn An|
|Chart axis (12-week trend)|Week label on the x-axis; price in millions of dong on the y-axis|Tuần 01 / 850 triệu|

## 3\. Components (use these names in every Screen Spec)

|Component|When to use|Notes|
|-|-|-|
|Top bar|Top of every screen except Home|Back arrow on the left, screen name in the centre|
|Primary button|The one main action of a screen|One per screen; full width; "Gửi yêu cầu bán xe" is fixed at the bottom of S-F01-02|
|Secondary button|Alternative actions (for example "Đóng", "Hủy")|Outline style, no fill|
|Text input|VIN, current mileage, desired price, phone number|Label above, error message below (inline); matching keyboard (numeric for mileage, price, phone)|
|Section card|Each block of the Vehicle Profile (identification, mileage, maintenance, battery, legal status, market data)|Block title plus a link on the right: "Xem chi tiết" / "Xem tất cả" / "Xem xu hướng"|
|List item|One row of information (a vehicle on Home, a vehicle spec)|The whole row is tappable if it opens another screen|
|Timeline item|Toll history, maintenance and repair history|Newest on top; date dd/MM/yyyy at the start of the row|
|Status badge|Legal status (inspection, unpaid traffic fines, mortgage), sale request status|Always icon + text, never colour alone|
|Line chart|12-week price trend|Axis labels shown; latest week's value also written as text|
|Banner|Non-blocking information: mileage unverified, price being calculated, waiting for Carpla|Sits inside the screen, never covers content|
|Modal|Upload invoice; system error notification; success notification|Never for navigation; always has a close button|

## 4\. Messages

All message text shown to the user is in Vietnamese.

|Situation|Pattern|Example|
|-|-|-|
|Validation error (FE)|Shown inline, right below the field. What is wrong + how to fix. No request is sent to the server.|"VIN cần đủ 17 ký tự gồm chữ và số (không dùng I, O, Q). Bạn kiểm tra lại nhé." / "Số điện thoại cần đủ 10 chữ số, bắt đầu bằng 0. Bạn nhập lại nhé."|
|System error (BE)|Shown as a popup after the maximum retries. What happened + what the user can do now. Buttons "Thử lại" and "Đóng".|"Hệ thống chưa lấy được dữ liệu từ VETC. Bạn bấm Thử lại, hoặc đóng và quay lại sau."|
|Empty state|Why it is empty + the next action.|Home: "Bạn chưa có xe nào. Bấm 'Tạo báo cáo xe' để thêm xe bằng số VIN." / Maintenance: "Chưa có lịch sử bảo dưỡng tại Carpla Service. Bạn có thể tải hóa đơn bảo dưỡng ở nơi khác lên."|
|Success|Shown as a popup with a "Đóng" button. What was done.|"Đã gửi yêu cầu bán xe. Carpla sẽ báo giá sơ bộ cho bạn." / "Đã tải hóa đơn lên. Lịch sử bảo dưỡng đã được cập nhật."|
|Unavailable data (not an error)|Banner: which data is missing + why.|"Chưa xác minh được số km vì không có bản ghi qua trạm thu phí." / "Chưa đủ dữ liệu sạc để tính sức khỏe pin."|
|Waiting / loading|Banner: what the system is doing + what the user should do.|"Đang tính giá thị trường cho xe của bạn. Bạn quay lại sau nhé." / "Yêu cầu đã gửi. Đang chờ Carpla báo giá sơ bộ."|

## 5\. Accessibility minimums

* Body text contrast at least 4.5:1 (WCAG 2.2, success criterion 1.4.3).
* Information is never carried by colour alone (WCAG 2.2, 1.4.1): legal status, errors and chart lines always carry an icon or text as well.
* Tap targets at least 24 by 24 CSS pixels (WCAG 2.2, 2.5.8); primary buttons and the "Xem chi tiết" / "Xem tất cả" links should be at least 44 px high for finger use.
* Smallest supported screen width: 360px, single column, vertical scroll only; no horizontal scroll.
* Body text at least 14 px; the layout must not break when the user enlarges the system font size.

## 6. Design tokens

**Direction:** TASCO Corporate - Modern Mobility (professional, modern, minimal, consistent, accessible). Screens take colours, fonts, spacing, corners and frame width only from this section. Component names come from section 3. All sizes are CSS px.

### 6.1 Colour tokens

#### A. Brand colours

| Token | HEX | Intended usage | Components |
|---|---|---|---|
| color-brand-primary | #253264 | TASCO identity colour; main action and key emphasis | Primary button fill, Top bar title and back arrow, Line chart line, Section card title |
| color-brand-secondary | #3F5099 | Lighter navy for links and secondary emphasis. Links are always underlined (see 6.6 G) | "Xem chi tiết" / "Xem tất cả" / "Xem xu hướng" links, Secondary button outline and text |
| color-brand-tint | #E8EBF5 | Very light navy for soft backgrounds | Banner (information), Status badge (information) |

#### B. Neutral colours

| Token | HEX | Intended usage | Components |
|---|---|---|---|
| color-bg-main | #F6F7F9 | Page background | All screens |
| color-bg-surface | #FFFFFF | Card and overlay background | Section card, List item, Text input, Modal, Top bar |
| color-text-primary | #161B2E | Main text | Body text, headings, input values |
| color-text-secondary | #4A5266 | Supporting text | Labels, hints, dates in Timeline item, chart axis labels |
| color-border-default | #D5D9E0 | Decorative lines only. Never the only way to recognise a control. | Divider between List items, Section card outline |
| color-border-input | #848E97 | Functional border of a control the user must recognise (3:1, WCAG 1.4.11) | Text input border |
| color-border-seat-tile | #848E97 | Seat Tile border, as required by the lecturer | None. TASCO has no Seat Tile in section 3 (Q1); not assigned to any other component |

#### C. Semantic colours

Each status is shown with icon + text (section 5), never colour alone. Icons use the same colour as the text.

| Token | HEX | Intended usage | Components |
|---|---|---|---|
| color-success | #1B7A43 | Success text and icon | Success modal, Status badge (inspection valid, sale request completed) |
| color-success-bg | #E7F4EC | Background behind success text | Status badge |
| color-warning | #8A5300 | Warning text and icon | Status badge (unpaid traffic fine, mortgage), Banner (mileage unverified) |
| color-warning-bg | #FFF3DC | Background behind warning text | Status badge, Banner |
| color-error | #B42318 | Error text and icon | Inline validation error, system error Modal |
| color-error-bg | #FDECEA | Background behind error text | System error Modal |
| color-info | No new HEX: text and icon `color-brand-primary` (#253264) on background `color-brand-tint` (#E8EBF5) | Neutral information | Banner (price being calculated, waiting for Carpla), Status badge (information) |

#### D. Interactive colours

| Token | HEX | Intended usage | Components |
|---|---|---|---|
| color-hover | #1C2650 | Darker navy when a pointer hovers (only when opened with a mouse; touch has no hover) | Primary button |
| color-pressed | #141C3B | Darker navy while pressed | Primary button |
| color-hover-soft | #F3F5FA | Hover and pressed fill for non-filled items | Secondary button, List item |
| color-disabled-bg | #E4E6EB | Fill of a disabled control | Primary button, Secondary button |
| color-disabled-text | #6B7280 | Text of a disabled control (exempt from contrast rules). A disabled Secondary button has no outline: its outline is removed and only the fill and text above remain | Primary button, Secondary button |
| color-focus | #2F5BEA | Focus ring, 2 px thick, drawn with a 2 px offset so it never touches the control's own fill | All keyboard-focusable interactive elements defined in Section 3 |

**Token usage audit.** Every defined token has an assigned purpose. `color-border-seat-tile` is the only reserved token without an assigned TASCO component because Seat Tile is not defined in Section 3. No additional accent colours are used. Rules: no gradients, no shadows, and no unlisted colours. Any new colour must be defined in this section before use.

### 6.2 Typography

| Role | Token | Size / Line height (CSS px) | Weight | Colour | Used for |
|---|---|---|---|---|---|
| Page title | type-title | 24 / 32 | 700 | color-text-primary | Screen name in Top bar |
| Section heading | type-heading | 18 / 26 | 600 | color-text-primary | Section card title |
| Body text | type-body | 16 / 24 | 400 | color-text-primary | Information rows, messages, banners |
| Label | type-label | 14 / 20 | 500 | color-text-secondary | Text input label, Status badge text |
| Button | type-button | 16 / 24 | 600 | per button | Primary button, Secondary button, section links (section links are underlined) |
| Supporting text | type-support | 14 / 20 | 400 | color-text-secondary | Dates, hints, chart axis labels |
| Form input | type-input | 16 / 24 | 400 | color-text-primary | Text input value |

**Font family:** Be Vietnam Pro, with Roboto, "Segoe UI", system-ui, sans-serif as fallback fonts. Be Vietnam Pro is the project's standard UI typeface, selected for its support of Vietnamese diacritics and consistent readability across interface elements. This specification does not identify it as TASCO's official corporate brand font.

The values above are CSS px. At build time they may be written as rem (1 rem = 16 px) so the layout survives a larger system font (section 5); this changes how the value is written, not the design value.

The smallest size is 14 px, in line with section 5.

### 6.3 Spacing and touch target

Base unit 4 px. Only these values are used.

| Token | Value (CSS px) | Usage | Components |
|---|---|---|---|
| space-1 | 4 | Icon to text gap | Status badge, Timeline item |
| space-2 | 8 | Gap inside a component; label to input | Text input, List item, Banner |
| space-3 | 12 | Padding between rows | List item, Timeline item |
| space-4 | 16 | Screen side margin; card inner padding | All screens, Section card, Modal |
| space-5 | 24 | Gap between Section cards | Vehicle Profile blocks |
| space-6 | 32 | Gap above the primary button | Primary button area |

**Touch target.** `size-tap-min` = 44 × 44 CSS px for every element the user can tap. [Proposed] This is stricter than section 5, which sets 24 × 24 as the minimum (WCAG 2.5.8) and 44 px high for primary buttons and the "Xem chi tiết" / "Xem tất cả" links; section 5 is unchanged and this rule is consistent with it. The target is the tappable area, not the visible shape: a small icon keeps its drawn size and gets padding or an invisible hit area up to 44 × 44. Icon drawing size is set in the Screen Spec, not here.

| Component (section 3) | Tappable element | Rule |
|---|---|---|
| Top bar | Back arrow | Hit area 44 × 44 |
| Primary button | Whole button | Height at least 44, full width |
| Secondary button | Whole button | Height at least 44 |
| Section card | Links "Xem chi tiết" / "Xem tất cả" / "Xem xu hướng" | Hit area at least 44 high |
| List item | Whole row, when it opens a screen | Row height at least 44 |
| Text input | Whole field | Height at least 44 |
| Modal | Close button | Hit area 44 × 44 |

Not tappable, no rule: Status badge, Timeline item, Line chart, Banner. Section 3 has no separate "Icon button" component; icon-only controls are covered by the Top bar and Modal rows above. Not yet checked against a Screen Spec (none provided), so no layout was changed.

**Card separation.** Because `color-border-default` on `color-bg-main` is only 1.32:1 and shadows are not allowed, Section cards are separated by the `space-5` (24 px) gap and the white `color-bg-surface` fill, not by the outline alone. To be confirmed against the Screen Spec.

### 6.4 Border radius

| Token | Value (CSS px) | Components |
|---|---|---|
| radius-sm | 4 | Status badge |
| radius-md | 8 | Primary button, Secondary button, Text input, Banner |
| radius-lg | 12 | Section card, Modal |

No Seat Tile radius is defined because Seat Tile is not part of the TASCO component inventory in Section 3.

### 6.5 Frame width

| Item | Value |
|---|---|
| Frame width of screen images for S-F01-02 (all five state images and the numbered image) | 360 px |
| Orientation | Portrait |
| Layout | Single column, vertical scroll only, no horizontal scroll |
| Minimum supported screen width | 360 px |

### 6.6 WCAG 2.2 AA contrast checks

Ratios are computed with the WCAG relative-luminance formula, not estimated by eye. Thresholds: normal text 4.5:1; large text, UI components and graphical objects 3:1. No HEX was changed to obtain a result.

#### A. Text

| Foreground | Background | Ratio | Required | Result |
|---|---|---|---|---|
| #161B2E text primary | #F6F7F9 main background | 15.92:1 | 4.5:1 | PASS |
| #161B2E text primary | #FFFFFF surface | 17.07:1 | 4.5:1 | PASS |
| #161B2E text primary | #F3F5FA hover-soft | 15.65:1 | 4.5:1 | PASS |
| #4A5266 text secondary | #F6F7F9 main background | 7.28:1 | 4.5:1 | PASS |
| #4A5266 text secondary | #FFFFFF surface | 7.81:1 | 4.5:1 | PASS |
| #4A5266 text secondary | #F3F5FA hover-soft | 7.16:1 | 4.5:1 | PASS |
| #FFFFFF button text | #253264 primary button | 12.22:1 | 4.5:1 | PASS |
| #FFFFFF button text | #1C2650 hover | 14.56:1 | 4.5:1 | PASS |
| #FFFFFF button text | #141C3B pressed | 16.68:1 | 4.5:1 | PASS |
| #253264 outline button text | #FFFFFF | 12.22:1 | 4.5:1 | PASS |
| #3F5099 link / secondary button text | #FFFFFF surface | 7.45:1 | 4.5:1 | PASS |
| #3F5099 link / secondary button text | #F6F7F9 main background | 6.95:1 | 4.5:1 | PASS |
| #3F5099 link / secondary button text | #F3F5FA hover-soft | 6.83:1 | 4.5:1 | PASS |
| #3F5099 link | #E8EBF5 tint (link inside an information Banner) | 6.26:1 | 4.5:1 | PASS |
| #253264 info text | #E8EBF5 tint | 10.26:1 | 4.5:1 | PASS |
| #1B7A43 success | #E7F4EC | 4.74:1 | 4.5:1 | PASS |
| #1B7A43 success | #FFFFFF | 5.37:1 | 4.5:1 | PASS |
| #1B7A43 success | #F6F7F9 main background | 5.01:1 | 4.5:1 | PASS |
| #8A5300 warning | #FFF3DC | 5.76:1 | 4.5:1 | PASS |
| #8A5300 warning | #FFFFFF | 6.33:1 | 4.5:1 | PASS |
| #8A5300 warning | #F6F7F9 main background | 5.90:1 | 4.5:1 | PASS |
| #B42318 error | #FDECEA | 5.75:1 | 4.5:1 | PASS |
| #B42318 error | #FFFFFF | 6.57:1 | 4.5:1 | PASS |
| #B42318 error | #F6F7F9 main background | 6.13:1 | 4.5:1 | PASS |

#### B. Decorative borders

Not subject to 1.4.11, because no control depends on them.

| Foreground | Background | Ratio | Required | Result |
|---|---|---|---|---|
| #D5D9E0 divider / card outline | #FFFFFF | 1.42:1 | none | Not applicable (decorative). Content stays readable without the line |
| #D5D9E0 divider / card outline | #F6F7F9 main background | 1.32:1 | none | Not applicable (decorative). Cards stay separated by the `space-5` gap and the white fill (see 6.3) |

#### C. Interactive control borders and graphical objects (WCAG 1.4.11, 3:1)

| Foreground | Background | Ratio | Required | Result |
|---|---|---|---|---|
| #848E97 input border | #FFFFFF surface | 3.33:1 | 3:1 | PASS |
| #848E97 input border | #F6F7F9 main background | 3.11:1 | 3:1 | PASS |
| #848E97 input border | #F3F5FA hover-soft | 3.06:1 | 3:1 | PASS |
| #848E97 input border | #E8EBF5 brand tint | 2.80:1 | 3:1 | FAIL. Rule: a Text input must not sit directly on color-brand-tint (for example inside an information Banner) |
| #3F5099 secondary button outline | #FFFFFF | 7.45:1 | 3:1 | PASS |
| #3F5099 secondary button outline | #F6F7F9 | 6.95:1 | 3:1 | PASS |
| #3F5099 secondary button outline | #F3F5FA hover-soft (hover and pressed) | 6.83:1 | 3:1 | PASS |
| #253264 chart line | #FFFFFF | 12.22:1 | 3:1 | PASS |

#### D. Focus indicator

The ring is 2 px thick with a 2 px offset [Proposed]; WCAG 2.2 AA requires 3:1 against adjacent colours (1.4.11) but sets no thickness (the thickness criterion 2.4.13 is AAA). Because of the offset, both sides of the ring show the surface behind the control, so the ring is checked against every surface a focusable control can sit on, not only white.

| Foreground | Background (adjacent surface) | Ratio | Required | Result |
|---|---|---|---|---|
| #2F5BEA focus ring | #FFFFFF surface | 5.52:1 | 3:1 | PASS |
| #2F5BEA focus ring | #F6F7F9 main background | 5.15:1 | 3:1 | PASS |
| #2F5BEA focus ring | #F3F5FA hover-soft | 5.06:1 | 3:1 | PASS |
| #2F5BEA focus ring | #E8EBF5 brand tint | 4.63:1 | 3:1 | PASS |
| #2F5BEA focus ring | #E7F4EC success-bg | 4.87:1 | 3:1 | PASS |
| #2F5BEA focus ring | #FFF3DC warning-bg | 5.02:1 | 3:1 | PASS |
| #2F5BEA focus ring | #FDECEA error-bg | 4.83:1 | 3:1 | PASS |
| #2F5BEA focus ring | #253264 navy fill, if the ring touched it | 2.21:1 | 3:1 | Not applicable by design. The offset is mandatory: the ring must never touch a navy fill, because it would fall to 2.21:1 |

#### E. Disabled controls

| Foreground | Background | Ratio | Required | Result |
|---|---|---|---|---|
| #6B7280 disabled text | #E4E6EB disabled fill | 3.87:1 | none | Exempt (inactive component, WCAG 1.4.3). Shown for reference only |

#### F. Seat Tile border (lecturer requirement)

`color-border-seat-tile` = #848E97, unchanged.

| Foreground | Background | Ratio | Required | Result |
|---|---|---|---|---|
| #848E97 seat tile border | #FFFFFF | 3.33:1 | 3:1 | PASS |
| #848E97 seat tile border | #F6F7F9 | 3.11:1 | 3:1 | PASS |
| #848E97 seat tile border | #E8EBF5 brand tint | 2.80:1 | 3:1 | FAIL |

**Rule:** the reserved `color-border-seat-tile` token must not be applied to `color-brand-tint` because the measured contrast is below 3:1. If a Seat Tile component is added in a future revision, use a white or #F6F7F9 adjacent surface with this border, or revise the border colour and recheck contrast before implementation. The token is currently reserved and must not be assigned to another component.

#### G. Link distinction (WCAG 1.4.1)

| Foreground | Compared with | Ratio | Required | Result |
|---|---|---|---|---|
| #3F5099 link | #161B2E body text | 2.29:1 | 3:1 (only when colour is the sole cue) | Colour alone is not enough. Links "Xem chi tiết" / "Xem tất cả" / "Xem xu hướng" are always underlined |


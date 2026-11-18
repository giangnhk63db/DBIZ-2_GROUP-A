# UI Conventions: Carpla CRM (Carpla Staff, web)

Scope: the screens of Carpla CRM used by Carpla staff, designed for a desktop web browser. Currently this is the screen S-F03-02 (Sale Request Review & Quote), where staff review a sale request that comes from the TASCO Verified Vehicle Report System and send the preliminary quote. The Car Owner screens of the TASCO Verified Vehicle Report System (mobile) have their own file: ui-conventions-car-owner.md.

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
| Page header | Top of the screen | Link "Quay lại" on the left (returns to the previous page of Carpla CRM), screen title "Xem xét yêu cầu bán xe và báo giá" on the left below it. Screen title is in Vietnamese, sentence case: capitalise only the first letter of the first word, plus abbreviations and proper names (VIN, SoH, VETC, Carpla); all other words lowercase |
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

## 6. Design tokens

**Design direction:** TASCO Corporate - Modern Mobility (professional, modern, minimal, consistent, accessible). This is the design direction chosen for this system; it is not a statement of TASCO's or Carpla's official brand identity.

Screens take colours, fonts, spacing, corners and frame width only from this section. Component names come from section 3. All sizes are CSS px.

**Principle:** every token in this design system must correspond to an actual component, UI role or interaction state defined in this document. A token with no current use is removed rather than kept in reserve.

### 6.1 Colour tokens

This section lists every colour used in the interface. Each colour is stored as a named design token (a fixed name that stands for one HEX value), so that designers and developers refer to "color-text-primary" instead of repeating a code, and a colour can be changed in one place. Tokens are needed so that the same role always looks the same on every screen. In the tables below, read each row as: token name, its HEX value, what it is used for, and which section 3 components use it.

#### Token naming convention

Colour token names follow the pattern `[category]-[role/purpose]-[state/variant]`, so the meaning of a token can be read from its name without looking at the HEX value. The last part is written only when it is needed; a token without it is the default form of that role.

| Part | Meaning | Examples |
|---|---|---|
| `color` | Fixed first word: this is a colour token | all colour tokens |
| Category / function | What the colour is for: `brand`, `bg`, `text`, `border`, `success`, `warning`, `error`, `info`, `hover`, `pressed`, `disabled`, `focus` | `brand` in `color-brand-primary`; `text` in `color-text-primary` |
| Role | The specific role within the category: `primary`, `secondary`, `tint`, `main`, `surface`, `default`, `input` | `primary` in `color-brand-primary`; `main` in `color-bg-main`; `input` in `color-border-input` |
| State / variant (optional) | A state or variant of the role: `bg` (background behind the text), `soft`, `text` | `bg` in `color-success-bg`; `text` in `color-disabled-text`; `soft` in `color-hover-soft` |

Examples: `color-brand-primary` is the primary brand colour; `color-bg-main` is the main page background; `color-text-primary` is the main text colour; `color-border-input` is the border of an input field; `color-success-bg` is the background behind success content; `color-hover` is the hover colour; `color-disabled-text` is the text colour of a disabled control.

#### Colour categories

Colours are divided into four categories so that each colour has one clear job and a new colour can be placed without ambiguity:

- **A. Brand colours:** colours that represent the visual identity of this system's design direction, used for primary visual emphasis.
- **B. Neutral colours:** background, text and border colours that form the structural foundation of the interface.
- **C. Semantic colours:** colours whose meaning comes from the state or information they communicate: success, warning, error and information.
- **D. Interactive colours:** colours that show interaction states: hover, pressed, disabled and focus.

#### A. Brand colours

| Token | HEX | Intended usage | Components |
|---|---|---|---|
| color-brand-primary | #253264 | Main action and key emphasis | Primary button fill, Banner text and icon (through `color-info`) |
| color-brand-secondary | #3F5099 | Lighter navy for links and secondary emphasis. Links are always underlined (see 6.6 F) | Link "Quay lại" in Page header, invoice link in Data table, Secondary button outline and text |
| color-brand-tint | #E8EBF5 | Very light navy for soft backgrounds | Banner |

#### B. Neutral colours

| Token | HEX | Intended usage | Components |
|---|---|---|---|
| color-bg-main | #F6F7F9 | Page background | Page header, all screens |
| color-bg-surface | #FFFFFF | Panel and overlay background | Info panel, Data table, Quote form, Text input, Modal |
| color-text-primary | #161B2E | Main text | Screen title, Info panel values, Data table cells, input values, messages |
| color-text-secondary | #4A5266 | Supporting text | Info panel labels, Text input label, Data table column headers |
| color-border-default | #D5D9E0 | Decorative lines only. Never the only way to recognise a control. | Data table row divider, Info panel outline, Data table outline |
| color-border-input | #848E97 | Functional border of a control the user must recognise (3:1, WCAG 1.4.11) | Text input border |

#### C. Semantic colours

Each status is shown with icon + text (section 5), never colour alone. Icons use the same colour as the text.

| Token | HEX | Intended usage | Components |
|---|---|---|---|
| color-success | #1B7A43 | Success text and icon | Success Modal, Status badge (inspection valid) |
| color-success-bg | #E7F4EC | Background behind success text | Status badge |
| color-warning | #8A5300 | Warning text and icon | Status badge ("Chưa xác minh", unpaid traffic fine, mortgage) |
| color-warning-bg | #FFF3DC | Background behind warning text | Status badge |
| color-error | #B42318 | Error text and icon | Inline validation error, system error Modal |
| color-error-bg | #FDECEA | Background behind error text | System error Modal |
| color-info | No new HEX: text and icon `color-brand-primary` (#253264) on background `color-brand-tint` (#E8EBF5) | Neutral information | Banner (a value in the report is not verified) |

#### D. Interactive colours

| Token | HEX | Intended usage | Components |
|---|---|---|---|
| color-hover | #1C2650 | Darker navy when the mouse pointer is over the control | Primary button |
| color-pressed | #141C3B | Darker navy while pressed | Primary button |
| color-hover-soft | #F3F5FA | Hover and pressed fill for non-filled controls | Secondary button |
| color-disabled-bg | #E4E6EB | Fill of a disabled control | Primary button, Secondary button |
| color-disabled-text | #6B7280 | Text of a disabled control (exempt from contrast rules). A disabled Secondary button has no outline: its outline is removed and only the fill and text above remain | Primary button, Secondary button |
| color-focus | #2F5BEA | Focus ring, 2 px thick, drawn with a 2 px offset so it never touches the control's own fill | All keyboard-focusable interactive elements defined in Section 3 |

**Token usage audit.** Every defined token has an assigned purpose and maps to a component, UI role or interaction state in this document. Rules: no gradients, no shadows, and no unlisted colours. Any new colour must be defined in this section before use.

### 6.2 Typography

Typography defines how text looks: size, line height and weight for each kind of text, so that the screen title, panel headings, body text, labels and buttons have a clear visual hierarchy and look the same on every screen. Each text role has its own token (for example `type-title`) so that developers do not choose sizes case by case. Read each row as: the role of the text, its token, its size and line height, weight and colour, and where it is used.

| Role | Token | Size / Line height (CSS px) | Weight | Colour | Used for |
|---|---|---|---|---|---|
| Page title | type-title | 24 / 32 | 700 | color-text-primary | Screen title in Page header |
| Section heading | type-heading | 18 / 26 | 600 | color-text-primary | Title of an Info panel block, title of the Data table and of the Quote form |
| Body text | type-body | 16 / 24 | 400 | color-text-primary | Info panel values, Data table cells, messages, Banner |
| Label | type-label | 14 / 20 | 500 | color-text-secondary | Info panel label, Text input label, Data table column header, Status badge text |
| Button | type-button | 16 / 24 | 600 | per button | Primary button, Secondary button, links "Quay lại" and invoice link (links are underlined) |
| Form input | type-input | 16 / 24 | 400 | color-text-primary | Text input value |

**Font family:** Be Vietnam Pro, with Roboto, "Segoe UI", system-ui, sans-serif as fallback fonts. Be Vietnam Pro is the project's standard UI typeface, selected for its support of Vietnamese diacritics and consistent readability across interface elements. This specification does not identify it as TASCO's or Carpla's official corporate brand font.

The values above are CSS px. At build time they may be written as rem (1 rem = 16 px); this changes how the value is written, not the design value.

The smallest size is 14 px.

### 6.3 Spacing and click target

Spacing defines the gaps and padding between elements, giving the interface a regular rhythm and keeping content from looking crowded. The click target is the minimum clickable area of a control; Carpla Staff uses a mouse and keyboard on a desktop browser, so the target can be smaller than on a phone, but it must still be easy to hit. The first table lists the allowed spacing values; the second table states the click-area rule for each clickable component.

Base unit 4 px. Only these values are used.

| Token | Value (CSS px) | Usage | Components |
|---|---|---|---|
| space-1 | 4 | Icon to text gap | Status badge, Banner |
| space-2 | 8 | Gap inside a component; label to input; label to value | Text input, Info panel, Banner |
| space-3 | 12 | Padding between rows | Info panel, Data table |
| space-4 | 16 | Panel inner padding; gap between the two Text inputs | Info panel, Data table, Quote form, Modal |
| space-5 | 24 | Gap between Info panel blocks, Data table and Quote form | Page content |
| space-6 | 32 | Page side margin; gap above the Primary button | Page header, Primary button area |

**Click target.** `size-click-min` = 24 × 24 CSS px for every element the user can click, as set by section 5 (WCAG 2.5.8). [Proposed] Buttons and Text inputs are at least 40 px high for comfortable mouse use; this is stricter than section 5 and consistent with it. The target is the clickable area, not the visible shape: a small icon keeps its drawn size and gets padding or an invisible hit area up to 24 × 24. Icon drawing size is set in the Screen Spec, not here.

| Component (section 3) | Clickable element | Rule |
|---|---|---|
| Page header | Link "Quay lại" | Hit area at least 24 × 24 |
| Data table | Invoice link in a row | Hit area at least 24 high |
| Text input | Whole field | Height at least 40 |
| Primary button | Whole button | Height at least 40 |
| Secondary button | Whole button | Height at least 40 |
| Modal | Close button | Hit area at least 24 × 24 |

Not clickable, no rule: Info panel, Status badge, Banner, Quote form (a layout container).

**Panel separation.** Because `color-border-default` on `color-bg-main` is only 1.32:1 and shadows are not allowed, Info panel blocks and the Data table are separated by the `space-5` (24 px) gap and the white `color-bg-surface` fill, not by the outline alone.

### 6.4 Border radius

Border radius defines how rounded the corners of a component are. Using a small set of fixed radius values gives buttons, inputs, panels, banners, badges and modals a consistent shape across screens. Read each row as: the radius token, its value in CSS px, and the components that use it.

| Token | Value (CSS px) | Components |
|---|---|---|
| radius-sm | 4 | Status badge |
| radius-md | 8 | Primary button, Secondary button, Text input, Banner |
| radius-lg | 12 | Info panel, Data table, Modal |

### 6.5 Frame width

Frame width is the screen width used when drawing the screen images and writing the specifications. Fixing it, together with the orientation and layout rules, ensures that all screens are designed for the same browser size and that nothing needs horizontal scrolling. Read each row as a layout constraint that every screen image must follow.

| Item | Value |
|---|---|
| Frame width of screen images for S-F03-02 | 1280 px |
| Orientation | Landscape (desktop web browser) |
| Layout | Vertical scroll only, no horizontal scroll; the two Text inputs of the Quote form sit side by side |
| Minimum supported screen width | 1280 px |

### 6.6 WCAG 2.2 AA contrast checks

Contrast is the difference in brightness between two colours, such as text and its background; WCAG 2.2 AA is the accessibility standard that sets the minimum contrast so that text and controls remain readable for people with low vision. These checks measure the colour pairs used in this system instead of relying on visual judgement alone. In each table, the ratio is the measured contrast, "Required" is the minimum for that kind of element, and "Result" shows whether the pair passes.

Ratios are computed with the WCAG relative-luminance formula, not estimated by eye. Thresholds: normal text 4.5:1; large text and UI components 3:1. No HEX was changed to obtain a result. Only colour pairs that occur in the components of section 3 are listed.

#### A. Text

| Foreground | Background | Ratio | Required | Result |
|---|---|---|---|---|
| #161B2E text primary | #F6F7F9 main background | 15.92:1 | 4.5:1 | PASS |
| #161B2E text primary | #FFFFFF surface | 17.07:1 | 4.5:1 | PASS |
| #4A5266 text secondary | #F6F7F9 main background | 7.28:1 | 4.5:1 | PASS |
| #4A5266 text secondary | #FFFFFF surface | 7.81:1 | 4.5:1 | PASS |
| #FFFFFF button text | #253264 primary button | 12.22:1 | 4.5:1 | PASS |
| #FFFFFF button text | #1C2650 hover | 14.56:1 | 4.5:1 | PASS |
| #FFFFFF button text | #141C3B pressed | 16.68:1 | 4.5:1 | PASS |
| #3F5099 link / secondary button text | #FFFFFF surface | 7.45:1 | 4.5:1 | PASS |
| #3F5099 link / secondary button text | #F6F7F9 main background | 6.95:1 | 4.5:1 | PASS |
| #3F5099 secondary button text | #F3F5FA hover-soft | 6.83:1 | 4.5:1 | PASS |
| #3F5099 secondary button text | #FDECEA error-bg (Secondary button "Đóng" in the system error Modal) | 6.51:1 | 4.5:1 | PASS |
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
| #D5D9E0 row divider / panel outline | #FFFFFF | 1.42:1 | none | Not applicable (decorative). Content stays readable without the line |
| #D5D9E0 panel outline | #F6F7F9 main background | 1.32:1 | none | Not applicable (decorative). Panels stay separated by the `space-5` gap and the white fill (see 6.3) |

#### C. Interactive control borders (WCAG 1.4.11, 3:1)

| Foreground | Background | Ratio | Required | Result |
|---|---|---|---|---|
| #848E97 input border | #FFFFFF surface | 3.33:1 | 3:1 | PASS |
| #848E97 input border | #F6F7F9 main background | 3.11:1 | 3:1 | PASS |
| #848E97 input border | #E8EBF5 brand tint | 2.80:1 | 3:1 | FAIL. Rule: a Text input must not sit directly on color-brand-tint (for example inside a Banner) |
| #3F5099 secondary button outline | #FFFFFF | 7.45:1 | 3:1 | PASS |
| #3F5099 secondary button outline | #F6F7F9 | 6.95:1 | 3:1 | PASS |
| #3F5099 secondary button outline | #FDECEA error-bg (system error Modal) | 6.51:1 | 3:1 | PASS |

#### D. Focus indicator

The ring is 2 px thick with a 2 px offset [Proposed]; WCAG 2.2 AA requires 3:1 against adjacent colours (1.4.11) but sets no thickness (the thickness criterion 2.4.13 is AAA). Section 5 requires a visible outline on the focused element (2.4.7). Because of the offset, both sides of the ring show the surface behind the control, so the ring is checked against every surface a focusable control can sit on, not only white.

| Foreground | Background (adjacent surface) | Ratio | Required | Result |
|---|---|---|---|---|
| #2F5BEA focus ring | #FFFFFF surface | 5.52:1 | 3:1 | PASS |
| #2F5BEA focus ring | #F6F7F9 main background | 5.15:1 | 3:1 | PASS |
| #2F5BEA focus ring | #FDECEA error-bg (system error Modal) | 4.83:1 | 3:1 | PASS |
| #2F5BEA focus ring | #253264 navy fill, if the ring touched it | 2.21:1 | 3:1 | Not applicable by design. The offset is mandatory: the ring must never touch a navy fill, because it would fall to 2.21:1 |

#### E. Disabled controls

| Foreground | Background | Ratio | Required | Result |
|---|---|---|---|---|
| #6B7280 disabled text | #E4E6EB disabled fill | 3.87:1 | none | Exempt (inactive component, WCAG 1.4.3). Shown for reference only |

#### F. Link distinction (WCAG 1.4.1)

| Foreground | Compared with | Ratio | Required | Result |
|---|---|---|---|---|
| #3F5099 link | #161B2E body text | 2.29:1 | 3:1 (only when colour is the sole cue) | Colour alone is not enough. Link "Quay lại" and the invoice link in the Data table are always underlined |

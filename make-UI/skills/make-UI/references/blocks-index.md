# SnowUI Block Index

Page-level building blocks with exact specs. Sources: `docs/patterns/dashboard-layout.md`, `docs/patterns/authentication-flows.md`, `docs/patterns/settings-flows.md`

---

## Sidebar (Dashboard)

- **Dimensions:** 212 x 1024px
- **Layout:** VERTICAL, padding 16 all, gap 8
- **Tokens:** `paddingLeft/Top/Right/Bottom: "16"`, `itemSpacing: "8"`, `strokes: "Black/10%"`
- **Content width:** 180px (212 - 16 - 16)
- **Children:** Profile+Favorites section (168px), Dashboards section (160px), Pages section (440px), Logo (bottom-pinned at y=968)
- **Nav item:** 36px tall, HORIZONTAL, pad 8, gap 4, radius 12, icon 20x20, text 14px Regular
- **Active state:** fill `#000000 @ 0.04`, radius 12
- **Section header:** 14px Regular, `#000000 @ 0.4`, height 28px
- **Width range:** min 212px, max 40% of page
- **Source:** `docs/patterns/dashboard-layout.md`

---

## Header (Dashboard)

- **Dimensions:** 948 x 68px
- **Layout:** HORIZONTAL, padding 28/20/28/20
- **Tokens:** `paddingLeft: "28"`, `paddingTop: "20"`, `strokes: "Black/10%"`
- **Left (243px):** Sidebar toggle (24x24) + Star (24x24) + Breadcrumb (179x24, gap 8, inactive 12px @ 0.4, separator "/" @ 0.1, active 12px @ 1.0)
- **Right (300px):** Search bar (160x28, radius 16, fill `#000000 @ 0.04`, placeholder "Search" 14px @ 0.2) + 4 icon buttons (24x24 each, pad 4, radius 12)
- **Fixed:** always at top of page
- **Source:** `docs/patterns/dashboard-layout.md`

---

## Content Grid (Dashboard Overview)

- **Dimensions:** 892 x 1082px (scrollable)
- **Layout:** HORIZONTAL (wrapping), gap 28 both axes
- **Tokens:** `itemSpacing: "28"`, `counterAxisSpacing: "28"`
- **Grid math:** 4 KPI cards: `4*202 + 3*28 = 892`. 2 half-blocks: `2*432 + 28 = 892`. Large+small chart: `662 + 28 + 202 = 892`
- **Children:** KPI cards row, Revenue chart (662x330), Pie chart (202x330), 2x half-width charts (432x280), full-width chart (892x280)
- **Source:** `docs/patterns/dashboard-layout.md`

---

## KPI Card

- **Dimensions:** 202 x 108px (dashboard) / 279 x 112px (projects) / 280 x 112-116px (card page)
- **Layout:** VERTICAL, padding 24 all, gap 8, radius 20
- **Tokens:** `paddingLeft/Top/Right/Bottom: "24"`, `topLeftRadius: "20"`, alternating `fills: "Background/4"` (#edeefc) / `"Background/5"` (#e6f1fd)
- **Typography:** Label 14px Regular, Value 24px SemiBold, Delta 12px Regular
- **Source:** `docs/patterns/dashboard-layout.md`

---

## Chart Block

- **Layout:** VERTICAL, padding 24 all, gap 16, radius 20
- **Tokens:** `paddingLeft/Top/Right/Bottom: "24"`, `itemSpacing: "16"`, `topLeftRadius: "20"`, `fills: "Background/2"`
- **Sizes:** 662x330 (large), 202x330 (small), 432x280 (half), 892x280 (full)
- **Source:** `docs/patterns/dashboard-layout.md`

---

## Right Bar

- **Dimensions:** 280 x 1023px
- **Layout:** VERTICAL, padding 16 all, gap 16
- **Tokens:** `paddingLeft/Top/Right/Bottom: "16"`, `itemSpacing: "16"`, `strokes: "Black/10%"`
- **Content width:** 248px
- **Children:** Notifications section (52px rows), Activities section (52px rows), Contacts section (40px rows)
- **Row spec:** HORIZONTAL, pad 8, gap 8, radius 12, icon container 24x24 (pad 4, radius 8), text 14px + timestamp 12px @ 0.4
- **Width range:** min 212px, max 40% of page
- **Source:** `docs/patterns/dashboard-layout.md`

---

## Table Block (Order List)

- **Action bar:** 892 x 44px, HORIZONTAL, pad 8, gap 16, fill `#f9f9fa`, radius 16
- **Table:** 892 x 440px, row height 40px uniform, 1 header + 10 data rows
- **Columns:** Checkbox 32px, Order ID 88px, User 149px, Project 149px, Address 176px, Date 149px, Status 110px, Actions 40px
- **Header text:** 12px Regular @ 0.4. **Data text:** 12px Regular @ 1.0
- **Status badges:** Dot (16px) + label. Colors: In Progress=#b899eb, Complete=#71dd8c, Pending=#7dbbff, Approved=#ffcc00, Rejected=#000000
- **Pagination:** 892 x 28px, gap 8, radius 8
- **Source:** `docs/patterns/dashboard-layout.md`

---

## Auth Card

- **Dimensions:** 680 x 700px, positioned x=380, y=162 (centered)
- **Layout:** VERTICAL, padding 24, gap 24, radius 32
- **Fill:** light=#ffffff, dark=#333333. Effects: BACKGROUND_BLUR radius=40
- **Tokens:** `itemSpacing: "24"`, `paddingLeft/Top/Right/Bottom: "24"`
- **Content frame:** 313px wide, centered in 632px card interior, gap 28
- **Input:** 313 x 56px, radius 16, pad 16/20, stroke `#000000` weight=0.5, text 18px
- **Submit button:** 313 x 56px, radius 16, fill `#000000` (dark: `#adadfb`), text 16px white
- **Social buttons:** 99 x 48px each, radius 20. Google=#8156fa, Apple=#000000 (dark: #adadfb), Facebook=#1877f2
- **OTP cells:** 56x56 (6-digit) or 56x52 (4-digit), radius 16, gap 8
- **Source:** `docs/patterns/authentication-flows.md`

---

## Settings Modal Shell

- **Backdrop:** 1440x1024, gradient `#d7d0ff @ 0.2` to `#cbddff @ 0.5`, radius 16
- **Outer frame:** 1176 x 748px, centered (x=132, y=138), gap 28
- **Title bar:** 1176 x 56px, HORIZONTAL, title "Settings" 48px/600, close button 40x40 r12 bg=#000000
- **Popup body:** 1176 x 664px, radius 32, fill `#ffffff`, bg-blur 40, HORIZONTAL
- **Settings sidebar:** 280 x 664px, fill `#f9f9fa`, pad 24, gap 4, nav items 232x56 r16 pad 16 gap 8
- **Settings content:** 896 x 664px, pad 28/24, gap 8, content width 848px
- **Nav active:** fill `#000000`, text `#ffffff`. **Inactive:** transparent, text `#000000`
- **Source:** `docs/patterns/settings-flows.md`

---

## Form Section (Settings)

- **Setting row (single-line, h=36):** pad 8/12, gap 16, label 14px, value 14px, arrow 20x20
- **Setting row (two-line, h=56):** label 14px + description 12px, value + arrow
- **Toggle row (h=72):** label + description, toggle 32x32
- **Section header:** 848px wide, h=28, pad 0/12, text 18px/600
- **Divider:** 848x16 outer, 824x0 inner, stroke `#000000 @ 0.04`
- **Modal input:** h=56, radius 16, pad 16/20, stroke `#000000` weight=0.5, text 18px
- **Primary button:** 240 x 56px, radius 20, pad 12/20, fill `#000000`, text 16px white
- **Destructive button:** same as primary, fill `#ff4747`
- **Notification toast:** h=40, radius 16, pad 8/12, gap 8, bg `#000000`, text 14px white, check icon `#71dd8c`
- **Source:** `docs/patterns/settings-flows.md`

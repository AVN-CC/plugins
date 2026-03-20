# SnowUI Data Format Rules

How to display time, dates, numbers, and localized content. Source: `docs/patterns/interactive-guidance.md` Section 19 (Data Formats).

> "Written after referring to Atlassian Design and Material System."

---

## Relative Time — Past (Commonly Used)

| Threshold | Display Format |
|-----------|---------------|
| < 1 minute | "Just now" |
| < 1 hour | "{n} minutes ago" |
| < 12 hours | "{n} hours ago" |
| > 12 hours (AM) | "Today, {time} AM" |
| > 12 hours (PM) | "Today, {time} PM" |
| > 1 day | "Yesterday" |
| > 2 days | "Feb 2, 2026" (absolute date) |

---

## Relative Time — Verbose Past

| Threshold | Display |
|-----------|---------|
| < 1 minute | "Just now" |
| ~1 minute | "A minute ago" |
| ~1 hour | "1 hour ago" |
| ~2.5 hours | "2 hr 32 min ago" |
| Yesterday | "Yesterday, 8:00 AM" |
| ~1 day | "1 day ago" |
| ~1 week | "1 week ago" |

---

## Relative Time — Future

| Threshold | Display |
|-----------|---------|
| < 1 minute | "Shortly" |
| ~1 minute | "In 1 minute" |
| ~1 hour | "In 1 hour" |
| ~2.5 hours | "In 2 hr 32 min" |
| Next day | "Tomorrow, 8:00 AM" |
| ~1 day | "In 1 day" |
| ~1 week | "In 1 week" |

---

## Date Formats

| Format | Example |
|--------|---------|
| 24h time | `20:00` |
| 12h time | `8:00 AM` |
| Today + time | `Today, 11:59 AM` |
| Date + time | `Feb 2, 8:00 AM` |
| Date only | `Feb 2, 2026` |
| Full date + time | `Feb 2, 2026, 8:00 AM` |
| ISO 8601 | `20/02/2026, 08:59:59 AM` |
| Input format | `MM/DD/YYYY` |

---

## Date and Time Range Formats

| Format | Example |
|--------|---------|
| Time range (same day) | `20:00 - 00:00` |
| Time range (12h) | `8:00 - 11:59 AM` |
| Cross-day range | `Feb 2, 11:59 AM - Mar 3, 12:00 PM` |
| Date range (same month) | `Feb 2 - 10, 2026` |
| Date range (cross-month) | `Feb 2, 2026 - Mar 3, 2026` |
| Full range | `Feb 2, 2022, 11:59 AM - Mar 3, 2026, 12:00 PM` |
| ISO range | `20/02/2026, 08:59:59 AM - 25/02/2026, 12:00:00 PM` |
| Input format | `MM/DD/YYYY - MM/DD/YYYY` |

---

## Month Formats

| Full | Abbreviated |
|------|-------------|
| January | Jan |
| February | Feb |
| March | Mar |
| April | Apr |
| May | May |
| June | Jun |
| July | Jul |
| August | Aug |
| September | Sep |
| October | Oct |
| November | Nov |
| December | Dec |

---

## Weekday Formats

| Full | Abbreviated |
|------|-------------|
| Monday | Mon |
| Tuesday | Tue |
| Wednesday | Wed |
| Thursday | Thu |
| Friday | Fri |
| Saturday | Sat |
| Sunday | Sun |

---

## Time Unit Formats

| Full | Medium | Short |
|------|--------|-------|
| Hour | hr | h |
| Minute | min | m |
| Second | sec | s |

---

## Number Formats

| Type | Format | Example |
|------|--------|---------|
| Integer | Comma-separated thousands | 48,291 |
| Currency | `$X,XXX.XX` | $1,234.56 |
| Percentage | `X.XX%` | 11.01% |
| Negative currency | `$-X.XX` in `Secondary/Indigo` | $-2.60 |
| Large numbers | Abbreviate with K/M/B | 1.2M |

---

## Time Auto-Correction

If user enters "13" in hour field, auto-switch to PM and display "01".

---

## Locale Support

| Element | English | Chinese |
|---------|---------|---------|
| Date | Mar 20, 2026 | 2026年3月20日 |
| Time | 2:30 PM | 14:30 |
| Number separator | 1,234.56 | 1,234.56 |
| Relative time | "5 minutes ago" | "5分钟前" |

---

## Content Truncation

- **Single-line overflow:** Ellipsis (`...`) after container width exceeded
- **Multi-line:** Clamp at 2-3 lines with ellipsis
- **Tooltip reveal:** Hover over truncated text shows full content in tooltip
- **Table cells:** Single-line truncation with tooltip on hover

---

## Empty States

- Use placeholder text in `Black/20%`
- Skeleton loading shapes match target content dimensions
- Never show "null", "undefined", or "N/A" — show dash "—" or hide the field entirely

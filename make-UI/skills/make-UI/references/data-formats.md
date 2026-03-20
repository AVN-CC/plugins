# SnowUI Data Format Rules

How to display time, dates, numbers, and localized content. Source: `docs/patterns/interactive-guidance.md` Section 16 (Data Format).

---

## Time Display (Relative → Absolute)

| Elapsed | Display | Example |
|---------|---------|---------|
| < 1 minute | "Just now" | Just now |
| 1-59 minutes | "N minutes ago" | 5 minutes ago |
| 1-23 hours | "N hours ago" | 3 hours ago |
| 1-6 days | "N days ago" | 2 days ago |
| 7-29 days | "MMM DD" | Mar 15 |
| 30+ days | "MMM DD, YYYY" | Mar 15, 2026 |

**Rule:** Always use relative time for recent content (< 7 days). Switch to absolute dates after 7 days.

---

## Date Formats

| Context | Format | Example |
|---------|--------|---------|
| Inline date | `MMM DD, YYYY` | Mar 20, 2026 |
| Date picker display | `MM/DD/YYYY` | 03/20/2026 |
| Table date column | `MMM DD, YYYY` | Mar 20, 2026 |
| Chart axis | `MMM DD` or `MMM` | Mar 20, Mar |
| Timestamp | `MMM DD, YYYY HH:MM AM/PM` | Mar 20, 2026 2:30 PM |

---

## Time Formats

| Context | Format | Example |
|---------|--------|---------|
| 12-hour (default) | `H:MM AM/PM` | 2:30 PM |
| Duration | `Xh Ym` | 2h 30m |
| Timer | `MM:SS` | 05:30 |

**Auto-correction rule:** If user enters "13" in hour field, auto-switch to PM and display "01".

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

## Locale Support

SnowUI supports English and Chinese locales:

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

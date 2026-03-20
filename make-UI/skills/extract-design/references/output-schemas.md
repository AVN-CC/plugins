# Output File Schemas

Templates for each reference file type. Use these as the target format when normalizing raw extraction data. Replace bracketed placeholders with extracted values.

---

## tokens.md

```markdown
# [Design System Name] Design Tokens

Source: [source description]
Extracted: [date]

---

## Colors ([N] modes: [mode1], [mode2], ...)

### [Category Name] (e.g., Backgrounds, Opacity Scales, Primary/Secondary)

| Token | [Mode 1] | [Mode 2] | ... |
|-------|----------|----------|-----|
| [token/name] | [hex or rgba] | [hex or rgba] | ... |

[Repeat per category]

---

## Spacing ([N] modes: [mode names])

| Token | [Mode 1] | [Mode 2] | ... |
|-------|----------|----------|-----|
| [value] | [px] | [px] | ... |

---

## Corner Radius ([N] modes)

| Token | [Mode 1] | [Mode 2] | ... |

---

## Size ([N] modes)

| Token | [Mode 1] | [Mode 2] | ... |

---

## Typography Scale

| Style | Size | Weight | Line Height | Formula |
|-------|------|--------|-------------|---------|
| [name] | [px] | [weight] | [px] | [if pattern detected] |

---

## Effect Styles

| Style | Spec |
|-------|------|
| [name] | [shadow/blur/glow spec] |
```

**Normalization rules for tokens:**
- Color values: Convert `{r, g, b, a}` (0-1 range) to hex. If `a < 1`, use 8-digit hex (`#rrggbbaa`).
- Alias tokens: Show as `alias: [target token name]` in the table.
- Group by collection name from Figma.
- Sort tokens within each group alphabetically or by semantic hierarchy.

---

## color-system-qa.md

```markdown
# [Design System] Color System — QA Reference

## 1. Text Color Rules

| Role | Token | Light Value | Dark Value |
|------|-------|-------------|------------|
| [role description] | [token name] | [value] | [value] |

## 2. Icon Color Rules

| Role | Token | Notes |
|------|-------|-------|
| [role] | [token] | [when to use] |

## 3. Background Color Rules

| Token | Light | Dark | Usage |
|-------|-------|------|-------|
| [token] | [value] | [value] | [where used] |

### Text-on-Background Pairing Rules

| Background | Text Tokens Allowed | Icon Tokens Allowed |
|------------|--------------------|--------------------|
| [bg token] | [text tokens] | [icon tokens] |

**BUILD RULES**: [any critical pairing rules discovered — e.g., static bg requires static text tokens]

## 4. Stroke/Border Rules

| Token | Light | Dark | Usage |
|-------|-------|------|-------|

## 5. Dark Mode Collision Rules

[Any cases where naive token usage creates invisible text or unreadable UI]

| Surface | Text Token | Why |
|---------|-----------|-----|

## 6. Anomalies and Exceptions

[Any hardcoded values, non-standard bindings, or designer overrides]
```

**Normalization rules for color QA:**
- Derive text/icon/bg roles from boundVariable data (Script 2 output grouped by node type)
- Derive pairing rules by finding which text tokens appear inside which bg frames
- Detect collisions: any text token that inverts to same color as a non-inverting background

---

## component-states.md

```markdown
# [Design System] Component State Matrix

## The [N] States

| State | Visual Change | Trigger |
|-------|--------------|---------|
| [state] | [what changes visually] | [what triggers it] |

---

## State Matrix by Component

### [Component Name]

```
[State]: [property=value] descriptions per state
```

[Repeat per component]

---

## Universal Rules

1. [Rule derived from patterns across all components]
```

**Normalization rules for states:**
- Use Script 6 output to build variant deltas
- "Default" variant is the baseline — all other states describe changes FROM default
- If opacity goes to 0.4, that's "Disabled"
- If a shadow appears with spread, that's "Focus"
- If fill changes to Primary, that's "Active/Selected"

---

## composition-rules.md

```markdown
# [Design System] Composition Rules

## [N]. [Rule Title]

> "[Designer quote if available from text annotations]"

```
[structural example or code block]
```

[Repeat per rule, numbered sequentially]
```

**Normalization rules:**
- Extract rules from repeated patterns in boundVariable data
- If the same padding/gap pattern appears 5+ times, it's a rule
- Designer quotes come from TEXT nodes in annotation frames

---

## interaction-patterns.md

```markdown
# [Design System] Interaction Patterns

## [Category Name]

### [Sub-pattern]

[Behavioral description with specs]

| [relevant columns] |

[Repeat per category]
```

**Normalization rules:**
- Behavioral rules come from: annotation text nodes, state variant analysis, visibility bindings
- Group by interaction type: loading, hover, focus, validation, scroll, animation, etc.

---

## data-formats.md

```markdown
# [Design System] Data Format Rules

## [Format Category]

| Threshold/Type | Display Format | Example |
|---------------|---------------|---------|

[Repeat per category: time, date, number, truncation, empty states]
```

**Normalization rules:**
- Data format rules usually come from annotation text or documentation, not from Figma node properties
- Look for frames labeled "Data formats", "Date", "Time", etc.

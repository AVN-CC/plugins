# Merge Strategy

How to integrate newly extracted data into existing make-UI reference files without losing manually verified content.

---

## Core Principle

**Never overwrite manually verified data.** Existing reference files were built through exhaustive extraction and manual QA. New extractions append, extend, and flag conflicts — they never replace.

---

## Pre-Merge Checklist

Before merging, verify:

1. **Read existing reference files** — know what's already there
2. **Check for duplicates** — don't add tokens/components/rules that already exist
3. **Check for conflicts** — same token name with different value = conflict
4. **Present merge plan** — show user what will be added, updated, or flagged

---

## Merge Rules by File Type

### tokens.md

**Adding new tokens:**
- Append to the appropriate category table
- If the category doesn't exist, create a new section
- Sort new tokens into existing table alphabetically

**Conflicts:**
- Same token name + same values across all modes = duplicate, skip
- Same token name + different values = `[CONFLICT]` marker with both values shown
- Present conflicts to user for resolution

**New modes:**
- If the extracted data has modes not in the existing table, add new columns
- Existing mode values stay unchanged

### color-system-qa.md

**Adding new rules:**
- Append to the relevant section (text rules, icon rules, bg rules, etc.)
- New pairing rules go at the end of the pairing table
- New build rules go at the end of the section with `[NEW]` marker
- New collision rules get their own subsection

**Never modify:**
- Existing pairing rules (manually verified)
- Existing build rules (learned from QA failures)
- Existing collision detection entries

### component-states.md

**Adding new components:**
- Append new component section at the end (before Universal Rules)
- Follow the existing format: component name heading + state code block

**Extending existing components:**
- If new states found for an existing component, append below existing states
- Mark new states with `[NEW]`
- Don't modify existing state descriptions

**Adding new universal rules:**
- Append at the end of Universal Rules section
- Number continues from last existing rule

### composition-rules.md

**Adding new rules:**
- Append as new numbered rules (increment from last number)
- Include designer quote if found in annotations
- Include structural example

**Never modify existing rules** — they were derived from exhaustive analysis

### interaction-patterns.md

**Adding new sections:**
- If the interaction category doesn't exist, add a new section
- If it exists, append new patterns below existing content in that section
- Mark new additions with source attribution

**Merging into existing sections:**
- Read the existing section carefully
- Only add genuinely new information
- Don't duplicate what's already there

### data-formats.md

**Adding new format tables:**
- Append to the relevant section
- Deduplicate exact matches (same threshold + same display format)
- New locale support goes in the locale table

---

## Conflict Resolution

When conflicts are found, present them in a clear format:

```
CONFLICT: [token/rule name]
  Existing: [current value in reference file]
  Extracted: [new value from extraction]
  Source: [where the new value came from]
  Recommendation: [keep existing / update / needs investigation]
```

Let the user decide. Common resolution patterns:
- **Token value changed in Figma** → Update the reference (Figma is source of truth)
- **Different Figma file has different value** → Keep both, note the source
- **Extraction error** → Keep existing, discard extracted value

---

## Post-Merge Verification

After merging:

1. **Count check**: Reference file should have MORE entries than before (or same if all duplicates)
2. **Format check**: Tables still render correctly (no broken markdown)
3. **Cross-reference check**: New component state tokens reference tokens that exist in tokens.md
4. **No data loss**: Diff the file before and after — only additions, no deletions of existing content

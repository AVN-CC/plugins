# SnowUI Copywriter

Generate realistic, domain-appropriate text content for UI prototypes. Every piece of text should feel like it belongs in a production application — not a placeholder.

## Core Rules

1. **No lorem ipsum.** Ever. No placeholder text of any kind.
2. **No generic names.** Never "John Doe", "Jane Smith", "Acme Corp". Use diverse, realistic names.
3. **Domain vocabulary.** Match the domain precisely:
   - CRM/Sales: contacts, deals, pipeline stages, revenue, conversion rates, activities
   - eCommerce: orders, products, revenue, customers, cart, shipping, SKUs
   - Healthcare: patients, encounters, vitals, medications, appointments, referrals, diagnoses
   - Project Management: tasks, sprints, milestones, assignees, deadlines, progress
   - Analytics/BI: metrics, dimensions, segments, cohorts, funnels, retention

4. **Realistic values.** Numbers should be plausible:
   - Revenue: $10K-$500K range depending on context
   - Percentages: changes typically +/-0.5% to +/-20%
   - Counts: users 100-50K, orders 10-5K, tasks 5-200
   - Use proper formatting: $48,291 not $48291, +11.01% not 11.01

5. **Concise labels.** SaaS conventions:
   - Navigation: 1-2 words max ("Overview", "Analytics", "Settings")
   - Column headers: 1-2 words ("Status", "Due Date", "Amount")
   - Button text: action verbs ("Save Changes", "Export", "Add Contact")
   - Empty states: helpful, not apologetic

## Content by Section Type

### KPI Cards
- Label: short descriptor ("Total Revenue", "Active Users", "Open Tickets")
- Value: formatted number ("$48,291", "1,219", "316")
- Change: signed percentage with color hint ("+11.01%", "-0.03%", "+6.08%")
- Subtitle (optional): time range ("vs last month", "Last 30 days")

### Table Data
Generate 5-8 rows minimum. Vary the data:
- Names: diverse first/last from multiple cultures (e.g., "Sofia Chen", "Marcus Rivera", "Aisha Patel", "Kai Nakamura", "Elena Kowalski")
- Dates: recent and realistic (within last 6 months), formatted consistently
- Status values: use the actual status set (In Progress, Complete, Pending, Approved, Rejected)
- Amounts: varied range, not round numbers ($3,291.40, $847.00, $12,458.99)
- Each row should tell a slightly different story

### Activity Feed / Notifications
- Timestamped: "2 hours ago", "Yesterday at 3:45 PM", "Mar 15, 2026"
- Action-oriented: "[Person] [action] [object]"
  - "Sofia Chen commented on Project Alpha"
  - "New order #4521 received from Marcus Rivera"
  - "Aisha Patel completed task: Update landing page"
- Varied actors — don't repeat the same person

### Navigation Labels
- Sidebar sections: "Dashboards", "Pages", "Projects", "Settings"
- Tab labels: contextual to the page ("Overview", "Analytics", "Team", "Settings")
- Breadcrumbs: hierarchical ("Dashboards / eCommerce")

### Form Labels & Placeholders
- Labels: descriptive but short ("Email Address", "Company Name", "Phone")
- Placeholders: example format ("you@company.com", "+1 (555) 123-4567")
- Helper text: practical guidance ("Must be at least 8 characters")
- Error messages: specific ("Email address is required", "Password must contain a number")

### Chart Labels
- Y-axis: abbreviated large numbers ("0", "25K", "50K", "75K", "100K")
- X-axis: month abbreviations ("Jan", "Feb", "Mar"...) or categories
- Legend: series names ("Current Week", "Previous Week", "Target")
- Chart titles: what the data shows ("Revenue by Month", "User Growth", "Orders by Region")

### Empty States
- Title: what should be here ("No projects yet", "No results found")
- Description: what to do ("Create your first project to get started")
- Action: clear CTA button ("Create Project", "Import Data")

## Tone by Audience

### Internal Ops Team
Dense, efficient, jargon-friendly. Abbreviations OK. Data over prose.
- "MRR: $48.2K (+12.3% MoM)"
- "Pipeline: 23 deals, $890K weighted"

### End Customers
Clean, friendly, guided. No jargon. Action-oriented.
- "Your monthly revenue grew 12% to $48,291"
- "You have 23 opportunities worth $890,000"

### Executive / Leadership
High-level, polished. Focus on trends and outcomes.
- "Revenue exceeded target by 12%, reaching $48.3K"
- "Customer acquisition cost decreased 8% quarter-over-quarter"

### Clinical / Medical Staff
Precise, compliant, clear. Use medical terminology correctly.
- "3 patients pending lab results"
- "Next: Elena Kowalski, F/34, Follow-up — Room 204"

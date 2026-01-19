# PRD Lite: Complete Guide & Example

This file contains detailed writing guidelines and a complete example for creating a PRD Lite.

---

## Writing Guidelines

### Problem
- Keep it concise - one clear problem statement
- Focus on the immediate pain point or need

### Solution
- Brief description of what you're building
- Include links to designs, docs, or resources inline
- Can include technical notes if relevant

### Requirements
- Use a markdown table with dynamic columns based on needs
- Common requirement types: Location, Label, Visibility, Behavior, Style, Implementation
- Be specific and implementation-focused

**Example Requirements Table:**
```markdown
| Requirement | Detail |
| -- | -- |
| **Location** | Left sidebar, above language switcher |
| **Label** | Help icon + "User Guide" |
| **Visibility** | Premium users only |
| **Behavior** | Opens documentation in new tab |
```

### Acceptance Criteria
- Write as testable bullet points
- Each point should be verifiable (passes/fails clearly)
- Focus on user-visible behavior and conditions

---

## Complete Example: Export to CSV Button

# Export to CSV Button (for Reports Page)

## Problem
Users need to export report data for offline analysis or sharing with stakeholders who don't have platform access, but currently must manually copy data or request exports from support.

---

## Solution
Add an **"Export CSV"** button to the reports page header that downloads the current report view as a CSV file.

Design reference: [Figma mockup link]

---

## Requirements

| Requirement | Detail |
| -- | -- |
| **Location** | Reports page header, right-aligned next to the date filter |
| **Label** | Download icon + "Export CSV" |
| **Style** | Secondary button (outlined), matches existing header buttons |
| **Visibility** | Users with "Reports" permission enabled |
| **Behavior** | Downloads CSV file with current filters applied; filename format: `report-name_YYYY-MM-DD.csv` |
| **Data** | Includes all visible columns; respects current sort order; max 10,000 rows (show warning if truncated) |

---

## Acceptance Criteria
* Button appears in reports header for users with Reports permission
* Button hidden for users without Reports permission
* Clicking button downloads CSV with current report data
* Downloaded filename includes report name and current date
* CSV respects active filters and sort order
* If data exceeds 10,000 rows, user sees warning before download
* Button shows loading state during export generation

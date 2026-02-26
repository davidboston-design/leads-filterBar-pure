# CCPI Search Module — Prototype Shell

## What We're Building
A static, non-interactive prototype of the ConstructConnect Project Intelligence (CCPI) search page. This is a visual shell for user research — all elements should render in their correct visual states but nothing needs to be functional yet. Dropdowns don't open, filters don't apply, search doesn't execute. Use placeholder/mock data throughout.

## Page Sections (top to bottom)

### Top Nav Bar
Full-width dark teal bar. ConstructConnect logo on the left, placeholder avatar/nav items on the right. This just needs to look present — it's not the focus.

### Page Header
"Search" as the page title. A filled "Save Search" button with a bookmark icon. A "Start a New Search" text link. All sitting in a row.

### Filter Bar (this is the primary component — invest the most attention here)
A horizontal bar inside a white card container with a subtle top border and light drop shadow. Elements are arranged left to right with consistent spacing:

1. **Load Search button** — A dropdown trigger with a bookmark icon and a chevron-down. Gray background, dark text.
2. **Keyword input** — A text input field (220px wide, 32px tall) with italic placeholder text "Search by keyword" and a magnifying glass icon inside on the right.
3. **Filter buttons** — A row of filter dropdown triggers: Stage, Category, Location, Project Value, Date Range. Each is 32px tall with a chevron-down icon.
4. **More Filters +** — A teal text link that would eventually let users add additional filters.

**Filter button visual states** (build all as separate CSS states, not interactive):
- **Default/unselected:** Gray background, dark bold text. Example: "Stage ▾"
- **Applied, single value:** Light blue tinted background, teal text showing the filter name in medium weight followed by the selected value in bold. Example: "Stage **Planning** ▾"
- **Applied, multiple values:** Same light blue background, filter name in teal, then a small gray count badge with white text. Example: "Category **+2** ▾"
- **Added/removable:** Same as applied but includes an × close button for removal. Used for filters added via "More Filters." Example: "Trade **+128** ×"
- **Active/open (dropdown visible):** Same as applied but with a 1px solid teal border around the button.

When any filters are applied, a vertical divider line and a "Clear All" text link appear at the end of the bar.

### Results Tabs
Two tabs: "Projects" with a filled badge showing a count (e.g., 300), and "Companies" with a plain count (219). On the right side of this row: view toggle buttons for List, Map, and Show Preview, plus a column settings icon.

### Results Table
A data table with columns: checkbox, star/favorite, Project Name (blue link text, truncated with ellipsis, overflow menu icon), Project Value (right-aligned currency), Location (City, State), Stage, Bid Date (some rows empty), and Start Date. Populate with ~15 rows of realistic construction project data — project names like "2023 Concrete Cutting", values ranging from $300K to $11M, locations across US/Canada, stages like Conceptual/Design/Post-Bid.

### Pagination Footer
Right-aligned: "Rows per page" with a dropdown showing "100", a result count like "1-100 of 170538", and left/right page arrow buttons.

## What NOT to Build Yet
- No dropdown panels or menus
- No working search, filter, or sort logic
- No API calls or routing
- No responsive/mobile layout — desktop only
- No interactions — this is a visual shell

## Why This Matters
The filter bar is the testable surface for upcoming user research. Getting it visually accurate across all its states is the top priority. The surrounding page (nav, table, pagination) provides context so testers understand where the filter bar lives, but those elements just need to look reasonable, not perfect.


#  Azure DevOps: Queries & Dashboard Configuration

## 1. Prerequisites

- **Access level**: Project member with *Basic* (or Stakeholder in public projects)
- **Permissions**:
  - *Contribute* permission on Shared Queries folder
  - Dashboard editor or Project Admin permissions for adding widgets

---

## 2. Create & Save Queries

### 2.1 Flat‑List Query (required for charts)
- Navigate to **Boards → Queries** or **Work Items → New query**
- Select **Flat list of work items** → set clauses (e.g., `Assigned To = @Me`)
- Optionally add filters, groupings, numeric fields, etc.
- Click **Save → Save as…** into *Shared Queries* (for dashboards) or *My Queries*

### 2.2 Tree & Direct Links Queries
- Use **Tree of work items** for hierarchies (epic → features → tasks)
- Use **Work items and direct links** to track dependencies

---

## 3. Organize & Share Queries

- Use **My Queries** for personal; **Shared Queries** for team-wide
- Create folders, move, rename, set permissions
- Mark shared queries as *team favorites* if required

---

## 4. Create Query‑Based Charts

- Charts require **flat‑list queries** saved in Shared Queries
- From query’s **Charts** tab, click **New chart**
  - Select chart type: Pie, Column, Trend, Pivot
  - Choose grouping field (State, Assigned To, Iteration Path, etc.)
  - Optionally sum numeric fields (e.g. Story Points)
- Save the chart

---

## 5. Add to Dashboards

### 5.1 Query Tile Widget
- From **Queries**: click ⋯ → **Add to dashboard** → select dashboard
- Displays count of results; configurable color & rules

### 5.2 Query Results Widget
- In Dashboards → **Edit** → add **Query Results** widget
- Click ⚙️ → select shared query → displays live list in dashboard

### 5.3 Chart for Work Items Widget
- Add **Chart for Work Items** → configure it to show your saved chart
- Auto-populates based on saved flat‑query charts

### 5.4 Markdown Widget
- Optional: annotate dashboards with **Markdown widget**
- Supports headings, links, tables, lists (no scripts or attachments)
- Useful for team goals, links to backlogs, deadlines

---

## 6. Sample Use Cases

| Use Case                         | Query Type       | Dashboard Widget        | Purpose                              |
|----------------------------------|------------------|--------------------------|--------------------------------------|
| My active work                   | Flat‑list        | Query tile + results    | Quick access to personal work items |
| Bugs by state this sprint        | Flat‑list + chart| Chart widget (pie/chart)| Visual bug triage/status monitoring |
| Epic → Features → Tasks overview| Tree or direct links | Query results widget | Trace hierarchical work item structure |

---

## 7. Tips & Best Practices

- Always save showable charts in **Shared Queries**
- Group charts by supported fields (no free-text or date)
- Limit items to ≤ 1,000 for smoother trend charts
- Use query folders to stay organized—permissions are folder-based
- Use Markdown widget for context, instructions, or hyperlink to work items

---

## 8. Example `.md` Content

```markdown
# Team Dashboard Overview 

##  My Active Work
- A tile showing **items assigned to @Me**, plus list view.

##  Active Bugs By State
- A pie chart grouping bugs (State ≠ Closed).

##  Sprint Burndown
- A trend chart tracking remaining work (Sum of Remaining Work).

##  Backlog Hygiene
- Links to flat-query:
  - Unassigned work in current sprint
  - Work without defined Description/Story Points
- View list using *Query Results* widget.

---

###  Notes
- Use **Shared Queries** folder for all dashboard-linked queries.
- Edit dashboard → drag in **Chart for Work Items** or **Query Results** widgets.
- Use **Markdown** widget for goals & links (no scripts or file attachments).
```

---

##  Final Steps

1. Create the required **flat‑list queries** in Shared Queries.
2. Design charts on each query’s **Charts** tab.
3. Add **Query tile**, **Query Results**, and **Chart for Work Items** widgets to your dashboard.
4. Optionally add a **Markdown** widget for context and links.

With this setup, your team dashboard will provide up-to-date work item counts, charts, lists, and helpful guidance to stay on track.

# Use Work Items in Azure DevOps Pipelines

## Objective
Track and link **work items (user stories, tasks, bugs)** automatically when running builds or deploying code through Azure DevOps pipelines.

 Video Reference: [YouTube – Azure DevOps Pipeline Work Items](https://www.youtube.com/watch?v=xH5EY7FCFQw&t=77s)

---

## Benefits of Using Work Items in Pipelines
- Automatically link commits and pull requests to work items.
- Track which work items were deployed in each release.
- Improve traceability and auditability of code changes.

---

## Step-by-Step: Use Work Items in Pipelines

### Step 1: Ensure Proper Work Item Linking
1. Open your Azure DevOps project.
2. Use **#ID** of a work item in your commit messages or pull request description:
   ```bash
   git commit -m "Fix login issue #123"
   ```
   - `#123` links to Work Item ID 123.

---

### Step 2: Enable Work Item Linking in Pipeline Settings
1. Go to **Pipelines > Pipelines**.
2. Select and open your YAML or Classic build pipeline.
3. Click on the **pipeline name** and then **Edit**.
4. Go to **Options** (gear icon).
5. Enable **“Report work items”**.

---

### Step 3: View Linked Work Items After a Build
1. After running the pipeline, go to the **pipeline run summary**.
2. Scroll down to the **Related Work Items** section.
3. All work items referenced in commits or PRs are shown.

---

### Step 4: View Work Items in Releases (Optional)
1. Go to **Pipelines > Releases**.
2. Select a release and click on a stage.
3. Scroll to **Work Items** tab to see what was deployed.

---

## Tips for Better Tracking
- Use consistent work item references (e.g., `#321`, `Fixes AB#123`, `Resolves WI#456`)
- Mention multiple work items in one commit message if necessary.
- Enforce work item link policy via PR templates or commit linting.

---

## Example

Commit message:
```text
Added validation to form inputs #456 #457
```

Work Items:
- Bug #456: Fix field validation
- Task #457: Implement UX improvements

Pipeline run will show both work items automatically.

---

## Conclusion
By integrating work items into pipelines:
- Teams gain traceability from idea to production.
- Project tracking, reporting, and audits become easier.
- You ensure only approved or referenced work items are moved through environments.

---
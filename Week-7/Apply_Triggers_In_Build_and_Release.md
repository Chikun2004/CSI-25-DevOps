# Apply Triggers in Build and Release Pipelines – Azure DevOps

## Objective
Configure automated **build and release triggers** in Azure DevOps to run pipelines based on events such as code commits, scheduled times, or the completion of other pipelines.

## Prerequisites
- Azure DevOps project with a Git repository and at least one YAML or classic pipeline.
- Permissions to edit pipelines.

---

## Part A: Configure Triggers in Build Pipelines

### Step A1: Navigate to Pipelines
1. Go to https://dev.azure.com.
2. Select your organization and project.
3. From the left-hand menu, select **Pipelines > Pipelines**.

---

### Step A2: Edit the Build Pipeline
1. Click on the desired build pipeline.
2. Click **Edit** to open the YAML editor or pipeline designer.

---

### Step A3: Enable CI (Continuous Integration) Trigger

For YAML-based pipelines, add:

```yaml
trigger:
  branches:
    include:
      - main
      - feature/*
```

- This configuration will trigger the build on any push to `main` or any `feature/*` branch.

---

### Step A4: Add Path Filters (Optional)

```yaml
trigger:
  branches:
    include:
      - main
  paths:
    include:
      - src/*
    exclude:
      - docs/*
```

- This configuration triggers the build only when files in `src/` are changed and ignores changes in `docs/`.

---

### Step A5: Schedule Build Triggers (Optional)

```yaml
schedules:
  - cron: "0 3 * * 1-5"  # Every weekday at 3:00 AM UTC
    displayName: Daily weekday build
    branches:
      include:
        - main
    always: true
```

- This will trigger the build every weekday at 3:00 AM UTC, regardless of file changes.

---

## Part B: Configure Triggers in Release Pipelines

### Step B1: Navigate to Release Pipelines
1. Go to **Pipelines > Releases**.
2. Open or create a release pipeline.

---

### Step B2: Enable Artifact Trigger
1. Click on the **continuous deployment trigger** (lightning icon) on the artifact box.
2. Enable **“Continuous deployment trigger”**.
3. Select the source build pipeline.

- The release pipeline will now trigger automatically upon the successful completion of the selected build pipeline.

---

### Step B3: Enable Schedule Trigger (Optional)
1. Click on the **Stage** in the release pipeline.
2. Go to **Triggers** tab.
3. Enable **“Schedule release”**.
4. Set frequency, time zone, and days for automated release.

---

## Best Practices
- Use branch filters in CI triggers to avoid unnecessary builds.
- Use scheduled triggers for nightly or off-peak builds.
- Use artifact triggers in release pipelines for CI/CD automation.
- Always test pipeline behavior after modifying triggers.

---

## References
- Microsoft Docs: https://learn.microsoft.com/en-us/azure/devops/pipelines/build/triggers?view=azure-devops
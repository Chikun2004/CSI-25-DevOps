# Apply a Pull Request in Azure DevOps

## Objective
Create and complete a Pull Request (PR) in Azure DevOps to securely merge changes from a feature branch into the main branch, following a collaborative review process.

## Prerequisites
- An Azure DevOps project with at least two branches (`main` and a feature branch).
- Appropriate permissions to create and complete pull requests.

## Step 1: Open Azure DevOps Project
1. Go to https://dev.azure.com.
2. Select your organization and the target project.

## Step 2: Navigate to Repos > Branches
1. Select "Repos" from the left-hand menu.
2. Click "Branches".
3. Confirm the presence of both `main` and your feature branch (e.g., `feature/my-feature`).

## Step 3: Make and Push Changes to Feature Branch
1. Clone the repo or open it in your development environment.
2. Create and switch to a new branch:
   ```bash
   git checkout -b feature/my-feature
   ```
3. Make your code changes.
4. Stage and commit the changes:
   ```bash
   git add .
   git commit -m "Added feature XYZ"
   git push origin feature/my-feature
   ```

## Step 4: Create a Pull Request
1. In Azure DevOps, go to "Repos" > "Pull Requests".
2. Click "New Pull Request".
3. Set the following fields:
   - Source: `feature/my-feature`
   - Target: `main`
4. Add a title and a clear description of your changes.
5. Assign one or more reviewers.
6. Click "Create".

## Step 5: Review the Pull Request
- Reviewers will be notified.
- They can:
  - Leave comments
  - Approve the PR
  - Request changes

## Step 6: Complete the Pull Request
1. After approval, click "Complete".
2. Optional settings before completing:
   - Delete source branch after merging
   - Squash commits
3. Click "Complete Merge".

## Step 7: Sync Your Local Repository
```bash
git checkout main
git pull origin main
```

## Best Practices
- Always create pull requests from feature branches.
- Use descriptive PR titles and commit messages.
- Request reviews before merging into `main`.
- Enable auto-complete and policies where needed.

## Additional Resources
- Microsoft Docs: https://learn.microsoft.com/en-us/azure/devops/repos/git/pull-requests?view=azure-devops&tabs=browser
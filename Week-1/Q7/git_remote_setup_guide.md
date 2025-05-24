# 🚀 Git Remote Repository Setup and Workflow

## ✅ Prerequisites

Ensure you have the following installed:

- Git (`git --version`)
- GitHub or any remote Git provider account

---

## 1. 🗂️ Initialize Local Repository

```bash
mkdir my-project
cd my-project
git init
```

> This creates a new local Git repository.

---

## 2. 🌐 Create Remote Repository

Create a new repository on GitHub (e.g., `my-project`) **without** initializing with a README.

Then, link it to your local repository:

```bash
git remote add origin https://github.com/Chikun2004/CSI-25-DevOps.git
```

Verify the remote:

```bash
git remote -v
```

---

## 3. 📄 Create and Add a File

```bash
echo "# My Project" > README.md
```

Add the file to staging:

```bash
git add README.md
```

---

## 4. 💬 Commit the Changes

```bash
git commit -m "Initial commit with README"
```

---

## 5. ⬆️ Push to `master` Branch

If your default branch is `master`:

```bash
git push -u origin master
```

> ⚠️ If your default branch is `main`, replace `master` with `main`.

---

## 6. ✅ Final Check

Check status:

```bash
git status
```

Check log:

```bash
git log --oneline
```

---

## 📝 Summary

| Step | Command |
|------|---------|
| Initialize Repo | `git init` |
| Add Remote | `git remote add origin <repo-url>` |
| Create File | `echo "content" > file` |
| Stage File | `git add <file>` |
| Commit | `git commit -m "message"` |
| Push to Master | `git push -u origin master` |

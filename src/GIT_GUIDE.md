# Essential Git & GitHub Guide for Contributors

Welcome! If you are collaborating on this project or new to Git version control, this practical guide covers the fundamental concepts, day-to-day commands, and best practices.

---

## 1. Core Mental Model: The 3 Git Stages

Git tracks your files across three primary areas locally before syncing with GitHub:

```
+------------------+        +-------------------+        +--------------------+        +-----------------+
|  Working Tree    |  add   |   Staging Area    | commit |  Local Repository  |  push  |  GitHub Remote  |
| (Modified files) | -----> | (Index / Staged)  | -----> |   (.git folder)    | -----> | (origin/main)   |
+------------------+        +-------------------+        +--------------------+        +-----------------+
        ^                                                                                       |
        |                                       git pull / clone                                |
        +---------------------------------------------------------------------------------------+
```

1. **Working Tree**: The actual files and code you are editing in your folders.
2. **Staging Area (`Index`)**: A preparatory buffer where you select which modified files will be included in the next snapshot.
3. **Local Repository**: Permanent commit snapshots saved in your computer's local `.git` database.
4. **Remote (GitHub)**: The centralized online repository shared with other contributors.

---

## 2. Initial Setup (One-Time Configuration)

Before making your first commit, set your name and email so your contributions are properly credited:

```bash
git config --global user.name "Your Full Name"
git config --global user.email "your.email@example.com"

# Set default pull behavior to merge (avoids divergent branch errors)
git config --global pull.rebase false
```

---

## 3. Getting Started with a Repository

### Scenario A: Clone an Existing GitHub Repository
To download this repository to your local machine:
```bash
git clone https://github.com/Shadys-Garage/AGV-Robotic_Arm_for-quality_check.git
cd AGV-Robotic_Arm_for-quality_check
```

### Scenario B: Initialize a New Local Folder as a Git Repo
```bash
cd /path/to/your/project
git init -b main
git remote add origin https://github.com/Shadys-Garage/AGV-Robotic_Arm_for-quality_check.git
```

---

## 4. Daily Workflow: Make Changes, Stage, Commit, Push

Follow this sequence whenever you write or modify code:

### Step 1: Check Current Status
Always inspect what files have changed:
```bash
git status
```

### Step 2: Stage Modified Files
Choose which files or directories to include in your commit:
```bash
# Stage a specific file
git add src/my_package/node.py

# Stage an entire folder
git add src/my_package/

# Stage all tracked & new modified files
git add .
```

### Step 3: Commit with a Clear Message
Save your snapshot locally:
```bash
git commit -m "Add trajectory planning service for robotic arm"
```

### Step 4: Sync & Push to GitHub
Upload your local commits to the remote repository:
```bash
# First, pull any new changes made by others to prevent conflicts:
git pull origin main

# Push your commits:
git push origin main
```

---

## 5. Branching & Collaboration

Working directly on `main` is discouraged when collaborating. Use feature branches to isolate changes:

```bash
# 1. Create and switch to a new branch
git checkout -b feature/pointcloud-filtering

# 2. Make your edits and commit them
git add .
git commit -m "Implement statistical outlier removal filter"

# 3. Push your feature branch to GitHub
git push -u origin feature/pointcloud-filtering

# 4. Open a Pull Request (PR) on GitHub for review and merging.
```

To switch between existing branches:
```bash
# Switch to main branch
git checkout main

# Pull latest updates on main
git pull origin main

# Delete branch after it is merged
git branch -d feature/pointcloud-filtering
```

---

## 6. Understanding `git fetch` vs `git pull`

| Command | What It Does | Modifies Working Files? | Risk of Conflicts |
|---|---|---|---|
| `git fetch origin` | Downloads new commits from GitHub into background tracking branches (`origin/main`). | **No.** Local files remain unchanged. | **None.** Safe to inspect first. |
| `git pull origin main` | Runs `git fetch` followed immediately by `git merge`. | **Yes.** Updates your working directory. | **Yes**, if local and remote files conflict. |

```bash
# Formula:
git pull = git fetch + git merge
```

---

## 7. Common Troubleshooting & Fixes

### 1. `[rejected] - main -> main (fetch first)` or `non-fast-forward`
This happens when GitHub contains commits that are not yet on your local machine.
```bash
git pull origin main --no-edit
git push origin main
```

### 2. Nested `.git` Folder Issues (Submodule Glitch)
If you clone an external package into `src/` and it has its own `.git` folder, Git won't track its inner files properly:
```bash
# Remove the nested .git folder inside the subpackage
rm -rf src/my_package_folder/.git

# Stage the files properly
git add src/my_package_folder/
git commit -m "Track my_package_folder files directly"
```

### 3. Never Commit Build Artifacts (`.gitignore`)
Always ensure generated build, install, and log files are ignored:
```bash
# In your repository root, ensure .gitignore contains:
build/
install/
log/
*.bag
*.mcap
__pycache__/
*.pyc
```

### 4. Undo Uncommitted Local Changes
```bash
# Revert modifications in a specific file
git checkout -- path/to/file.py

# Unstage a file without losing edits
git restore --staged path/to/file.py
```

---

## 8. Git Command Quick Reference Card

| Command | Description |
|---|---|
| `git status` | Shows modified, untracked, and staged files |
| `git diff` | Shows line-by-line differences in uncommitted files |
| `git add <path>` | Stages files for the next commit |
| `git commit -m "msg"` | Records staged snapshot with a descriptive message |
| `git pull origin main` | Fetches and integrates remote changes |
| `git push origin main` | Pushes local commits to GitHub |
| `git log --oneline -n 10` | Displays commit history cleanly |
| `git branch -a` | Lists all local and remote branches |
| `git checkout -b <name>` | Creates and switches to a new branch |

---
*For issues or questions, consult project maintainers or refer to [official Git documentation](https://git-scm.com/doc).*

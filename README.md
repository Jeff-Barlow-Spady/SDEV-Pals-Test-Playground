# SDEV Pals Test Playground

Welcome to the **SDEV Pals Test Playground**! This repository is a friendly, collaborative sandbox for classmates to share study materials, lab notes, and practice real-world Git and GitHub workflows together.

---

## 📂 Repository Structure

```text
SDEV-Pals-Test-Playground/
├── Notes/
│   └── C#/
│       ├── HTML/        # Exported cheat sheets and HTML reference guides
│       └── Markdown/    # Raw, editable Markdown notes and lesson summaries
└── README.md            # Collaboration guide and repository documentation
```

> **Feel free to expand!** As we take more classes or topics, you can add new directories under `Notes/` (e.g., `Notes/SQL/`, `Notes/Web/`, or `Practice/`).

---

## 🛠️ First-Time Setup

Before you start collaborating, ensure your local environment is configured with Git.

### 1. Configure Git Identity
If you haven't already configured Git on your machine, open your terminal (PowerShell, Bash, or terminal of choice) and set your name and email matching your GitHub account:

```bash
git config --global user.name "Your Name"
git config --global user.email "your-email@example.com"
```

### 2. Clone the Repository
Clone the repository to your local machine:

```bash
git clone https://github.com/Jeff-Barlow-Spady/SDEV-Pals-Test-Playground.git
cd SDEV-Pals-Test-Playground
```

*(If you are contributing via a Fork rather than direct collaborator access, fork the repository on GitHub first, then clone your fork).*

---

## 🚀 The Complete Collaboration Workflow

Follow these steps every time you want to add notes, fix a typo, or contribute sample code.

```text
┌──────────────┐     ┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│  git pull    │ ──> │ git checkout │ ──> │ Make changes │ ──> │  git commit  │
│ (from main)  │     │  -b branch   │     │ & git status │     │  & git push  │
└──────────────┘     └──────────────┘     └──────────────┘     └──────────────┘
                                                                      │
┌──────────────┐     ┌──────────────┐     ┌──────────────┐            │
│ Local branch │ <── │  Merge PR    │ <── │ Review & PR  │ <──────────┘
│   cleanup    │     │  on GitHub   │     │  Discussion  │
└──────────────┘     └──────────────┘     └──────────────┘
```

### Step 1: Start Fresh from `main`
Always pull the latest changes from GitHub before creating a new branch to avoid outdated code:

```bash
git checkout main
git pull origin main
```

---

### Step 2: Create a Feature / Topic Branch
Never commit directly to the `main` branch. Always work in a dedicated branch:

```bash
git checkout -b <branch-name>
```

#### 💡 Branch Naming Conventions:
Use clear, descriptive branch names with a prefix:
- `notes/<topic>` — e.g., `notes/linq-aggregations`
- `feature/<name>-<topic>` — e.g., `feature/jeff-xunit-cheatsheet`
- `fix/<description>` — e.g., `fix/markdown-formatting-lab2`
- `practice/<name>` — e.g., `practice/alex-git-test`

---

### Step 3: Make Your Changes
Add or edit notes, sample code, or resources. Keep related changes grouped together.

Check what files have been changed or added:

```bash
git status
```

To see exact line-by-line edits:

```bash
git diff
```

---

### Step 4: Stage and Commit Your Work
Stage the files you want to include in your commit:

```bash
# Stage specific files (recommended)
git add Notes/C#/Markdown/Lesson-13-notes.md

# Or stage all modified/new files in the repository
git add .
```

Write a clear, descriptive commit message:

```bash
git commit -m "docs: add notes on LINQ query syntax and lambda methods"
```

> **Commit Message Tip:** Start with a type prefix such as:
> - `docs:` for documentation or notes updates
> - `feat:` for new files or practice features
> - `fix:` for fixing typos, links, or bugs
> - `refactor:` for reorganizing files or cleaning up content

---

### Step 5: Keep Your Branch Synchronized
If other classmates have merged work into `main` while you were editing, bring your branch up to date:

```bash
git checkout main
git pull origin main
git checkout <branch-name>
git merge main
```

*(Resolve any merge conflicts if prompted, stage the resolved files, and commit the merge).*

---

### Step 6: Push Your Branch to GitHub
Send your branch and commits up to GitHub:

```bash
git push -u origin <branch-name>
```

*(The `-u` flag sets the upstream tracking so future pushes on this branch only require `git push`).*

---

### Step 7: Open a Pull Request (PR)
1. Go to the repository on GitHub: [Jeff-Barlow-Spady/SDEV-Pals-Test-Playground](https://github.com/Jeff-Barlow-Spady/SDEV-Pals-Test-Playground).
2. You should see a yellow banner: **"<branch-name> had recent pushes"** with a green **"Compare & pull request"** button. Click it.
3. **Title:** Give your PR a concise summary (e.g., `Add LINQ aggregation notes`).
4. **Description:** Mention what you added, changed, or why it helps.
5. Under **Reviewers**, request a review from a classmate or tag them in the comments (`@username`).
6. Click **Create pull request**.

---

### Step 8: Peer Review and Updates
- Classmates can view your diffs, leave constructive comments, or approve the PR.
- **Need to make changes based on feedback?** No need to open a new PR! Just edit the files on your branch locally, commit, and push again:
  ```bash
  git add .
  git commit -m "docs: incorporate review feedback"
  git push
  ```
  The existing Pull Request will automatically update with your new commits.

---

### Step 9: Merge the Pull Request
Once approved:
1. Click **Merge pull request** (or **Squash and merge**).
2. Confirm the merge.
3. Click **Delete branch** on GitHub to keep the repository tidy.

---

### Step 10: Clean Up Your Local Environment
Switch back to your local `main` branch, download the newly merged changes, and delete your local branch:

```bash
git checkout main
git pull origin main
git branch -d <branch-name>
```

---

## 🧰 Useful Git Commands Cheat Sheet

| Command | What It Does |
|---|---|
| `git status` | Displays working tree status (staged, unstaged, untracked files) |
| `git log --oneline --graph` | Shows a clean, visual timeline of recent commits |
| `git branch -a` | Lists all local and remote branches |
| `git checkout -b <name>` | Creates and immediately switches to a new branch |
| `git restore <file>` | Discards local uncommitted changes to a file |
| `git restore --staged <file>` | Unstages a file (removes it from staging without losing changes) |
| `git stash` | Temporarily saves dirty working directory changes away |
| `git stash pop` | Reapplies stashed changes back into your working branch |

---

## ⚡ Handling Merge Conflicts Without Panic

Merge conflicts happen when two people edit the same lines of a file. They are normal and a great learning opportunity!

1. Git will pause and mark the conflict in the affected file:
   ```text
   <<<<<<< HEAD (Current change on main)
   Topic: Introduction to LINQ
   =======
   Topic: LINQ & Lambda Expressions
   >>>>>>> your-branch (Incoming change)
   ```
2. Open the file in your editor (e.g., VS Code).
3. Decide which version to keep (or combine both), and delete the conflict markers (`<<<<<<<`, `=======`, `>>>>>>>`).
4. Save the file.
5. Stage and finalize the resolution:
   ```bash
   git add <resolved-file>
   git commit -m "fix: resolve merge conflict between main and feature branch"
   git push
   ```

---

## 🌟 Collaboration Etiquette

- **🛡️ Safe Sandbox:** Don't worry about making mistakes—this playground is designed for experimenting and learning!
- **🚫 Protect `main`:** Avoid pushing directly to `main`; always use branches and Pull Requests.
- **💬 Kind & Supportive Reviews:** Provide helpful, encouraging feedback to peers.
- **📁 Organized Files:** Put notes and practice files in descriptive folders so others can easily find them.

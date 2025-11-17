# Git & SourceTree Hands-On Tutorial

Welcome! This tutorial will teach you Git using both **Command Line** and **SourceTree** (GUI).

## Prerequisites

1. **Install Git**: https://git-scm.com/downloads
2. **Install SourceTree**: https://www.sourcetreeapp.com/
3. **Project files**: You should have the Task Manager project ready

---

## Part 1: Getting Started

### Exercise 1.1: Initialize Git Repository

#### Using Command Line:
```bash
# Navigate to project folder
cd git-learning-project

# Initialize Git repository
git init

# Configure your identity (first time only)
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"

# Check status
git status
```

#### Using SourceTree:
1. Open SourceTree
2. Click **"Create"** (or File → New)
3. Click **"Create Local Repository"**
4. Browse to `git-learning-project` folder
5. Click **"Create"**
6. SourceTree opens with empty repository

**What you learned:** Initializing a Git repository

---

### Exercise 1.2: Your First Commit

#### Using Command Line:
```bash
# See what files are untracked
git status

# Stage all files
git add .

# Or stage individual files
git add index.html
git add styles.css
git add script.js
git add README.md
git add .gitignore

# Check status again (files should be green/staged)
git status

# Commit with message
git commit -m "Initial commit: Add task manager application"

# View commit history
git log
```

#### Using SourceTree:
1. Look at **"Workspace"** or **"File Status"** tab
2. You'll see all **Unstaged files** on the left
3. Select all files (or click checkbox at top)
4. Click **"Stage Selected"** (or drag to "Staged files" area)
5. Files move to **"Staged files"** section
6. At bottom, enter commit message: "Initial commit: Add task manager application"
7. Click **"Commit"** button
8. Switch to **"History"** tab to see your commit

**What you learned:** Staging files and creating commits

---

## Part 2: Making Changes

### Exercise 2.1: Modify Files and Commit

Let's improve the app by adding a task counter.

#### Task: Modify index.html

Add this line after the `<h1>Task Manager</h1>` line:
```html
<p id="taskCounter">Total tasks: 0</p>
```

#### Task: Update styles.css

Add this at the end:
```css
#taskCounter {
    text-align: center;
    color: #666;
    margin-bottom: 20px;
    font-size: 14px;
}
```

#### Task: Update script.js

Add this inside the `renderTasks()` function, after `taskList.innerHTML = '';`:
```javascript
    // Update task counter
    document.getElementById('taskCounter').textContent = `Total tasks: ${tasks.length}`;
```

#### Using Command Line:
```bash
# Check what changed
git status

# See detailed changes
git diff

# Stage changes
git add index.html styles.css script.js

# Or stage all
git add .

# Commit
git commit -m "Add task counter feature"

# View history
git log --oneline
```

#### Using SourceTree:
1. Look at **"File Status"** tab
2. You'll see 3 **Unstaged files** with a dot (●) indicating changes
3. Click on each file to see **diff** (changes highlighted)
4. Stage all 3 files
5. Enter commit message: "Add task counter feature"
6. Click **"Commit"**
7. View in **"History"** tab (you should see 2 commits now)

**What you learned:** Making changes, viewing diffs, and committing

---

## Part 3: Working with Branches

### Exercise 3.1: Create a Feature Branch

#### Using Command Line:
```bash
# Create and switch to new branch
git checkout -b feature/dark-mode

# Or do it in two steps
git branch feature/dark-mode
git checkout feature/dark-mode

# Verify current branch
git branch
# The * shows your current branch
```

#### Using SourceTree:
1. Click **"Branch"** button in toolbar
2. Enter branch name: `feature/dark-mode`
3. Make sure **"Checkout New Branch"** is checked
4. Click **"Create Branch"**
5. Look at left sidebar - you'll see the new branch highlighted
6. In **"History"** tab, notice the branch tag

**What you learned:** Creating and switching branches

---

### Exercise 3.2: Develop Feature on Branch

Let's add a dark mode toggle!

#### Task: Modify index.html

Add this after the `<h1>` tag:
```html
        <button id="darkModeToggle" onclick="toggleDarkMode()">🌙 Dark Mode</button>
```

#### Task: Update styles.css

Add at the end:
```css
#darkModeToggle {
    position: absolute;
    top: 20px;
    right: 20px;
    padding: 10px 15px;
    background-color: #333;
    color: white;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    font-size: 14px;
}

#darkModeToggle:hover {
    background-color: #555;
}

body.dark-mode {
    background-color: #1a1a1a;
}

body.dark-mode .container {
    background-color: #2d2d2d;
    color: #e0e0e0;
}

body.dark-mode h1 {
    color: #ffffff;
}

body.dark-mode #taskInput {
    background-color: #3d3d3d;
    border-color: #555;
    color: #e0e0e0;
}

body.dark-mode .task-item {
    background-color: #3d3d3d;
    color: #e0e0e0;
}
```

#### Task: Update script.js

Add at the end:
```javascript
// Dark mode toggle
function toggleDarkMode() {
    document.body.classList.toggle('dark-mode');
    const btn = document.getElementById('darkModeToggle');
    btn.textContent = document.body.classList.contains('dark-mode') ? '☀️ Light Mode' : '🌙 Dark Mode';
}
```

#### Using Command Line:
```bash
# Check you're on feature branch
git branch

# View changes
git diff

# Stage and commit
git add .
git commit -m "Add dark mode toggle feature"

# View branch history
git log --oneline --graph --all
```

#### Using SourceTree:
1. Verify you're on `feature/dark-mode` branch (check left sidebar)
2. Go to **"File Status"** tab
3. Review changes in diff viewer
4. Stage all files
5. Commit with message: "Add dark mode toggle feature"
6. In **"History"** tab, use the **graph view** to see branches

**What you learned:** Working on a feature branch

---

## Part 4: Merging Branches

### Exercise 4.1: Merge Feature into Main

#### Using Command Line:
```bash
# Switch to main branch
git checkout main

# Verify you're on main
git branch

# Merge feature branch
git merge feature/dark-mode

# View combined history
git log --oneline --graph

# Optional: Delete feature branch after merge
git branch -d feature/dark-mode
```

#### Using SourceTree:
1. **Switch to main**: Double-click `main` in left sidebar (or right-click → Checkout)
2. Right-click on `feature/dark-mode` branch in left sidebar
3. Select **"Merge feature/dark-mode into main"**
4. In dialog, click **"OK"**
5. View **"History"** - you should see merge
6. Optional: Right-click `feature/dark-mode` → **"Delete feature/dark-mode"**

**What you learned:** Merging branches

---

## Part 5: Handling Merge Conflicts

### Exercise 5.1: Create a Conflict (Intentionally)

#### Using Command Line:
```bash
# Create and switch to new branch
git checkout -b feature/priority-tasks

# Modify index.html - change the h1 title
# Find: <h1>Task Manager</h1>
# Replace with: <h1>Priority Task Manager</h1>

# Commit on feature branch
git add index.html
git commit -m "Add priority to title"

# Switch back to main
git checkout main

# Modify the SAME line differently
# Find: <h1>Task Manager</h1>
# Replace with: <h1>My Personal Task Manager</h1>

# Commit on main
git add index.html
git commit -m "Personalize title"

# Now try to merge - this will create a conflict!
git merge feature/priority-tasks
# You'll see: CONFLICT (content): Merge conflict in index.html
```

#### Using SourceTree:
1. Create branch `feature/priority-tasks` and check it out
2. Edit `index.html`: Change `<h1>Task Manager</h1>` to `<h1>Priority Task Manager</h1>`
3. Stage and commit: "Add priority to title"
4. Switch to `main` branch
5. Edit `index.html`: Change same line to `<h1>My Personal Task Manager</h1>`
6. Stage and commit: "Personalize title"
7. Try to merge `feature/priority-tasks` into `main`
8. You'll see **conflict warning**!

**What you learned:** Creating a merge conflict

---

### Exercise 5.2: Resolve the Conflict

#### Using Command Line:
```bash
# Check status
git status
# Shows: both modified: index.html

# Open index.html in text editor
# You'll see conflict markers:
# <<<<<<< HEAD
#     <h1>My Personal Task Manager</h1>
# =======
#     <h1>Priority Task Manager</h1>
# >>>>>>> feature/priority-tasks

# Decide what to keep. Let's combine both:
# Replace conflict section with:
#     <h1>My Priority Task Manager</h1>

# Remove all conflict markers (<<<<, ====, >>>>)

# Stage resolved file
git add index.html

# Complete the merge
git commit -m "Merge feature/priority-tasks: Combine both titles"

# View result
git log --oneline --graph
```

#### Using SourceTree:
1. After conflict appears, go to **"File Status"** tab
2. You'll see file with **conflict indicator** (⚠️)
3. Right-click conflicted file
4. Choose **"Resolve Conflicts"** → **"Open External Merge Tool"**
   (Or just open file in text editor)
5. You'll see conflict markers in the file
6. Edit to resolve: `<h1>My Priority Task Manager</h1>`
7. Remove conflict markers
8. Save file
9. In SourceTree, right-click file → **"Mark Resolved"**
10. File moves to **"Staged files"**
11. Enter commit message: "Merge feature/priority-tasks: Combine both titles"
12. Click **"Commit"**

**What you learned:** Resolving merge conflicts

---

## Part 6: Viewing History and Differences

### Exercise 6.1: Explore History

#### Using Command Line:
```bash
# Basic log
git log

# Compact log
git log --oneline

# Graph view
git log --oneline --graph --all

# Show specific number of commits
git log -n 3

# Show commits by author
git log --author="Your Name"

# Show commits with files changed
git log --stat

# Search commits
git log --grep="dark mode"
```

#### Using SourceTree:
1. Go to **"History"** tab
2. View **graph view** on left (shows branches visually)
3. Click any commit to see:
   - Commit message
   - Author and date
   - Files changed (diff)
4. Right-click commit for options:
   - **"Checkout"** - go to that commit
   - **"Reset current branch to this commit"**
   - **"Reverse commit"** - undo changes
5. Use **Search** box at top right
6. Try different view options (toolbar buttons)

**What you learned:** Exploring repository history

---

### Exercise 6.2: Compare Changes

#### Using Command Line:
```bash
# Show unstaged changes
git diff

# Show staged changes
git diff --staged

# Compare two commits
git log --oneline  # Get commit hashes
git diff <commit1-hash> <commit2-hash>

# Compare branches
git diff main feature/dark-mode

# Show changes in specific file
git diff index.html
```

#### Using SourceTree:
1. **Unstaged changes**: Select file in "File Status" → view diff
2. **Between commits**: 
   - Select first commit in History
   - Right-click second commit → **"Diff against Current"**
3. **Between branches**:
   - History tab → **"Search"** → Select two branches to compare
4. **For specific file**:
   - Click file in any commit → see diff

**What you learned:** Comparing different versions

---

## Part 7: Undoing Changes

### Exercise 7.1: Undo Uncommitted Changes

#### Scenario: Made changes but want to discard them

#### Using Command Line:
```bash
# Make some changes to script.js (don't commit)

# Discard changes in specific file
git checkout -- script.js

# Or using newer command
git restore script.js

# Discard ALL changes
git checkout -- .
```

#### Using SourceTree:
1. Make changes to a file
2. In **"File Status"** tab, right-click the file
3. Select **"Discard"** or **"Reset"**
4. Confirm - changes are gone!

---

### Exercise 7.2: Undo Last Commit (Keep Changes)

#### Using Command Line:
```bash
# Undo last commit but keep changes
git reset --soft HEAD~1

# Changes are now unstaged
git status

# Can modify and re-commit
```

#### Using SourceTree:
1. Go to **"History"** tab
2. Right-click the commit BEFORE the one you want to undo
3. Select **"Reset current branch to this commit"**
4. Choose **"Soft"** mode
5. Changes appear as unstaged

---

### Exercise 7.3: Undo Last Commit (Discard Changes)

#### Using Command Line:
```bash
# Undo last commit AND discard changes
git reset --hard HEAD~1

# Everything gone - be careful!
```

#### Using SourceTree:
1. Right-click commit before the one to undo
2. **"Reset current branch to this commit"**
3. Choose **"Hard"** mode
4. ⚠️ Changes permanently deleted!

**What you learned:** Undoing mistakes

---

## Part 8: Working with Remote Repositories

### Overview of Remote Operations

**Remote repository** = Server copy of your code (GitHub, GitLab, Azure DevOps, Bitbucket)

**Key operations:**
- **Push**: Upload your commits to remote
- **Pull**: Download and merge remote changes
- **Fetch**: Download remote changes (don't merge yet)
- **Clone**: Copy entire remote repository

**Why use remotes?**
- ✅ Backup your work
- ✅ Collaborate with team
- ✅ Share code publicly
- ✅ Work from multiple computers
- ✅ Continuous integration/deployment

---

### Exercise 8.1: Create Remote Repository

You'll need an account on one of these platforms:

**Options:**
1. **GitHub** (most popular): https://github.com
2. **Azure DevOps** (enterprise): https://dev.azure.com
3. **GitLab**: https://gitlab.com
4. **Bitbucket**: https://bitbucket.org

#### Steps to create remote repository:

**On GitHub:**
1. Log in to GitHub
2. Click **"+"** (top right) → **"New repository"**
3. Name: `task-manager` (or any name)
4. Description: "Learning Git with task manager app"
5. **Public** or **Private** (your choice)
6. **DON'T** check "Initialize with README" (we already have code)
7. Click **"Create repository"**
8. Copy the **HTTPS URL** (looks like: `https://github.com/username/task-manager.git`)

**On Azure DevOps:**
1. Go to your organization
2. Create new project: "TaskManager"
3. Go to **Repos**
4. You'll see an empty repo
5. Copy the **Clone URL**

---

### Exercise 8.2: Connect Local to Remote

Now let's link your local repository to the remote.

#### Using Command Line:
```bash
# Make sure you're in your project folder
cd git-learning-project

# Add remote (replace URL with yours!)
git remote add origin https://github.com/YOUR-USERNAME/task-manager.git

# Verify remote was added
git remote -v
# Should show:
# origin  https://github.com/YOUR-USERNAME/task-manager.git (fetch)
# origin  https://github.com/YOUR-USERNAME/task-manager.git (push)

# View more details
git remote show origin
```

#### Using SourceTree:
1. Make sure your repository is open
2. Click **"Settings"** button (gear icon) or **Repository** → **Repository Settings**
3. Click **"Remotes"** tab
4. Click **"Add"**
5. **Remote name**: `origin` (this is standard)
6. **URL/Path**: Paste your repository URL
7. Click **"OK"**
8. Click **"OK"** again to close settings

**Verify:** Look in left sidebar under "REMOTES" - you should see `origin`

**What you learned:** Connecting local repository to remote server

---

### Exercise 8.3: Your First Push

Let's upload your code to the remote!

#### Using Command Line:
```bash
# Check current branch
git branch
# Make sure you're on 'main'

# Push to remote (-u sets upstream)
git push -u origin main

# Enter credentials if prompted
# (Username and password or personal access token)

# See the output - it uploads your commits!
```

**Explanation of flags:**
- `git push` = Upload commits
- `origin` = Remote name (where to push)
- `main` = Branch name (what to push)
- `-u` = Set upstream (links local main to remote main)

**After first push with `-u`, you can just use:**
```bash
git push  # Automatically pushes to origin/main
```

#### Using SourceTree:
1. Click **"Push"** button in toolbar (up arrow icon)
2. A dialog appears showing branches
3. Make sure **"main"** is checked
4. Check **"Track"** option (same as -u flag)
5. Click **"Push"**
6. Enter credentials if prompted
7. Wait for upload to complete

**Success!** Go to your GitHub/Azure DevOps page and refresh - your code is there! 🎉

**What you learned:** Pushing code to remote repository

---

### Exercise 8.4: Push Additional Branches

Let's push your other branches too.

#### Using Command Line:
```bash
# List your local branches
git branch

# Push specific branch
git push origin feature/dark-mode

# Or push ALL branches at once
git push --all origin

# View all branches (local and remote)
git branch -a
# Remote branches shown in red as: remotes/origin/main
```

#### Using SourceTree:
1. Click **"Push"** button
2. Check all branches you want to push
3. Click **"Push"**

**View in web interface:**
- Go to your repository on GitHub
- Click **"Branches"** dropdown (usually shows "main")
- You'll see all your pushed branches!

**What you learned:** Pushing multiple branches

---

### Exercise 8.5: Clone Repository (Simulate Teammate)

Let's simulate getting the code on another computer or another teammate cloning your repo.

#### Create a test folder:
```bash
# Go to a different location (not your project folder!)
cd ~/Desktop  # or any other location

# Create test folder
mkdir git-test
cd git-test
```

#### Using Command Line:
```bash
# Clone your repository (replace URL)
git clone https://github.com/YOUR-USERNAME/task-manager.git

# This creates a folder with the repo name
cd task-manager

# Look around
ls
# You have all your files!

# Check branches
git branch
# Shows: * main

# Check remote branches
git branch -r
# Shows: origin/main, origin/feature/dark-mode, etc.

# View remote info
git remote -v
# Already configured to your repo!
```

#### Using SourceTree:
1. Click **"Clone"** button (or File → Clone)
2. **Source URL**: Your repository URL
3. **Destination Path**: Where to save (e.g., ~/Desktop/task-manager-clone)
4. **Name**: Leave as default
5. Click **"Clone"**
6. SourceTree opens the cloned repository

**What you learned:** Cloning creates a complete copy with all history and remote configured

---

### Exercise 8.6: Understanding Fetch vs Pull

**Important distinction:**

```
FETCH = Download remote changes (don't merge)
PULL = Fetch + Merge (download and merge in one step)
```

Let's learn by doing!

#### Setup: Make changes on GitHub

1. Go to your repository on GitHub
2. Click on **"README.md"** (or any file)
3. Click **pencil icon** (Edit)
4. Add a line: `## Remote Edit`
5. Scroll down, add commit message: "Edit from GitHub"
6. Click **"Commit changes"**

**Now your remote has a commit that your local doesn't have!**

#### Using Command Line - Fetch:
```bash
# Go back to your ORIGINAL project folder (not the clone)
cd ~/git-learning-project

# Fetch changes (download but don't merge)
git fetch origin

# View what was fetched
git log --oneline --all --graph

# You'll see something like:
# * a1b2c3d (origin/main) Edit from GitHub
# * d4e5f6g (HEAD -> main) Your local commit

# Check difference between local and remote
git diff main origin/main

# Your local files haven't changed yet!
cat README.md  # Doesn't show "## Remote Edit"

# Now merge the fetched changes
git merge origin/main

# Or checkout to see them
git checkout main
cat README.md  # Now shows "## Remote Edit"!
```

#### Using Command Line - Pull (shortcut):
```bash
# Pull = Fetch + Merge in one command
git pull origin main

# Or just:
git pull  # Uses tracked branch
```

#### Using SourceTree - Fetch:
1. Click **"Fetch"** button in toolbar
2. Select **origin**
3. Click **"OK"**
4. Go to **History** tab
5. You'll see remote changes (marked with origin/main)
6. To merge: Right-click the commit → **"Merge origin/main into main"**

#### Using SourceTree - Pull:
1. Click **"Pull"** button in toolbar
2. Select **origin** and **main**
3. Click **"OK"**
4. Changes fetched and merged automatically!

**What you learned:** 
- Fetch is safe (lets you review before merging)
- Pull is convenient (fetch + merge in one step)
- Use Fetch when cautious, Pull when confident

---

### Exercise 8.7: Collaboration Workflow

Let's simulate a full collaboration cycle!

#### Scenario: You and a teammate both working

**Step 1: Make changes locally**
```bash
# Create new branch
git checkout -b feature/priority-levels

# Edit script.js - add priority field to tasks
# (You can add a simple priority dropdown)

# Commit
git add script.js
git commit -m "Add task priority levels"

# Push your branch
git push -u origin feature/priority-levels
```

**Step 2: Simulate teammate's work**

Let's use your cloned repo (from Exercise 8.5) to simulate a teammate:

```bash
# Go to cloned repo (simulating teammate's computer)
cd ~/Desktop/task-manager  # or wherever you cloned

# Make sure we have latest
git pull origin main

# Create different feature
git checkout -b feature/due-dates

# Edit script.js - add due date field
# (Just add a comment or simple change)

# Commit
git add script.js
git commit -m "Add task due dates"

# Push
git push -u origin feature/due-dates
```

**Step 3: Back to your original repo**

```bash
# Go back to original project
cd ~/git-learning-project

# Fetch to see teammate's work
git fetch origin

# List all branches (including remote)
git branch -a
# You'll see: remotes/origin/feature/due-dates

# Check out teammate's branch to review
git checkout feature/due-dates
# Look at their changes

# Go back to main
git checkout main
```

**Step 4: Merge both features**

```bash
# Merge your feature
git merge feature/priority-levels

# Fetch and merge teammate's feature
git fetch origin
git merge origin/feature/due-dates

# If conflicts, resolve them!

# Push merged result
git push origin main
```

#### Using SourceTree for Collaboration:

1. **Pull regularly**: Click Pull button to get updates
2. **Fetch often**: Click Fetch to see what teammates pushed
3. **Review changes**: Click remote branches to see what changed
4. **Merge when ready**: Right-click branch → Merge

**What you learned:** Complete collaboration workflow

---

### Exercise 8.8: Handling Remote Conflicts

Sometimes you and a teammate edit the same file. Let's practice!

#### Create conflict:

**On GitHub (or your clone):**
1. Edit index.html
2. Change the h1 to: `<h1>Team Task Manager</h1>`
3. Commit: "Update title on remote"

**Locally:**
```bash
# Don't pull yet!
# Edit index.html locally
# Change h1 to: <h1>Personal Task Manager</h1>

git add index.html
git commit -m "Update title locally"

# Try to push - will fail!
git push origin main
# Error: "Updates were rejected because the remote contains work..."
```

#### Resolve:

```bash
# Pull first to get remote changes
git pull origin main
# CONFLICT in index.html!

# Open index.html
# You'll see:
# <<<<<<< HEAD
# <h1>Personal Task Manager</h1>
# =======
# <h1>Team Task Manager</h1>
# >>>>>>> origin/main

# Resolve - choose or combine
# Example: <h1>Personal Team Task Manager</h1>

# Remove conflict markers
# Stage resolved file
git add index.html

# Complete the merge
git commit -m "Merge remote changes, resolve title conflict"

# Now push works!
git push origin main
```

#### Using SourceTree:
1. Try to push - get error about remote changes
2. Click **Pull** to fetch and merge
3. Conflict appears in File Status
4. Right-click file → Resolve Conflicts → Open External Merge Tool
5. Edit file to resolve
6. Right-click → Mark Resolved
7. Commit the merge
8. Push successfully

**What you learned:** Always pull before push to avoid conflicts

---

### Exercise 8.9: Advanced Remote Commands

#### View Remote Information:
```bash
# List remotes
git remote -v

# Detailed info about remote
git remote show origin

# List remote branches
git branch -r

# List all branches (local + remote)
git branch -a
```

#### Rename Remote:
```bash
# Rename remote (rarely needed)
git remote rename origin upstream
git remote -v  # Verify
```

#### Change Remote URL:
```bash
# If you need to change URL (e.g., moved repo)
git remote set-url origin https://new-url.git
git remote -v  # Verify
```

#### Remove Remote:
```bash
# Remove remote connection
git remote remove origin
git remote -v  # Verify it's gone
```

#### Prune Remote Branches:
```bash
# Remove references to deleted remote branches
git fetch --prune origin

# Or set it to prune automatically
git config --global fetch.prune true
```

#### Push Specific Commit:
```bash
# Push up to specific commit
git push origin <commit-hash>:main
```

#### Force Push (DANGEROUS!):
```bash
# Force push (overwrites remote - be careful!)
git push --force origin main

# Safer: Only force if no one else pushed
git push --force-with-lease origin main
```

**⚠️ Warning:** Only force push if you're SURE no one else has the code!

---

### Exercise 8.10: Tracking Branches

#### Understand Tracking:

When you push with `-u`, you set up **tracking**:
```bash
git push -u origin main
# Now 'main' tracks 'origin/main'

# Check tracking
git branch -vv
# Shows: main [origin/main] Your commit message
```

**Benefits:**
- Can use just `git push` (no need for `git push origin main`)
- Can use just `git pull`
- Git shows if you're ahead/behind remote

#### Set Tracking for Existing Branch:
```bash
git branch --set-upstream-to=origin/main main
```

#### Check Status vs Remote:
```bash
git status
# Shows: "Your branch is ahead of 'origin/main' by 2 commits"
# Or: "Your branch is behind 'origin/main' by 1 commit"
```

---

### Exercise 8.11: Working with Multiple Remotes

Sometimes you need multiple remotes (e.g., original repo + your fork).

```bash
# Add second remote
git remote add upstream https://github.com/original/repo.git

# View all remotes
git remote -v
# origin    https://github.com/you/repo.git (fetch)
# origin    https://github.com/you/repo.git (push)
# upstream  https://github.com/original/repo.git (fetch)
# upstream  https://github.com/original/repo.git (push)

# Fetch from specific remote
git fetch upstream

# Merge from upstream
git merge upstream/main

# Push to your fork
git push origin main
```

**Common scenario:** Contributing to open source
- `upstream` = Original project
- `origin` = Your fork

---

### Exercise 8.12: Remote Best Practices

#### Before Starting Work:
```bash
git checkout main
git pull origin main
git checkout -b feature/new-work
```

#### During Work:
```bash
# Commit often locally
git add .
git commit -m "Progress on feature"

# Push to backup (optional)
git push origin feature/new-work

# Pull main regularly to stay updated
git checkout main
git pull origin main
git checkout feature/new-work
git merge main  # Or rebase
```

#### Before Creating Pull Request:
```bash
# Update with latest main
git checkout main
git pull origin main
git checkout feature/new-work
git merge main

# Resolve conflicts if any

# Push final version
git push origin feature/new-work

# Create PR on GitHub/Azure DevOps
```

#### After PR Merged:
```bash
# Update local main
git checkout main
git pull origin main

# Delete local feature branch
git branch -d feature/new-work

# Delete remote feature branch
git push origin --delete feature/new-work
```

---

### Common Remote Workflows

#### Workflow 1: Solo Developer
```bash
# Start of day
git pull origin main

# Work
git add .
git commit -m "Add feature"

# End of day
git push origin main
```

#### Workflow 2: Team with Feature Branches
```bash
# Start feature
git checkout main
git pull origin main
git checkout -b feature/new-feature

# Work and commit
git add .
git commit -m "Part of feature"
git push origin feature/new-feature

# Update from main regularly
git checkout main
git pull origin main
git checkout feature/new-feature
git merge main

# When done, create PR on web
# After PR merged
git checkout main
git pull origin main
git branch -d feature/new-feature
```

#### Workflow 3: Fork and Pull Request
```bash
# Fork on GitHub
# Clone your fork
git clone https://github.com/you/repo.git

# Add original as upstream
git remote add upstream https://github.com/original/repo.git

# Create feature branch
git checkout -b feature/contribution

# Work and commit
git add .
git commit -m "Add contribution"

# Push to your fork
git push origin feature/contribution

# Create PR from your fork to original repo

# Keep fork updated
git fetch upstream
git checkout main
git merge upstream/main
git push origin main
```

---

## Remote Troubleshooting

### "Updates were rejected"
**Problem:** Remote has commits you don't have

**Solution:**
```bash
git pull origin main  # Get remote changes
# Resolve any conflicts
git push origin main
```

### "Permission denied"
**Problem:** No access or wrong credentials

**Solution:**
- Verify you have push access
- Check credentials (username/password or token)
- For GitHub: Use personal access token, not password
- For SSH: Set up SSH keys

### "Repository not found"
**Problem:** URL wrong or no access

**Solution:**
```bash
# Check remote URL
git remote -v

# Update if wrong
git remote set-url origin <correct-url>
```

### Push Taking Forever
**Problem:** Pushing large files

**Solution:**
- Add large files to .gitignore
- Use Git LFS for large binary files
- Check what's being pushed: `git diff origin/main..main --stat`

### Accidentally Pushed Wrong Code
**Solution 1 (if no one pulled yet):**
```bash
# Fix locally
git reset --hard HEAD~1
git push --force origin main
```

**Solution 2 (if others might have pulled):**
```bash
# Create new commit to fix
git revert HEAD
git push origin main
```

---

## Remote Quick Reference

```bash
# Setup
git remote add origin <url>         # Add remote
git remote -v                        # List remotes
git remote show origin               # Detailed info

# Getting code
git clone <url>                      # Clone repo
git fetch origin                     # Download changes
git pull origin main                 # Fetch + merge

# Sending code
git push origin main                 # Push branch
git push -u origin main              # Push and set tracking
git push --all origin                # Push all branches

# Branches
git push origin branch-name          # Push specific branch
git push origin --delete branch      # Delete remote branch
git branch -r                        # List remote branches
git branch -a                        # List all branches

# Comparing
git diff main origin/main            # Local vs remote
git log origin/main..main            # Commits not pushed
git log main..origin/main            # Commits not pulled

# Updating
git fetch --prune origin             # Remove stale references
git remote update                    # Fetch all remotes
```

**What you learned:** Complete remote repository workflows including push, fetch, pull, and collaboration!

---

## Part 9: Working with Submodules

### What Are Submodules?

A **submodule** is a Git repository embedded inside another Git repository. Think of it as a "repository within a repository."

**Real-world analogy:** Your main project is like a house, and submodules are like rooms that are actually separate apartments that you're renting and including in your house.

**Why use submodules?**
- ✅ Include external libraries/dependencies
- ✅ Share code between multiple projects
- ✅ Keep third-party code separate
- ✅ Version control for dependencies
- ✅ Maintain clean separation of concerns

**Common use cases:**
- Including a shared UI component library
- Adding a third-party authentication module
- Using common utility functions across projects
- Managing vendor dependencies

---

### Exercise 9.1: Understanding Submodules

Let's understand what submodules solve with an example.

**Scenario WITHOUT submodules:**
```
Your Project/
├── index.html
├── styles.css
├── script.js
└── shared-utils/
    ├── helpers.js       # Copied from another project
    ├── validators.js    # Copied from another project
    └── formatters.js    # Copied from another project
    
Problem: shared-utils gets updated in other project
→ You need to manually copy files again
→ No version tracking
→ Easy to get out of sync
```

**Scenario WITH submodules:**
```
Your Project/
├── index.html
├── styles.css
├── script.js
└── shared-utils/       ← Git submodule (linked to other repo)
    ├── .git/           (Has its own Git history)
    ├── helpers.js
    ├── validators.js
    └── formatters.js
    
Benefit: shared-utils is a link to another Git repo
→ Update with `git submodule update`
→ Version controlled
→ Always in sync
```

---

### Exercise 9.2: Create a Shared Library (Setup)

First, let's create a "shared library" that we'll use as a submodule.

**Step 1: Create shared library repository**

```bash
# Navigate to a different location (not your project!)
cd ~/Desktop

# Create shared library folder
mkdir git-shared-utils
cd git-shared-utils

# Initialize as Git repository
git init

# Create some utility files
cat > math-utils.js << 'EOF'
// Math utility functions
export function add(a, b) {
    return a + b;
}

export function multiply(a, b) {
    return a * b;
}

export function calculateAverage(numbers) {
    const sum = numbers.reduce((acc, num) => acc + num, 0);
    return sum / numbers.length;
}
EOF

cat > string-utils.js << 'EOF'
// String utility functions
export function capitalize(str) {
    return str.charAt(0).toUpperCase() + str.slice(1);
}

export function truncate(str, maxLength) {
    return str.length > maxLength ? str.slice(0, maxLength) + '...' : str;
}
EOF

cat > README.md << 'EOF'
# Shared Utilities

Common utility functions used across multiple projects.

## Features
- Math utilities
- String utilities
EOF

# Commit
git add .
git commit -m "Initial commit: Add utility functions"

# View the commit
git log --oneline
```

**Step 2: Push to remote (optional but recommended)**

```bash
# Create repo on GitHub named "git-shared-utils"
# Then push
git remote add origin https://github.com/YOUR-USERNAME/git-shared-utils.git
git push -u origin main
```

**If you skip GitHub:** You can still use the local path in the next exercise.

**What you learned:** Created a separate repository to use as a submodule

---

### Exercise 9.3: Add Submodule to Your Project

Now let's add the shared library as a submodule to your task manager project.

#### Using Command Line:

```bash
# Go back to your main project
cd ~/git-learning-project

# Add submodule (use GitHub URL or local path)
# Option 1: With GitHub
git submodule add https://github.com/YOUR-USERNAME/git-shared-utils.git libs/shared-utils

# Option 2: With local path (if not on GitHub)
git submodule add ~/Desktop/git-shared-utils libs/shared-utils

# What happened:
# 1. Created libs/shared-utils folder
# 2. Cloned the shared-utils repo into it
# 3. Created .gitmodules file
# 4. Registered submodule in your project

# View the .gitmodules file
cat .gitmodules
# Shows:
# [submodule "libs/shared-utils"]
#     path = libs/shared-utils
#     url = https://github.com/YOUR-USERNAME/git-shared-utils.git

# Check status
git status
# Shows:
# new file:   .gitmodules
# new file:   libs/shared-utils

# Look at submodule folder
ls libs/shared-utils/
# Shows: math-utils.js, string-utils.js, README.md

# Commit the submodule addition
git add .
git commit -m "Add shared-utils as submodule"
```

#### Using SourceTree:

1. Repository → Add Submodule
2. **Source Path/URL**: Enter GitHub URL or local path
3. **Local Relative Path**: `libs/shared-utils`
4. Click **OK**
5. SourceTree clones the submodule
6. Commit the changes (.gitmodules and new folder)

**What you learned:** Adding a submodule to your project

---

### Exercise 9.4: Clone Repository with Submodules

Let's see what happens when someone else clones your project.

**Step 1: Clone your project (simulating new team member)**

```bash
# Go to different location
cd ~/Desktop

# Clone your project
git clone ~/git-learning-project task-manager-clone
cd task-manager-clone

# Check the submodule folder
ls libs/shared-utils/
# It's EMPTY! 😱

# Why? Submodules are not cloned by default
```

**Step 2: Initialize and update submodules**

```bash
# Initialize submodule configuration
git submodule init

# Download submodule content
git submodule update

# Now check again
ls libs/shared-utils/
# Now you see: math-utils.js, string-utils.js, README.md ✓

# Or do both in one command:
git submodule update --init --recursive
```

**Shortcut when cloning:**

```bash
# Clone with submodules in one step
git clone --recursive ~/git-learning-project task-manager-clone2

# This automatically initializes and updates all submodules
```

#### Using SourceTree:

When opening a repository with submodules:
1. SourceTree shows warning: "Submodules not initialized"
2. Click **Initialize Submodules**
3. Click **Update Submodules**
4. Or: Repository → Update Submodules

**What you learned:** Cloning repositories that contain submodules

---

### Exercise 9.5: View Submodule Information

Let's explore submodule commands.

```bash
# List all submodules
git submodule

# Shows:
# <commit-hash> libs/shared-utils (heads/main)

# Detailed status
git submodule status

# Shows each submodule with:
# - Current commit
# - Path
# - Branch info

# Check submodule configuration
cat .gitmodules

# View submodule in Git config
git config --list | grep submodule

# Summary of submodules
git submodule summary
```

---

### Exercise 9.6: Update Submodule Content

What if the shared library gets updated? Let's practice updating.

**Scenario: Shared library maintainer adds new features**

**Step 1: Update the shared library (simulate library update)**

```bash
# Go to original shared library
cd ~/Desktop/git-shared-utils

# Add new feature
cat > date-utils.js << 'EOF'
// Date utility functions
export function formatDate(date) {
    return date.toLocaleDateString();
}

export function isWeekend(date) {
    const day = date.getDay();
    return day === 0 || day === 6;
}
EOF

# Commit
git add date-utils.js
git commit -m "Add date utilities"

# If using GitHub, push
git push origin main
```

**Step 2: Update submodule in your project**

```bash
# Go back to your main project
cd ~/git-learning-project

# Enter the submodule directory
cd libs/shared-utils

# This is a real Git repository!
git status
git branch
git log --oneline

# Pull latest changes
git pull origin main

# You now have the new date-utils.js!
ls
# Shows: math-utils.js, string-utils.js, date-utils.js, README.md

# Go back to main project
cd ../..

# Check status
git status
# Shows: modified:   libs/shared-utils (new commits)

# Commit the submodule update
git add libs/shared-utils
git commit -m "Update shared-utils submodule to include date utilities"
```

**Shortcut - Update all submodules from main project:**

```bash
# Update all submodules to latest
git submodule update --remote

# Update and merge
git submodule update --remote --merge

# Then commit
git add .
git commit -m "Update submodules to latest versions"
```

#### Using SourceTree:

1. Repository → Update Submodules
2. Check **Update to Remote Tracking Branch**
3. Click **OK**
4. Commit the update

**What you learned:** Updating submodules to get latest changes

---

### Exercise 9.7: Work Inside a Submodule

You can make changes directly in the submodule!

```bash
# Enter submodule
cd libs/shared-utils

# You're in a real Git repo
git status
git branch

# Create a branch
git checkout -b feature/color-utils

# Add new file
cat > color-utils.js << 'EOF'
// Color utility functions
export function hexToRgb(hex) {
    const result = /^#?([a-f\d]{2})([a-f\d]{2})([a-f\d]{2})$/i.exec(hex);
    return result ? {
        r: parseInt(result[1], 16),
        g: parseInt(result[2], 16),
        b: parseInt(result[3], 16)
    } : null;
}
EOF

# Commit in submodule
git add color-utils.js
git commit -m "Add color utilities"

# Push submodule branch (if using GitHub)
git push origin feature/color-utils

# Go back to main project
cd ../..

# Main project sees submodule changed
git status
# Shows: modified:   libs/shared-utils (new commits)

# Commit in main project
git add libs/shared-utils
git commit -m "Update shared-utils with color utilities"
```

**Important:** Changes in submodule and main project are independent!

**What you learned:** Working inside submodules (they're just Git repos!)

---

### Exercise 9.8: Pin Submodule to Specific Version

You can control which version of the submodule to use.

```bash
# View current submodule commit
git submodule status
# Shows: abc1234... libs/shared-utils (heads/main)

# Enter submodule
cd libs/shared-utils

# View history
git log --oneline

# Checkout specific commit
git checkout abc1234

# Or checkout specific tag
git checkout v1.0.0

# Go back to main project
cd ../..

# Commit the version pin
git add libs/shared-utils
git commit -m "Pin shared-utils to version 1.0.0"
```

**This is powerful!** Each project can use different versions:
- Project A: Uses shared-utils v1.0.0
- Project B: Uses shared-utils v2.0.0
- Both controlled and intentional

**What you learned:** Version control for dependencies!

---

### Exercise 9.9: Remove a Submodule

If you need to remove a submodule:

```bash
# Step 1: Deinitialize
git submodule deinit -f libs/shared-utils

# Step 2: Remove from Git
git rm -f libs/shared-utils

# Step 3: Remove from .git/modules (cleanup)
rm -rf .git/modules/libs/shared-utils

# Step 4: Commit
git commit -m "Remove shared-utils submodule"

# Verify it's gone
git submodule
# Should show nothing
```

#### Using SourceTree:

1. Right-click submodule in File Status
2. Select **Remove Submodule**
3. Confirm
4. Commit changes

**What you learned:** Removing submodules (it's a bit involved!)

---

### Exercise 9.10: Nested Submodules

Submodules can contain submodules!

```
Your Project
└── libs/shared-utils (submodule)
    └── vendor/lodash (nested submodule)
```

**Handling nested submodules:**

```bash
# Clone with ALL nested submodules
git clone --recursive <url>

# Update all nested submodules
git submodule update --init --recursive

# Update remote nested submodules
git submodule update --remote --recursive
```

---

## Submodule Common Workflows

### Workflow 1: Add External Library

```bash
# Add library as submodule
git submodule add https://github.com/library/repo.git vendor/library

# Commit
git add .
git commit -m "Add library as submodule"

# Push
git push origin main
```

### Workflow 2: Update All Submodules

```bash
# Update all to latest
git submodule update --remote --merge

# Commit updates
git add .
git commit -m "Update all submodules"

# Push
git push origin main
```

### Workflow 3: Team Member Clones

```bash
# Clone with submodules
git clone --recursive <project-url>

# Or clone then initialize
git clone <project-url>
cd project
git submodule update --init --recursive
```

### Workflow 4: Switch Branches (with submodules)

```bash
# Switch branch
git checkout feature-branch

# Update submodules for this branch
git submodule update --init --recursive

# Submodules might be at different commits!
```

---

## Submodule vs Alternatives

### When to Use Submodules:

✅ **Good for:**
- Shared libraries across multiple projects
- Third-party code you want version controlled
- When you need to modify the dependency
- Large dependencies that change infrequently
- When you want exact version control

❌ **Not ideal for:**
- Simple copy-paste shared code (just copy it!)
- Rapidly changing dependencies
- When package managers work (npm, pip, etc.)
- Beginners (adds complexity)

### Alternatives:

**1. Package Managers (npm, pip, composer)**
- Easier to use
- Better for most libraries
- Use when available!

**2. Git Subtree**
- Similar to submodules but simpler
- Embeds code directly
- Less powerful but easier

**3. Copy/Paste**
- Sometimes simplest is best!
- For small utilities
- No external dependencies

**4. Monorepo**
- All code in one repository
- No submodules needed
- Used by Google, Facebook

---

## Submodule Best Practices

### ✅ DO:

1. **Document submodules in README**
```markdown
## Setup

Clone with submodules:
\`\`\`bash
git clone --recursive <url>
\`\`\`
```

2. **Use specific commits/tags**
```bash
# Pin to stable version
cd submodule
git checkout v1.0.0
cd ..
git add submodule
git commit -m "Pin to v1.0.0"
```

3. **Update regularly**
```bash
# Weekly/monthly update routine
git submodule update --remote
```

4. **Keep .gitmodules in version control**
```bash
# Always commit .gitmodules
git add .gitmodules
```

### ❌ DON'T:

1. **Don't forget to initialize**
```bash
# Teammates will have empty folders!
# Always document initialization
```

2. **Don't make uncommitted changes**
```bash
# Changes in submodules can be lost
# Commit in submodule first
```

3. **Don't use for everything**
```bash
# Use package managers when possible
# npm, pip, cargo are easier
```

4. **Don't ignore submodule state**
```bash
# Always commit after updating submodules
git add submodule
git commit -m "Update submodule"
```

---

## Submodule Quick Reference

```bash
# Add submodule
git submodule add <url> <path>

# Clone with submodules
git clone --recursive <url>

# Initialize submodules (after clone)
git submodule init
git submodule update

# Initialize and update in one command
git submodule update --init --recursive

# Update submodules to latest
git submodule update --remote

# List submodules
git submodule

# Status of submodules
git submodule status

# Execute command in all submodules
git submodule foreach <command>

# Example: Pull in all submodules
git submodule foreach git pull origin main

# Remove submodule
git submodule deinit -f <path>
git rm -f <path>
rm -rf .git/modules/<path>

# View submodule configuration
cat .gitmodules

# Update specific submodule
cd <submodule-path>
git pull origin main
cd ..
git add <submodule-path>
git commit -m "Update submodule"
```

---

## Submodule Troubleshooting

### Problem: "Submodule folder is empty"

**Solution:**
```bash
git submodule init
git submodule update
```

### Problem: "Submodule not updating"

**Solution:**
```bash
# Force update
git submodule update --remote --force

# Or enter and pull manually
cd submodule-path
git pull origin main
cd ..
git add submodule-path
git commit -m "Update"
```

### Problem: "Detached HEAD in submodule"

This is normal! Submodules point to specific commits.

**To fix (if you want to work on branch):**
```bash
cd submodule-path
git checkout main
```

### Problem: "Can't remove submodule"

**Solution:**
```bash
# Complete removal
git submodule deinit -f path/to/submodule
git rm -f path/to/submodule
rm -rf .git/modules/path/to/submodule
git commit -m "Remove submodule"
```

### Problem: "Submodule changes not showing in main project"

**Solution:**
```bash
# Submodule changes must be committed TWICE:
# 1. In the submodule
cd submodule-path
git add .
git commit -m "Changes in submodule"

# 2. In the main project
cd ..
git add submodule-path
git commit -m "Update submodule reference"
```

---

## Summary: Submodules

**What you learned:**
- ✅ What submodules are and when to use them
- ✅ Adding submodules to projects
- ✅ Cloning projects with submodules
- ✅ Updating submodules
- ✅ Working inside submodules
- ✅ Version pinning
- ✅ Removing submodules
- ✅ Best practices
- ✅ Common workflows
- ✅ Troubleshooting

**Key takeaway:** Submodules let you include other Git repositories in your project while maintaining version control for both!

---

## Practice Exercises

Try these after completing Part 9:

### Exercise A: Shared Components
1. Create a "shared-components" repository
2. Add some UI components
3. Add as submodule to your project
4. Use the components
5. Update when components change

### Exercise B: Multiple Projects
1. Create two projects
2. Both use same submodule
3. Update submodule
4. Update both projects
5. See how changes propagate

### Exercise C: Version Control
1. Add submodule
2. Make commits in submodule
3. Pin to specific versions
4. Switch between versions
5. See project behavior change

---

**Congratulations! You now understand Git submodules! 🎉**

Submodules are powerful but complex - use them when you need them, but don't overuse them. Package managers are often simpler for most dependencies!

---

## Practice Exercises

Try these on your own:

### Exercise A: New Feature Branch
1. Create branch `feature/categories`
2. Add ability to categorize tasks
3. Commit changes
4. Merge back to main

### Exercise B: Bug Fix
1. Find a bug (or create one)
2. Create branch `bugfix/task-delete`
3. Fix the bug
4. Commit and merge

### Exercise C: Experimental Feature
1. Create branch `experiment/animations`
2. Add CSS animations
3. If you like it, merge to main
4. If not, delete the branch

### Exercise D: Create Conflict and Resolve
1. Create two branches that change the same line
2. Create a conflict
3. Practice resolving it

---

## Quick Reference Card

### Most Common Commands

```bash
# Status and info
git status                    # Show current state
git log --oneline --graph    # View history

# Branches
git branch                   # List branches
git branch <name>           # Create branch
git checkout <name>         # Switch branch
git checkout -b <name>      # Create and switch

# Staging and committing
git add .                   # Stage all
git add <file>              # Stage specific file
git commit -m "message"     # Commit

# Merging
git merge <branch>          # Merge branch into current

# Undoing
git restore <file>          # Discard changes
git reset --soft HEAD~1     # Undo last commit (keep changes)
git reset --hard HEAD~1     # Undo last commit (discard)

# Remote
git push origin <branch>    # Push to remote
git pull origin <branch>    # Pull from remote
```

### SourceTree Quick Tips

- **File Status Tab**: See current changes
- **History Tab**: View commits and branches
- **Drag and drop**: To stage/unstage files
- **Right-click**: Access most operations
- **Graph view**: Visualize branch structure
- **Double-click branch**: Switch to it
- **Search**: Find commits by message/author

---

## Troubleshooting

### "I'm on the wrong branch!"
```bash
# Don't panic! Just switch
git checkout main
```

### "I committed to the wrong branch!"
```bash
# Cherry-pick commit to correct branch
git checkout correct-branch
git cherry-pick <commit-hash>
```

### "I want to undo everything!"
```bash
# Reset to last commit (careful!)
git reset --hard HEAD
```

### "SourceTree is confusing!"
- Start with command line to understand concepts
- Use SourceTree for visualization
- Both are showing the same Git repository!

---

## Next Steps

1. **Practice regularly**: Create branches, commit often
2. **Read error messages**: Git tells you what's wrong
3. **Use `git status`**: When in doubt, check status
4. **Experiment safely**: Branches are cheap, use them!
5. **Learn gradually**: Master basics before advanced features

---

## Additional Resources

- **Git Documentation**: https://git-scm.com/doc
- **SourceTree Documentation**: https://confluence.atlassian.com/sourcetree
- **Interactive Git Tutorial**: https://learngitbranching.js.org/
- **Git Cheat Sheet**: https://education.github.com/git-cheat-sheet-education.pdf

---

Happy Git Learning! 🚀

Remember: Everyone struggles with Git at first. Keep practicing, and it will become second nature!

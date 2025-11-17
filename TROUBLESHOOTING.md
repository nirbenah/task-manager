# 🔧 Git Troubleshooting Guide

Your complete problem-solving reference for Git and SourceTree issues.

---

## Table of Contents

1. [Common Git Errors](#common-git-errors)
2. [Merge Conflict Problems](#merge-conflict-problems)
3. [Branch Issues](#branch-issues)
4. [Commit Problems](#commit-problems)
5. [SourceTree Specific Issues](#sourcetree-specific-issues)
6. [File Problems](#file-problems)
7. [Remote Repository Issues](#remote-repository-issues)
8. [Emergency Recovery](#emergency-recovery)
9. [Prevention Tips](#prevention-tips)

---

## Common Git Errors

### ❌ Error: "fatal: not a git repository"

**What it means:** You're not in a Git repository folder

**Solution (Command Line):**
```bash
# Check if you're in the right folder
pwd

# Navigate to your project
cd path/to/git-learning-project

# Verify it's a Git repo
ls -la  # Should see .git folder

# If .git doesn't exist, initialize
git init
```

**Solution (SourceTree):**
- Make sure you opened the correct folder
- File → Open → Select the project folder
- If not initialized, create new repository at that location

---

### ❌ Error: "Please tell me who you are"

**What it means:** Git doesn't know your identity

**Full error:**
```
*** Please tell me who you are.
Run
  git config --global user.email "you@example.com"
  git config --global user.name "Your Name"
```

**Solution (Command Line):**
```bash
# Set your identity globally
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"

# Verify
git config --global --list
```

**Solution (SourceTree):**
1. Tools → Options (Windows) or SourceTree → Preferences (Mac)
2. Go to "General" tab
3. Set "Default user information"

---

### ❌ Error: "Your local changes would be overwritten"

**Full error:**
```
error: Your local changes to the following files would be overwritten by checkout:
    script.js
Please commit your changes or stash them before you switch branches.
```

**What it means:** You have uncommitted changes that conflict with branch switch

**Solution 1: Commit changes**
```bash
git add .
git commit -m "Work in progress"
git checkout other-branch
```

**Solution 2: Stash changes (save for later)**
```bash
git stash
git checkout other-branch
# Later, restore changes:
git stash pop
```

**Solution 3: Discard changes (if you don't need them)**
```bash
git checkout -- .  # Discard all changes
git checkout other-branch
```

**SourceTree:**
1. **Commit**: Stage files → Commit
2. **Stash**: Right-click files → Stash → Include unstaged files
3. **Discard**: Right-click files → Discard

---

### ❌ Error: "fatal: refusing to merge unrelated histories"

**What it means:** Trying to merge branches with no common ancestor

**Solution (Command Line):**
```bash
git merge other-branch --allow-unrelated-histories
```

**SourceTree:**
- This shouldn't happen in normal workflow
- If it does, use command line with the flag above

---

### ❌ Error: "pathspec did not match any files"

**What it means:** File doesn't exist or wrong filename

**Solution:**
```bash
# Check exact filename (case-sensitive!)
ls

# Use correct filename
git add correct-filename.js

# Or add all files
git add .
```

---

### ❌ Error: "permission denied (publickey)"

**What it means:** Can't connect to remote (SSH key issue)

**Solution:**
```bash
# Use HTTPS instead of SSH
git remote set-url origin https://github.com/username/repo.git

# Or set up SSH keys (more advanced)
# Follow GitHub's SSH key guide
```

---

## Merge Conflict Problems

### ❌ Problem: "I have merge conflicts and I'm scared!"

**Don't panic!** Conflicts are normal and fixable.

**What you see:**
```javascript
<<<<<<< HEAD
const greeting = "Hello World";
=======
const greeting = "Hi there";
>>>>>>> feature-branch
```

**Understanding the markers:**
- `<<<<<<< HEAD` = Your current branch's version
- `=======` = Separator
- `>>>>>>> feature-branch` = The other branch's version

**Solution Steps:**

**Step 1: Identify conflicted files**
```bash
git status
# Shows: both modified: script.js
```

**Step 2: Open file and find conflict markers**
Look for `<<<<<<<`, `=======`, `>>>>>>>`

**Step 3: Decide what to keep**
- Keep HEAD version
- Keep other branch version
- Keep both (edit to combine)
- Write something completely new

**Step 4: Edit the file**
```javascript
// Original conflict:
<<<<<<< HEAD
const greeting = "Hello World";
=======
const greeting = "Hi there";
>>>>>>> feature-branch

// After resolution (choose one):
const greeting = "Hello World";  // Kept HEAD

// Or:
const greeting = "Hi there";  // Kept feature-branch

// Or:
const greeting = "Hello there World";  // Combined both
```

**Step 5: Remove ALL conflict markers**
Delete `<<<<<<<`, `=======`, `>>>>>>>`

**Step 6: Stage the resolved file**
```bash
git add script.js
```

**Step 7: Complete the merge**
```bash
git commit -m "Resolved merge conflict in script.js"
```

**SourceTree Solution:**
1. File Status → See conflicted files (⚠️ icon)
2. Right-click → Resolve Conflicts → Open External Merge Tool
3. Edit file, remove markers
4. Save file
5. Right-click → Mark Resolved
6. Commit

---

### ❌ Problem: "I messed up conflict resolution, want to start over"

**Solution (Command Line):**
```bash
# Abort the merge and start fresh
git merge --abort

# Try merge again
git merge branch-name
```

**SourceTree:**
- Actions → Abort Merge
- Try merge again

---

### ❌ Problem: "Conflict in binary file (image, etc.)"

**Solution:**
```bash
# Choose which version to keep
git checkout --ours image.png    # Keep your version
# Or
git checkout --theirs image.png  # Keep their version

# Stage the decision
git add image.png

# Complete merge
git commit
```

---

## Branch Issues

### ❌ Problem: "I'm on the wrong branch!"

**Scenario 1: Haven't committed yet**
```bash
# Just switch branches
git checkout correct-branch
```

**Scenario 2: Already committed**
```bash
# Find the commit hash
git log --oneline -5

# Switch to correct branch
git checkout correct-branch

# Cherry-pick the commit
git cherry-pick <commit-hash>

# Go back and remove from wrong branch
git checkout wrong-branch
git reset --hard HEAD~1  # Remove last commit
```

**SourceTree:**
1. Note the commit hash from wrong branch
2. Switch to correct branch (double-click)
3. Right-click the commit → Cherry Pick
4. Switch back to wrong branch
5. Right-click previous commit → Reset to this commit → Hard

---

### ❌ Problem: "Can't delete branch - not fully merged"

**What it means:** Branch has commits not in main

**Solution (if safe to delete):**
```bash
# Force delete
git branch -D branch-name
```

**Solution (if you want to keep the commits):**
```bash
# Merge it first
git checkout main
git merge branch-name
# Then delete
git branch -d branch-name
```

---

### ❌ Problem: "Accidentally deleted branch"

**Solution (if you know the commit hash):**
```bash
# Recreate branch at that commit
git branch recovered-branch <commit-hash>
```

**Solution (don't know commit):**
```bash
# Find the commit in reflog
git reflog

# Look for the branch name
# Find the commit hash, then:
git branch recovered-branch <commit-hash>
```

---

### ❌ Problem: "Lost in detached HEAD state"

**What you see:**
```
You are in 'detached HEAD' state...
```

**What it means:** You checked out a specific commit (not a branch)

**Solution (want to create branch here):**
```bash
git branch new-branch-name
git checkout new-branch-name
```

**Solution (want to go back to main):**
```bash
git checkout main
```

---

## Commit Problems

### ❌ Problem: "Wrong commit message"

**Scenario 1: Last commit only, not pushed**
```bash
# Amend the commit message
git commit --amend -m "Correct message"
```

**SourceTree:**
- Repository → Commit Options → Amend Latest Commit
- Edit message → Commit

---

### ❌ Problem: "Forgot to add files to commit"

**Solution:**
```bash
# Stage the forgotten files
git add forgotten-file.js

# Amend the commit
git commit --amend --no-edit
```

---

### ❌ Problem: "Committed to main instead of feature branch"

**Solution:**
```bash
# Create the feature branch at current position
git branch feature-branch

# Reset main to before the commit
git reset --hard HEAD~1

# Switch to feature branch (commit is there!)
git checkout feature-branch
```

**SourceTree:**
1. Create new branch at current position
2. Switch to main
3. Right-click previous commit → Reset main to this commit → Hard
4. Switch to feature branch (commit is safe there)

---

### ❌ Problem: "Committed sensitive data (password, API key)"

**⚠️ URGENT - If already pushed to remote:**
```bash
# 1. Remove the sensitive data from file
# Edit the file, remove the password/key

# 2. Commit the fix
git add .
git commit -m "Remove sensitive data"

# 3. Force push (if not shared with team yet)
git push --force

# 4. IMPORTANT: Rotate the credentials!
# Change the password/API key - it's compromised!
```

**If not pushed yet:**
```bash
# Remove file from last commit
git reset --soft HEAD~1
# Edit the file to remove sensitive data
# Commit again
git add .
git commit -m "Add feature (without sensitive data)"
```

---

### ❌ Problem: "Want to undo last commit completely"

**Keep changes:**
```bash
git reset --soft HEAD~1
# Files back to staging area
```

**Discard changes:**
```bash
git reset --hard HEAD~1
# Everything gone!
```

**SourceTree:**
- Right-click previous commit → Reset to this commit
- Choose Soft (keep) or Hard (discard)

---

## SourceTree Specific Issues

### ❌ Problem: "SourceTree won't open my repository"

**Solution 1: Remove and re-add**
1. Remove from SourceTree (doesn't delete files)
2. File → Open → Select folder again

**Solution 2: Check .git folder exists**
- Make sure project has `.git` folder
- If not, initialize: Repository → Create Local Repository

---

### ❌ Problem: "SourceTree shows wrong credentials"

**Solution:**
1. Tools → Options (or Preferences on Mac)
2. Authentication tab
3. Remove old credentials
4. Add correct credentials

---

### ❌ Problem: "Can't see diff/changes in SourceTree"

**Solution:**
1. Check you're on "File Status" tab
2. Click the file to see diff
3. Try: View → Refresh (F5)
4. Restart SourceTree

---

### ❌ Problem: "Push/Pull buttons greyed out"

**What it means:** No remote repository configured

**Solution:**
1. Repository → Repository Settings
2. Add remote (origin)
3. Buttons should activate

---

### ❌ Problem: "SourceTree is very slow"

**Solutions:**
1. Repository → Reindex
2. Tools → Options → Git → Disable unused features
3. Close other repositories
4. Upgrade to latest version

---

## File Problems

### ❌ Problem: "Accidentally staged wrong files"

**Solution (Command Line):**
```bash
# Unstage specific file
git reset HEAD filename.js

# Unstage all files
git reset HEAD
```

**SourceTree:**
- Select files in "Staged files"
- Click "Unstage Selected"
- Or drag back to "Unstaged files"

---

### ❌ Problem: "Want to ignore files already tracked"

**Solution:**
```bash
# Remove from Git but keep locally
git rm --cached filename.log

# Add to .gitignore
echo "*.log" >> .gitignore

# Commit
git add .gitignore
git commit -m "Start ignoring log files"
```

---

### ❌ Problem: "Deleted file by accident"

**Scenario 1: Not committed yet**
```bash
# Restore from last commit
git checkout HEAD -- filename.js
```

**Scenario 2: Already committed deletion**
```bash
# Find when file existed
git log -- filename.js

# Restore from specific commit
git checkout <commit-hash> -- filename.js
```

**SourceTree:**
- Right-click deleted file → Discard (if not committed)
- Or: Find commit with file → Right-click file → Reset

---

### ❌ Problem: "File permissions changed (chmod)"

**Solution (if you don't want to track permissions):**
```bash
# Disable file mode tracking
git config core.fileMode false

# Undo the change
git checkout -- .
```

---

## Remote Repository Issues

### ❌ Error: "fatal: remote origin already exists"

**Solution:**
```bash
# Remove existing remote
git remote remove origin

# Add correct remote
git remote add origin https://github.com/user/repo.git
```

---

### ❌ Error: "Updates were rejected because the tip of your current branch is behind"

**What it means:** Remote has commits you don't have

**Solution 1: Pull first**
```bash
git pull origin main
# Resolve any conflicts
git push origin main
```

**Solution 2: Force push (DANGEROUS - only if you're sure!)**
```bash
git push --force origin main
# ⚠️ This deletes remote commits!
```

---

### ❌ Problem: "Push is taking forever"

**Solution:**
```bash
# Check what's being pushed
git diff origin/main..main

# If pushing large files:
# - Use Git LFS for large files
# - Add large files to .gitignore
```

---

### ❌ Problem: "Clone failed - too large"

**Solution:**
```bash
# Shallow clone (less history)
git clone --depth 1 https://github.com/user/repo.git
```

---

## Emergency Recovery

### 🆘 "I deleted everything and panicked!"

**Solution (if you committed before):**
```bash
# Find the commit
git reflog

# Reset to that commit
git reset --hard <commit-hash>

# Your files are back! ✨
```

---

### 🆘 "Repository is completely broken"

**Nuclear option - start fresh:**
```bash
# 1. Backup your current work
cp -r project-folder project-backup

# 2. Delete .git folder
rm -rf .git

# 3. Re-initialize
git init

# 4. Commit everything fresh
git add .
git commit -m "Fresh start"
```

---

### 🆘 "I ran 'git reset --hard' by mistake"

**Solution (if you did it recently):**
```bash
# Check reflog
git reflog

# Find the commit before reset
# Look for: HEAD@{1}: commit: Your message

# Restore it
git reset --hard HEAD@{1}
```

---

### 🆘 "Merge went horribly wrong"

**Solution:**
```bash
# Abort the merge
git merge --abort

# Go back to before merge
git reset --hard HEAD~1

# Try again more carefully
```

---

---

## Submodule Issues

### ❌ Problem: "Submodule folder is empty after clone"

**What it means:** Submodules aren't cloned by default

**Solution:**
```bash
# Initialize and update
git submodule init
git submodule update

# Or in one command
git submodule update --init --recursive

# Or clone with submodules from start
git clone --recursive <url>
```

---

### ❌ Problem: "fatal: no submodule mapping found in .gitmodules"

**What it means:** .gitmodules file is missing or corrupted

**Solution:**
```bash
# Check if .gitmodules exists
cat .gitmodules

# If missing, re-add submodule
git submodule add <url> <path>

# If corrupted, fix manually
nano .gitmodules
```

---

### ❌ Problem: "Detached HEAD state in submodule"

**What it means:** Submodules point to specific commits (this is normal!)

**When it's OK:**
- Just using the submodule (no changes needed)

**When to fix:**
```bash
# If you need to work on the submodule
cd submodule-path
git checkout main  # Or other branch
# Make changes
git add .
git commit -m "Changes"
git push origin main

# Go back to main project
cd ..
git add submodule-path
git commit -m "Update submodule"
```

---

### ❌ Problem: "Can't update submodule"

**Solution 1: Force update**
```bash
git submodule update --remote --force
```

**Solution 2: Manual update**
```bash
cd submodule-path
git fetch origin
git checkout main
git pull origin main
cd ..
git add submodule-path
git commit -m "Update submodule"
```

**Solution 3: Reset submodule**
```bash
git submodule deinit -f submodule-path
git submodule update --init submodule-path
```

---

### ❌ Problem: "Changes in submodule not reflected in main project"

**What it means:** Need to commit in both places

**Solution:**
```bash
# Step 1: Commit in submodule
cd submodule-path
git add .
git commit -m "Changes in submodule"
git push origin main

# Step 2: Commit submodule reference in main project
cd ..
git status  # Shows: modified: submodule-path
git add submodule-path
git commit -m "Update submodule reference"
git push origin main
```

---

### ❌ Problem: "Can't remove submodule"

**Solution (complete removal):**
```bash
# Step 1: Deinitialize
git submodule deinit -f path/to/submodule

# Step 2: Remove from index
git rm -f path/to/submodule

# Step 3: Clean up .git/modules
rm -rf .git/modules/path/to/submodule

# Step 4: Commit
git commit -m "Remove submodule"

# Step 5: Remove .gitmodules entry if last submodule
# (Git handles this automatically)
```

---

### ❌ Problem: "Submodule URL changed"

**Solution:**
```bash
# Update .gitmodules with new URL
nano .gitmodules
# Change the url line

# Sync the configuration
git submodule sync

# Update submodule
git submodule update --remote

# Commit
git add .gitmodules
git commit -m "Update submodule URL"
```

---

### ❌ Problem: "Git says 'no submodule mapping found'"

**Solution:**
```bash
# Re-register the submodule
git submodule add <url> <path>

# Or if .gitmodules exists
git submodule sync
git submodule update --init
```

---

### ❌ Problem: "Submodule is ahead/behind"

**What it means:** Local submodule doesn't match committed reference

**Solution:**
```bash
# To match main project's reference
git submodule update

# To update main project to submodule's current state
git add submodule-path
git commit -m "Update submodule reference"
```

---

### ❌ Problem: "Merge conflict in submodule"

**Solution:**
```bash
# View conflict
git status
# Shows: both modified: submodule-path

# Enter submodule
cd submodule-path

# Resolve as normal Git conflict
git merge <branch>
# Or git pull

# Go back
cd ..

# Commit resolution
git add submodule-path
git commit -m "Resolve submodule conflict"
```

---

### ❌ Problem: "Accidentally committed submodule as folder"

**What it means:** Added folder without `git submodule add`

**How to tell:**
```bash
git ls-files submodule-path
# If shows individual files = wrong (it's a regular folder)
# Should show just the folder = right (it's a submodule)
```

**Solution:**
```bash
# Remove the folder from Git
git rm -rf submodule-path

# Re-add properly as submodule
git submodule add <url> submodule-path

# Commit
git add .
git commit -m "Fix: convert folder to submodule"
```

---

### ❌ Problem: "Submodule has uncommitted changes blocking update"

**Solution 1: Stash changes**
```bash
cd submodule-path
git stash
cd ..
git submodule update
cd submodule-path
git stash pop
```

**Solution 2: Commit changes**
```bash
cd submodule-path
git add .
git commit -m "WIP"
cd ..
git submodule update
```

**Solution 3: Discard changes**
```bash
cd submodule-path
git checkout -- .
cd ..
git submodule update
```

---

### ❌ Problem: "foreach command not working"

**Example:**
```bash
# Want to pull all submodules
git submodule foreach git pull origin main

# Error if some don't have 'main' branch
```

**Solution:**
```bash
# Add error handling
git submodule foreach 'git pull origin main || :'

# Or do individually
git submodule foreach 'echo "Updating $name"; git pull || echo "Failed"'
```

---

### ❌ Problem: "Nested submodules not updating"

**Solution:**
```bash
# Always use --recursive
git submodule update --init --recursive

# Clone with recursive
git clone --recursive <url>

# Update with recursive
git submodule update --remote --recursive
```

---

## Submodule Best Practices Checklist

**When adding submodule:**
- [ ] Use HTTPS URL (easier authentication)
- [ ] Document in README how to clone
- [ ] Commit .gitmodules file
- [ ] Tell team about new submodule

**When updating submodule:**
- [ ] Test after update
- [ ] Commit submodule reference
- [ ] Push both submodule and main project
- [ ] Tell team to run `git submodule update`

**When removing submodule:**
- [ ] Follow complete removal steps
- [ ] Test project still works
- [ ] Tell team to pull changes
- [ ] Document removal

**For team:**
- [ ] Always clone with `--recursive`
- [ ] Run `git submodule update` after pull
- [ ] Commit changes in submodule separately
- [ ] Don't ignore submodule changes

---

## Advanced Remote Issues

### ❌ Problem: "Forgot to pull before making commits"

**Scenario:**
```
Remote:  A → B → C → D
Local:   A → B → E → F (forgot to pull!)
```

**Solution 1: Merge (creates merge commit)**
```bash
git pull origin main
# Resolve conflicts if any
git push origin main

# Result: A → B → C → D → M (merge)
#              ↘ E → F ↗
```

**Solution 2: Rebase (cleaner history)**
```bash
git pull --rebase origin main
# Resolve conflicts if any
git push origin main

# Result: A → B → C → D → E' → F'
```

---

### ❌ Problem: "Pushed sensitive data (password, API key)"

**⚠️ URGENT - Follow these steps:**

**Step 1: Remove immediately**
```bash
# Edit file, remove sensitive data
git add .
git commit -m "Remove sensitive data"
git push origin main
```

**Step 2: ROTATE CREDENTIALS (CRITICAL!)**
- Change password immediately
- Generate new API key
- Old credentials are compromised forever!

**Step 3: Rewrite history (optional, complex)**
```bash
# Use BFG Repo Cleaner or git filter-branch
# Only if absolutely necessary
# Consult documentation
```

**Prevention:**
```bash
# Use .gitignore
echo ".env" >> .gitignore
echo "config.json" >> .gitignore

# Use environment variables
# Never hardcode secrets
```

---

### ❌ Problem: "Need to undo last push"

**If NO ONE pulled yet:**
```bash
# Reset locally
git reset --hard HEAD~1

# Force push (rewrites history)
git push --force origin main
```

**If OTHERS might have pulled:**
```bash
# Create revert commit (safe, no rewrite)
git revert HEAD
git push origin main
```

---

### ❌ Problem: "Fork is outdated, need to sync"

**Setup (one time):**
```bash
git remote add upstream https://github.com/original/repo.git
git remote -v
```

**Sync fork:**
```bash
git fetch upstream
git checkout main
git merge upstream/main
git push origin main
```

---

### ❌ Problem: "Remote branch deleted but still shows locally"

**Solution:**
```bash
# Remove stale references
git fetch --prune origin

# Or set automatic pruning
git config --global fetch.prune true
```

---

### ❌ Problem: "Local and remote diverged"

**Message:**
```
Your branch and 'origin/main' have diverged,
and have 2 and 3 different commits each.
```

**Solutions:**
```bash
# Option 1: Merge
git pull origin main

# Option 2: Rebase
git pull --rebase origin main

# Then push
git push origin main
```

---

### ❌ Problem: "Teammate force pushed, lost my work"

**Solution (your commits still exist!):**
```bash
# Find your commits in reflog
git reflog

# Create branch at lost commit
git branch recovered-work <commit-hash>

# Merge it back
git merge recovered-work
```

**Prevention:** Team rule - never force push shared branches!

---

### ❌ Problem: "Authentication with token"

**GitHub changed to require tokens instead of passwords**

**Solution:**
```bash
# 1. Generate Personal Access Token on GitHub:
#    Settings → Developer Settings → Personal Access Tokens

# 2. When pushing, use token as password
git push origin main
# Username: your-github-username
# Password: <paste-your-token>

# 3. Save credentials (optional)
git config --global credential.helper store
```

---

### ❌ Problem: "Work on same feature across multiple computers"

**Computer 1:**
```bash
git checkout -b feature/my-work
# Work
git commit -m "Progress"
git push origin feature/my-work
```

**Computer 2:**
```bash
git fetch origin
git checkout feature/my-work
# Continue working
git commit -m "More progress"
git push origin feature/my-work
```

**Back to Computer 1:**
```bash
git pull origin feature/my-work
# Continue working
```

---

## Prevention Tips

### ✅ Before Making Big Changes

1. **Check status first:**
```bash
git status
```

2. **Create a backup branch:**
```bash
git branch backup-$(date +%Y%m%d)
```

3. **Work on a feature branch:**
```bash
git checkout -b feature-name
```

---

### ✅ Good Habits

1. **Commit often** - Small commits are easier to undo
2. **Write clear messages** - Future you will thank you
3. **Pull before push** - Avoid conflicts
4. **Check branch** - `git branch` before committing
5. **Review before commit** - `git diff` to see changes
6. **Test before merge** - Make sure it works
7. **Keep main stable** - Only merge working code

---

### ✅ Safety Commands

```bash
# Always check where you are
git status
git branch

# See what would happen (dry run)
git merge --no-commit --no-ff branch-name

# View changes before committing
git diff

# See what would be pushed
git diff origin/main..main
```

---

## Quick Problem-Solving Flowchart

```
Problem occurred!
    ↓
Did you commit yet?
    ↓               ↓
   YES              NO
    ↓               ↓
Did you push?    Discard:
    ↓               git restore .
   YES              ↓
    ↓              FIXED! ✅
   NO
    ↓
Easy fix:
git reset --soft HEAD~1
OR
git commit --amend
    ↓
FIXED! ✅

If pushed:
    ↓
Is it shared?
    ↓           ↓
   YES         NO
    ↓           ↓
Create        Force push
new commit    (be careful!)
to fix        git push --force
    ↓           ↓
FIXED! ✅    FIXED! ✅
```

---

## Getting Help

### When Stuck:

1. **Read the error message** - Git tells you what's wrong
2. **Run `git status`** - Shows current state
3. **Check `git log`** - See recent commits
4. **Google the error** - Others had same problem
5. **Ask for help** - Provide error message and what you were doing

### Useful Commands for Debugging:

```bash
git status              # Current state
git log --oneline -10   # Recent commits
git reflog              # All recent actions
git diff                # Uncommitted changes
git branch              # Which branch am I on?
git remote -v           # What remotes exist?
```

---

## Remember

✅ **Git is forgiving** - Almost everything can be undone
✅ **Commits are safe** - Once committed, data is recoverable
✅ **Branches are cheap** - Use them liberally
✅ **Ask for help** - Everyone struggles with Git
✅ **Practice in safe environment** - Like this tutorial!

**Most important:** DON'T PANIC! Take a breath, read the error, and work through it step by step.

---

## Quick Reference: "I Want To..."

| I Want To... | Command |
|--------------|---------|
| Undo uncommitted changes | `git restore filename` or `git checkout -- filename` |
| Undo last commit (keep changes) | `git reset --soft HEAD~1` |
| Undo last commit (discard changes) | `git reset --hard HEAD~1` |
| Unstage files | `git reset HEAD filename` |
| Abandon a merge | `git merge --abort` |
| See what I changed | `git diff` |
| Go back to any commit | `git checkout <hash>` |
| Create branch at old commit | `git branch new-name <hash>` |
| Save work for later | `git stash` |
| Recover deleted commit | `git reflog` then `git reset --hard <hash>` |
| Force push (careful!) | `git push --force` |
| Change last commit message | `git commit --amend -m "new message"` |

---

You're now ready to handle problems! 💪

Remember: Mistakes are how you learn Git. This tutorial environment is safe to experiment in!

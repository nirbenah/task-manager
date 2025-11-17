# 📋 Git Quick Reference Cheat Sheet

Keep this handy while learning! Print it or keep it on a second screen.

---

## 🚨 Emergency Commands (Most Important!)

```bash
git status              # WHERE AM I? WHAT'S HAPPENING?
git log --oneline       # Show recent commits
git reflog              # Show ALL recent actions (lifesaver!)
git merge --abort       # Cancel a bad merge
git reset --hard HEAD   # Undo all uncommitted changes (DANGER!)
```

---

## 📁 Repository Setup

```bash
# Create new repo
git init

# Copy existing repo
git clone <url>

# Set identity
git config --global user.name "Your Name"
git config --global user.email "your@email.com"
```

---

## 💾 Basic Workflow

```bash
# 1. Check status
git status

# 2. Stage files
git add filename.js        # Specific file
git add .                  # All files

# 3. Commit
git commit -m "Message"

# 4. View history
git log --oneline
```

---

## 🌿 Branches

```bash
# List branches (* shows current)
git branch

# Create branch
git branch feature-name

# Switch branch
git checkout feature-name

# Create AND switch (shortcut)
git checkout -b feature-name

# Delete branch
git branch -d feature-name      # Safe delete
git branch -D feature-name      # Force delete

# Rename current branch
git branch -m new-name
```

---

## 🔀 Merging

```bash
# Merge branch into current
git checkout main              # Switch to main
git merge feature-name         # Merge feature into main

# Abort merge
git merge --abort

# View merged branches
git branch --merged
```

---

## 🔍 Viewing Changes

```bash
# Uncommitted changes
git diff

# Staged changes
git diff --staged

# Changes between commits
git diff commit1 commit2

# Changes in specific file
git diff filename.js

# Show commit details
git show <commit-hash>
```

---

## ⏪ Undoing Things

```bash
# Discard uncommitted changes
git restore filename           # New way
git checkout -- filename       # Old way
git checkout -- .             # All files

# Unstage file
git reset HEAD filename
git restore --staged filename  # New way

# Undo last commit (keep changes)
git reset --soft HEAD~1

# Undo last commit (discard changes)
git reset --hard HEAD~1

# Undo last N commits
git reset --soft HEAD~3        # Last 3 commits

# Amend last commit
git commit --amend -m "New message"
git commit --amend --no-edit   # Keep message
```

---

## 💾 Stashing (Save Work Temporarily)

```bash
# Save current changes
git stash
git stash save "Work in progress"

# List stashes
git stash list

# Apply most recent stash
git stash pop

# Apply specific stash
git stash apply stash@{0}

# Delete stash
git stash drop stash@{0}

# Clear all stashes
git stash clear
```

---

## 🌐 Remote Repository

```bash
# Add remote
git remote add origin <url>

# View remotes
git remote -v

# Show detailed remote info
git remote show origin

# Remove remote
git remote remove origin

# Rename remote
git remote rename origin upstream

# Change remote URL
git remote set-url origin <new-url>

# Push to remote
git push origin main
git push -u origin main              # Set upstream tracking
git push                             # Push to tracked branch
git push --all origin                # Push all branches
git push --tags origin               # Push all tags
git push --force origin main         # Force push (DANGER!)
git push --force-with-lease origin main  # Safer force push

# Pull from remote
git pull origin main
git pull                             # Pull from tracked branch
git pull --rebase origin main        # Pull and rebase

# Fetch (download without merge)
git fetch origin
git fetch --all                      # Fetch all remotes
git fetch --prune origin             # Remove stale references

# Clone remote
git clone <url>
git clone --depth 1 <url>            # Shallow clone (less history)
git clone -b branch-name <url>       # Clone specific branch

# View remote branches
git branch -r                        # Remote branches only
git branch -a                        # All branches (local + remote)

# Delete remote branch
git push origin --delete branch-name
git push origin :branch-name         # Alternative syntax

# Compare with remote
git diff main origin/main
git log origin/main..main            # Commits not pushed
git log main..origin/main            # Commits not pulled

# Check if ahead/behind
git status                           # Shows ahead/behind info

# Update remote tracking
git fetch --prune origin
git remote update                    # Update all remotes

# Multiple remotes
git remote add upstream <url>
git fetch upstream
git merge upstream/main
```

---

## 🔄 Common Remote Workflows

### Daily Solo Development
```bash
# Morning
git pull origin main

# Work
git add .
git commit -m "Add feature"

# Evening
git push origin main
```

### Feature Branch Development
```bash
# Start
git checkout main
git pull origin main
git checkout -b feature/my-feature

# Work
git add .
git commit -m "Progress"
git push -u origin feature/my-feature

# Update from main
git checkout main
git pull origin main
git checkout feature/my-feature
git merge main

# Create PR, get approved
# Cleanup after merge
git checkout main
git pull origin main
git branch -d feature/my-feature
```

### Fork and Contribute
```bash
# Setup
git clone <your-fork-url>
git remote add upstream <original-url>

# Work
git checkout -b feature/contribution
git add .
git commit -m "Add feature"
git push origin feature/contribution

# Sync with original
git fetch upstream
git checkout main
git merge upstream/main
git push origin main
```

### Sync Between Computers
```bash
# Computer 1
git commit -m "Work in progress"
git push origin feature-branch

# Computer 2
git fetch origin
git checkout feature-branch
git pull origin feature-branch
# Continue working
```

---

## 📦 Submodules

```bash
# Add submodule
git submodule add <url> <path>
git submodule add https://github.com/user/repo.git libs/library

# Clone repository with submodules
git clone --recursive <url>

# Initialize submodules (after clone)
git submodule init
git submodule update

# Initialize and update in one command
git submodule update --init --recursive

# Update submodules to latest
git submodule update --remote
git submodule update --remote --merge    # Update and merge

# Update specific submodule
git submodule update --remote path/to/submodule

# List submodules
git submodule

# Detailed status
git submodule status

# Summary of changes
git submodule summary

# Execute command in all submodules
git submodule foreach <command>
git submodule foreach 'git pull origin main'
git submodule foreach 'git checkout main'

# Work inside submodule
cd path/to/submodule
# Now it's a normal Git repo
git checkout -b feature
git add .
git commit -m "Changes"
git push origin feature
cd ../..
git add path/to/submodule
git commit -m "Update submodule"

# Pin submodule to specific version
cd path/to/submodule
git checkout v1.0.0  # Or specific commit hash
cd ../..
git add path/to/submodule
git commit -m "Pin submodule to v1.0.0"

# Sync submodule URLs (if changed)
git submodule sync
git submodule update --init

# Remove submodule (complete removal)
git submodule deinit -f path/to/submodule
git rm -f path/to/submodule
rm -rf .git/modules/path/to/submodule
git commit -m "Remove submodule"

# View submodule configuration
cat .gitmodules
git config --list | grep submodule
```

---

## 🔄 Submodule Workflows

### Adding External Library
```bash
# Add library as submodule
git submodule add https://github.com/library/repo.git vendor/library
git add .
git commit -m "Add library as submodule"
git push origin main
```

### Team Member Getting Code
```bash
# Clone with submodules
git clone --recursive https://github.com/user/project.git

# Or after clone
git clone https://github.com/user/project.git
cd project
git submodule update --init --recursive
```

### Updating Submodule Version
```bash
# Enter submodule
cd path/to/submodule
git pull origin main
cd ../..

# Commit new version
git add path/to/submodule
git commit -m "Update submodule to latest"
git push origin main
```

### After Pulling Changes with Submodules
```bash
# Always update submodules after pull
git pull origin main
git submodule update --init --recursive
```

### Making Changes in Submodule
```bash
# 1. Change in submodule
cd submodule-path
git checkout -b feature
# Make changes
git add .
git commit -m "New feature"
git push origin feature

# 2. Update main project reference
cd ../..
git add submodule-path
git commit -m "Update submodule with new feature"
git push origin main
```

---

## 🏷️ Tags

```bash
# Create tag
git tag v1.0

# Create annotated tag
git tag -a v1.0 -m "Version 1.0"

# List tags
git tag

# Push tags
git push origin v1.0           # Specific tag
git push origin --tags         # All tags

# Delete tag
git tag -d v1.0                # Local
git push origin :v1.0          # Remote
```

---

## 🔍 Searching & Finding

```bash
# Search commits by message
git log --grep="bug fix"

# Search by author
git log --author="John"

# Show files changed in commit
git show --name-only <commit-hash>

# Find who changed a line
git blame filename.js

# Search code
git grep "function name"
```

---

## 🛠️ Useful Options

```bash
# Compact log
git log --oneline

# Graph view
git log --oneline --graph --all

# Last N commits
git log -5

# Files changed
git log --stat

# Commits since date
git log --since="2 weeks ago"

# Pretty format
git log --pretty=format:"%h - %an, %ar : %s"
```

---

## 🔄 Cherry-Pick (Copy Commit)

```bash
# Copy specific commit to current branch
git cherry-pick <commit-hash>

# Cherry-pick without committing
git cherry-pick -n <commit-hash>
```

---

## 🧹 Cleanup

```bash
# Remove untracked files (dry run)
git clean -n

# Remove untracked files
git clean -f

# Remove untracked directories
git clean -fd

# Remove ignored files too
git clean -fdx
```

---

## 🆘 Recovery

```bash
# Find lost commits
git reflog

# Recover to specific state
git reset --hard HEAD@{2}

# Recover deleted branch
git branch recovered-branch <commit-hash>

# Show deleted files
git log --diff-filter=D --summary
```

---

## ⚙️ Configuration

```bash
# Global config
git config --global user.name "Name"
git config --global user.email "email"

# Local config (per repo)
git config user.name "Name"

# List config
git config --list

# Edit config file
git config --global --edit

# Set default branch name
git config --global init.defaultBranch main

# Aliases
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
```

---

## 🔐 .gitignore Patterns

```
# Comment

# Ignore file
secret.txt

# Ignore all files with extension
*.log
*.tmp

# Ignore directory
node_modules/
.vscode/

# Exception (don't ignore)
!important.log

# Ignore files in directory
logs/*.log

# Ignore nested directories
**/temp/

# Ignore in root only
/config.json
```

---

## 🎯 Common Workflows

### Starting New Feature
```bash
git checkout main
git pull origin main
git checkout -b feature/new-feature
# ... work ...
git add .
git commit -m "Add new feature"
git push origin feature/new-feature
# Create PR in web interface
```

### Fixing a Bug
```bash
git checkout main
git pull origin main
git checkout -b bugfix/issue-123
# ... fix ...
git add .
git commit -m "Fix issue #123"
git push origin bugfix/issue-123
# Create PR
```

### Updating Your Branch
```bash
git checkout main
git pull origin main
git checkout feature-branch
git merge main
# Or: git rebase main
```

---

## SourceTree Quick Reference

### Most Used Features

| Action | How To |
|--------|--------|
| Stage files | Drag to "Staged files" or click checkbox |
| Unstage | Drag back or click "Unstage Selected" |
| Commit | Enter message → Commit button |
| Create branch | Branch button → Enter name |
| Switch branch | Double-click branch name |
| Merge | Right-click branch → Merge into current |
| View diff | Click file in File Status tab |
| Discard changes | Right-click file → Discard |
| View history | History tab |
| Resolve conflict | Right-click file → Resolve Conflicts |
| Push | Push button in toolbar |
| Pull | Pull button in toolbar |
| Stash | Right-click files → Stash |

### Keyboard Shortcuts (Windows/Mac)

- `Ctrl/Cmd + 1` - File Status
- `Ctrl/Cmd + 2` - History
- `Ctrl/Cmd + 3` - Search
- `F5` - Refresh
- `Ctrl/Cmd + Shift + C` - Commit
- `Ctrl/Cmd + Shift + P` - Push

---

## 💡 Pro Tips

1. **Commit often** - Small commits are better
2. **Write good messages** - Explain WHY, not just WHAT
3. **Pull before push** - Avoid conflicts
4. **Use branches** - Keep main clean
5. **Review before commit** - Run `git diff`
6. **Don't panic** - Almost everything is recoverable
7. **Read error messages** - Git tells you what's wrong
8. **Check status frequently** - `git status` is your friend

---

## 📝 Commit Message Best Practices

### Good Format
```
Short summary (50 chars or less)

More detailed explanation if needed. Wrap at 72 characters.
Explain WHAT and WHY, not HOW.

- Bullet points are okay
- Use present tense: "Add feature" not "Added feature"
- Reference issues: "Fixes #123"
```

### Good Examples
```
✅ Fix login bug where session expired early
✅ Add dark mode toggle to settings page
✅ Refactor authentication logic for clarity
✅ Update dependencies to fix security vulnerability
```

### Bad Examples
```
❌ Fixed stuff
❌ Update
❌ asdf
❌ Final version (really this time)
```

---

## 🎨 Git Log Pretty Formats

```bash
# One line with colors
git log --oneline --decorate --graph --all

# With author and date
git log --pretty=format:"%h %an %ad %s" --date=short

# Detailed with files
git log --stat

# Custom format
git log --pretty=format:"%C(yellow)%h%Creset %C(blue)%an%Creset: %s"

# Create alias for pretty log
git config --global alias.lg "log --oneline --graph --all --decorate"
# Use: git lg
```

---

## 🔄 Merge vs Rebase

### Merge
```bash
git checkout main
git merge feature-branch
# Creates merge commit
```
**Use when:** Preserving history is important

### Rebase
```bash
git checkout feature-branch
git rebase main
# Rewrites history
```
**Use when:** Want clean, linear history

**⚠️ Never rebase public/shared branches!**

---

## 🚨 Common Mistakes & Fixes

| Mistake | Fix |
|---------|-----|
| Committed to wrong branch | `git checkout correct-branch` → `git cherry-pick <hash>` |
| Wrong commit message | `git commit --amend -m "New message"` |
| Forgot to add file | `git add file` → `git commit --amend --no-edit` |
| Want to undo commit | `git reset --soft HEAD~1` |
| Accidentally deleted file | `git checkout HEAD -- file` |
| Merge conflict panic | `git merge --abort` |
| Pushed wrong code | Create new commit to fix (or force push if alone) |

---

## 📱 When Working with Team

### Before Starting Work
```bash
git checkout main
git pull origin main
git checkout -b feature/my-work
```

### Before Creating PR
```bash
git checkout main
git pull origin main
git checkout feature/my-work
git merge main              # Or: git rebase main
# Resolve conflicts
git push origin feature/my-work
```

### After PR Merged
```bash
git checkout main
git pull origin main
git branch -d feature/my-work
```

---

## 🎓 Learn More

- Git Documentation: https://git-scm.com/doc
- Interactive Tutorial: https://learngitbranching.js.org/
- Visualizing Git: https://git-school.github.io/visualizing-git/
- GitHub Guides: https://guides.github.com/

---

**Remember: `git status` and `git log` are your best friends!**

Print this and keep it next to your computer! 🖨️

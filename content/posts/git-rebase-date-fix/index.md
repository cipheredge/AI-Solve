---
title: "How to Fix Commit Dates After Git Rebase"
date: 2025-02-13T22:35:10+04:00
slug: 'git-rebase-date-fix'
draft: false
cover: "https://jiejue.obs.ap-southeast-1.myhuaweicloud.com/20250213223635432.webp"
tag:
  - Git
  - Technical Practice
---

When using Git for version control, we occasionally need to modify historical commits. However, after using the rebase command, you might encounter a frustrating issue: all commits affected by the rebase have their timestamps reset to the time when the rebase was executed. This article will explain how to safely modify Git history without affecting commit timestamps.

<!--more-->

## Problem Background

Suppose you're maintaining a blog project, and one day you discover an error in an article from a few weeks ago that needs correction. You use the `git rebase -i` command to modify that historical commit, but then notice an unexpected situation: all commits after that one now show the date when you performed the rebase.

```bash
# Original commit history
abc1234 2024-01-15 post: add new article about docker
def5678 2024-01-20 post: add kubernetes guide
ghi9012 2024-02-01 post: add git basics

# Commit history after rebase
jkl3456 2024-02-13 post: add new article about docker
mno7890 2024-02-13 post: add kubernetes guide
pqr1234 2024-02-13 post: add git basics
```

The root of this issue lies in how Git constructs commit objects. Each commit contains:
- Reference to parent commit
- Author information
- Commit timestamp
- Commit message
- File tree state

When we modify a historical commit, its SHA-1 hash (commit ID) changes, and all subsequent commits that have it as an ancestor also receive new commit IDs. By default, these newly generated commits use the current time as their commit timestamp.

## How to Preserve Commit Timestamps

Git provides a special parameter `--committer-date-is-author-date` that can maintain original commit timestamps during rebase. Here's a practical example showing how to safely modify historical commits.

### Preparation

1. First, create a backup branch to save the current state:
```bash
git branch backup_branch
```

2. Use `git reflog` to view operation history and find the state before rebase:
```bash
git reflog
# Output example
1eab2a7 HEAD@{66}: commit: post: add regexp-advanced
11c25e4 HEAD@{67}: commit: post: add guide-to-docker
db01edd HEAD@{68}: commit: post: add kube-basics
```

### Executing the Fix

1. Return to the state before rebase:
```bash
git reset --hard HEAD@{66}  # Use the correct position found
```

2. Execute rebase with the new parameter:
```bash
git rebase -i --committer-date-is-author-date <commit-id>^
```

3. After modifying the target commit in the interactive editor:
```bash
git add <modified files>
git rebase --continue
```

4. Confirm that commit timestamps are correctly preserved:
```bash
git log --pretty=format:"%h %ad %s" --date=short
```

5. Recover new commits from the backup branch:
```bash
git cherry-pick <latest commit from backup_branch>
```

6. Push updates and clean up:
```bash
git push -f
git branch -D backup_branch  # Delete backup branch after confirming everything is correct
```

## Understanding the Mechanism

1. Commit Timestamps: Each commit in Git has two timestamps:
   - Author Date: When the patch was originally created
   - Committer Date: When the patch was applied to the repository

2. The role of `--committer-date-is-author-date`:
   - During rebase, sets each reapplied commit's committer date to its original author date
   - This ensures continuity and accuracy in the commit history's timeline

## Best Practice Recommendations

1. **Use Rebase with Caution**
   - For commits already pushed to remote, prefer using new commits to fix errors
   - If history modification is necessary, ensure these changes are limited to personal, unshared branches

2. **Protective Measures**
   - Create backup branches before important operations
   - Use `git reflog` to record operation history for potential rollbacks
   - Verify changes meet expectations before force-pushing updates

3. **Team Collaboration**
   - In team projects, ensure other members are aware of your planned history modifications
   - Choose appropriate timing to avoid disrupting others' work
   - Promptly notify team members to sync their repositories after execution

## Conclusion

Git's rebase functionality is powerful but requires careful use. Understanding how to properly maintain commit timestamps not only makes your commit history more accurate but also helps you safely modify historical commits when necessary. Remember, safety backups and careful verification are essential steps when performing such operations.

Through this practical example, we've not only learned how to fix commit timestamps after rebase but, more importantly, understood Git's underlying principles when handling commit history. This knowledge will help you better utilize Git as a powerful version control tool in your daily work.

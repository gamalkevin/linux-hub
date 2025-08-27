# Git Usage
Continuing from [git setup](./git-setup.md), we can now delve into `git` usage. Let's see what are the most-used commands on `git`. A cheat sheet, if you may.

---

## **1. Starting a project**
```
git init                # start a repo in an existing folder
git clone <url>         # copy repo from GitHub/GitLab
```


## **2. Checking status**
```
git status				# Shows what files changed, what’s staged, and what’s untracked.
```

## **3. Tracking changes**
```
git add file.txt        # stage one file
git add .               # stage all changes
git commit -m "Message" # commit with a message

# Rule of thumb: add → commit = record your work.
```

## ** 4. Looking back in time**
```
git log                 # show commit history
git diff                # see changes not staged
git diff --staged       # see changes staged for commit
```

## **5. Syncing with remote**
```
git pull                # get latest changes from remote
git push                # send your commits to remote
```

## **6. Branching (for features/experiments)**
```
git branch              # list branches
git branch new-feature  # create branch
git checkout new-feature # switch to branch 

# OR modern shortcut:
git switch -c new-feature
```

When done:
```
git checkout main
git merge new-feature   # merge into main
```

## **7. Remotes (GitHub, GitLab, etc.)**
```
git remote -v                     # show remotes
git remote add origin <url>       # add a remote
git push -u origin main           # first push
```

## **8. Undoing mistakes**
```
git restore file.txt          # undo unstaged changes
git restore --staged file.txt # unstage a file
git reset --hard HEAD         # reset everything to last commit (danger!)
```

---

## ⚡ Git Mindset
- **Commit often, push often**. Small commits with clear messages are easier to manage.
- Branches are cheap. Use them to keep main clean.
- Don’t panic. Git rarely loses data; it just hides it until you know how to restore it.
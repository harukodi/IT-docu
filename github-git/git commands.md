## Allow empty commit
```bash
git commit --allow-empty -m "message"
```
## Checkout branch
```bash
git checkout branch
```
## Merge branch
```bash 
git merge branch
```
## Add new files
```bash
git add .
```

## Switch branch
```bash
git switch branch
```

## Restore changes
```bash
git restore .
```

## Checkout and create new branch
```bash
git checkout -b new-branch-name
```
## Check git log
```bash
git log
```

## Move HEAD up
```bash
git checkout HEAD^
```

## Move HEAD up in a branch
```bash
git checkout repo^
```

## Get git autocompletion
**NOTE:** Source or add this to .bashrc
```bash
source /usr/share/bash-completion/completions/git
```

## Remove files that have not yet been added to the index of git
**NOTE:** `-f = force` `-d directories`
```bash
git clean -f -d
```

## Remove branch on remote
```bash
git push origin --delete branch-name
```

## Move files from one branch to another
```
git checkout branch-name -- path-or-folder
```

## Cherry-pick commits
```bash
git checkout branch-name
## Select the commit-hash with the command below
git cherry-pick branch-name commit-hash
```

## Git revert 
```bash
git revert commit-hash -n
```

## Git rebase (Helps to avoid conflict after a long cycle of development take changes from main to your side branch to see if you got any conflicts easier when coding on your side barnch for a long time)
```bash
git rebase branch-name
```

## Update local ref branches
```bash
git fetch -p
```
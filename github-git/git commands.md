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
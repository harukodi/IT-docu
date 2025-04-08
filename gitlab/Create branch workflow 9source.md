# Step 1
Create an issue on gitlab on this link [9source-issue-board](https://git.chasacademy.dev/chas-challenge-2025/grupp9_ad/-/boards)
# Step 2
Check what issue number you get on your issue. Your issue number can be seen on the issue board, on the bottom of the issue card/s.
# Step 3
## Create a new feature branch and connect to issue number
```bash
git checkout -b <issue-number>-FEATURE-<feature-name>
```
**Example:**
```bash
git checkout -b 34-FEATURE-my-feature
```
## Create a new bug branch and connect to issue number
```bash
git checkout -b <issue-number>-BUG-<bug-name>
```
**Example:**
```bash
git checkout -b 34-BUG-my-bug
```
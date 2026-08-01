---
name: Git bisect
tags: [Git, Shell]
---

# Git bisect

[Git bisect](https://git-scm.com/docs/git-bisect) is a convenient tool that uses binary search to find the source of bugs.

1. To start, enter the command
```bash
git bisect start
```

2. Find the not working commit
```bash
# working commit
git bisect good HASH
# not-working commit
git bisect bad main
# find the source of the not-working commit
# e.g., git bisect run bash -c '[[ $(PROGRAM CODE) == "OUTPUT" ]]'
git bisect run COMMAND
# stop the git bisect
git bisect reset
```

3. Go to the not-working commit and fix the bug
```bash
# ~/~1 or ^/^1 means the parent of NOT_WORKING_COMMIT
git switch -c FIX_BUG_BRANCH NOT_WORKING_COMMIT~1
```

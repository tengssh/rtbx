---
name: Git rebase
tags: [Git, Shell]
---

# Git rebase

[Git rebase](https://git-scm.com/docs/git-rebase) is an advanced Git command that can be used to reorder commits and modify the past committed content/messages.
This command could be useful for tidying up the commit history graph. However, rebasing a branch on which someone else has also worked may create a divergence in the history that becomes hard to maintain.

Here are examples of using typical merge and rebase, and the interactive rebase.

## Typical merge vs. rebase examples

0. Assume the following commit history with two branches (e.g., `main` and `feat`):
```bash
git log --oneline --graph --all
```
```
* 71e2971 (HEAD -> main) 3rd commit
| * 824b414 (feat) 2.1 commit
|/  
* cf13074 2nd commit
* 67e8634 1st commit
```

1. Merge
```bash
git merge feat # on main branch
# handle conflicts if any
git commit -am "merge commit"
git log --oneline --graph --all
```
```
*   45615e4 (HEAD -> main) merge commit
|\  
| * 824b414 (feat) 2.1 commit
* | 71e2971 3rd commit
|/  
* cf13074 2nd commit
* 67e8634 1st commit
```

2. Rebase
```bash
git switch feat
git rebase main
# handle conflicts if any
git add .
git rebase --continue
git switch main
git merge feat
git log --oneline --graph --all
```
```
* f630d68 (HEAD -> main, feat) 2.1 commit
* 71e2971 3rd commit
* cf13074 2nd commit
* 67e8634 1st commit
```

## Interactive rebase

0. Assume the following commit history with 6 commits on the `main` branch
```bash
git log --oneline --graph --patch --all
```
```
* 1b01694 (HEAD -> main) 6th commit
| diff --git a/print_one.py b/print_one.py
| index be38829..a3018ba 100644
| --- a/print_one.py
| +++ b/print_one.py
| @@ -1,2 +1,2 @@
| -# printing one
| +# print one
|  print(1)
* 55caaee 5th commit
| diff --git a/print_one.py b/print_one.py
| index a3018ba..be38829 100644
| --- a/print_one.py
| +++ b/print_one.py
| @@ -1,2 +1,2 @@
| -# print one
| +# printing one
|  print(1)
* 5379756 4th coolmit (typo in message)
| diff --git a/print_one.py b/print_one.py
| index b917a72..a3018ba 100644
| --- a/print_one.py
| +++ b/print_one.py
| @@ -1 +1,2 @@
| +# print one
|  print(1)
* a104f14 3rd commit
| diff --git a/print_one.py b/print_one.py
| index 421ef2f..b917a72 100644
| --- a/print_one.py
| +++ b/print_one.py
| @@ -1 +1 @@
| -print(1/0)
| +print(1)
* 7d90f4b 2nd commit (bug in code)
| diff --git a/print_one.py b/print_one.py
| index b917a72..421ef2f 100644
| --- a/print_one.py
| +++ b/print_one.py
| @@ -1 +1 @@
| -print(1)
| +print(1/0)
* 11d9a8d 1st commit
  diff --git a/print_one.py b/print_one.py
  new file mode 100644
  index 0000000..b917a72
  --- /dev/null
  +++ b/print_one.py
  @@ -0,0 +1 @@
  +print(1)
```

1. Assign actions to commits
    - fix the bug in the code of the 2nd commit
    - fix the typo in the commit message of the 4th commit
    - drop the 5th commit
    - squash the 6th commit with the previous commit
```bash
git rebase -i --root
#git rebase HEAD~5 # previous 5 commits
#git rebase 5097ddf
```
```vim
pick 11d9a8d 1st commit
edit 7d90f4b 2nd commit (bug in code)
pick a104f14 3rd commit
reword 5379756 4th coolmit (typo in message)
drop 55caaee 5th commit
squash 1b01694 6th commit
```

2. Work on each action
```bash
vi print_one.py # fix the bug
git add print_one.py
git rebase --continue # update 2nd commit message
git rebase --continue # update 4th commit message
# drop 5th commit
git rebase --continue # update squashed commit message
git log --oneline --graph --patch --all
```
```
* 4bb5b0d (HEAD -> main) 4th commit
| diff --git a/print_one.py b/print_one.py
| index b917a72..a3018ba 100644
| --- a/print_one.py
| +++ b/print_one.py
| @@ -1 +1,2 @@
| +# print one
|  print(1)
* e4fcd8b 2nd commit
* 11d9a8d 1st commit
  diff --git a/print_one.py b/print_one.py
  new file mode 100644
  index 0000000..b917a72
  --- /dev/null
  +++ b/print_one.py
  @@ -0,0 +1 @@
  +print(1)
```
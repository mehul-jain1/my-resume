---
layout: default
title:  "My Personal Git Cheat Sheet"
date:   2024-07-19 16:02:06 +0530
categories: git cheatsheet
---
I can never remember how to do everything, but here are the notes I have on
my work laptop that I refer to all the time. Maybe you'll find them useful.

Note: multi-line examples are sometimes just different ways to do the same thing,
and sometimes the recipe actually requires all the lines. You'll have to try them
out if it's not obvious from the context.

determine if commit is in a branch
```
git log | grep <commit_id>
git branch --contains $COMMIT_ID
git branch --contains <commit-id> | grep <branch-name>
```

show history of a file even if it was deleted
```
git log --full-history  -- myfile
```

clean up old remote branches

```
git remote prune origin --dry-run
git fetch origin --prune
git config remote.origin.prune true
```

undo local changes
```
git checkout .
```

revert changes made to index
```
git reset
```

revert a single committed change
```
git revert <sha>
```

remove untracked files
```
git clean -f
```

remove untracked files and directories
```
git clean -fd
```

get first commit
```
git config --global alias.first "rev-list --max-parents=0 HEAD"
git first
```

pull and merge master into branch
```
git pull origin master
```

reset branch to be just like remote repo
```
git fetch origin
git reset --hard origin/branchname
```

search between revs for a regex
```
git grep <regexp> $(git rev-list <rev1>..<rev2>)
```

undo a reset

https://stackoverflow.com/questions/2510276/how-to-undo-git-reset

```
git reset 'HEAD@{1}'
```

checkout a file from a stash

https://stackoverflow.com/questions/1105253/how-would-i-extract-a-single-file-or-changes-to-a-file-from-a-git-stash

```
git checkout stash@{0} -- <filename>
```

list only files changed in stash
```
git diff --name-only stash@{0}
```

show untracked files in a stash

https://stackoverflow.com/questions/12681529/in-git-is-there-a-way-to-show-untracked-stashed-files-without-applying-the-stas

"Untracked files are stored in the third parent of a stash commit."

```
git show stash@{0}^3
```

update deleted remote branches
```
get fetch -p
```

undo last commit but keep changes
```
git reset HEAD^
```

rebase on first commit
```
git rebase -i --root
```

cherry pick range

note: first hash is oldest and not included

```
git cherry-pick <older ref>..<newer ref>
```

to include the oldest hash in the range:
```
git cherry-pick <older ref>^..<newer ref>
```

compare commits of two branches
```
git log branch1 branch2
git log --left-right --graph --cherry-pick --oneline feature...branch
```

undo a commit in the middle of a commit sequence
```
git revert <sha>
```

check out commit as branch
```
git checkout -b branchname <sha>
```

split an existing commit

during rebase, mark commit to split with "edit"

when git presents the commit to edit:

1. reset state to previous commit with `git reset HEAD^`
2. use `git add` and `git commit` as usual to re-commit the changes over multiple commits
3. run `git rebase --continue` when satisfied with the new commits

change author of previous commit

interactive rebase, mark commit with "edit"

when git prompts for change:
```
git commit --amend --author="Author Name <email@address.com>" --no-edit
```
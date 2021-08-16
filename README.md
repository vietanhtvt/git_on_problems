# git_on_problems
This content involves dangerous operations, so please take full responsibility.
Iead from: https://qiita.com/muran001/items/dea2bbbaea1260098051


## On local branch

### Issue 1: When you what to edit last commit.
```
$ git commit --amend
#Then, change your commit messsage.
```

### Issue 2: When commited with unwanted author

#### - Case 1: If you want to change the HEAD user name or email address
```
$ git commit --amend -m "your commit" --author="user.name <user.email>"
```
#### - Case 2: If you want to change the previous commit
```
$ git rebase -i <commit>

====
# Edit commit you want

# Before edit
pick aa11bbc コミットメッセージ１
pick b2c3c4d コミットメッセージ２
pick 4e56fgh コミットメッセージ３
・・・

# After edit
edit aa11bbc コミットメッセージ１
pick b2c3c4d コミットメッセージ２
pick 4e56fgh コミットメッセージ３
・・・
====

# After that edit like case 1
$ git commit --amend -m "コミットメッセージ" --author="user.name <user.email>"

# then use --continue
$ git rebase --continue
```
#### - Case 3: (DANGER) Rewrite the entire commit history
```
git filter-branch --commit-filter '
        if [ "$GIT_COMMITTER_NAME" = "<Old Name>" ];
        then
                GIT_COMMITTER_NAME="<New Name>";
                GIT_AUTHOR_NAME="<New Name>";
                GIT_COMMITTER_EMAIL="<New Email>";
                GIT_AUTHOR_EMAIL="<New Email>";
                git commit-tree "$@";
        else
                git commit-tree "$@";
        fi' HEAD
```
### Issue 3: When you have committed an extra file but want to ignore it
#### - Case 1: Using new commit to delete
```
# First remove the files you want to remove
$ git rm --cached <tên file>

# Then add to .gitignore
$ echo '<tên file>' >> .gitignore

# Then create a new commit
``` 
#### - Case 2: Remove like this file never appeared 
```
# Delete only from history (remains in working tree)
$ git filter-branch -f --index-filter 'git rm --cached -rf --ignore-unmatch <ファイル名>' HEAD

# Delete from history and working tree
$ git filter-branch -f --index-filter 'git rm -rf --ignore-unmatch <ファイル名>' HEAD
```
### Issue 4: When you want to rename your branch name
```
# When you current on the branch
$ git branch -m <new_branch_name>
# When you on the other branch
$ git branch -m <old_branch_name> <new_branch_name>
```
### Issue 5 : When I was confused and wanted to do without the commit itself
#### - Case 1: When no one knows (= others have not pulled back yet).
```
# # Rewind history with one of three depending on your case
# 1.Restore only HEAD
$ git reset --soft HEAD~

# 2.Restore HEAD and index
$ git reset HEAD~

# 3 Restore the index and working tree up to the previous commit
$ git reset --hard HEAD~
```
#### - Case 2: When someone pull this commit 
```
# If you change your commit it will take a some conflicts let use revert
$ git revert <commit>
```
 

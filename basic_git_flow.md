# Basic Git flow

## Create a repository (outside of GitHub)

Usually create the repo in GitHub and clone. But to create locally:

`git init` in directory where you want repo

And create a default main branch

`git checkout -b main`

## Track changes

`git diff` to see changes between current tracked files and committed files
`git add [filename]` to add changed file toi staging area
`git diff --staged` to see differences in staged files from committed files

`git diff [id] [filename]` Show differences between current and committed version
Note: ID can also be `HEAD` or `HEAD~2` for two commits behind head

`git status` to see files with tracked changes

`git commit -m "[message text]"` To commit with comment

`git log` to see commits
`git log -[n]` to see last n commits
`git log --oneline -[n]` for a shorter output (commit ID and commit mesdsage)


## Common working branches

* `main` - never usually work on this branch itself. Update it from the `dev`
branch only pretty sure `dev` is working well.

* `dev` - never usually work on this branch itself if there is more than one 
worker. Used to collate changes/additions from multiple workers. When everything
appears OK, update `master`.

* Worker branches, such as `mike` or `tom` or more descriptive, such as 
`features/mike` or `bug_fixes\mike`. Work on this branch and merge with `dev`.

## To copy a file between branches

Run this from the branch where you want the file to end up:

`git checkout otherbranch myfile.txt`

## To check which branch you are on, change branches, or create a branch

`git status` checks which branch you are on

New:

`git switch [branch name]` switches to [branch name]
`git switch -c [branch name]` creates and switches to [branch name]

Old:

`git checkout [branch name]` switches to [branch name]
`git checkout -b [branch name]` creates and switches to [branch name]

(or git branch newname and git checkout newname)

Use `git push` to push up a local copy of a branch. If the branch does not exist
in the remote git repo git will give you the command to type in.

## To update a local branch

`git switch dev` - go to `dev`
`git pull` - update dev or `git fetch` and `git checkout` (fetch isolates content without updating local content. checkout will then update local content).

`git checkout [my branch]` - switch to my branch (now `git switch [my branch]`)
`git merge dev` - to merge `dev` (and any updates) into my branch

## To update dev from my local branch

`git add [files]` - to add local new files 
`git commit` - to commit changes
`git push` - to push up to remote branch of the same name

Then go to remote git repo on the web and raise a pull or merge request, merging
your branch into dev. This will usually delete your branch on the remote repo 
(this is what you want to happen normally). To delete a local branch use
`git branch -d [branch name]`

## Prune old branch references (local - remote links)

git fetch --prune

## Restore a previous version of a file

New:

`git restore [commit id] [filename]`

Old: 

`git checkout [commit id] [filename]`
`git checkout HEAD [filename]` will reverse any changes to files and restore to HEAD


## How to reset a Git branch to a remote repository

Save the state of your current branch in another branch,  named my-backup,in 
case something goes wrong

`git commit -a -m "Backup."`
`git branch my-backup`

Fetch remote branch and reset local. e.g. to reset master from origin(remote)
`git fetch origin`
`git reset --hard origin/master`

## Logs

git fetch --all 
git log --all (includes all history)

## Git merge

* Checkout branch to merge into 
* git merge branchname_merged_from

* git branch -D branch_to_delete


## Tag

Tagging is ofen best done on GitHub

git tag -a v0.0.1 -m "First release, 0.0.1"

or

git tag 0.1.0

git push --tags

## Clean (uncommited files)

If you just clean untracked files, run git clean -f
If you want to also remove directories, run git clean -f -d
If you just want to remove ignored files, run git clean -f -X
If you want to remove ignored as well as non-ignored files, run git clean -f -

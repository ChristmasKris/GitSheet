# GitHub/Git/Terminal commands

- [Correct order of operations](#correct-order-of-operations "Go to section")
- [Navigation](#navigation "Go to section")
- [Repository](#repository "Go to section")
- [Branch](#branch "Go to section")
- [Commit](#commit "Go to section")
- [Staging](#staging "Go to section")
- [Stash](#stash "Go to section")
- [Nano text editor](#nano-text-editor "Go to section")
- [Defaults](#defaults "Go to section")

## Correct order of operations

**How to generate SSH keys for secure authentication with GitHub?**

1. Navigate to your SSH directory like so: ```cd ~/.ssh```.
2. Remove any existing ssh keys which are the ```id_*******``` and ```id_*******.pub``` or ```id_rsa``` and ```id_rsa.pub``` files inside of the .ssh directory.
3. Generate a new SSH key using the Ed25519 algorithm like so: ```ssh-keygen -t ed25519 -C "your-email@gmail.com"```.
4. When prompted to "Enter a file in which to save the key," press Enter to accept the default location which is ```(~/.ssh/id_ed25519)```.
5. When prompted to "Enter passphrase," you can either enter a secure passphrase or leave it empty for no passphrase. Leaving it empty is easier.
6. Start the SSH agent in the background like so: ```eval "$(ssh-agent -s)"```.
7. Add your new SSH key to the SSH agent like so: ```ssh-add ~/.ssh/id_ed25519```.
8. Add the SSH key to your GitHub account by viewing your public key like so ```cat ~/.ssh/id_ed25519.pub```.
9. Log in to your GitHub account in your browser.
10. Go to settings > SSH and GPG keys > New SSH key.
11. Give the key any title that you prefer.
12. Pick authentication as the key's type in the dropdown.
13. Paste your key without the last space and the e-mail address part which will look something like: ```ssh-ed25519 your-key-will-be-here your-email@gmail.com```.
14. Test the SSH connection like so ```ssh -T git@github.com```.
15. Then you should see a message like ```Hi username! You've successfully authenticated, but GitHub does not provide shell access.```.
16. Done.

**How to initialize a folder with an existing remote repository?**

1. Use [```git clone```](#cloneCommand "Go to command") to clone a repository into your current folder by copying their SSH url which looks something like ```git@github.com:username/repository.git```.
2. Done.

**How to upload changes from your current branch to your remote repository?**

1. Update your local branch with the remote master branch with [```git pull```](#pullCommand "Go to command") to be able to resolve any conflicts that may occur locally instead of remotely.
2. Make all your changes here and proceed when done with everything for the upcoming commit.
3. Stage the changes with [```git add```](#addCommand "Go to command").
4. Commit the changes with [```git commit```](#commitCommand "Go to command").
5. Push the changes with [```git push```](#pushCommand "Go to command").
6. If you want to do the next steps through the terminal then continue here, otherwise finish it on GitHub.
7. Go into the master branch.
8. Pull remote master branch into your local master branch with [```git pull```](#pullCommand "Go to command").
9. Merge branch into master if everything is good with [```git merge```](#mergeCommand "Go to command").
10. Delete merged branch locally with [```git branch -d```](#deleteLocalBranchCommand "Go to command") and remotely with [```git push origin --delete```](#deleteRemoteBranchCommand "Go to command").
11. Create a pull request on GitHub to merge your current branch into the remote main (or master) branch.
12. Done.

**How to delete a file from the local & remote repository?**

1. Delete the file with [```git rm```](#deleteRemoteCommand "Go to command").
2. Commit the changes with [```git commit```](#commitCommand "Go to command").
3. Push the changes with [```git push```](#pushCommand "Go to command").
4. Done.

## Navigation

<span id="cloneCommand"/>

Clone a remote repository into your local folder. The ```.``` at the end specifies the current directory as the destination for the cloned repository instead of creating another folder inside of the current directory.

```shell
git clone https://github.com/username/repository.git .
```

View settings.

```shell
git config --list
```

Move to directory.

```shell
cd ./myDirectory
```

Edit in VS Code.

```shell
code ./myFile.txt
```

Edit in terminal.

```shell
nano ./myFile.txt
```

Check status of staged files.

```shell
git status
```

List all branches and see which you are currently one.

```shell
git branch
```

List all remote branches.

```shell
git branch -r
```

Switch to a branch.

```shell
git checkout branch-name
```

View existing remote repositories.

```shell
git remote -v
```

View the current directory.

```shell
pwd
```

View the current directory's contents.

```shell
ls
```

Go into the desktop folder in git-bash.

```shell
cd ~/Desktop
```

<span id="deleteRemoteCommand"/>

Delete a file from repository

```shell
git rm ./myFile.txt
```

Delete a file from local system

```shell
rm ./myFile.txt
```

## Repository

Fetch latest changes from remote repository.

```shell
git fetch
```

Link your local repository to a remote repository without cloning the remote one.

```shell
git remote add origin remote-repository-url
```

<span id="pushCommand"/>

Push changes from branch to remote repository, use ```-u``` flag when command has not been used yet for the local branch to track the remote branch.

```shell
git push -u origin branch-name
```

```shell
git push origin branch-name
```

<span id="pullCommand"/>

Update local branch (recommended to do this regularly) with changes from remote repository without altering files that you have worked in locally. Then merge the fetched changes into your local branch (see 2nd command). Or do it all in one command (see 3rd command) or (see 4th command) if 3rd command used already at least once to track remote branch.

```shell
git fetch origin
```

```shell
git merge origin/branch-name
```

```shell
git pull origin branch-name
```

```shell
git pull
```

## Branch

Create and switch to a new branch.

```shell
git checkout -b new-branch-name
```

View differences between 2 branches.

```shell
git diff branch-name1 branch-name2
```

<span id="mergeCommand"/>

Merge another branch into the current branch.

```shell
git merge other-branch-name
```

<span id="deleteLocalBranchCommand"/>

Delete local branch if fully merged. Use ```-D``` (2nd command) to force delete even if it has not been merged.

```shell
git branch -d branch-name
```

```shell
git branch -D branch-name
```

<span id="deleteRemoteBranchCommand"/>

Delete remote branch.

```shell
git push origin --delete branch-name
```

## Commit

<span id="addCommand"/>

Add all files in current directory and subdirectories to next commit.

```shell
git add .
```

Add a specific file to next commit.

```shell
git add ./myFile.txt
```

<span id="commitCommand"/>

Commit staged files to local repository and include short message. Or include long message (see 2nd command).

```shell
git commit -m "Brief description comes here"
```

```shell
git commit
```

View commit history. Or view them on one line (see 2nd command).

```shell
git log
```

```shell
git log --oneline
```

Revert changes in a specific file to last committed state.

```shell
git restore ./filename.txt
```

Revert all changes in the current directory to last committed state.

```shell
git restore .
```

Revert the most recent commit that has been made already.

```shell
git revert HEAD
```

Revert a specific commit.

```shell
git revert commit-hash-here
```

Delete all recent local commits.

```shell
git reset --hard origin/branch_name
```

View commit log.

```shell
git log
```

Exit commit log.

```shell
Q
```

## Staging

View differences between local worked in files and the staging area.

```shell
git diff
```

View changes that are staged for the next commit.

```shell
git diff --staged
```

Unstage a specific file.

```shell
git reset HEAD ./filename.txt
```

Unstage all files.

```shell
git reset HEAD
```

## Stash

Stash current changes, also staged changes and return directory to clean state.

```shell
git stash
```

List all stashed changes.

```shell
git stash list
```

Apply most recent stash.

```shell
git stash apply
```

Apply and remove most recent stash.

```shell
git stash pop
```

Apply a specific stash.

```shell
git stash apply stash@{stash number}
```

Apply and remove a specific stash.

```shell
git stash pop stash@{stash number}
```

Delete a specific stash.

```shell
git stash drop stash@{stash number}
```

Delete all stashes.

```shell
git stash clear
```

## Nano text editor

Edit/create a file.

```shell
nano ./myFile.txt
```

Display help menu.

```
CTRL + G
```

Save file.

```
CTRL + O
```

Exit file (prompt to save/discard changes will pop up).

```
CTRL + X
```

## Defaults

Set default text editor to VS Code and wait until editor window closed to proceed.

```shell
git config --global core.editor "code --wait"
```

Set default text editor to nano (terminal editor).

```shell
git config --global core.editor "nano"
```

View default text editor configuration.

```shell
git config --global core.editor
```

Show branch in terminal PS (process status). Paste the following into the ```.bashrc``` file. This will change the PS in non-git directories to the current path like so: ```~/Desktop>``` and in git directories like so: ```repository/branch-name>```. It will also make the branch name cyan colored.

```shell
function parse_git_repo {
   git rev-parse --show-toplevel 2>/dev/null | xargs basename
}

function parse_git_branch {
	git branch 2>/dev/null | sed -n '/\* /s///p'
}

PS1="\$(if git rev-parse --is-inside-work-tree &>/dev/null; then echo \"\$(parse_git_repo)/\[\033[0;36m\]\$(parse_git_branch)\[\033[0m\]\"; else echo \"\w\"; fi)> "
```
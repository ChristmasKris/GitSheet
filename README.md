# GitSheet: A Comprehensive Git & Terminal Command Reference

## What is GitSheet?

GitSheet is a curated, easy-to-navigate reference for essential Git, GitHub, and terminal commands. It’s designed to help developers quickly find the right command for common version control and workflow tasks, from repository setup to advanced branching and deployment. The project also includes practical tutorials for real-world scenarios, such as SSH key management and live deployment.

## Why I Built This Project

As a developer, I often found myself searching for the same Git commands and best practices, especially when switching between projects or onboarding new team members. I built GitSheet to solve this problem: to have a single, reliable resource that covers not just the commands, but also the context and reasoning behind them. It’s meant to save time, reduce mistakes, and help others ramp up quickly with Git and terminal workflows.

## Technologies & Tools Used

- **Markdown** for clear, portable documentation
- **Git** and **GitHub** for version control and collaboration
- **Shell scripting** for advanced configuration and automation examples
- **Nano** and **VS Code** as featured editors in workflow examples

## Key Features & Code Examples

### 1. Step-by-Step Tutorials

The project includes detailed, copy-paste-ready tutorials for common and advanced tasks. For example, generating SSH keys for GitHub:

```shell
ssh-keygen -t ed25519 -C "your-email@gmail.com"
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
cat ~/.ssh/id_ed25519.pub
```

### 2. Command Reference with Explanations

Every major Git operation is covered, with concise explanations and real command examples:

```shell
git clone https://github.com/username/repository.git .
git status
git branch
git checkout -b new-branch-name
git add .
git commit -m "Initial commit"
git push -u origin master
```

### 3. Advanced Workflow Tips

The documentation goes beyond basics, including deployment and branch management:

```shell
# Initialize a bare repository for live deployment
ssh user@your-server-ip
cd /home/username/
git init --bare repositoryName.git

# Example post-receive hook for auto-deployment
#!/bin/bash
GIT_WORK_TREE=/home/username/public_html
export GIT_WORK_TREE
git checkout -f master -- . ':!.gitignore'
```

### 4. Editor Shortcuts and Customization

Quick reference for using Nano and setting up your preferred editor:

```shell
git config --global core.editor "code --wait"
git config --global core.editor "nano"
```

### 5. Shell Prompt Customization

For power users, GitSheet includes shell prompt customization to show the current repo and branch:

```shell
function parse_git_repo {
	git rev-parse --show-toplevel 2>/dev/null | xargs basename
}

function parse_git_branch {
	git branch 2>/dev/null | sed -n '/\* /s///p'
}

PS1="$(if git rev-parse --is-inside-work-tree &>/dev/null; then echo \"$(parse_git_repo)/\033[0;36m$(parse_git_branch)\033[0m\"; else echo \"\w\"; fi)> "
```

## What Have I Learned?

Building GitSheet deepened my understanding of Git’s inner workings and the value of clear, actionable documentation. I learned how to distill complex workflows into simple, repeatable steps, and how to anticipate the needs of both beginners and experienced developers. This project also reinforced the importance of automation and customization in developer productivity.

If you’d like to know more about the project, feel free to reach out!

# GitHub/Git/Terminal commands

- [Tutorials](#tutorials "Go to section")
- [Navigation](#navigation "Go to section")
- [Repository](#repository "Go to section")
- [Branch](#branch "Go to section")
- [Commit](#commit "Go to section")
- [Staging](#staging "Go to section")
- [Stash](#stash "Go to section")
- [Nano text editor](#nano-text-editor "Go to section")
- [Defaults](#defaults "Go to section")

## Tutorials

**How to generate SSH keys on your pc for secure authentication with GitHub?**

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
<br><br>

**How to generate SSH keys on your pc for secure authentication with your webhost?**

If you already have an SSH key for GitHub for example, you can reuse that public key for your webhost aswell.

1. Execute the following command: ```ssh-keygen -t rsa -b 4096 -C "yourEmail@gmail.com" -f ~/.ssh/id_rsa_custom_name```.
2. Press enter to accept the default file location.
3. You can enter a password for extra protection if you would like to.
4. Add the SSH key to the SSH agent by running the following command: ```eval "$(ssh-agent -s)"```.
5. Add your SSH private key to the agent by running the following command: ```ssh-add ~/.ssh/id_rsa_custom_name```.
6. Add the public key ```~/.ssh/id_rsa_custom_name.pub``` to your webhost.<br>
If you want to create a custom profile for easier SSH login, then follow steps 7 and 8, otherwise skip these steps.
7. Edit the SSH config file with the following command: ```nano ~/.ssh/config``` and paste the following code. Replace what's necessary with your credentials.
```
Host yourdomain.com
	HostName yourdomain.com
	User yourCPanelUsername
	Port 0000
	IdentityFile ~/.ssh/id_rsa_custom_name
```
8. Now you can SSH into your webhost by running the following command: ```ssh yourdomain.com```.
9. Done, you can now SSH into your webhost with an SSH key and without a password by starting an SSH connection your usual way ```ssh -p port username@yourdomain.com```.
<br><br>

**How to Initialize a GitHub Repository for an Existing Project & create a .gitignore?**

1. Create a new repository on GitHub without a README.md.
2. Initialize Git in your project's local directory with ```git init```.
3. Temporarily move any config or environment variables outside of your project if you would not like these to be uploaded aswell.
4. Create a ```.gitignore``` file in your project's local main directory.
5. Include all paths/files in this file that you would like Git to ignore so that they stay offline like so:
	```shell
	.env
	php/config.php
	js/general/data.js
	```
6. Stage your files with ```git add .```.
7. Commit your files with ```git commit -m "Initial commit"```.
8. Link your local repository to your GitHub repository and replace ```username``` with your GitHub username and ```repository-name``` with your online repository's name. Run the following command: ```git remote add origin https://github.com/username/repository-name.git```.
9. Push your local commits to GitHub with ```git push -u origin master```. You can name your main branch anything, in this case it's ```master```.
10. Put your ignored files back into your local project and they will stay offline.
11. Done.
<br><br>

**How to move your progress/changes into a new branch?**

1. With ```git stash``` you can temporarily "stash" your changes from your current branch.
2. Then with ```git stash branch new-branch-name``` you can put the changes into a new branch and the stash will get deleted automatically.
<br><br>

**How to set up production on a web host for live deployment?**

1. SSH into your live server.
2. In your ```home/username/``` directory, create a folder where your repository will be located in. The name of this folder can be anything. (do not put the folder into public_html or any web-accessible directory)
3. Then go into this new folder and run the following command to initialize a bare repository: ```git init --bare repositoryName.git```. The name of this repository can be anything.
4. Go into your repository and then go into the generated ```hooks``` folder.
5. Run the following command: ```nano post-receive```. This will create a file called ```post-receive```. This file will not have or need an extension.
6. Paste the code below into the file and replace ```/home/username/public_html``` with the path to your web-accessible directory. Then save and exit the file. This also excludes the ```.gitignore``` file from being uploaded to your web-accessible directory.
	```
	#!/bin/bash
	GIT_WORK_TREE=/home/username/public_html
	export GIT_WORK_TREE
	git checkout -f master -- . ':!.gitignore'
	```
7. Make the file executable by running the following command: ```chmod +x post-receive```. You may now close the connection to your server.
8. Open your local terminal and navigate to your local project directory.
9. Run the following command to add the live repository to your local project: ```git remote add production ssh://user@your-server-ip:port/home/username/repo/repositoryName.git```. Replace what's necessary with your credentials.
10. To upload your master branch to your live website, run the following command: ```git push production master```.
11. Done.
<br><br>

**How to initialize a folder with an existing remote repository?**

1. Use [```git clone```](#cloneCommand "Go to command") to clone a repository into your current folder by copying their SSH url which looks something like ```git@github.com:username/repository.git```.
2. Done.
<br><br>

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
<br><br>

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

Create a directory

```shell
mkdir myFolder
```

Create a file

```shell
touch myFile.txt
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

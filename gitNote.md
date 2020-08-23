# Git learning note ------- Yijuan Zhi

## Git and Github Setup

### basic setup
1. `git --version`print out the version of my git
2. `git config --global user.name "Yijuan Zhi"` set up my name
3. `git config --global user.email "zjzyj520@gmail.com"` set up my email

### Connect Git to Github Account
1. `ssh-keygen -t rsa -C 'zjzyj520@gmail.com'` >>>create a key pair, and connect computer to github
2. `cd ~/.ssh` >>> the folder we used to execute commands in github remotely
3. (in .ssh foler) `cat id_rsa.pub` >>> get the key which is to verify your computer and your github account is the same person
4. (in the folder I want to upload to github) `git remote add origin url` >>> add the folder in the github folder
5. `git remote -v` >>> showing the folder we have in github
6. `git push origin master` >>> push the master branch content to master branch in github
7. `git push origin develop` >>> push the develop branch content to develop branch in github
8. `git flow feature publish new-feature` >>> push the feature/new-feature, if we don't have this branch in github, it's going to create the branch in github and our computera
   

## Git Commands

#### Git Basics
1. `git init .` >>> Initialize empty Git repository
2. `git status` >>> showing the stage for the time being, files added but not committed yet
3. `git add file.f` >>> tracking the file(s), can add serveral files into one **stage**
4. `git add .` >>> add all files into track list, put them on **stage**
5. `git commit -m "note"` >>> commit file(s) and throw a message
6. `git log` >>> showing the log of the file
7. `git diff file.f` >>> showing the difference since last time modified
8. `git commit -am "note"` >>> commit all the files, then throw a message
9. `git push -u origin master` push the commited files into github master branch
10. `git commit -am "note"` is equal to `git add .` plus `git commit -m "note"`

#### Version Control(unstage, rollback etc)
1. `git restore --staged <file>` to unstage the file
2. `git restore <file>` to discard changed in working directory, rollback to the previous committed version


#### Git stash
Use git stash when you want to record the current state of the working directory and the index, but want to go back to a clean working directory. The command saves your local modifications away and reverts the working directory to match the HEAD commit.
1. `git stash`  stash the current state into a temp folder, and revert to the head of last commit
2. `git stash apply` pull the stash back into the current working tree
3. `git stash list` list all the stashes stored
4. `git stash clear` clear the stash store

#### Git branch, switch, merge and delete
1. `git branch newBranch` >>> create a new branch
2. `git branch` >>> showing us what branches we have and which one we are in
3. `git switch newBranch` >>> switch to the branch newBranch or you can use 'git checkout newBranch'
4. `git checkout -b newBranch2` >>> create a branch newBranch2 and switch to it (newBranch2 is created base on the current branch, say it is newBranch)
5. `git merge newBranch2` >>> merge newBranch2 into current branch(it also commit changes into the newBranch)
6. `git branch -d newBranch2` >>> delete the branch newBranch2

#### Git flow
1. `git flow init -d` >>> initialize git flow and use the default setting (create a branch develop and switch to it)
2. `git flow feature start newfeature1` >>> create a branch feature/newfeature1 and switch to it
3. `git flow feature finish newfeature1` >>> merge the feature/newfeature1 to develop and delete the branch (use 'i' to insert comments, 'esc' to exit insert mode, write ':wq' to finish commenting)
4. `git flow feature publish new-feature` >>> push the feature/new-feature, if we don't have this branch in github, it's going to create the branch in github and our computer
5. `git flow feature` >>> show the branches we have in feature

#### Synchronizing Files Between Computers

##### clone from repo url or connect to remote repo
- `git remote add origin <url>` requires this at the first time, connect to github repo
- `git clone <url>` clone the target files into the directory that you are in, by default it clones and only clones the master branch
- `git clone -b <branchName> <url>` clone and only clone specific branch from the remote repo

##### pull modified files back to another computer
- `git fetch` add the new added file into local git but doesn't merge
- `git diff master origin/master` after downloading files, we check the difference
- `git pull origin master` if we are fine with the diff, fetch and merge the files back into the computer
- `git pull` is equal to `git fetch` plus `git merge`, the later one is prefered, you can use `git diff` after `git fetch`to see if everything is good

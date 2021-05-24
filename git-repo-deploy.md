# Setting up Git Repo to be a Production Deploy
For when you want to setup your development environment to easily publish to production through a Git Repo. An SSH user is required.
https://www.youtube.com/watch?v=-SCRORru2Gk

## To Do on Server
SSH into server and do the following procedures.
* Create Git repo directory outside of public_html
  * `git init --bare [folder]`
* Go to hooks folder (within repo folder)
* Create new file: post-receive
  * `touch post-recieve` 
  * `chmod +x post-receive`
* Edit post-receive with script (modify script as needed to work with your environment)
  * `vim post-receive` 
  * Press i to go into insert mode
  * Escape to get out of insert mode
  * :wq to save and exit

```#!/bin/bash<br>
TARGET="/home/[[domain-folder]]/public_html"
GIT_DIR="/home/[[domain-folder]]/[[repo-folder]]/[[sub-repo-folder]]"
BRANCH="master"

while read oldrev newrev ref
do
        # only checking out the master (or whatever branch you would like to deploy)
        if [ "$ref" = "refs/heads/$BRANCH" ];
        then
                echo "Ref $ref received. Deploying ${BRANCH} branch to production..."
                git --work-tree=$TARGET --git-dir=$GIT_DIR checkout -f $BRANCH -- dist
                cd $TARGET
                cp -r dist/* .
                rm -rf dist
        else
                echo "Ref $ref received. Doing nothing: only the ${BRANCH} branch may be deployed on this server."
        fi
done
```
  
## Setup Local Repo
* Create local repo on your computer
  * `git init`
* Link with Remote Repo
  * `git remote add [remote-name] ssh://user@host/home/[[domain-folder]]/[[repo-folder]]` 
  * [remote-name] - typically go with production
* Push to branch - DONE
  * `git push [remote-name]`
    
## Setup SSH Connection on MAC so don't have to do password each time
  * Terminal - create keygen
    * `ssh-keygen` (follow prompts)
    * `ssh-copy-id user@host`

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
  
## Setup Local Repo
* Create local repo on your computer
  * `git init`
* Link with Remote Repo
  * `git remote add [remote-name] ssh://user@host/[folder-location]` 
  * [remote-name] - typically go with production
  * [folder-location] - location on the server setup in step 1
* Push to branch - DONE
  * `git push [remote-name]`
    
## Setup SSH Connection on MAC so don't have to do password each time
  * Terminal - create keygen
    * `ssh-keygen` (follow prompts)
    * `ssh-copy-id user@host`

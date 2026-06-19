# git2github testing

Learning to tie a local git project into a github repository

* create local project

* git init -b main

* git configure
    * --local core.sshCommand "ssh -i ~/.ssh/<GH_SSH_KEY>"
    * --local user.name <GH_USERNAME>
    * --local user.email <GH_EMAIL>

* gh auth login

* gh repo create <GH_REPO_NAME> --public --source=. --remote=origin --push

* git remote -v

< make and commit changes >

* git push

< add action / workflow from GUI >

* git pull origin main

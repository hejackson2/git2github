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

* modify ~/.ssh/config to have Host section unique for the <GH_USERNAME> IdentityFile

```
## github IdentityFiles
Host github.com-<GH_USERNAME>
  Hostname github.com
  User git
  IdentityFile ~/.ssh/github_<GH_USERNAME>_id_ed25519
```

* add git remote URL
```
git remote add origin git@github.com-<GH_USERNAME>:<GH_USERNAME>/<GH_REPO_NAME>
```

NOTE:  The 'github.com-<GH_USERNAME>' is what ssh uses to use the correct IdentityFile for push/pulls

* git remote -v

< make and commit changes >

* git push

< add action / workflow from GUI >

* git pull origin main

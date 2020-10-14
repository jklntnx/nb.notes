# nb setup
## set up nb notebook on a mac to sync with a remote private github repo

# create a github account
*  user = jklntnx
*  email = jason.lindemuth@nutanix.com
*  pass = <pass>
*  follow instructions to verify account

# tell guthub to use 'master' as default
*  log into github.com as <user>
*  click on user icon in upper right and select "Settings"
*  on the left, select "Repositories"
*  change default branch from "main" to "master" and Update

# set up github private project
*  log into github as <user>
*  create a new repository
   -  name = notes
   -  set to private

# create a github personal access token (PAT)
*  log into github.com
*  click on user icon in upper right and select "Settings"
*  on the left, select "Developer Settings"
*  on the left, select "Personal access tokens"
*  click on "Generate new token" button
   -  add a note specifying its purpose (eg "nutanix notes")
   -  select "repo" in scope
   -  hit "Generate token" button
   -  save the PAT string in a safe place
* reference:
* https://docs.github.com/en/free-pro-team@latest/github/authenticating-to-github/creating-a-personal-access-token

set up github credential helper to cache the token
```
$ git config --global credential.helper osxkeychain
```

set up the user name you want to show up in commits
```
$ git config --global user.name "Jason Lindemuth"
```

set up email address
```
$ git config --global user.email jason.lindemuth@nutanix.com
```

confirm git global settings
```
$ git config --global -l
user.name=Jason Lindemuth
user.email=jason.lindemuth@nutanix.com
credential.helper=osxkeychain
```

# start a new local notebook
```
$ nb notebooks add notes
```

# make it the current
```
$ nb use notes
```

# set the notebook remote repo
```
$ nb remote set https://github.com/jklntnx/notes.git
```

# force git authentication (something appears broken, thus this step)
```
$ cd ~/.nb/notes
$ git push --set-upstream origin master
paste PAT string and hit enter twice
```

# confirm on github.com that .index is in the repo

# add the first file
```
$ nb add -t "nb setup"
```

# confirm the new file "nb_setup.md" is in github.com repo

# now, all nb adds and edits will automatically sync local notebook changes to remote repo

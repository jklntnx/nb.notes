# pass: portable cli password manager

**description**: how to set up a command-line password manager on a mac backed by
a local git repo and synchronized to a github.com private repo online.  


## *prerequisite*: install apple xcode cli tools (CLT)
```
$ xcode-select --install
```
this launches a dialog window...on catalina it fails with an error in this case,
install the xcode tools manually from:
```
https://developer.apple.com/downloads/more
```
download "Command Line Tools for Xcode 12" and install

## install homebrew for mac os package management
```
$ /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
```

## install pass itself
```
$ brew install pass
```
this will install pass and all dependencies (including gpg)

## create a gpg key for pass
```
$ gpg --full-generate-key
```
I recommend the following input:
- use 1) RAS and RSA key type (default)
- choose 4096 for keysize
- no expiration
- enter your name
- enter your email address
- enter a description in comment, eg "pass master key"
- enter a passphrase for the key (this should be a very strong passphrase and
  one you cannot forget)

## note the key ID
```
$ gpg --list-keys
/Users/jason.lindemuth/.gnupg/pubring.kbx
-----------------------------------------
pub   rsa4096 2019-09-06 [SC]
      CB59A40E5ECD1EB873FB399BD6E3FAC6F018459B
uid           [ultimate] Jason Lindemuth (pass master key) <jason.lindemuth@nutanix.com>
sub   rsa4096 2019-09-06 [E]
```
the key ID is the long string CB59A40E5ECD1EB873FB399BD6E3FAC6F018459B

## create an new password store using that key
```
$ pass init CB59A40E5ECD1EB873FB399BD6E3FAC6F018459B
```

## configure git global parameters if necessary
```
$ git config --global user.name "Jason Lindemuth"
$ git config --global user.email "jason.lindemuth@nutanix.com"
```
this info shows up in git commits, fyi

## make the password store db a git repo
```
$ pass git init
```

## create a new password to test the setup
```
$ pass add test
```
enter a password twice

## now set up a github.com repo for upstream storage
go to github.com
- create a new account (if needed)
- create a new repo
  + give it a name (eg pass or password-store)
  + give it a description (eg pass db)
  + make it private (passwords are safely encrypted but no need to expose it)
  + don't add any extra README or license files
  + click "Create Repository"

## create a personal access token (PAT) on github for the account
go to github.com as the user above
 - click on the user icon top right and select "Settings"
 - in the left menu, click "Developer Settings"
 - click on "Generate new token" button
 - add a note detailing the purpose (eg "USER github personal access token")
 - select the top level "repo" for the scope
 - hit "generate token" button
 - save the token string in a safe place

## tell git to use the osx keychain to cache the tokens
```
$ git config --global credential.helper osxkeychain
```

## tell git to use the url path as context for authentication
```
$ git config --global credential.https://github.com.useHttpPath true
```
this allows multiple github accounts, eg a work and a personal, to work in harmony

## tell pass to set an upstream repo
```
$ pass git remote add origin https://github.com/jasonlindemuth/pass.dev.git
```

## tell pass to push the local repo to the remote
```
$ pass git push -u --all
```
paste the PAT string obtained above into the user name, and hit enter twice.
this should push the pass git repo into github.

## confirm the new test password file is in the github repo
look at github.com repo you created for the new password file as well as a
.gitattributes and .gpg.id file

## confirm we can make new passwords and push the changes to github
```
$ pass add test2
enter passwords
$ pass git push
```
on github.com, confirm the new test2.gpg file is in the github repo

# basics of pass
pass uses the file system for the db namespace, so you can organize your
passwords and other sesnitive data in whatever way is intuitive to you, eg
subdirectories for categories, etc.

to add a new password, simply specify the command 'add' and the name for the
entry.
```
$ pass add work/github.nutanix
```
this will prompt the user to enter the new password twice and store it as
work/github.nutanix.  to read the password:
```
$ pass show work/github.nutanix
```
and to copy it into the system clipboard instead:
```
$ pass show -c work/github.nutanix
```
to generate a new 10 character password with symbols and copy it into the clipboard:
```
$ pass generate -c work/github.nutanix 10
```
pass assumes the first line in a file is the password, but you can add multiple
lines to a password file using your preferred $EDITOR:
```
$ pass edit work/github.nutanix
```
**NB**: pass does not automatically push and/or pull from the remote repo, so
it's important to run:
```
$ pass git push
```
this will push local changes to the github.com upstream repo.

## next, let's set this up on another machine for cross-platform access


# TODO: advanced setup...set up a second pass db on a linux system
https://medium.com/@antisyllogism/linux-windows-password-manager-pass-e3ad2681ecf3
## _anyone want to figure this out?_

# references: 
```
https://brew.sh/
https://www.passwordstore.org/
https://medium.com/@chasinglogic/the-definitive-guide-to-password-store-c337a8f023a1
```

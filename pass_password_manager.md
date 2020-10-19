# pass password manager

# references: 
```
https://brew.sh/
https://www.passwordstore.org/
https://medium.com/@chasinglogic/the-definitive-guide-to-password-store-c337a8f023a1
```

# install apple xcode cli tools (CLT)
$ xcode-select --install
### this launches a dialog window...on catalina it fails with an error
### in this case, install the xcode tools manually from:
```
https://developer.apple.com/downloads/more
```
## download "Command Line Tools for Xcode 12" and install

# next, install homebrew for mac os package management
```
$ /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
```

# next, install pass itself
```
$ brew install pass
```
### this will install pass and all dependencies (including gpg)

# next, create a gpg key for pass
```
$ gpg --full-generate-key
```
### I recommend the following:
- use 1) RAS and RSA key type (default)
- choose 4096 for keysize
- no expiration
- enter your name
- enter your email address
- enter a description in comment, eg "pass master key"
- enter a passphrase for the key

# note the key ID
```
$ gpg --list-keys
/Users/jason.lindemuth/.gnupg/pubring.kbx
-----------------------------------------
pub   rsa4096 2019-09-06 [SC]
      DB39F40E3ECC2EB073FB399BD6E3FAC6F018449D
uid           [ultimate] Jason Lindemuth (pass master key) <jason@bloom.us>
sub   rsa4096 2019-09-06 [E]
```
## the key ID is the long string DB39F40E3ECC2EB073FB399BD6E3FAC6F018449D

# now create a password store using that key
```
$ pass init DB39F40E3ECC2EB073FB399BD6E3FAC6F018449D
```

# next, if git has not been set up yet, provide git the global config info
```
$ git config --global user.name "Jason Lindemuth"
$ git config --global user.email "jason@bloom.us"
```
### this info shows up in git commits, fyi

# next make the password store db a git repo
```
$ pass git init
```

# create a new password to test the setup
```
$ pass add test
# enter a test password twice
```

# to set up a remote repo for pass (for plain old backup, for one, and for easy
# cross-platform portability...see steps below), set up github.com remote repo
go to github.com
- create a new account (if needed)
- create a new repo
  + give it a name (eg pass or password-store)
  + give it a description (eg pass db)
  + make it private (passwords are safely encrypted but no need to expose it)
  + don't add any extra README or license files
  + click "Create Repository"

# create a personal access token (PAT) on github for the account
go to github.com as the user above
click on the user ion top right and select "Settings"
in the left menu, click "Developer Settings"
click on "Generate new token" button
add a note detailing the purpose (eg "USER github personal access token")
select the top level "repo" for the scope
hit "generate token" button
save the token string in a safe place

# tell git to use the osx keychain to cache the tokens
```
$ git config --global credential.helper osxkeychain
```

# tell git to use url path as context for authentication (this allows multiple
# github accounts, eg a work and a personal, to work in harmony)
```
$ git config --global credential.https://github.com.useHttpPath true
```

# tell pass to set an upstream repo
```
$ pass git remote add origin https://github.com/jasonlindemuth/pass.dev.git
```

# tell pass to push the local repo to the remote
```
$ pass git push -u --all
paste the PAT string obtained above into the user name, and hit enter twice
this should push the pass git repo into github
```

# confirm the new test.gpg password (along with the .gitattributes and .gpg.id)
# files are in the github repo
look at github.com

# confirm we can make new passwords and push the changes to github
```
$ pass add test2
enter passwords
$ pass git push
```
### confirm the new test2.gpg file is in the github repo

# next, let's set this up on another machine for cross-platform access


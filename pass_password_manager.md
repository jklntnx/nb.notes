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
### use 1) RAS and RSA key type (default)
### choose 4096 for keysize
### no expiration
### enter your name
### enter your email address
### enter a description in comment, eg "pass master key"
### enter a passphrase for the key

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

# now create a password store
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


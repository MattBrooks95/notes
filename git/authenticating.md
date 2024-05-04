# Git

## GitHub

### Authenticating
- short version:
    - use `ssh-keygen` to generate a public/private ssh key pair
    - use a passphrase to protect the key
    - use `ssh-add` to add the ssh key to the ssh-agent, so you don't have to enter the passphrase every time
    - open the *.pub key file and copy the public key (NOT the private key)
    - go to GitHub account settings from the profile image in the top right
    - in the 'SSH and GPG keys' section, you can add a new key by giving it a name, and pasting in the entire contents of the *.pub public key file
    - remember to use the "ssh format" when you clone from GitHub
        - the HTTP way was `$ git clone http://www.github.com/<user>/<repository>`
        - for SSH auth to work, you need to do `$ git clone git@github.com:<user>/<repository>`
        - if you have a project directory that already has the origin set with an HTTP URL, you will need to use `$ git remote remove <remote>` to remove the old one and then use `$ git remote add <local name of remote> <remote ssh URL>` to add the origin in a way that works with SSH

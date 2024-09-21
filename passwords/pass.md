# [Pass](https://www.passwordstore.org/)

[online man page](https://git.zx2c4.com/password-store/about/)

- terminal-based password manager
- has a [Community Android App](https://github.com/android-password-store/Android-Password-Store)
- need a GPG key
    - [guide](https://docs.fedoraproject.org/en-US/quick-docs/create-gpg-keys/)

# Init
`$ pass init <email>`
Initialize the password database

`$ pass insert --echo <URL>`
insert a password for URL, the `--echo` command will make it so that you can see the password as you enter it into the terminal.

`$ pass show <URL>`
show the password registered for URL

`$ pass -c <URL>`
copy the password (the first line of the password file) into the cliboard. Uses xclip on an xorg machine, for Wayland it would need to use something else.

`$ pass generate <URL>`
automatically generates a password and stores it in a file for URL

# Git
I decided I'm going to try to use my git server to keep the password database synced across my devices. I'll have to figure out how to do this on Android, but for my normal computers just cloning the repository off of the git server and getting my gpg key should be enough.
[medium arcticle that might disappear or get paywalled](https://antisyllogism.medium.com/password-manager-pass-importing-and-exporting-b206a7eaaa70)

I was able to move my GPG key to another computer (to access the password store) by copying the <backup filename>.asc file onto it, and then using `$ gpg import <backup filename>.asc` (or was the verb 'restore'?) to make it useable again. I had to enter the original private key password in order for this to work. After doing so, I was able to use `pass` on another computer, by syncing my password files with git.

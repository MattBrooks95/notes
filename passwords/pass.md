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

## Android
I installed [Android Password Store](https://github.com/android-password-store/Android-Password-Store) and [OpenKeyChain](https://www.openkeychain.org/).

It didn't work. There was some error from OpenKeyChain. The 'latest' release from APS was from 2021, so installed the nightly. It still didn't work but gave me a better error message. GnuPG has some sort of non-standard feature that APS doesn't support, so it had me re-encrypt all my keys with another pgp key (I had to generate a dummy one) to get `$ pass init <dummy id>` to actually re-encrypt the passwords. Then, do `$ pass init <actual id>` to re-re-encrypt them with the fixed pgp key. Then, updating the password repository and synching the repository on my phone got it to work.

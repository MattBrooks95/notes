# [Pass](https://www.passwordstore.org/)

- terminal-based password manager
- has a [Community Android App](https://github.com/android-password-store/Android-Password-Store)
- need a GPG key
    - [guide](https://docs.fedoraproject.org/en-US/quick-docs/create-gpg-keys/)

# Init
`$ pass init <email>`
Initialize the password database

`$ pass show <URL>`
show the password registered for URL

`$ pass -c <URL>`
copy the password (the first line of the password file) into the cliboard. Uses xclip on an xorg machine, for Wayland it would need to use something else.

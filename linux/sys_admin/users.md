## Creating a user
### In NixOs
There is a 'users.users.' attribute set in `/etc/nixos/configuration.nix` where you can create users. There will be one entry by default from the Nix installer, that is in the 'wheel' group and can use sudo. I'm going to try creating another user who's not in the wheel group to ssh into.

### Setting a new users password
You need to be super user (`$ su` -> enter password), and then you can use the `passwd` command like `passwd <user>`, where it will prompt you to enter that user's new password.

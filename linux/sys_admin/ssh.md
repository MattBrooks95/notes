# SSH
Be very careful when doing anything involving setting up SSH.

## In Nix
in the `/etc/nixos/configuration.nix` file, there'll be a commented out attribute set called "services.openssh". You need to uncomment it out, and set it to `enabled`. If you need to, you can specify other settings like the portnumber, controlling whether the root user can be logged into over ssh or not.

## Commands
- ssh-keygen <- on the *client* to generate a public/private key pair
    - the private key stays on the client
    - the public key needs copied to the ssh host
- use `$ ssh-copy-id -i <pubkey> <serverside username>@<serverhostname>` copy the PUBLIC key (ends in .pub) to the server
    - you will be prompted for the user's password
    - after it succeeds, you can log in with `$ ssh <user>@<host>`
- use `$ ssh-add <path-to-PRIVATE-key>` to add the key to the ssh agent so that you don't need to authenticate everytime

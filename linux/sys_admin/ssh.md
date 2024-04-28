# SSH
Be very careful when doing anything involving setting up SSH.

## In Nix
in the `/etc/nixos/configuration.nix` file, there'll be a commented out attribute set called "services.openssh". You need to uncomment it out, and set it to `enabled`. If you need to, you can specify other settings like the portnumber, controlling whether the root user can be logged into over ssh or not.

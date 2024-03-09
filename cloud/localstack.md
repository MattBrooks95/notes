## Installation
nix flake could look like this:
```nix
{
  description = "gets localstack working to emulate a cloud environment";

  inputs = {
    nixpkgs.url = "github:nixos/nixpkgs?ref=22.11";
    flake-utils.url = "github:numtide/flake-utils";
  };

  outputs = { self, nixpkgs, flake-utils }:
    flake-utils.lib.eachDefaultSystem (system:
      let
        pkgs = nixpkgs.legacyPackages.${system};
        slocalstack = pkgs.writeScriptBin "slocalstack" ''
        FILESYSTEM_ROOT=~/.cache/localstack/volume localstack start -d
        '';
      in {
        devShells.default = pkgs.mkShell {
          buildInputs = with pkgs; [
            localstack
            slocalstack
          ];
        };
      }
    );
}
```

ensure:
- that you tell localstack somewhere to put its cache that isn't inside the nix read only filesystem
    - `$ localstack start -d` is the command that they suggest be used to start localstack. When I first ran it, it did not work. Nixos has a readonly filesystem at /nix, and I guess localstack was trying to write stuff there.
    - `$ FILESYSTEM_ROOT=~/.cache/localstack/volume localstack start -d` (this command is in the flake) was necessary to make it work.
- that your user is added to the `docker` group. In nixos, this meant that I had to edit my configuration.nix's user entry for my user account and then reboot.

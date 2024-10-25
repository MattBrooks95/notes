# Nix

## Flakes

### Flake Templates
- you can create a template for nix projects by creating a flake.nix file, and exporting a record of `template-name: { path: ./path-to-template-directory; description: "some string"; };` key value pairs.
- then, when you start a project, you can say `$ nix flake init -t (<some_url>|<path to flake that containstemplates>)#template-name`, and nix will copy the `flake.nix` and other files from the template directory into your working directory

### Flake Tips
- do not use flakeUtils, knowledgeable people in the discord discourage its usage
- do not use the flake input option `flake = false;`, like `rescript-lsp = { flake = false; url = "..." };` when all you need is the source of a github repository
    - you should instead point the `src` attribute of your `mkDerivation` at the github tarball using the `fetchFromGitHub` helper

## Issues
- I am not managing haskell packages correctly. I have a project running on ghc928, but when I tried to use that same flake.nix file again, something for the package `ormolu` failed to build.
- When I try to make these flakes for my haskell projects, they often end up compiling ghc and a ton of packages from scratch. My nix setup is misconfigured and I'm not hitting their cached versions of GHC. I think
    - I changed the nixpkgs url in the flake to `nixpkgs.url = "github:nixos/nixpkgs/23.05";`, and by specifying the stable version of nixpkgs (23.05) I was able to get the Haskell dev environment running without much compilation
    - I also changed the resolver for the stack.yaml file to be `lts-20.24`, which lines up with GHC927

## System76 Kernel Module Fails to Build
- if you see gcc errors preventing some of the nixos modules from building, you can turn off the system76 kernel modules if the Linux kernel you have supports the hardware reasonably well. This probably won't work on brand-new models, but it seems to be working fine for me and my Lemur is around 2 years old.

## Gotchas
* if you're debugging a flake that doesn't want to build, and you're frequently making small changes and doing `$ nix develop`, there's a chance that your nesting direnv shells every time you do it. Eventually, some direnv stuff will get too long and terminal commands will start to fail because of the argument list (to something) being too long
    * I got out of it by `$ exit`ing several times until I made it out of the nested sessions
    * you can check how deep your shell is with `$ echo SHLVL`
* if you use the `patches = [ .... ];` attribute to make file modifications necessary to get the package to work, realize that the patch is applied to the file in the root directory of your in-progress derivation.
    * to patch foo.bar, you would need to use `git diff` to generate a patch file, and add it to the `patches` attribute
        * you need the filename and path of the modified version to be the same as the normal path
        * the patch will be applied to file `foo/bar.hs` before the build phase, and the path to access that file in the build phase is just `foo/bar.hs`
    * `foo/bar.hs` and `${someSourceInput}/foo/bar.hs` are two different things! The first one will have the results of patches being applied, and the second one won't.
    * this means that if you do `cp ${someSrcInput/foo/bar.hs $out/foo/bar.hs`, you are going to copy the <strong>unpatched</strong> version of the file into your deriviation output! Use the `cp foo/bar.hs $out/foo/bar.hs` path
* `$ sudo nixos-rebuild switch` fails because "device out of storage" means that the /boot partition of the disk is filled with old Linux kernels. By default, Nix will store an infinite amount of boot entries, which will eventually fill the partition. You can fix this by setting configureLimit to a lower number than infinity. I'm going to try 30.
`boot.loader.systemd-boot.configurationLimit = 30;`

# Nix

## Flakes

### Flake Templates
- you can create a template for nix projects by creating a flake.nix file, and exporting a record of `template-name: { path: ./path-to-template-directory; description: "some string"; };` key value pairs.
- then, when you start a project, you can say `$ nix flake init -t (<some_url>|<path-to-local-template>)`, and nix will copy the `flake.nix` and other files from the template directory into your working directory

## Issues
- I am not managing haskell packages correctly. I have a project running on ghc928, but when I tried to use that same flake.nix file again, something for the package `ormolu` failed to build.
- When I try to make these flakes for my haskell projects, they often end up compiling ghc and a ton of packages from scratch. My nix setup is misconfigured and I'm not hitting their cached versions of GHC. I think


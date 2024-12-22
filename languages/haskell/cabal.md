# Cabal
- `$ cabal freeze` creates a lock file of sorts that guarantees that the exact package versions and flags can be used to reproduce the build on another system (I don't think it fixes the GHC version)
    - when you want to update, the `$ cabal outdated` command can be used to print the outdated versions of packages in the freeze file
- you have to be careful when you upgrade packages because some packages depend on higher versions of GHC that may not be in nixpkgs yet

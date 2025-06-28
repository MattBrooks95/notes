# Cabal
- `$ cabal freeze` creates a lock file of sorts that guarantees that the exact package versions and flags can be used to reproduce the build on another system (I don't think it fixes the GHC version)
    - when you want to update, the `$ cabal outdated` command can be used to print the outdated versions of packages in the freeze file
- you have to be careful when you upgrade packages because some packages depend on higher versions of GHC that may not be in nixpkgs yet

# Profiling
[Cabal Profiling Documentation](https://cabal.readthedocs.io/en/latest/how-to-enable-profiling.html)

## Build With Profiling
`$ cabal configure enabel-profiling` will add `profiling: True` to the cabal.project.local file, which will tell Cabal to pass profiling flags to GHC

Unfortunately seems to cause a lengthy recompile of all of the application's components and its dependencies. This makes sense because it needs to insert code to keep track of performance metrics. Hopefully Cabal will keep the profiling builds of the dependencies in the Cabal cache directory.
    ^ This might have been a lie. `$ cabal build` wasn't doing anything (using previous build), so I deleted dist-newstyle to force it to rebuild. It looks like it rebuilt the project and the `-prof` argument wasn't given to GHC. That was a waste of about 6 minutes.

I'm really confused because `$ cabal build web-backend --enable-profiling --enable-library-profiling --enable-executable-profiling` will cause it to rebuild and I see Cabal acknowledging the `--enable-profiling` flag, but then when I try to run it with `$ cabal run web-backend +RTS -pj -RTS` it says that the app needs to be built with the `-prof` flag. Maybe the GHC version and Cabal don't work well together?


## Run With Profiling
I'm going to try a script that looks like this. The parts that I added to try and run a web server with profiling is the `+RTS -pj -RTS` section. All of the parameters after the `--` are parameters to the application itself.
```bash
#!/usr/bin/env bash
cabal run web-backend +RTS -pj -RTS --\
	-p 5432\
	-s password\
	-u game-db-dev-user\
	-n game-db-dev\
	-h localhost\
	-a "http://localhost:3000/api"\
	--memhost "localhost"\
	--memport 6379\
	--memname "0"\
	--memuser battle-monsters-web-backend\
	--mempass "password"
```

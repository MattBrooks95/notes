# Haskell

## HLS (Haskell Language Server)
* it works better with an explicit hie.yaml file
    ```
    cradle:
      stack:
    ```
    * `hie.yml` doesn't seem to work! Be careful about the extension.
    * If you see an error at the very top of Main.hs about not being able to resolve the project's package, it's probably because HLS can't find the correct `cradle`

## Strings
- the pain
The 'persistent' storage library connection functions expect ByteStrings, not normal Haskell `String`s. Data.ByteString.Lazy.UTF8 will not work, it has to be the strict version Data.ByteString.UTF8. Use the `fromString` method.

## Importing and Exporting Type Family Instances
I was trying to use the [Esqueleto]() library to implement an INNER JOIN. The example code would not work for me because it kept saying that entity ID fields like `MonsterId` or `StatsMonsterId` were undefined. It turns out that this happened because Esqueleto and [Persistent]() use type families. When Template Haskell generates code for the type families to allow querying and the use of the Esqueleto EDSL, it's difficult to know how to import them.

```haskell
module Schema (StatsMonsterId) where
```

Would not work. The `StatsMonsterId` export would cause a compiler error.

I was able to fix this by exporting the instance of the type family:
```haskell
module Schema (EntityField(StatsMonsterId)) where
```

Where `EntityField` is the Persistent type family, and `StatsMonsterId` is the name of a specific constructor for an instance of that type family.

Reading [this](https://ghc.gitlab.haskell.org/ghc/doc/users_guide/exts/type_families.html#import-and-export) was invaluable.

Using GHCI also helped me debug this issue:
1. change export to not have an explicit export list `module Schema where`
2. `$ cabal repl`
3. (in the repl \#) `# import Schema`
4. `# :browse Schema`
That will dump a bunch of information about the symbols in the module that are able to be imported. After doing this, I used Tmux's copy mode to search for "StatsMonsterId", and it showed there was a `data instance Stats...` for it.

## Writing Eq Instances

Here I caused an infinite loop because I accidentally passed the outermost objects to '==', which is the method I was defining here. I meant to call it on the inner value, the PreGameSelectionsState.
```haskell
left@(PreGameStateUpdate (PreGameSelectionsState _ _)) == right@(PreGameStateUpdate (PreGameSelectionsState _ _)) =
    -- oh shit this was an infinite loop because I was calling the '==' function
    -- here instead of calling it on the contents of the PreGameStateUpdate
    left == right
```

It took me a long time to figure this out because I cannot get Cabal to build my code or the test suite with the profiling information such that GHC can run the profiler. I'm wondering if it's a bug that has to do with multiple subpackages existing, or perhaps my version of cabal doesn't work well with my version of GHC.

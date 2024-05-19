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

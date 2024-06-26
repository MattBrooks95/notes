# Haskell FFI
* it is recommended to use the `capi` instead of `ccall`
* when you import C libraries, you mustn't add the angled brackets
    * NG
    ```
    foreign import capi "<wlr/util/log.h> wlr_log_init"
        wlr_log_init :: CInt -> Ptr a -> IO ()
    ```
* Trying to call a `wlroots` function (working on a Wayland compositor)
    * this will cause an error saying that the function wlr_log_init couldn't be found
    ```
    foreign import capi "wlr/util/log.h wlr_log_init"
        wlrLogInit :: CInt -> Ptr () -> IO ()
    ```
    * this will cause an error about the file not being found
        * does this imply that the version that failed with "wlr_log_init can't be found" actually managed to atleast find the file?
    ```
    foreign import capi "include/wlr/util/log.h wlr_log_init"
        wlrLogInit :: CInt -> Ptr () -> IO ()
    ```
    * this fixed it, you need to add this entry to the root level of the *package.yaml* file for Stack to know to link in the *libwlroots.so* file
    ```
    extra-libraries:
      - wlroots
    ```
* If you need to make local C header files (files that would git included directly or indirectly like this `#include "my_c_stuff_blah_blah_blah.h`), you can tell Cabal about them with the `include-headers:` directive, like this:
```cabal
library
    include-dirs:
        protocol-headers
```
where `protocol-headers` would be a path to a directory that contains the local C header files

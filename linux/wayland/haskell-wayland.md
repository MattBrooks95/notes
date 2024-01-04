# On Creating Wayland Software with Haskell

## Wayland Projects with Haskell
* there was a spin-off thread of a [thread](https://discourse.haskell.org/t/xmonad-for-wayland-call-for-help/7812/43) about porting xmonad to Wayland, about creating bindings to wlroots [link](https://discourse.haskell.org/t/haskell-wlroots-bindings/8426)
    * the idea is to create Haskell bindings to the library Wlroots, so that we can have a good basis for making a Wayland Compositor
    * bradrn & burning witness shared some opinions about how a project like this should be done

## C Foreign Function Interface
* [burning witness](https://github.com/BurningWitness/vulkan/tree/f86863468c8517b83ca39abad6c0b8afd493910b) has an example of some 'raw' bindings to C in his vulkan repository
    * looks like you can import things from the C headers
    * he's using the C pre-processor [sample](https://github.com/BurningWitness/vulkan/blob/develop/vulkan-raw/src-gen/Vulkan/Core_1_0.hsc)
    * this library assumes that the vulkan header is available, I don't see anything that attempts to assume that it's there
        * I was hoping for our wlroots bindings we could use Nix to manage our tooling and C library dependencies in a reproducible way
    * there's [an example](https://github.com/BurningWitness/vulkan/blob/develop/vulkan-raw/src-gen/Vulkan/Ext/VK_AMD_display_native_hdr.hsc) of using pattern synonyms for C constants instead of newtypes
* there is a vlog about the best practices for using the FFI [link](https://www.haskell.org/ghc/blog/20210709-capi-usage.html)
    * notably, it reiterates that we should use `CApiFFI` instead of `ccall`
* apparently you cannot pass a struct to C by value, so you must create a C wrapper function that accepts a pointer, then derefences it and passes that into the original C function
    * https://stackoverflow.com/questions/10903940/haskell-ffi-how-to-handle-c-functions-that-accept-or-return-structs-instead-of
    * this post is really old, this may have changed

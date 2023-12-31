# C FFI
* Pure, threadsafe C code doesn't need to be in the IO monad
    * non-pure C code should be in the IO monad (return type of IO CDouble instead of just CDouble)
* Real World Haskell is talking about using the `foreign import ccal "math.h sin"` syntax, but I think modern advise is to use the `capi` one
* the Real World Haskell chapter is about making bindings to PCRE, a C regular expression library
* if you use the `{-# LANGUAGE CPP #-}` extension, you can use the C preprocesser to define constants like `#define N 16`
    * but apparently this is dangerous because the C preprocesser doesn't know anything about Haskell and could modify your other source code in such a way that it could become invalid
* it says that it's better to use `hsc2hs`, a Haskell tool for making bindings
    * the file extension must be .hsc, to be able to do things like include `#include <pcre.h>`
* a `CInt` would work for representing the integer flags to the PCRE library, but that would be 'sloppy'
    * we are encouraged to use Haskell's type system to restrict the values that could be passed to these flags to just the values that are valid, and also to prevent users of our library from doing math with flags
    * we can get this type safety by using `newtype`, and not exporting the constructor for the `newtype`. This ensures that users can't just use the constructor to wrap integers that don't match the flags to the C library
* in a .hsc file, you can use `#const` C preprocessor directives
    * it looks like `hsc2hs` lets you access the values (the actual numbers themselves) from the C header file directly
* when the .hsc file has been written, you run `$ hsc2hs <file>.hsc`, and it will create `<file>.hs` for you, with the constants from the C header expanded
* instead of writing the newtypes yourself, you can use the `#enum` feature of hsc2hs to define them in a more succinct way
    * the syntax starts with `#{ enum ...`
* there is a common C pattern where functions accept these integer flags, and those flags can be bitwise or'ed together
    * in Haskell, that would be a bit clunky and reveals the nature of the thing we're trying to abstract, so it's good if the Haskell library provides a function that combines flags together with a fold
* in Haskell, the `Ptr` type can be used to represent pointers to C types, like `Ptr CInt` or `Ptr Word8`
    * you can even do pointer math with `plusPtr`
    * `poke` can be used to modify the value the pointer points to
    * `peek` derefences the pointer and gives you access to the object
* if the C api returns an abstract type, you often need to represent that in Haskell with `Ptr ()`
* if the return value of the C function is a pointer, we need to have the Haskell side of that function be in the IO monad, because the returned pointer could be different even if the inputs to the Haskell function were the same
* with the `EmptyDataDecls` language pragma, you can declare data types that have no constructors, like `data PCRE`
    * this means that we can make pointers to this type, but we can't ever dereference it. The compiler can stop us from accidentally dereferencing a pointer that shouldn't be dereferenced

### Safety
* most FFI calls are in `safe` mode by default
    * safe calls are less efficient, but "guarantees that the Haskell system can be safely called into from C" (I thought Haskell was calling C, not the other way around???)
* the `unsafe` mode is much more efficient, but the C code that is called must NOT call Haskell code
    * it says that it is rare for C to call Haskell, so in practice people use the `unsafe` version most often

### Memory
* you can have a Haskell type hold the reference to th C value that needs to be eventually de-allocated, and you can tell the Haskell garbage collector to delete that data on the C side when the Haskell value goes out of scope
    * `newForeignPtr :: FinalizerPtr a -> Ptr a -> IO (ForeignPtr a)`
    * `FinalizerPtr a` is a function that will be ran when the data goes out of scope, and a pointer to the C data. `newForeignPtr` will then return a pointer that cleans up after itself when it goes out of scope


## Misc
* the newtype definition example is interesting, it uses the record syntax and it also generates a function that can be used to get the original value
    * `newtype PCREOption { unPCREOption :: CInt } deriving (Eq, Show)`
* `.|.` is bit-wise or in Haskell

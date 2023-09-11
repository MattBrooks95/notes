## Background
- haskell is a pure functional programming language
    - pure in this context means separating side-effectful code from non-side-effectful code
- types are statically checked by the compiler
- programs are evaluated lazily
    - expressions are not evaluated until their result is actually needed

- referential transparency (Haskell forcing the coder to declare functions to be side-effectful) is key for applying forman verification techniques (chapter 15)
- strong static typing differs from dynamic typing
    - type checks do not occure at runtime
    - the compilation output will not be created if the program fails the static type checks at compile time
- parametric polymorphism is being able to write functions on lists without caring about the types of the elements that the list contains
- Haskell is considered to be the successor of the programming language, Miranda

## GHCI
- you can execute Haskell with GHCI without entering the REPL, by doing something like ghci -e 5 + 3
    - however, I couldn't get the same result with stack ghci -e 5 + 3, and ghci isn't installed globally on this computer because Nix
- :{ can be entered into GHCI to start a multi-line block, which then must end with :}
    - :{
    - do
    -    print "lol"
    -    print "XD"
    - :}


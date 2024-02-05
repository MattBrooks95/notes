# Chapter 14
* considered skipping this, ended up skimming it

### DSLs
* deeply embedded DSLs can result in a lot of boilerplate if written with Vanilla Haskell, so the Utrecht University Attribute Grammar (UUAGC) Compiler can be used to alleviate this
* "Happy" is the parser generator included in the Haskell platform
    * it processes a grammar description and generates Haskell code for you (like YACC?)
    * it is an external preprocessor
* AspectAG is an attribute grammar library in Haskell, so you don't need an external tool but it uses type level programming a lot
* this book will teach UUAGC
    * can be installed from Hackage
    * you write an *.ag file that you need to write looks similar to ANTLR/YACC
    * there is also a syntax for writing Haskell code in the *.ag file
    * I skipped to pg 513

## Programming with Data Types (from pg 513)
* "any Haskell value can be seen as a tree and decorated using attributes which flow either top-down or bottom-up (?? I'll have to revisit this)
* folds can be a similar generalization for types
* "data type generic programming" based on the shape of the data type
    * references "studios on folding lists" from chapter 3
* catamorphisms - using a fold recursively in place of a data constructor
    * need to revisit this with another learning resource, it wasn't making sense for me
* an 'algebra' is a tuple that contains all of the functions required to fold over a datatype
    * there is a code example on pg 514 that was relatively understandable

## Data Type Generic Programming
* also referred to as "generics"
* GHC.Generics
* Generics work in Haskell because types are made of three different building blocks
    * fields - are values of a certain type
    * products - represented by the symbol :*:, put several fields in a constructor
    * choice of constructors :+:
* a proxy is a dummy type that only exists to get the code to compile
    * available in Data.Proxy
    * you saw this in the Halogen framework's types when you tried Purescript
* GHC can derive Generic for you
* I skipped a lot of this because it wasn't making sense to me

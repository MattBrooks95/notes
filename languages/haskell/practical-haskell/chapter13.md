# Begins Part 4, Type Level Programming and DSLs
## DSLs
* Domain Specific Language
    * not interested in general computation but optimized for ease of use for specific problems
    * css & sql are examples of DSLs
    * well designed DSLs can be written by non-programmer stakeholders
    * it is important to figure out how the DSL and the application will interoperate with one another
        * there are a few ways of doing this, each with advantages and disadvantages
    * the language that operates the DSL is called the host language
* external or stand-alone DSLs are DSLs that can be parsed and processed by any language that has an implementation for it
* embedded or internal DSLs are written in the host language and are tightly coupled to it
* "deep embedding" is when you make a syntax tree for your DSL using Haskell data types
* there is an example on page 444 of modeling a language for manipulating a stack
* tips
    * always keep your DSL as small as possible, define new functionalities without extending the syntax of the language
    * you need to use type level features to ensure that it is impossible to write unsound programs in your DSL
    * the `EmptyDataDecls` language extension allows you to create Data types that have no constructors, which can be useful for "tagging" types in a DSL

### GADTs - Generalized Algebraic Data Types
* Haskell's data declarations can't return different types depending on the constructor of a value, and GADTs get rid of this restriction
* you need the {-# LANGUAGE GADTs #-} language extension

## Type Level Programming
* dependent type systems let you make decisions at the type level that depends on values
    * Haskell is NOT a dependently typed language
    * Idris, Agda and Coq are dependently typed languages
* Author claims that the rest of this chapter is skippable for beginners, and encourages going through it later in your Haskell journey

## Type Level Features in Haskell
* Type Families (TFs) and Functional Dependencies (FDs) can accomplish the same things but they are different and incompatible with one another
    * using both might even lead to bugs from GHC
* FDs
    * constrain parameters of a type class
    * Haskell compiler can then infer the type of a type parameter based on the constraints
* TFs
    * allows you to create functions that assign a type based on a set of input types
        * open type families can have rules added to them
        * closed type families cannot be extended (I think? I can't remember... hand written notes aren't clear. I will have to revisit these concepts anyway)
* this section of the book talks about the Peano numbers and doing arithmetic in the types, which you already studied in the Lambda Calculus book you read
### Using FDs
* you need to enable the MultiParamTypeClasses extension
* it's probably easier just to look at the book's examples again instead of reiterating the hand written notes here
* you can get an error "could not deduce .... arising from use of `<someFunc>` from the context MyTypeClass a b", and the solution is to declare the functional dependencies
    * the functional dependency declaration is a constraint that gives the compiler enough information to figure out what the types should be
* FDs come from database theory
    * sounds like something that could have happened at Microsoft
* page 460 and 461 explain why FDs are necessary in a succint way
* I guess it's like the compiler can't infer a type because multiple possibilities exist, so you need to add FDs to your typeclass to tell the compiler "in this case, this type needs to be this specific type"
* pg 462 has an example of using type level programming to write a non-partial head function for a vector
    * the function would only accept non-empty vectors, and the compiler can figure this out at the type level
* FDs are like Prolog, a logic programming language

## Type Families pg 466
* it's anecdotal but I hear about TFs in Youtube videos talking about Haskell, and I never hear about FDs
* a TF is a function at the type level
* it takes types as a parameter and gives you types as a result
* Syntax
    * starts with "type family"
    * function definitions must be indented below the "type family"
    * functions must start with uppercase letters
    * 'where' keyword must be written after the type class data
* need the "TypeFamilies" language extension
* open TFs are more backwards compatible than closed TFs
    * closed TFs have the 'where' keyword, open TFs do not
* book has examples on pgs 466 and 467
* when you add new rules to an open type family, you do so by typing "type instance" followed by the rule
* closed TFs can have overlapping patterns because they must have all of their rules defined in one place
    * open TFs cannot have overlapping patterns because the rules could be written in different modules
* type-level signatures in a type class are known as associated types
* there is a TF example on page 471
* an "equality constraint" x~y states that X and Y must be syntactically equal after all type families have been resolved
    * "they don't need to be the same type, but they need to define the same operations"
* TFs discussed in the books are actually "type synonym families", and there exists another kind called "data families"

## Data Type Promotion & Singletons
### Data Type Promotion
* skipping most of these handwritten notes, and the handwritten notes also skipped some stuff
* data type promotion is a language extension that creates term-level types from data types, for safer type level programming
    * I think this is the one where a function can accept only a subset of the constructors of a Data type
    * needs the DataKinds extension
    * prefix data name with a "'" single apostrophe to explicitly refer to the kind generated by the DataKinds extension
* more details on pgs 476 and 477

### Singletons
* points out some maintainability pain points with FDs and TFs
* the singleton package uses Template Haskell to create boilerplate code for you
* working with singletons almost always requires the UndecidableInstances extension

pg 478 has an elaborate example of using type level programming to enforce business rules

singletons, TFs and FDs are summarized on pg 484

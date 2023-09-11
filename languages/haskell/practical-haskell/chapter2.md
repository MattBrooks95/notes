## Ints and Chars
- ints and chars are not interchangeable in Haskell as they are in C
- one char in haskell is 1 unicode charactor
- the chr function can be passed an integer, and it will return the corresponding char
- Haskell ints (Int) are signed, and have the range +-536870911 (2^29 - 1)
    - ints have the native width of the architecture, and are the fastest
- Integer has an infinite numeric range, at the cost of memory & speed
- Data.Ratio has a lot of methods for converting numbers, and constants that correspond to numbers that are mathematically interesting
    - you can also do fraction math: `1 % 3 + 1 % 2` => `2/6 + 3/6 = 5/6` (actually evaluates to 5 % 6 in ghci
    - 1 / 3 + 1 / 2 is 0.833333333 in Python
- it can be difficult to determine the actual type of a number
    - number types are polymorphic, 5 can be any type in the Num typeclass
    - however a number like 3.4 can only be in the Fractional category (Float or Double)
    - these 'typeclasses' are a representation of a group of types that have similar properties
    - for instance, addition (+) is defined for both Double and Int
- we are warned here that `atan -4` is a type error, because Haskell will think that this is `atan(the function itself) - 4`
    - the issue is fixed with parenthesis: `atan (-4)`

## Strings
- Strings are just lists of Characters in Haskell: `:t "Matt"` will give an output of "Matt" :: \[Char\]

## Lists
- functions like '++' (list concatenation) and 'reverse' have type definitions that include 'type variables'
    - type variables are in the definitions of functions and mean that the type of the parameter really doesn't matter
    - getting the list of a string of ints and a string of chars is the same process, so the string length function's element's type declaration is written as a variable
    - type variables are always lowercased
- Haskell lists are homogenous, which means that the elements of any given list must all be of the same type
    - \['a', 'b'\] ++ \[1, 2\] is a type error in Haskell
- Haskell lists are implemented as linked lists
- the empty list '[]' is called 'nil' and the operator to add an item to the end of a list (:) is called 'cons'
- the 'null' function checks whether a list is empty
- 'head' gets the first element
- 'tail' gets every element in a list except for the first element

## Booleans
- && and || are short-circuiting, because the language is lazy
- 'and' and 'or' functions exist, and they perform the logical operations (&&) and (||) on lists of expressions
    - or \[True, False, and \[False, True, True\]\] => True
- in Haskell, if-then-else is an expression
    - which means that it returns a value `x = if True then 10 else 5` will set the value x to be 10
    - it also means that the values of the then and else blocks must be of the same type
- in most other languages the (!=) operator means 'not equal'. In Haskell, that same logic exists in the (/=) operator

## Exercises
I won't use pattern matching, because it hasn't been covered yet.
Also, writing it in this way is kinda nice, because they become one-liners
2.1
1. no, I don't want to use the cons syntax
2. isEmptyOrHeadIsEmpty x = if null x then True else null (head x)
3. isOneElementList x = length x == 1
4. combineTwo x = if length x /= 2 then [] else head x ++ (head . tail) x
    - `(head . tail) x` could be written `head $ tail x`, but function composition is cool

## Section2
- used `$stack new Chapter2 new-template` to generate the stack project directory
- I didn't need to add Chapter2/Section2/Example.hs to the .cabal exposed-modules property, it looks like stack did it for me when I made the file
- `$stack setup` gives 'error, attribute 'ghc925' is missing, from some sort of Nix file
- time to figure out how to fix nix...
    - https://docs.haskellstack.org/en/stable/nix_integration/
    - this article from Tweag looks to be the best way to do this: https://www.tweag.io/blog/2022-06-02-haskell-stack-nix-shell/
    - what I did: `$stack ls snapshots --lts remote`, which showed that 19.30 was available, so I changed the last bit of the stack.yaml file's resolver: url: field to match that, and the project built
- Algebraic Data Types (ADTs) are defined by two pieces of data:
    - a name for the type that will be used to represent its values
    - a set of constructors that can be used to create new values
        - these constructors can have arguments that hold values of the specified types
    - ADTs can be arguments to other ADTs (you can nest them)
    - you must derive Show to be able to print an ADT in ghci
        - you can just add `deriving Show` to the ADT declaration, and Haskell will create implement it for you
    - constructor names and type names exist in separate namespaces, so they can have exactly the same name as one another
        `data Person = Person String` creates a type called 'Person', and a constructor that can build them also called 'Person'
    declares
- Capitalization
    - capitalization matters in Haskell
    - function names, function parameters, bindings and type variables must start with a lowercase letter
    - operator names cannot start with ':'
    - types, constructors, type classes and kinds must start with a capital letter
        - operator names must start with the ':' symbol
- functions that do not cover the entire range of the types of their possible inputs are called 'partial functions'
    - they will throw an exception when they are called with insufficient parameters
    - functions who DO cover every possible type of their inputs are called 'total functions'
    - failing to have a case in a pattern match for every constructor for the parameters will cause this issue
        - however, by default it seems that these functions simply become warnings from GHC
- pattern matching
    - you can pattern match within a 'let' or 'where' declaration, but syntactically you can only match on one case
        - this is useful when you know the structure of the data beforehand
        ```
        -- instead of writing this,
        let name = case companyName client of
            Just n -> n
        -- you can write this, if you've already proven by other means that the result of
        -- companyName will be a Just value
        let Just name = companyName client
    - pattern matches will not backtrack, there is a convuluted example on page 48 where the order of cases in a pattern match determines whether the default match will be made, or whether an exception will be thrown
    - pattern matches are evaluated in order, so the order of them matters
        - for instance, if you put a wildcard match in the first slot, none of the other cases will ever be entered. The below example will always evaluate to "=("
        ```
        case foo of
            _ -> "=("
            "bar" -> "baz"
        ```
    - it is usually cleaner to implement pattern matching by using the multiple function declarations instead of a case statement
    - instead of
    ```
    foo x = case x of
        "bar" -> "baz"
        _ -> "=("
    ```
    you can write:
    ```
    foo "bar" = "baz"
    foo _ = "=("
    ```
- as pattern
    - you can use a pattern match to confirm the structure of something, and simultaneously bind a different section of that thing into a local value
    ```
    -- I don't think this is working code, but in this way the (x:y) portion can be bound
    -- to 'r', so we still match on the (x:y) part but we can reference it with one value, 'r'
    getFirstTwo :: [a] -> [a]
    getFirstTwo (r@(x:y):_) = r
    getFirstTwo [] = []
    ```
- guards
    - guards are like pattern matching, but can be used to check conditions between the parameters, like if parameter1 and parameter2 are equal to eachother
    - you cannot do this with pattern matching, because `foo x x = 1; foo x y = 0` is a compiler error: x is bound twice 
        - with guards: `foo x y | x == y = 1; foo x y = 0`
    - any function (or expression?) that returns a boolean value can be used within a guard
        - this is huge, because you probably cannot do that with pattern matching
- view patterns
    - not a part of the Haskell 2010 standard
        - must be enabled with the language extension ViewPatterns
    - allow you to apply a function to avalue and then perform a pattern match on the result

# Records
- the language extension {-# LANGUAGE NamedFieldPuns #-} lets you do something like the javascript trick where a value can automatically become a name and a field in an object, but during a pattern match
    - (js) `const foo = "bar"; return { foo };` makes an object with the shape: { foo: "bar" }
    - Haskell field puns let you do
    ```
    data Foo = Foo { bar :: Int, baz :: Int }
    --myFunc Foo { bar = b } = b
    myFunc Foo { bar } = bar
    ```
- the language extension {-# LANGUAGE RecordWildCards #-} can also reduce boilerplate, by matching on all available fields of a record
```
myFunc Foo { .. } = bar + baz
```
- a common pattern is to create a record that represents the arguments to a function
    - you've done this in Typescript & Perl
    - you will also create a constant of that record type that has the default values set within it
    - then, when you use it you can only update the options that you need to tweak
    - it can be a good idea to not export the actual constructor the type, and only export the default instance of that type. By doing so, the API of the actual constructor can change easily, because you will only need to update the defalt record
    - there is a good example of this in pages 60~62 of the book

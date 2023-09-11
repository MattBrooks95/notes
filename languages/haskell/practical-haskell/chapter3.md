## Functions
- are treated the same as values in Haskell
- first-order means "not higher order; no functions as values/parameters"
- higher order means, manipulating functions as if they were values/data
- I thought that '$' was just syntactic sugar for function arguments to not need parenthesis, but apparently it is semantic
    - ($) :: (a -> b) -> a -> b is the type signature, so fun1 (fun2 (fun3 x)) can be written fun1 $ fun2 $ fun3 x
- anonymous functions
    - an anonymous function is also called a lambda abstraction, and the \x -> x * x lambda's '\x' part is supposed to resemble '(actual lambda character)x'
        - I guess in the lambda calculus it is λx. x * x
    - anonymous functions have no name, so recursion is forbidden
    - you can pattern match in a lambda, but only once
        - therefore, you need a case statement within the lambda expression, or use the LambdaCase language extension
    - lambda functions are also called closures, because they can carry context with them
- you can omit a parameter to a function if it appears on both sides of the equation, it's called Partial Application
    - it says that the omission of a parameter can mess with tho commutative property, so you need to be careful
    - for example, the divisions `(/2)` and `(2/)` will create different results when mapped across a list of numbers
- operator functions, when used as values, need to be enclosed in parenthesis
    - `map *x [1, 2, 3]` will not work; it needs to be `map (*x) [1, 2, 3]`

## Point-Free Style
- in math, arguments to function are referred to as "points"
- point-free programming is a style of programming in which you combine partial functions to avoid having to pass into them parameters
- of course, the dot operator (.) can be used to combine two functions together
- these functions that combine other functions together are called combinators

## Currying
- functions that take sequences of arguments are called 'curried' functions, like `myFunc :: a -> a -> a`
    - the uncurried version would be `myFunc :: (a, a) -> a`
    - the singular parameters in the curried version are converted to a single tuple in the uncurried version
- the 'flip' function that takes a function and reverses the orders of its arguments looks like
```
flip :: (a -> b -> c) -> (b -> a -> c)
flip f = \x y -> f y x
```

## Polymorphism
- you can use polymorphism in data constructors too, not just function parameters `data Wrapper someType = { value :: someType }`


## Modules
- talks about Prelude (Haskell's built-in library)
- talks about qualified imports
- talks about import hiding
- you can use the module export list to selectively export things from your module
    - this is particulary useful for when you want to hide data constructors, and make them use an init function
    - dare I say an "object factory"
    - if Haskell does get refinement types, will these 'smart constructors' become unnecessary?
    - if you don't export the data constructor, code outside of your module can't pattern match on that data type
        - the solution to this is to create a new data type that represents the 'API' of that type, and then provide a function that does a view pattern pattern match of "real data" -> "data 'API'", that can be used outside the module
        - you will need the ViewPatterns language pragma for this
    - the next step is to create a 'pattern synonym' (need language pragma PatternSynonyms)
        - a pattern synonym is composed of three parts:
            1. a type signature of a function that lines up with the data constructor
            2. then a matcher (the -> arrow is reversed here, becoming <-)
            3. finally, a where clause that says that matching on the pattern is the same as calling the smart constructor
        - this is pretty complicated (or rather, I am not familiar with it), there is an example on page 82
    - with a smart constructor and pattern synonyms, you can hide the actual data constructor and still allow users to pattern match on it

## Errors
- you can use the 'error' keyword to throw an error, even if you're not in a monad? is this referentially transparent?

## Lists
- foldr is called 'foldr' because the recursion associates to the right, and it rolls the operation across the list
    - like (1 + (2 + (3)))
- foldl goes the other way, like (((1) + 2) + 3)
- if the operation is commutative, it doesn't matter whether you fold left or right, like with addition
    - for example, subtraction is not commutative, so foldr and foldl will give different results
- foldr1 and foldl1 are the same as foldr and foldl, but it does not take a default initial value
    - it uses the first (foldr) or last (foldl) element of the list as the starting value
    - these operations are called 'reduce' in other languages
- partition, dropWhile, takeWhile and span are all pre-defined functions in Data.List that look very useful
    - partition is like filter, except it returns the elements that passed the check in the first list, and the failed elements in the second
    - span is kind of like partition, but group1 is the elements up to the first failing element, and group2 is everything passed that
    - dropWhile returns the elements after and including the first failed item, and takeWhile returns the elements that passed up to the failure point

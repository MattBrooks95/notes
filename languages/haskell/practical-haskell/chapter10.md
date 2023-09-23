# Chapter 10
## TypeClasses
### Applicative
from page 361
* `<*>` extracts a function from a context and applies one argument to it `f (a -> b)-> f a -> f b`
* `pure :: a -> f a` is basically the same as `return`
* `<$> :: (a -> b) -> f a -> f b`
* lists are `Applicative`, you can use `<*>` with a list of functions on the left, and a list of arguments on the right, and the result wil be the results of each of the functions having each of the arguments being applied to each of the functions in turn, kind of like a product. There is an example on page 363
* `<$>` can be used to call a function with several different arguments, good example on page 363
* Applicative's `<$>` is the same as Functor's `fmap`
* every `Applicative` must also be a `Functor`
* `Applicative` is stronger than `Functor` because `Applicative` works with functions of many arguments
* every `Monad` is also an `Applicative`, this means that you can use applicative operators on `Monads`, like you have with `<$>` instead of `fmap`
* `Monad` is stronger than `Applicative` in a way, because they can do things that `Applicative`s can't.
    * The key to understanding this is to look at the types of `Monad`'s `=<<` operator and `Applicative`'s `<*>` operator
        * `=<< :: Monad m => (a -> m b) -> m a -> m b`
        * `<*> :: Applicative f => f (a -> b) -> f a -> f b`
        * `=<<`'s first arg returns a monadid value `m b`, and `<*>`'s only returns a `b`
        * so with the `Applicative` operator's wrapped function can't change the execution context afterwards, whereas `=<<`s can
        * I still don't understand this... I thought trying to put it in my own words would help. I probably need an example.
        * the `Applicative` operator can leave you with a type (for example, if it was a `Maybe` value) like `Maybe (Maybe a)`, where there's two layers of `Maybe`
        * if it was a `Monad`, we have access to the `join` function (or `.$` operator) that has type `m (m a) -> m a`, so it can "compress" the extra layers into just one for the rest of the expression
### Alternative
* `Alternative` is the type class that works with `<|>`, of type `f a -> f a -> f a`
    * I liked using this operator alot with `Parsec` for parsing
### MonadPlus
* `MonadPlus` is `Monad` + `Alternative`
    * useful for parsing, because of the functions `many` and `some`, which can be used to repeatedly apply parser rules
### Traversable
* (potentially effectful) things that can be iterated over
* `SequenceA` can turn a list of monadic values into a single monadic list `[Maybe a] -> Maybe [a]`
    * `SequenceA` and `Sequence` do what is called "commuting" a functor, the type of the operation is `t (f a) to f (t a)`
    * from Hoogle: `(Traversable t, Applicative f) => t (f a) -> f (t a)`
* `Traversable` is useful because it lets us process a list w/ some side effects, but it doesn't change the **structure** of the `Traversable` data itself
* `mapM_`, the function that you use often to apply `putStrLn` to a list of items, is `traverse` but specialized to monadic actions
* unlike `Functor` or `Foldable`, `Traversable` processing must be done left-to-right, because order may matter
* the `traverse` function is useful for when you need to implement `Traversable` for a data structure

### GHC Pragmas
* using `{-# LANGUAGE DeriveFunctor, DeriveFoldable, DeriveTraversable #-}`, you can add `deriving (Show, Functor, Foldable, Traversable)` to your data structures and GHC will create the instances for you

## JSON (w/ Aeson)
* `lens-aeson` package makes working with nested objects easier
    * keys may not exist, so you have to use `^?` instead of `^.` (not that I know how to use lenses still...)
* usually, you will not parse JSON using pattern matching, you will use an Applicative Parser
    * similar to `attoparsec` or `parsec`
    * `.:` operator extracts a value with the specified key
* the `ToJSON` and `FromJSON` typeclasses are the most convenient way to use Aeson
* aeson works with **lazy bytestrings**, and Conduit works with **strict bytestrings**, so you have to do a conversion when you use **Aeson** and **Conduit**
* a sample decode flow could be `Bytestring -> Value -> SomeUserType`, where the first arrow is the `decode` function, and the second is `FromJSON`
* with the `{-# LANGUAGUE DeriveGeneric #-}` pragma, GHC can derive `ToJSON` and `FromJSON` for you
    * this only works for `data` declarations using the Record syntax

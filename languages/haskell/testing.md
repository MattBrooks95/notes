# Testing in Haskell

## HUnit
This is the first testing tool I learned. It's close to JUnit and seemed like it could be familiar to me because I've written a lot of Jest tests.

### HUnit tips
use (?:) to append a label to your test cases to get easier to read failure messages:
```haskell
"p1 cannot switch" ~: Right 2 ~=? length <$> join playerOneChoices
, "p2 cannot switch" ~: Right 2 ~=? length <$> join playerTwoChoices
```

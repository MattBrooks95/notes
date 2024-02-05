## Chapter 15
from pg 525

* Haddock
    * can make HTML documentation pages for you
    * `-- |` is used to make a comment appear in the documentation, and is written above a line of code
        * `-- ^` does the same but can be written beneath the code
    * single quotes in a Haddock comment can create a link to it for you
        * you can add a link to a module by placing the module name in double quotes
    * `*` makes unnumbered lists of items, and numbered ones with `n.`
        * or `n)`
        * each element should be separated by blank lines
    * code in the comment must be prefixed with >
* HLint
    * gives you code suggestions
    * I think you see these messages in the HLS (Haskell Language Server) messages

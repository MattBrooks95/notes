# Web Applications
- Happstack is a veteran framework
    - book's wording makes it sound old
    - happstack-foundation gives you a more opinionated set of packages that makes it easier to actually make an app
- WAI - Web Application Interface
    - WAI is an interface definition
    - "Warp" is practically the only implementation of it used in production
- Snap is a framework that promotes creating modular web applications
    - units of code code are called 'snaplets'
    - uses Heist as the html templating library
- Yesod is a framework that handles everything, and is configured with embedded DSLs, little languages that you write in quasiquoters to describe your application
    - uses Shakespear for templating, which has some submodules
        - Hamlet (HTML)
        - Cassius & Lucius (stylesheets/css?)
        - Julius (Javascript)
- Servant
    - focuses on using types to model the application
    - generates routing, templating and JSON marshaling code for you, from the types
- Spock (is explained in this chapter)
    - minimalistic (like the Flask framework in Python)
    - most production systems also use `digestive-functors` library to handle data from the client

## Spock
- use PCPool to run a handler that has access to a pool of database connections that you got from persistent
    - `cfg <- defaultSpockCfg () (PCPool pool) ()`
- for a route with an argument, like `person/<id>`, you can use Spock's `var` function to get the value at `<id>` as a Haskell type
    - you need to tell Spock what type the variable should be, so the `ScopedTypeVariables` language extension is nice for this
    - ScopedTypeVariables allows you to write `"person" <//> var $ \(pId :: Integer) -> do ...`
        - where you can specify the type for var inside the parameter list of the lambda
    - `RecordWildCards` language extension is also nice, because it lets you pattern match on a record and bring all of its fields into scope by writing something like `Person { .. }`
    - there is an example on, you guessed it, page 420 that shows using Spock w/ blaze-html to build up an HTML response
- blaze-html lets you build HTML using Haskell functions
- Hamlet lets you build HTML using a DSL in a quasiquoter
- digestive-functors gives an Applicative interface for handling Form data
    - the vanilla way of doing things in Spock is to use the `param*` functions
    - the book implies that using Spock's native solution is unwieldy
    - digestive-functors and Aeson both have a (.:) operator, so qualify the name if you are using digestive-functor and Aeson in one file
    - you have to write types that define your form inputs and do some validation
        - you must specify how to handle validation errors
    - the `spock-digestive` package provides some utilities that makes using Spock and digestive-functor together

## Frontend with Elm
- Skipping the Elm tutorial
- 'commands' are new to me
    - they are a way to get Messages from the outside world, like the results of HTTP requests
    - commands interop with the Message system
    - to use Commands, you must use Browser.element instead of Browser.sandbox to run your Elm application
    - you can use the `init` function to run commands at the startup of the frontend application
- 'subscriptions' poll the backend and create Messages at a configurable interval


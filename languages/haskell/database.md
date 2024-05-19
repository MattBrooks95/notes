# Database

## Persistent
- references:
    - https://www.yesodweb.com/book/persistent
    - https://mmhaskell.com/real-world/databases <- so good
- Peristent abstracts over many kinds of DBs, but for a web app you probably just want Postgres
- you need these libraries added to the cabal file
    - to define schemas and use Template Haskell to generate the marshalling boilderplate you will also need:
        - persistent-template
    - to just run DB actions against a DB, you need these:
        - persistent
        - persistent-postgresql
    - for writing relational database queries in Haskell, try Esqueleto with the 'experimental' (soon to be default syntax)

running a migration might look like this:
```haskell
runStdoutLoggingT $ withPostgresqlConn connString $ \backend -> do
    runReaderT (runMigration myMigration) backend
```

### Persistent Gotchas
- if you're going to use a function like `withPostgresqlConn`, the typescripts for the monad in the function signature should be (MonadIO m, MonadUnliftIO m)
    - MonadIO you're familiar with ("give me any monad that can liftIO and we're good")
    - MonadUnliftIO is strange, I don't fully understand it and it's in a separate package, unliftio-core

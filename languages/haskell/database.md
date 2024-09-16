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

## Esqueleto
Here, I'm trying to do a left join that will find all of the redirect uri's registered for a single oaouth2.0 client. I'm making a mock identity provider for local development. In this code, I get an error message: 
```
Diagnostics:
1. • Couldn't match type: E.Key Client
                    with: Maybe Schema.ClientId
     Expected: E.SqlExpr (E.Value (Maybe Schema.ClientId))
       Actual: E.SqlExpr (E.Value Schema.ClientId)
```

```haskell
foundClients  <- flip runSqlPool pool $ E.select $ do
    (clientsTable E.:& redirectsTable) <- E.from $ E.table @Client
        `E.leftJoin` E.table @Redirect
        `E.on` (\(clientsTable E.:& redirectsTable) ->
                let redirectClientId = redirectsTable E.?. RedirectClientId
                    clientId = clientsTable E.^. ClientId
                in redirectClientId E.==. clientId 
            )
```
and this is happening because a redirect_uri may not exist in the database, but the client definitely will (if it's found). So, I'm trying to compare a maybe value to a non-maybe value. I initially tried to say `clientId = Just $ clientsTable E.^. ClientId`, but this does not work because it would become `Just (SqlExpr (.....))` and I need the `Just` to be inside the SqlExpr for Esqueleto's query building language to work. [This code from the hackage page is a good example of how to do this](https://hackage.haskell.org/package/esqueleto-3.5.11.2/docs/Database-Esqueleto-Experimental.html). My mistake was that I shouldn't use the 'normal' `Just` constructor, I need to use `E.just` from the Esqueleto package.

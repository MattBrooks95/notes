


# Esqueleto Experimental

## Gotchas

### Missing `^.` Operator
missing `^.` operator error. This took a long time to debug because the type names of everything were very similar. The hint was that the hover doc types for the table in the lambda parameters was wrong, it claimed to be a function that needed another parameter.
error:
```
1. • Couldn't match type: E.SqlExpr (Entity AuthorizationCode)
                    with: EntityField AuthorizationCode Schema.ClientId
                          -> E.SqlExpr (E.Value Schema.ClientId)
       arising from a functional dependency between:
         constraint ‘Database.Esqueleto.Experimental.From.ToFrom
                       (E.From (E.SqlExpr (Entity AuthorizationCode)))
                       (EntityField AuthorizationCode Schema.ClientId
                        -> E.SqlExpr (E.Value Schema.ClientId))’
           arising from a use of ‘E.innerJoin’
```

code: 
```haskell
findUserAuthorizationCode :: T.Text -> ST.ActionT AppRunEnv (Maybe (E.Entity AuthorizationCode, E.Entity Client, E.Entity EndUser))
findUserAuthorizationCode code = do
    now <- liftIO Clock.getPOSIXTime
    liftIO $ putStrLn $ "using server time:" <> show now <> " to check the auth code expiration"
    pool <- asks appDbPool
    flip runSqlPool pool $ E.selectOne $ do
        (authCodeTable E.:& clientTable E.:& endUserTable) <-
            E.from $ E.table @AuthorizationCode
            `E.innerJoin` E.table @Client
            `E.on` (\(authCodeTable E.:& clientTable) ->
                    let clientTableId = clientTable E.^. ClientId -- :: E.SqlExpr (E.Value (E.Key Client))
                        authCodeClientId = authCodeTable AuthorizationCodeClient -- :: E.SqlExpr (E.Value (E.Key Client))
                    in authCodeClientId E.==. clientTableId 
                )
-- -> From
--      (SqlExpr (Entity AuthorizationCode) :& SqlExpr (Entity EndUser))
            `E.innerJoin` E.table @EndUser
            `E.on` (\(authCodeTable E.:& _ E.:& endUserTable) ->
                    let endUserTableId = endUserTable E.^. EndUserId
                        authCodeTableEndUserId = authCodeTable E.^. AuthorizationCodeEndUser
                    in endUserTableId E.==. authCodeTableEndUserId
                )
        E.where_ (
            authCodeTable E.^. AuthorizationCodeCode E.==. E.val (T.unpack code)
            E.&&. authCodeTable E.^. AuthorizationCodeUsed E.==. E.val False
            E.&&. authCodeTable E.^. AuthorizationCodeExpires E.<. E.val (round now)
            )
        pure (authCodeTable, clientTable, endUserTable)
```

can you find the error?
```haskell
let clientTableId = clientTable E.^. ClientId -- :: E.SqlExpr (E.Value (E.Key Client))
    authCodeClientId = authCodeTable AuthorizationCodeClient -- :: E.SqlExpr (E.Value (E.Key Client))
in authCodeClientId E.==. clientTableId 
```

there is no `^.` operator in between `authCodeTable` and `AuthorizationCodeClient`!

If you look at the error again:
```
with: EntityField AuthorizationCode Schema.ClientId
      -> E.SqlExpr (E.Value Schema.ClientId)
```

And the type of the operator:
`(^.) :: forall typ val. (PersistEntity val, PersistField typ) => SqlExpr (Entity val) -> EntityField val typ -> SqlExpr (Value typ)`

You can kind of see that they have the `EntityField val typ` in common. Maybe recognizing that sooner could have saved me 40 minutes.

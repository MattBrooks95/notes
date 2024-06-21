
## Defining middleware

A middleware accepts the web application (`innerApp`), and has a chance to inspect the request before the application is ran. Inside the middleware, you can use the `request` parameter to analyze the contents of the request, like the path. Then, depending on what kind of request it was, you can choose to return early (using the `response` parameter) or call the underyling application. This example just prints the path before calling the inner application.

```haskell
-- | just prints the path info to the console for developing
loggingMiddleware :: Middleware
loggingMiddleware innerApp request respond = do
    print (rawPathInfo request)
    innerApp request respond
```

## Middleware policies
The idea is to match a path and then rewrite it.

### Static Files Middleware
rewrite a request who's path begins with "assets" such that the server looks into the `public` directory. Note that the default static middleware disables caching, and should have defaults that prevent paths like "../" that would be security concerns. This prevents malicious URLs from reading files inside of folders that it shouldn't have access to. 

Note that you match on the "absolute" url, but you need to return a `Just` that is the local path so that the server can find the file.

If you've rewritten the path correctly, the Wai static files middleware can find the html file in the `public` dir, and serve it.

```haskell
matchAssetsDir :: M.Policy
matchAssetsDir =
    M.hasPrefix "assets" M.>->
    trace "match assets"
        (M.policy (\s -> Just $ "./public/" <> let x = drop 6 s in trace (show x) x))
```

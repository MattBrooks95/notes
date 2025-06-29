# WebSockets
It looks like WebSockets will be done away with eventually, and replaced with the [WebTransport API](https://developer.mozilla.org/en-US/docs/Web/API/WebTransport_API). MDN says "Limited Availability", but the only holdout seems to be Safari.

# Gotchas

The server & the client must agree on a subprotocol. If the 'Sec-WebSocket-Protocol' header doesn't match, the websocket connection can be terminated by the browser (I think that's what happened), with a useless event type on the resulting erorr object. The server could choose to reject the connection if it didn't like the connection sub-protocols asked for by the client.

## Successful Example of Setting The Sub-protocol
(in Haskell, btw)
Where `pending` has type [PendingConnection](https://hackage.haskell.org/package/websockets-0.13.0.0/docs/Network-WebSockets.html#t:PendingConnection)
```haskell
    let pendingRequestHead = pendingRequest pending
    if ("Sec-WebSocket-Protocol", "json") `elem` requestHeaders pendingRequestHead 
    then do
        let acceptRequestObj = defaultAcceptRequest {
            acceptSubprotocol=Just "json"
            }
        wsConn <- liftIO $ acceptRequestWith pending acceptRequestObj
        liftIO $ putStrLn "accepted a websockets connection"
        forever $ do
            handleMessage wsConn
```

(in Rescript, btw)
```rescript

@new external getWebSocket: (string, array<string>) => t = "WebSocket"

...
let url = `ws://${EnvVariables._SERVER_HOST}`
let webSocketConnection = WebSocket.getWebSocket(url, ["json"])
```

In the browser's Network tab, you'll see `Sec-WebSocket-Protocol: json` in the 'Response Headers' of the WebSocket handshake response.

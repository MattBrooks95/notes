# Cookies

## Why?
Need to give the client some kind of ID that they can use to access the API after they've been authenticated.

## Gotchas
- if the request is cross origin (like a frontend on localhost:8080 talking to an API on localhost:3000), then you will need to set the `Access-Control-Allow-Credentials" value to "true" on the response.
    - TODO I think right now I made a middleware that is putting this header into every response from the server, but it might only need to be set on the request where the response was set...
- the default `path` of the cookie will be the path of the server response. If the path is not set, the browser will not send that cookie to another path. This means that if you authenticate the user on one path, and then the cient needs to hit an api endpoint at another path, the cookie will not be sent. 
    - If you set the path to "/", it'll work.
    - You will also need to set the domain, like `<cookie stuff ...>; Domain=localhost;`
- The browser (fetch api) will not send cookies unless `.withCredentials` is set to true.

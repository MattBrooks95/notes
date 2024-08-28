# Sessions
## A simple, old, insecure? session setup
- looking into this for the purposes of mocking an oauth/oidc identity provider. After a fake login, I need the server to be able to know which user (a user who registers clients with the provider) is logged in, and if they are logged in. I think an old fashioned username/password sign on + session cookies setup will be 'real' enough.
- after authenticating with the server, the server will [set some headers](https://developer.mozilla.org/en-US/docs/Web/HTTP/Cookies) containing the session id for the user
    - note the section about securing the cookies, it looks like there's a simple 'Secure' setting for cookies that will prevent the browser from ever sending the cookie over HTTP (except for localhost), that way it's encrypted while it's going through the network, and could help prevent cookie jacking (session jacking)

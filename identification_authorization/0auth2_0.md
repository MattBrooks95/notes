## Client Types
- 'public' for browser applications
- web applications (web servers, NOT clients like browsers) can use the Authorization Code Flow
    - https://auth0.com/docs/get-started/authentication-and-authorization-flow
- public web <strong>clients</strong> like Single Page Applications are more difficult, they require something like an Authorization Code Flow + PKCE setup
    - [reference](https://auth0.com/docs/get-started/authentication-and-authorization-flow/authorization-code-flow-with-pkce)
    - [spec](https://datatracker.ietf.org/doc/html/rfc7636)
    - Auth0 provides this. I think it's not necessary to implement this in a local mock for testing and local development.

# For Single Page Applications
- [ref](https://www.oauth.com/oauth2-servers/single-page-apps/)
    - don't use a client_secret
    - will use a client_id, which you obtain by regestering your SPA with the provider

# Bearer Tokens
- [ref](https://datatracker.ietf.org/doc/html/rfc6750#section-2.1)
- the resource server must support the 'Authorization' http header and the 'Bearer' authorization scheme
- URI and Form Encoded Body Parameter both seem insecure and/or unusable, use the 'Authorization' header

# Providers
- [Auth0](https://auth0.com/)
- [Amazon Cognito](https://aws.amazon.com/cognito/?nc1=h_ls)

# Mocks
Want to be able to mock this authentication stuff completely in local development and testing, in a way that doesn't cost $.

- [mock-auth2-server](https://github.com/navikt/mock-oauth2-server/tree/master)
    - beware, government project (Norway)

# In Production
- you *must* use HTTPs. Most browsers won't work without it anyway, and it's particularly important for Bearer authentication because if you just use HTTP, the bearer token is sent over the network in plain text and could easily be stolen
- it is recommended that you use short-lived (1 hour or less) bearer token validity periods. I think the idea here is that the refresh token could be used to get another bearer token without inconveniencing the user

## Try to Mock
- [must mock the client registration flow](https://datatracker.ietf.org/doc/html/rfc6749#section-2)
    - for SPAs, SPAs will be of client type 'public'

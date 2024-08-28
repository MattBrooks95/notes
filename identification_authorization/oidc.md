# OpenID Connect

OpenID Connect is a protocol that uses oath2.0 to also confirm the identity of the user, in addition to authorizing the service providers app to ask information about the user signing up.

# Network Protocol
OIDC always uses HTTPS (though, if you make a mock for local development http is fine), because the tokens mustn't be transferred in plain text over the network


# Claims
Claims are basically how you tell the authorization provider which pieces of information you want.

For my purposes (making SaaS tools and games), the bare minimum claims to ask for seem to be:
- email - just the user's email address
- email_verified - a boolean indicating whether or not the user's email address has been validated

# Getting the user's email
[try here](https://developers.google.com/identity/openid-connect/openid-connect#obtaininguserprofileinformation)

# Be Careful
You must validate JWSTs on your server if they come from public clients (which your SPA will be)
[Google has a guide for this](https://developers.google.com/identity/openid-connect/openid-connect#validatinganidtoken)

# Provider
The identity provider is the web service that does the authentication. The client will direct users who are not logged in to the provider, and send it a `redirect_uri`, a link that the provider will send the user to when authorization is done. This is how they get back to the client after finishing their authentication. Here, the server should return a 302 'found' status code, and all of the required fields for the flow being used.

# JWT Libraries
## Server Side
comprehensive list [here](https://jwt.io/libraries)
[jose](https://github.com/panva/jose), has a significantly smaller [bundle size](https://bundlephobia.com/package/jose@5.7.0) than jsrasign
## Client Side
I imagine platforms like Android or IOS probably have built-in libraries, but Javascript has libraries like [oidc-client-ts](https://github.com/authts/oidc-client-ts/tree/main)

This library looks small and uses browser APIs with no dependencies: [oauth2-client](https://github.com/badgateway/oauth2-client). It doesn't have oidc features because the [code necessary to deal with the jwts would bloat the library considerably](https://github.com/badgateway/oauth2-client/pull/131)

references:
[use Google OIDC](https://developers.google.com/identity/openid-connect/openid-connect#python)

## Configuration Info
There is a configuration endpoint for providers at `.well-known/openid-configuration` that will describe which parts of the spec the provider implements. [This has an example](https://help.akana.com/content/current/cm/api_oauth/oauth_discovery/m_oauth_getOpenIdConnectWellknownConfiguration.htm), and you can even hit [Google's endpoint directly to see their real configuration](https://accounts.google.com/.well-known/openid-configuration)

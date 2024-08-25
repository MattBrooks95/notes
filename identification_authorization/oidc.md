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

references:
[]()
[use Google OIDC](https://developers.google.com/identity/openid-connect/openid-connect#python)
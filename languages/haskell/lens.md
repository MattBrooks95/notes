# Lens

Lens combines the capabilities of a getter and a setter into one function. It allows for dot access notation like in OOP languages (but is still just function composition and operators).

## Discovery
- I think Lenses were briefly mentioned in Mena's "Practical Haskell", but I skipped over it at the time because it seemed like a non-essential, advanced library.
- When I was working on making a mock Oauth2.0/OIDC Authorization Provider, I had to use the [JOSE](https://hackage.haskell.org/package/jose-0.11/docs/Crypto-JOSE-JWK.html#t:JWKAlg) libary to verify JWTS. Some of the data types didn't have any obvious way to get or set the value for a JWK (Json Web Key). In reality, the `Lens'` type that was defiend for the JWKs means that I can use the `set` function from lens to set that value. This ended up being useful because I needed to set the algorithm type for the public key JWK entry to state that the algorithm was "RS256", because the mock provider will use the private key to sign messages, and the APIs who need to validate the contents of the ID token will need to use the public key to ensure that the data was not tampered with.

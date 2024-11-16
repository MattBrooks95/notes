# Json Web Tokens

[JOSE Header](https://datatracker.ietf.org/doc/html/rfc7519#section-5)

You should use RS256 for SPA clients. The Authorization Provider (your mock server you made, for example) will have a *private key* and it will communicate the *public key* to the backend (API), so that the backend can verify the ID/access token.

HMAC is useful for web clients that you control (NOT browser applications), because you can just share a key. RS256 is necessary for SPAs because they cannot hold a secret key.

**WRONG** `fromOctets` is the function to use to get a JWK from a ByteString (that you may have read in from a file).
    Actually, this is incorrect if you are trying to use RSA 256 private & public key pair. The OctetMaterial type in the Jose library is supposed to be used for symmetric keys. If you try to sign a JWT with it, it'll throw an error about not being able to use the RS256 algorithm with a symmetric key. See #Getting a PrivateKey From a File

# Getting a Private Key From a File
This isn't simple or straightforward. A hoogle search for how to go from a ByteString to a PrivateKey returns nothing.

- https://stackoverflow.com/questions/20318751/rsa-sign-using-a-privatekey-from-a-file

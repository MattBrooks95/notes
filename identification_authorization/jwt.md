# Json Web Tokens

[JOSE Header](https://datatracker.ietf.org/doc/html/rfc7519#section-5)

You should use RS256 for SPA clients. The Authorization Provider (your mock server you made, for example) will have a *private key* and it will communicate the *public key* to the backend (API), so that the backend can verify the ID/access token.

HMAC is useful for web clients that you control (NOT browser applications), because you can just share a key. RS256 is necessary for SPAs because they cannot hold a secret key.

# Getting a Private Key From a File
**WRONG** `fromOctets` is the function to use to get a JWK from a ByteString (that you may have read in from a file).
    Actually, this is incorrect if you are trying to use RSA 256 private & public key pair. The OctetMaterial type in the Jose library is supposed to be used for symmetric keys. If you try to sign a JWT with it, it'll throw an error about not being able to use the RS256 algorithm with a symmetric key. See #Getting a PrivateKey From a File


This isn't simple or straightforward. A hoogle search for how to go from a ByteString to a PrivateKey returns nothing.

- https://stackoverflow.com/questions/20318751/rsa-sign-using-a-privatekey-from-a-file
- Let's try [Crypto.Store](https://hackage.haskell.org/package/cryptostore-0.3.0.1/docs/Crypto-Store-PKCS8.html)
    - Also going to need [X509](https://hackage.haskell.org/package/x509-1.7.7/docs/Data-X509.html#t:PrivKey) for types
    - This is not going to work because the functions that can read a pem file from x509 are using types (namely `PrivateKey`) declared in a deprecated package `cryptonite`
    - `crypton` is Kazu's fork of `cryptonite`, and the JOSE library that I'm using depends on `crypton` and not `cryptonite`
- Let's try [OpenSSL.PEM](https://hackage.haskell.org/package/HsOpenSSL-0.11.7.8/docs/OpenSSL-PEM.html#v:readPrivateKey)
    - I can't figure out how to get a `PrivateKey` from an `RSAKeyPair`

I ended up needing to write something like this, where `bs` is the bytestring that I read from the .pem private key file:
```haskell
getPrivateKeyMaterial :: BS.ByteString -> IO (Maybe JWK.KeyMaterial)
getPrivateKeyMaterial bs = do
    someKeyPair <- PEM.readPrivateKey asString PEM.PwNone
    let keyPair = toKeyPair @RSAKeyPair someKeyPair
    let materialFromFile = case keyPair of
            Nothing -> Nothing
            Just rsaKeyPair ->
                let d = RSA.rsaD rsaKeyPair
                    n = RSA.rsaN rsaKeyPair
                    e = RSA.rsaE rsaKeyPair
                    -- P is optional
                    -- p = RSA.rsaP rsaKeyPair
                    privateKeyParameters = JWK.RSAPrivateKeyParameters (Base64Integer d) Nothing
                    keyParameters = JWK.RSAKeyParameters (Base64Integer n) (Base64Integer e) (Just privateKeyParameters)
                    material = JWK.RSAKeyMaterial keyParameters
                in Just material
    pure materialFromFile
    where
    asString = (T.unpack . T.decodeUtf8) bs
```
There was no straightforward way to get a `KeyMaterial` that JOSE requires to make a `JWK`, which is required to sign the claims. I had to import and use the haskell openssl bindings (this meant adding `openssl` as a dependency for the project's Nix flake) to read in the private key from a file, then get the RSA parameters in the math equation (`d`, `n` and `e`), which I could then use to create the types that JOSE wanted (RSAKeyMaterial)

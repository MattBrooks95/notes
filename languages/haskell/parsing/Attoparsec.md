# Attoparsec

### Gotchas
- functions for parsing numerical values are in `Data.Attoparsec.ByteString.Char8`
    - if imported qualified as `APC`, you can use `APC.decimal` to match an unsigned decimal number
    - I thought `APC.signed` could be used as-is for negative numbers, but the type is `Integral a => Parser a -> Parser a`. Looking at the d
ocumentation didn't give me a hint of what the argument should be, but I found out that it could be a function for the purpose of parsing a double or an integer
    - so, `APC.signed APC.decimal` seems to be the key to quickly parsing an integer that could be negative

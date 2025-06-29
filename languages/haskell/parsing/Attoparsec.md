# Attoparsec

### Gotchas
- functions for parsing numerical values are in `Data.Attoparsec.ByteString.Char8`
    - if imported qualified as `APC`, you can use `APC.decimal` to match an unsigned decimal number
    - I thought `APC.signed` could be used as-is for negative numbers, but the type is `Integral a => Parser a -> Parser a`. Looking at the d
ocumentation didn't give me a hint of what the argument should be, but I found out that it could be a function for the purpose of parsing a double or an integer
    - so, `APC.signed APC.decimal` seems to be the key to quickly parsing an integer that could be negative
- `parse` expects to eventually be explicitly given a 'message' that says 'this is the complete end of the input'. This is because it's for lazy parsing messages in chunks.
    - this will cause a message that should work to fail, even though the input string is parseable. It's better to use `parseOnly` when you aren't doing lazy streaming, because this concern goes away and `takeByteString` will work as you expect it to.

### Debugging
If you use `maybeResult`, you'll throw away any hints you have about why a parser failed.Even if you want the code that you are writing to have a `Maybe` result, there should be some step that uses `eitherResult` instead of `maybeResult`, because at that section you can use `Debug.Trace.trace` to print the error message from the `Left` error constructor when it fails. It can be easy to not understand why a parser is failing because the input "looks correct", but you mave forgotten to use the entire input in your parser. In this case, `eitherResult` returns `Left err` where `err` will be a message like "incomplete input
"
It's mentioned in the [section](#gotchas), but you should use `parseOnly` when you're not trying to do streaming parsing. In my case, I was trying to parse Redis messages, so I should have been using `parseOnly` to avoid a long and painful debugging session.

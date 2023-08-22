# Halogen
## References
- good component introduction: https://github.com/purescript-halogen/purescript-halogen/blob/master/docs/guide/02-Introducing-Components.md
- this section shows an example of how to use a text input https://github.com/purescript-halogen/purescript-halogen/blob/master/docs/guide/03-Performing-Effects.md#an-aff-example-http-requests

## Tips
- `HH.div` is a function that takes a list of properties from the HP module (import statement below) and then a list of child elements
    ```
    import Halogen.HTML.Properties as HP
    ```
    - there's a pattern where, HTML functions have another version with an underscore, and these only accept a list of children like `HH.div_ [ *children here*]`

# Halogen

## Tips
- `HH.div` is a function that takes a list of properties from the HP module (import statement below) and then a list of child elements
    ```
    import Halogen.HTML.Properties as HP
    ```
    - there's a pattern where, HTML functions have another version with an underscore, and these only accept a list of children like `HH.div_ [ *children here*]`

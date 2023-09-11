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
- It's mentioned in the docs, but having an `Input` type that is used by `InitialState` to set the component's state isn't enough to get the component to re-render when new data comes in through input, you have to use the `receive` field of the `eval` record to fire off an `Action`, and then have the `Action` to the state update
```purescript
data Action = SelectFile String
  | Receive (S.Set String)
...

H.mkComponent
{ initialState
, render
, eval: H.mkEval $ H.defaultEval {
    handleAction = handleAction
    , receive = Just <<< Receive
    }
}

...

handleAction :: forall output m. Action -> H.HalogenM State Action () output m Unit
handleAction action = case action of
  ...
  Receive filenames ->
    H.modify_ (\oldState@({ fileState: fs }) -> oldState { fileState=fs { available=filenames } })
```

## Gotchas

I added a package to my project with `spago install matrices`, but in the editor when I wrote `import Matrix as M`, the LSP was giving me an error about the package not being found
In the package.json of the project, I had this line: `  "watch": "spago build --watch --then \"npm run bundle\"",` that I was using to run spago in watch mode. The issue was that spago must not have told the compiler about the new package that was added while it was running. I had to restart the `watch` command to get the project to compile.

`[]` declares an array that is compatible with a Javascript array in Purescript, not a List
It kinda looks like you use Arrays more than Lists in purescript, since the handy syntax `[]` is reserved for arrays. Importing Data.List gives you Haskell-like lists, but maybe they're not used as much as they are in Haskell.

I didn't like this `forall` bit: `renderInputs :: forall w i. M.Matrix String -> Array (HH.HTML w i)`. I assumed that you could leave it out, but it doesn't look like that's the case.
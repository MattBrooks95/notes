## Getting Rescript Going
- installed the Vim plugin to my neovim-flake project's flake
    - had to learn how to actually layout the vim runtime `pack` directory to get all the plugins to work

### LSP
- the LSP looks like it's simple happy fun times for vs**** users
- I'm having a hell of a time getting it to work with Nix + Neovim
    - it expects a global executable `rescript-language-server` to be present. I tried using a shell alias to trick it into doing `npx @rescript/language-server`, and using a local version installed in the Rescript project's package.json, but that did not work. Errors from the nix store about not being able to link dynamic libraries.
- what's nice is that just installing the NPM package lets you open a terminal and say `npx rescript -w` to put it into a watch mode. The compiler error messages look good, and it takes like 38 msec for it to react to changes in their Vite program example
    - I *could* start learning Rescript with just this, but figuring out the editor integration would be nicer.
- I'm thinking I might try and make a flake that builds the rescript language serer from source, and then I can install it into my project flakes.
    - if I actually got it to work, I could try putting it into nixpkgs

# Input Elements
I'd like to learn a less verbose way to get the string from a text input in the onChange handler.
```rescript
	let (userName, setUserName) = React.useState(_ => "")
	let (password, setPassword) = React.useState(_ => "")
	<Frame>
	<div>
		<div>{React.string("Login")}</div>
		<label>{React.string("Account Name:")}
			<input type_="text"
				defaultValue=userName
				onChange={(evt: JsxEvent.Form.t) => {
					let targ = evt->JsxEvent.Form.target
					let val = targ["value"]
					setUserName(_ => val)
				}}
			/>
    ...
```

# Gotchas
## External
I was trying to make bindings to [a Typescript library](https://github.com/authts/oidc-client-ts) and I had a hell of a time trying to figure out why the generated Javascript caused runtime errors.
```rescript
type user
type oidcManager

@new @module external userManager: unit => oidcManager = "UserManager"
```
Where 'UserManager' is the name of a module and a class in the package. The JS that Rescript would create would have an import statement like `import * from "UserManager"`, and Vite wouldn't be able to find that Javascript dependency. The problem is that I needed to specify the package name as well, which can be done with the string argument to the `@module` decorator when you write the FFI bindings.

# 10:00~ The Vim Project After Bram, Christian Brabandt
They had to update a lot of the website and project infrastructure after Bram passed. They were able to get in contact with his family to be able to add themselves as maintainers of critical repos so they could pick up the project.

It sounds like they're still having a hard time getting the project's infrastructure working the way that the want it to. He shared an anecdote about him pushing a change, and he thought that CI would run but it didn't, so he broke a bunch of stuff briefly.

VIM development has been continuing despite the difficulties of migrating the project.

Wayland support is being improved. Issues with the clipboard, because Wayland's clipboard is different than XORG's.

Backwards compatibility is still important to Christian.

He (Christian) claims that Vim is more or less in "maintenance-mode". He doesn't think the current maintainers are going to have the time & funding to develop new features. They are talking about treesitter support for example, but he says that doing that would be difficult.
Focusing on:
- documenting
- translating
- bug fixing

## What Can be Improved?
Christian would like for the website to be updated to be more modern, but it is not going very well.

The "styling" of the existing code may be updated. It's currently written in a style that Bram liked, but Christian would like to format it in a bit more standard way to encourage contributions and to have the codebase be easier for new users.

How can we make Vim9 script be successful? Christian says that people seem to like it.

The project will continue using C. Christian wants to use "safe & defensive C style" for security.

Apparently Vim still has a Python2 interface. He would like to get rid of it because Python 2 is no longer maintained and the Python community has moved on to Python 3.

Christian would like Vim to support GTK4 for the GUI build.

Maintaining Vim is a full time job, you have to maintain the code, community and expectations.

Q&A
"What you you think about Neovim?"
Christian: "I think Neovim is more popular for developers, it doesn't tend to be available on remote servers, and that's where Vim shines. I think Vim & Neovim can work quite nicely together. We should work towards ensuring an amount of compatibility between Vim & Neovim (in regards to configuration, runtime files..)"

"I have a single question, I'm an old guy. I don't have a GitHub account, but I use vim.org to upload my scripts. I'm the author of the <what?> language, my project's tarball is quite large, and I can't update it to the Vim website"
Christian: "Let's talk about this offline, so we can figure it out. How big are these files?"
"It's about 20Mb"
Christian: "I think the limit is 10MB, but I'm just guessing."

another person: "Do you have any advice to start contributing to Vim?"
Christian: "It depends on what yo uwant to contribute, it depends on what you want to do. You need to start small. The runtime files are mainly legacy vimscript, and maybe some vim9script, if you want to go into the core features, you'd need to use C. Neovim is using Lua, but C is quite different than C. If you know C or vimscript, you should be able to contribute to the code."

Another person:
"How is the Vim project management going to be moving forward? Will it be more community-driven, or are you the new benevolent dictator for life?"
Christian: "I am not the BDFL. I am the one who merges changes to the source repository right now. We need to increment version numbers so all new releases go through me. Even if Ken had a patch that was good, I would be the one that pull the changes to ensure that we don't have a merge conflict on the version #. It's not me making decisions, I don't feel comfortable making such decisions. I want to have community input before I make decisions. The other maintainers don't want to merge anything and are waiting for me to merge their patches because t hat's what makes them comfortable."

another person:
"Do you have any issues with the Japanese-English language barrier? Or other languages in Europe."
Christian: "Since it's an international project, English is lingua franca. I can't speak Japanese even though I would like to. The only way to communicate across different maintainers is to use English."

another person:
(in Japanese) "How do you want new young people to contribute to the project?"
Christian: "Do you want to contribute to Vim?"
other person: "Do you have any requests of beginners who want to contribute to the problem?"
Christian: "I suggest that you connect with the Japanese community, and work together to be able to contribute to the project. I think that you should emphasize having good translations for the help files to help new young Japanese users get into the project. Host events, make friends, maintain the strong community. Report bugs, and when you see the bug fix, check the source code to try to see why it was changed, how it was changed, and then learn how the code works by doing so."

# ~11:20 Teej, "Neovim made me a better developer"
What is a "Better" Software developer?
"A good software developer is other software developers being able to enjoy working with my code. Customers enjoy working with me. Git Blame is not my enemy"
Neovim was Teej's first editor in college, after Eclipse (an IDE).

Start with terrible arguments (These are the things that I do not mean to say), then we'll move to better arguments. Then I'll answer the question.

Worst arguments:
- "It's written in C so it must be fast!"
    - even if the software you use is fast, that is not going to make YOU a better developer
- "Hah, you use a browser to edit text!"
    - not good to talk down to people
- "Only noobs use VSC*de"
    - tools do not make the software developer. Some of Teej's "best coworkers used VSCode or Jetbrains, and I loved working with them even though they were wrong XD"

Yes, But No:
- about responsiveness:
    - responsiveness is not everything, sometimes it just means that you see your bad code faster
    - "Some people can go very fast in Sublime Text (like Ginger Bill who made the Odin language).
- "Plugins make me go faster"
    - "no amount of plugins will turn a junior developer into a senior developer"

Yes, But Just Barely No:
- "Open source"
    - "It's hard to replicate all of the factors that help you become an open source contributor. Right project, right help, right issues, right time, right mentors in your life where you can have a chance to do some great open source work and propel your career forward.
    - "There are great chances to learn and be mentored in open source, but it's difficult to replicate."
    - "I don't think that's the primary way that I was able to become a bettor development."
- Joy
    - Feeling Joy in what you do.
    - Programming is a career. A career is a marathon, not a sprint.
    - Personalized Devopment Environment (PDE), a way to bend your editor to be how you like it so that it can bring joy & happiness to your workflow.
    - But this isn't perfect because you can become happier but you may not necessarily become a better developer.
- Yes! With Stories
    - First corporate software job. Worked at Epic (not the Fortnite company). Medical record billing software.
        - few hundred million users
        - one in a million happens a few times a day in that setting
        - must be very careful with people's medical records
    - transforming insurance data into data that could go into patient charts. Very complicated data pipeline. No opinions about how it should work.
        - creation of data/software was "far away from me"
        - customers were "far away from me"
        - big disconnect, and the latency between wanting to know something from a customer and getting their answer back took a long time.
    - cadence of releases was difficult
        - could take 6 months to a year before changes would make it to a production environment
        - makes it hard to get the project right when you have that kind of latency on the feedback
- Story 2: Neovim Configuration Experience
    - Writing my own config, a small plugin for me and my coworkers.
    - the project is for me, I care about it, it's something that I want
    - I can evaluate if the project meets my needs
    - I have a customer (myself) that I can talk to. I can trust that they'll give feedback.
    - the cadence of this release->feedback->release loop means that I can actually *practice* what I do. Reading about good software development is not enough. You need to practice.

The talk isn't about Neovim, it's about you finding a playground to practice coding in. It may not even be vim or neovim, but you need to find the place where you can iterate on your ideas, try new things and practice all of these aspecs of becoming a better developer. Find the thing where you care about the product, where you know (or are) the customer.
Could even by emacs warota

# Lunch 12:20 ~
Sukiyaki. It was good. Grabbed one to bring home to share.

# Mastering Quickfix - daisuzu
basics:
1. create list (grep, vimgrep)
2. :copen <- open quickfix (:cclose to close)

`:cc` jump to entry
`ctrl+w <cr>` will open entry in a new window!
`:clast` jump to final entry
`:chistory` quickly dispaly quickfix list entry history
`:cdo` execute a command for each entry in the list (like a substitution! `:cdo s/OLD/NEW/g`
`:cfdo` execute command for each file

location list
- very similar to quickfix
    - quickfix is a single global list
    - location list is local to a single window

you can save your quickfix lists to a file, to be easily resumed later
`:w <filename>` while in the quickfix list to save the contents of it to a file

he explains how to use vim macros

# Kota Kato (kat0h on Slack) - hacking vim script
2nd year university student

The Vim codebase is huge, and there are a lot of macros that use a lot of CPU. He recommends building the CTags for all of the files in the project before you start working on it.

function definitions are added to `global_functions`	
must define a no-oup function f_debug() in src/evalfunc.c

he also described how to set up a Vimm debug build, so that you can use gdb to figure out what your code is doing

eval1~eval9 functions pares and execute expression strings step by step

vimscript's memory management is accomplished by reference counting, which means that it cannot free circular references

# Switch Projects Like a Ninja, Yuki Ito
- presenter works at 'newmo'
Vim has a built in session feature! `:h session`
worth looking into

I think you save a session with a `mksession` command or something like that

you can even save your undo history!

# Vim Meets Local LLM
Japanese presenter. I don't see his name written anywhere (yuys13 is his screen name)
[実践VIM/Practical Vim](https://tatsu-zine.com/books/practical-vim) <- there is a book that the talk is based about (or talking about?)

LLM doesn't interfere with normal Vim usage.

Local LLMs can now run well, even on laptops

The local LLM is called Ollama

What is "Fill-in-the-middle?"
He's talking about using the model to fill in the important parts of a program by specifying the prefix and suffix of the solution and then having the LLM figure out what should go in between the two regions? I'm not understanding the intent here. He's explaining the Vim and Neovim APIs for getting regions of text very well.

His slides actually describe the steps involved in making a small LLM auto completion plugin.

# Creating the Vim Version of Vs\*\*\*e Dev Containers
store configurations in separate devcontainer.vim and devcontainer.json??? files

vim cannot yank from the container because it is sandboxed, can't make it to the system clipboard

he made a tool to yank the text with TCP

# Neovim for Frontend Developers - RYOPPIPPI
RYOPPIPPI lives in the UK, also participates in the Svelte community

neotest can run unit tests and show results in neovim

chat keeps talking about oil+sonictemplate for file operations

he has it set up so that he can click on an element in the browser, and it will jump his Nvim instance to the source code for that component

# Buliding Neovim Plugins - Abishek Keshri

The presenter makes Youtube videos.

Why plugin development?
- boost productivity
    - make your own tools
    - enhance integration
    - empower community

`{ nargs = '*' }` after a function definition in Lua will allow it to take configuration options
^ I didn't type this fast enough, he's on another slide. The syntax written here might be incorrectly written down (by me).

you can annotate your keybindings when you add them to a plugin via configuration, and the presenter recommends doing this

I think he's talking about his plugin `octohub` here

[plenary](https://github.com/nvim-lua/plenary.nvim) seems to be really nice for plugin development, he says that it provides some nice APIs for doing asynchronous stuff in Lua

# Can't Help Falling in Vim - Arisue
Making his own fuzzy finder.

fuzzy finders are different from tree viewers in that fuzzy finders do not need the content to be searched to be organized. A tree viewer requires a sensible hierarchy.

Arisue-san has made several plugins.

he likes beer, sushi and ramen

A history of fuzzy finders.
- 2007 FuzzyFinder (vim 7.1 plugin)
- 2010 unite.vim (made by Sho)
    - modular, customizable design
- 2011 ctrlp.vim
- 2015 fzf.vim
- 2016 denite.nvim - used Neovim remote plugin capability
- 2019 fzf-preview.vim
    - emphasized the preview feature
    - this feature was eventually added to fzf
- 2020
    - telescope.nvim
    - written in Lua, great performance
    - customizable
- 2021 ddu.vim
    - written in Typescript with Denops
- 2024 Fall
    - combines all of the best aspects of all of the listed fuzzy finders (the author hopes)

## Fall
support vim and neovim

focuses on the core of what fuzzy finding should be, back to basics

programmer friendly, "typescript-first design makes developer's lives easier"

hard to support both neovim and vim, because they are diverging technically (lua vs vimscript 9)

Fall uses Denops as well

Fall is a modeless UI, because the only user input that is necessary is a user's input (the search string)

Fall has a keymap display feature

you can start a submatch with Fall
1. match something with your first search string
2. start a nested fall instance which inherits the result of the first Fall results
3. continue typing a new search string to further refine the results

# The Latest dark deno powered - Shougo
Shougo
- makes vim/neovim patches 
- vim-jp member
- working for vim/neovim 2008-2024


people have different environments and want different features, that's why configuration is important

"your time exists to be used for the text editor!"

real minimal plugins are
- configured by yourself
- extensible
- do not work if you don't install or configure them
- no side effects

"commonly used plugins are too user-friendly"
"it feels uncomfortable when plugins work without any configuration"
"I don't consider the users who don't want to configure the plugin"

# Lightning Talks
lightning talk summaries are written [here](https://vimconf.org/2024/sessions/), I probably can't type fast enough to follow

good talk about using vim's `:help` feature
`:helpgrep <keyword>` searches through help entries based on a search string
`:help quotes` has a list of humurous and insightful anecdotes
the company that makes Pairs! (the dating app) is hiring

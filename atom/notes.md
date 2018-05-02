# OpenSourcePot for Atom

Getting into the Atom repo as a programmer who's just starting to use the editor. What would I be contributing to, and how can I contribute? Here's my understanding of the project.

## Landscape
Main page: https://github.com/atom/
Docs: https://atom.io/docs

Repos:
- atom text editor: the main project
- **xray**:	the bleeding edge project in Rust
- **github**: integrating the editor into GitHub
- **teletype**: remote sharing built into Atom (use case: share workspace through e.g. chat link)
- **atom.io**: for website and apm package API feedback
- **flight-manual.atom.io**: autogen documentation
- ...plus a multitude of repos' worth of hackery support for various languages and uses

Problems and solutions:
- discussing
	- https://discuss.atom.io/t/join-us-on-slack/16638?source_topic_id=25406
	- there's an atom forum but it does say to join Slack: https://discuss.atom.io/
	- forum actively receives posts but >0 messages refer to the help found on Slack
- traversing and documenting
	- Atom Manual and associated issues in its repo
	- Atom Environment manual for packages API
	- site and apm feedback through atom.io issues
- GitHub issues
	- many open issues across projects, including tagging for "beginner" or "good first issue"
	- the core atom repo has a lot of stuff tagged "more information needed"
	- definitely doing active triaging
- PRs
	- quite a few over the last months
	- even split between passing/failing
	- can't spot a pattern in proposed merges
	- tests: circleci, appveyor, travis

### Whispers
- Atom is developed on an old version of Electron
- the team wants to update Atom to work on the latest Electron (a complex task)
- they've added Rust and other enhancements but what breaks when you upgrade?
- might also help to work on the GitHub integration package (big help for support)

## Key bindings
Use the base keymap as a kind of (cheat sheet)[https://github.com/atom/atom/blob/master/keymaps/base.cson].

## Flight manual
https://flight-manual.atom.io/

### 1. Getting Started

#### Why Atom?
- editor property matrix
	- +convenient, -extensible: Sublime
	- -convenient, +extensible: Vim, Emacs
	- +convenient, +extensible: Atom (you are here!)
- Nucleus
	- native web: unchained from browser
		- built on Chromium, essentially like a local JS client rendering pages
		- take advantage of everything a Node app provides
		- no worries about assets, script concatenation, async modules
		- just define and require scripts like normal
		- Node constrains it to be small package driven
	- JS & C++
		- easy interaction with native code
		- example: writing a wrapper around Oniguruma regex engine for TextMate grammar
			- "Node integration made it easy."
		- Atom also exposes APIs for the editor itself (app/context menu items, window, ...)
	- web tech
		- always latest Chromium (avoids shims/polyfills)
		- layout based on flexbox ("emerging standard")
		- instead of tethering to native UI focus on web toolbox as standard
- Open Source
	- fits with GitHub mission: "building better software by working together"
	- lasting community that supports future growth needs to be open source

#### Installing Atom
- usually simple to download and install locally from the Atom website
- first time it runs Atom will try to install `atom` and `apm` commands for terminal
- for portable/removable storage install and create `.atom` directory alongside the app
	- stores configuration and state so must be set to writeable
	- an existing directory can be moved
	- can also store Electron data inside it with a `electronUserData` subdirectory
	- _or_ set `ATOM_HOME` environment variable to point to a directory
- Hacking on Atom core (we'll get there) shares how to clone and build source code
- SSL and proxy issues

#### Atom Basics
- startup welcomes you with the Welcome screen
- look up terms in the [Glossary](https://flight-manual.atom.io/resources/sections/glossary/)
- Command Palette (Cmd+Shift+P)
	- "the most important command in Atom"
	- use it as a quick search through all commands
	- see associated keybindings
	- these are the terms used in the manual
- Settings View (Cmd+ or _Atom>Preferences_)
	- change themes, text wrapping, font, scroll speed, tab size, ...
	- also the place to install new packages and themes (more on Atom Packages later)
	- changing theme
		- 4 UI themes and 8 syntax themes ship with Atom
		- change a theme or install a new one under the Themes tab in Settings sidebar
		- UI themes are for the presentation elements
		- syntax themes are for the text highlighting
		- customizing is possible (later in Style Tweaks)
		- creation is possible (later in Creating a Theme)
	- whitespace and wrapping
		- Soft Tabs inserts spaces on tab input
		- Tab Length
		- Soft Wrap does text wrapping to panel
		- Soft Wrap at Preferred Line Length wraps at a character value (default: 80)
	- more customization later in Basic Customization
	- files
		- typical ways of opening and closing and saving, but also through `atom` command
		- also multiple directories by passing all to `atom` command line tool
		- show/hide the tree: `tree-view:toggle` (Cmd+\\)
		- focus the tree: Ctrl+0
			- add files or folders: A
			- move files or folders: M
			- delete files orfolders: Delete
		- Tree View is a standalone package shipped with Atom (it's "core")
	- project files
		- have a project open
		- use fuzzy finder to find any project file: Cmd+T, Cmd+P
		- search currently opened files: Cmd+B
		- search new or modified files since last commit: Cmd+Shift+B
		- filter settings used during search:
			- `core.ignoredNames` (interpreted as glob patterns)
			- `fuzzy-finder.ignoredNames` (interpreted as glob patterns)
			- `core.excludeVCSIgnoredPaths`
			- for larger projects, customize ignored patterns
			- glob patterns follow [minimatch](https://github.com/isaacs/minimatch)
- "Core" packages ship with Atom, "Community" packages do not
	- you can edit the core packages the same way as any other feature
	- `core.ignoredNames` is shorthand for "Ignored Names in Core Settings"

### 2. Using Atom
#### Atom Packages
- ships with 90+ basic core packages (Tree View, Settings View, Welcome screen, spell checker, Fuzzy Finder, ...)
- open Settings View (Cmd+) and click "Install" tab to start searching for new packages
- Package Settings
	- once installed, packages show up under this tab within the Settings View
	- search under the "Installed Packages" heading
	- packages have their own Settings screens
	- upgrade here too when Atom automatically detects new release
- Atom Themes
	- search for themes in the Settings View too
	- remember these include UI or syntax themes
	- often see a preview and decide whether to install
- Command Line
	- `apm` can install themes and packages: `apm help install`
	- basic structure of the command: `apm install <package_name>`
	- installing a specific version: `apm install <package_name>@<package_version>`
	- search package registry for a specific term: `apm search <search_term>`
	- find more info about a specific package: `apm view <package_name>`

#### Moving in Atom
- arrow keys:
	- up, down, left, right
	- word start: Alt + left
	- word end: Alt + right
	- line start: Ctrl + left
	- line end: Ctrl + right
	- file start: Cmd + up
	- file end: Cmd + down
- avoiding moving hand to arrow keys:
	- move right or left 1 character: Ctrl + F / B
	- move up or down 1 character: Ctrl + P / N
	- word start: Alt + B
	- word end: Alt + F
	- line start: Ctrl + A
	- line end: Ctrl + E
- specific line: Ctrl + G
	- even takes input for specific row:column like `40:23`
- movement commands without default keybindings
	- add them to your `keymap.cson`
	- or just access them in the Command Palette
	- example: move to line at top of screen: `editor:move-to-beginning-of-screen-line`
	- example: select to end of last word: `editor:select-to-previous-word-boundary`
- the following movement and selection commands have default keybindings:
```
	editor:move-to-beginning-of-next-paragraph
	editor:move-to-beginning-of-previous-paragraph
	editor:move-to-beginning-of-screen-line
	editor:move-to-beginning-of-line
	editor:move-to-beginning-of-next-word
	editor:move-to-previous-word-boundary
	editor:move-to-next-word-boundary
	editor:move-to-previous-subword-boundary
	editor:select-to-beginning-of-previous-paragraph
	editor:select-to-end-of-line
	editor:select-to-beginning-of-line
	editor:select-to-beginning-of-word
	editor:select-to-beginning-of-next-word
	editor:select-to-next-word-boundary
	editor:select-to-previous-word-boundary
```
- (the juxtaposition above confused me - I'm writing up an issue)
- Navigating by Symbols
	- open the Symbols View: Cmd + R
	- fuzzy filter (much like Cmd + T did for files and folders)
	- searching for symbols across the whole project
		- generate a `tags` file using [ctags](https://ctags.io/)
		- install ctags and run a command to generate `tags`
		- use generated `tags` to search project: Cmd + Shift + R
		- go to declaration of symbol under cursor: Alt + Cmd + down
		- return from declaration of symbol under cursor: Alt + Cmd + up
		- create `.ctags` customize how `tags` are generated ([here's an example](https://github.com/atom/symbols-view/blob/master/lib/ctags-config))
	- symbols navigation is implemented in [Symbols View](https://github.com/atom/symbols-view)
- Bookmarks
	- jump to specific lines across your project
	- toggle a bookmark: Cmd + F2
	- small bookmark symbol added to line gutter
	- cycle to next bookmark in focused file: F2
	- cycle to previous bookmark: Shift + F2
	- see all current project bookmarks: Ctrl + F2

#### Atom Selections
- supports many actions: scoping delete/indent/search, marking text to  quote/bracket, ...
- selection keybindings add Shift
	- select up: Shift + up / Ctrl + Shift + P
	- and so on for word start/end, line start/end, ...
	- includes Cmd + Shift + up/down for selecting to top/bottom of file
- specific selections for whole content areas
	- select entire file contents: Cmd + A
	- select entire line contents: Cmd + L
	- select current word: Ctrl + Shift + W

#### Editing and Deleting Text
- Basic Manipulation (of buffer text)
	- join next line to this: Cmd + J
	- move this line up/down: Cmd + Ctrl + up/down
	- duplicate this line: Cmd + Shift + D
	- uppercase/lowercase this word: Cmd + K Cmd + U/L
	- transpose two characters (metathesis): Ctrl + T
	- hard-wrap paragraph at current `editor.preferredLineLength`: Alt + Cmd + Q
- Deleting and Cutting (text out of the buffer)
	- delete this line: Ctrl + Shift + K
	- delete to word start: Alt + Backspace/H
	- delete to word end: Alt + Delete/D
	- cut to line start: Cmd + Backspace
	- cut to line end: Cmd + Delete
	- delete to line start: Cmd + Backspace
	- delete to line end: Cmd + Delete
- Multiple Cursors and Selections (especially for long lists)
	- add cursor: Cmd + click (continue holding Cmd to add more)
	- add cursor above/below current cursor: Ctrl + Shift + up/down
	- select next instance of this same word: Cmd + D
	- select all instances of this same word: Cmd + Ctrl + G
	- convert multiline selection to cursors: Cmd + Shift + L
	- example uses: change variable case, or duplicate lines
- Whitespace (whitespace package)
	- search Command Palette for converting tabs <-> spaces
	- the whitespace package settings are available in Settings View
	- default settings include removing trailing whitespace when buffer saved
	- default settings include adding trailing newline on buffer save
- Brackets (bracket-matcher package)
	- includes highlighting of complement to current bracket in pair
		- does the same with HTML or XML tags
	- autocomplete for enclosing syntax tokens (parens, backticks, quotes, ...)
	- if selection, typing the opening character will enclose the selection
	- jump to the next matching closing bracket: Ctrl + M
	- select all text inside these brackets: Cmd + Ctrl + M
	- close this HTML/XML tag: Alt + Cmd + .
	- bracket-matcher settings available in Settings View
- Encoding (encoding-selector package)
	- open encoding support to change encoding: Ctrl + Shift + U
		- changes to encoding are persisted on next buffer save
	- Atom autodetects opened file encoding
	- default for undetected or newly opened files is UTF-8

#### Find and Replace
- find-and-replace package, which uses Node `scandal` for searching
- search buffer: Cmd + F
- search project: Cmd + Shift + F
- find next: Cmd + G (or just press Enter)
- clear find panel: Esc
- Find and Replace options in panel:
	- toggle case sensitivity
	- perform regex matching
	- scope search to selections
	- limit search to subset of files with glob patterns
	- limit search to a folder by entering folder name

#### Snippets
- snippets package
- type a short string, press tab, see snippet expand
- example: `habtm` expanding to `has_and_belongs_to_many`
- `language-html` package comes with many snippets
	- type `html` then press tab for a basic page snippet
	- notice the cursor position between title tags
	- pressing tab again will move to body tags
- `snippets:available` lists all snippets through Command Palette
	- use fuzzy search to find one
	- select one below the selection box to execute
- creating snippets
	- your `~/.atom` directory contains `snippets.cson`
	- menu Atom > Snippets opens that file
	- format
		- left (top) key selects scope
		 	- starts with dot like CSS class selector
			- find scopes in a language package listed as its Scope (Settings View)
		- example: look up `language-java` package to find Scope `source.java`
		- the next level key is a readable snippet display name
		- the next inside that is a `prefix` used to trigger the snippet
		- beside the `prefix` is a `body` to insert once triggered
		- inputting `$` plus number indicates a tab stop
			- on tab the cursor will stop at this location in the buffer
			- multiple same number stops create multiple cursors
			- stops can select, like ${1:"string"} will leave the string selected
```
'.source.js':
	'My Snippet':
		'prefix': 'hi'
		'body': 'Hello World!'
```
	- multiline snippet bodies: surround with `"""` above and below body
	- snippet to create snippets: `snip` then hit tab
	- multiple snippets
		- just include snippet name, prefix, body under the scope
		- CSON keys are not repeatable (last overwrites just like JSON)
```
'.source.js':
	'My Snippet':
		'prefix': 'hi'
		'body': 'Hello World!'

	'My Other Snippet':
			'prefix': 'bye'
			'body': 'Goodbye World!'
```

#### Autocomplete
- autocomplete-plus package
- use tab or enter
- defaults to matching strings in current open file
- Settings for the package can toggle looking through all open buffers

#### Folding
- hover mouse over gutter to hide code blocks
- hide keybinding: Alt + Cmd + [
- unhide keybinding: Alt + Cmd + ]
- fold everything: Alt + Cmd + Shift + [
- unfold everything: Alt + Cmd + Shift + ]
- fold at an indentation depth: Cmd + K Cmd + 0-9
- fold arbitrary selection: Alt + Cmd + C
	- "Fold Selection" also available through Command Palette

#### Panes
- split any pane horizontally: Cmd + K left/right
- split any pane vertically: Cmd + K up/down
- switch between panes: Cmd + K Cmd + up/down/left/right
- _pane items_ are represented by tabs
	- drag files between panes
	- you can disable the tabs package
	- close all pane items: Cmd + W
	- core settings "Remove Empty Panes" to auto-close when all items closed

#### Pending Pane Items
- (formerly Preview Tabs)
- single clicking a new file from Tree View shows title in italics
	- this file is pending
	- next pending file will replace it
- confirm the pending file:
	- double click the tab
	- double click the file in Tree View
	- edit the file contents
	- save the file
- disable pending pane items through Core Settings "Allow Pending Pane Items"
	- single clicks will select but no longer open files

### 3. Hacking Atom
-


### 4. Behind Atom

## Stepping into the code
- [ ] Read the flight manual
- [ ] Reread the flight manual
- [ ] Document the layout of the code
- [ ] Read the tests
- [ ] Write a test
- [ ] Make a small change
- [ ] See if it works
- [ ] Pick up an issue/bug
- [ ] Test a potential fix
- [ ] See if it works
- [ ] Propose merge/PR

# Flight manual
https://flight-manual.atom.io/

## 1. Getting Started

### Why Atom?
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

### Installing Atom
- usually simple to download and install locally from the Atom website
- first time it runs Atom will try to install `atom` and `apm` commands for terminal
- for portable/removable storage install and create `.atom` directory alongside the app
	- stores configuration and state so must be set to writeable
	- an existing directory can be moved
	- can also store Electron data inside it with a `electronUserData` subdirectory
	- _or_ set `ATOM_HOME` environment variable to point to a directory
- Hacking on Atom core (we'll get there) shares how to clone and build source code
- SSL and proxy issues

### Atom Basics
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

## 2. Using Atom

### Atom Packages
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

### Moving in Atom
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

### Atom Selections
- supports many actions: scoping delete/indent/search, marking text to  quote/bracket, ...
- selection keybindings add Shift
	- select up: Shift + up / Ctrl + Shift + P
	- and so on for word start/end, line start/end, ...
	- includes Cmd + Shift + up/down for selecting to top/bottom of file
- specific selections for whole content areas
	- select entire file contents: Cmd + A
	- select entire line contents: Cmd + L
	- select current word: Ctrl + Shift + W

### Editing and Deleting Text
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

### Find and Replace
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

### Snippets
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
	- snippet to create snippets: `snip` then press tab
	- multiple snippets
		- just include snippet name, prefix, body under the scope
		- CSON keys are not repeatable (last overrides just like JSON)
```
'.source.js':
	'My Snippet':
		'prefix': 'hi'
		'body': 'Hello World!'

	'My Other Snippet':
			'prefix': 'bye'
			'body': 'Goodbye World!'
```

### Autocomplete
- autocomplete-plus package
- use tab or enter
- defaults to matching strings in current open file
- Settings for the package can toggle looking through all open buffers

### Folding
- hover mouse over gutter to hide code blocks
- hide keybinding: Alt + Cmd + [
- unhide keybinding: Alt + Cmd + ]
- fold everything: Alt + Cmd + Shift + [
- unfold everything: Alt + Cmd + Shift + ]
- fold at an indentation depth: Cmd + K Cmd + 0-9
- fold arbitrary selection: Alt + Cmd + C
	- "Fold Selection" also available through Command Palette

### Panes
- split any pane horizontally: Cmd + K left/right
- split any pane vertically: Cmd + K up/down
- switch between panes: Cmd + K Cmd + up/down/left/right
- _pane items_ are represented by tabs
	- drag files between panes
	- you can disable the tabs package
	- close all pane items: Cmd + W
	- core settings "Remove Empty Panes" to auto-close when all items closed

### Pending Pane Items
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

### Grammar
- grammar-selector package
- grammar as the language Atom associates with file
	- example: "Java"
	- this is what was used above to create scopes for snippets
- Atom automatically guesses grammar
	- mostly from file extension
	- sometimes from checking a little of the content
- defaults to "Plain Text"
- change selected grammar: Ctrl + Shift + L

### Version Control in Atom
- basic Git and GitHub integration when project contains Git repo
- checkout the `HEAD` of the current file: Alt + Cmd + Z
	- equivalent to running the following on the file path:
		- `git checkout HEAD -- <path>` to update the working tree
		- and `git reset HEAD -- <path>` to update the head index back
	- the action is on the undo stack: Cmd + Z
- Git status list
	- fuzzy-finder-package provides some tools
	- open files in the project: Cmd + T
	- jump to open editor: Cmd + B
	- display all untracked and modified project files: Cmd + Shift + B
		- (same as files seen by running `git status`)
		- right-side icon indicates if it's untracked or modified
- Commit editor
	- language-git package
		- syntax highlighting for edited commit, merge, rebase messages
		- colorizes first lines of commit messages longer than 50 or 65 chars
	- set Atom as commit editor: `git config --global core.editor "atom --wait"`
- Status bar icons
	- status-bar package
	- Git decorations on right side of status bar
	- shows current branch name
	- shows number of commits branch is ahead of upstream
	- displays info when file falls outside of commit data
		- icon if file is untracked, modified or ignored
		- number of lines added or removed since last commit
- Line diffs
	- git-diff package
	- colorized gutter lines
	-	added lines are green
	- modified lines are yellow
	- deleted lines are red
	- move cursor to previous/next diff: Alt+G up/down
- Open on GitHub
	- open file: Alt + G O
	- open file Blame view: Alt + G B
	- open file History view: Alt + G H
	- copy file uri to clipboard: Alt + G C
	- branch compare: Alt + G R
		- shows commits to current branch that are not also on mainline branch

### GitHub Package
- github package
- Git and GitHub integration in Atom
- open Git panel: Ctrl + 9
- open GitHub panel: Ctrl + 8
- toggle Git panel from Status Bar: click changed files icon
- initialize repo from the panel
- clone repo by running the `GitHub: Clone` command
- branch tooltip: click branch icon in Status Bar
	- create or switch branches
- stage changes (select file and viewing in Unstaged Changes view)
	- "Stage All" button in "Unstaged Changes" bar
	- double click or select one file and press Enter
	- "Stage Hunk" button or select hunk and press Enter
	- "Stage Selection" button after select one or multiple lines
	- use slash to toggle hunk vs line mode, then Enter to stage one line
	- switch between list view and diff view: left/right
		- left/right also handles unstaging
- discard changes (select file and view in Staged Changes view)
	- it's like staging but it's behind a context menu
	- all: right click "Unstaged Changes" then click "Discard All Changes"
	- files: right click one or more files then "Discard Changes"
	- hunk: click trash icon in top bar of hunk
- commit message: enter, expand, finalize at bottom right
- amend: toggle to change previous commit
- push: up arrow at bottom Status Bar
	- Atom offers to create branch if local doesn't exist in remote repo
- pull: fetch button (down) in bottom Status Bar
- resolve conflicts
	- Merge Conflicts list shows up between staged and unstaged changes
	- pick a version to resolve or make more edits
	- stage and commit
- view PRs
	- status of PRs show up in the GitHub panel
	- to show timeline click "Conversation"

### Writing in Atom
- Asciidoc or Markdown to write prose
- automatic spellcheck for markup text grammars
	- spell-check package
	- dashed red line underlines misspells
	- bring up suggested corrections: Cmd + Shift + ;
		- also available as "Correct Spelling" from Command Palette
		- also available from right-click context menu
- add more grammars to the spell checker
	- Settings view, Spell Check package settings
	- defaults: `text.plain`, `source.gfm`, `text.git-commit`
	- example to add: `source.asciidoc`
- markdown previews
	- toggle preview mode: Ctrl + Shift + M
	- copy rendered HTML: "Markdown Preview Copy HTML" from Command Palette
- snippets
	- try executing markdown snippets like `img` or `table`
	- "only a handful" for md, like `b`, `i`, `code`
	- find all by choosing "Snippets: Available" in Command Palette

### Basic Customization
- all but the stylesheet and Init Script are in CSON
	- objects marked with indentation
	- key values can be strings, numbers, objects, booleans, `null`, or array
	- duplicate keys will be overwritten by last occurrence
		- repeating scopes in attempt to add more keys will actually override
		- this was seen with snippets above
- styles in `styles.less` accessible through Atom > Stylesheet
	- use Chromium Developer Tools to inspect the DOM: Alt + Cmd + I
	- learn more about Less as a preprocessor at [http://lesscss.org/]
- keybindings
	- define meaning of a keypress in different contexts
	- map key to which command it will trigger
	- custom `keymap.cson` loads with Atom (see Atom > Keymap)
	- see all configured keybindings in Keybindings tab in Settings View
	- open the Keybinding Resolver: Cmd + .
		- watch Atom catch input and see which command it executes
- global config in `config.cson`
	- open in Atom > Config
	- key for all settings `*` vs language specific keys like `.python.source`
	- core settings grouped under core namespaces `core` or `editor`
	- other settings grouped by package name beneath those
	- description for each of the keys in the config:  https://flight-manual.atom.io/using-atom/sections/basic-customization/#configuration-key-reference
- language-specific config
	- scoped to editor's language
	- example: different tab widths for different languages
	- select packages for a specific language in Settings View
	- _or_ open `config.cson` with "open config" in the Command Palette
		- override `*` settings in a scope like `.source.ruby`
		- then within that object set some key:value under the `editor:` key
	- language's scope name
		- listed to the right of "Scope:" ina  grammar package in Settings View
		- _or_ show all scopes at current cursor position: Alt + Cmd + P
		- these also act as class names for your stylesheet!
 	- language recognition
		- add an extension to `core` in `config.cson`
```CSON
'*':
	core:
		customFileTypes:
			'source.ruby': [
				'rooobie'
			]
```
- change customization storage
	- defaults to home directory of user running Atom: `~/.atom`
	- if `ATOM_HOME` environment variable exists it will be used
	- Portable Mode when `.atom` directory is a sibling to the application
		- allows for running from USB or cloud storage
		- Atom uses that storage directory for machines where it's synced
		- optional command line parameter: `atom --portable`
			- this simply moves your current settings to a sibling directory

## 3. Hacking Atom

### Tools of the Trade
- Atom is implemented using web tech like JavaScript and CSS
	- specifically CoffeeScript for JS
	- specifically Less for CSS

### The Init File
- `init.coffee` runs from your `~./atom` directory
	- access it through Atom > Init Script
	- call Atom API
	- allows for startup customizations
	- if customizations grow consider making a package
- the file can also be named `init.js` and run JS code instead
- syntax is like below (for adding to text editor)
	- or see [one example]( https://flight-manual.atom.io/hacking-atom/sections/the-init-file/#the-init-file)
```
atom.commands.add 'atom-text-editor', 'command:name', ->
	return unless editor = atom.workspace.getActiveTextEditor()

	// actions
```
- newly defined commands are in the Command Palette and can be keybound

### Package: Word Count
- example of creating "a very simple package"
	- display small modal window with number of words in current buffer
- Package Generator
	- package-generator package
	- run "Generate Package" from Command Palette
	- on create Atom will make a named directory under `.atom/packages`
		- if a package on atom.io has the same name your package may not load
	- atom creates a bunch of files
```
my-package/
├─ grammars/
├─ keymaps/
├─ lib/
├─ menus/
├─ spec/
├─ snippets/
├─ styles/
├─ index.js
└─ package.json
```
- `package.json` contains metadata
	- Node info like dependencies
	- extra info like path to `main` JS file, styles, keymaps, snippets
	- `activationCommands` are the commands triggering the package
		- loading is delayed until one of those is triggered
		- if left out `activate()` will be called once package loads
	- `activationHooks` delay loading until one of array of hooks triggers
	- make sure to update the repo URL
	- fill out name, description and license
- source code
	- write one top-level module (singleton managing extension lifestyle)
	- export that module from the `main` file
		- if `main` is missing from `package.json` it will try `index.js`/`index.coffee`
	- all other code lives in `lib`
	- all `lib` code should be required from the top-level module
	- required method:
		- `initialize(state)`: called before workspace finished setting up
		- initialize sets up before deserializers and view providers
	- optional methods:
		- `activate(state)`: gets state from last time window was serialized
			- assuming module implements serialize()
			- do work here after package starts
			- use it to e.g. set up DOM events
		- `serialize()`: returns state when window shuts down
			- this data gets passed to `activate()`
		- `deactivate()`: use it to release external resources when window shuts down
			- if subscribing to window don't worry since it's "getting torn down anyway"
- style sheets
	- Less sheets live in `styles` directory
	- these load and apply to DOM
	- loading order
		- optionally add a `styleSheets` array to your `package.json`
	- keep it to structural elements and avoid colors and sizing
		- if you do need them take them from active theme's `ui-variables.less`
- keymaps
	- set up a keymap in `keymaps/`
	- key-command pairs apply to the selected element
		- `atom-workspace` is parent of everything in Atom UI
- menus
	- for context menu on right click or application menu
	- load alphabetically by default but `package.json` can have a `menus` array
	- application menu
		- create an application menu "for common actions ... that aren't tied to a specific element"
		- will show up for your package under the Packages menu
	- context menu
		- for commands tied to specific UI
		- determines selected element then adds items matching selector
		- add submenus with `"submenu"` key instead of command
		- add separators with a simple item `{"type": "separator"}`
- developing
	- breakdown of the generated code: https://flight-manual.atom.io/hacking-atom/sections/package-word-count/#understanding-the-generated-code
		- split between View class and the main entry point file
		- review lifecycle methods (only `activate` is required)
	- flow from perspective of Atom:
		1. start up
		2. start loading packages
		3. read `package.json`
		4. load keymaps, styles, menus and main
		5. finish loading packages
		6. detect user executing package command
		7. execute `activate` in main
		8. execute package command as registered
		9. detect user executing package command again
		10. execute package command as registered
		...
		11. shut down, triggering serializations
	- that flow differs if you do not use `activationCommands`
	- example of making changes to the code and calling active text editor methods: https://flight-manual.atom.io/hacking-atom/sections/package-word-count/#counting-the-words
- debugging
	- familiar looking Chromium console
	- open developer console: Alt + Cmd + I
	- _or_ select View > Developer > Toggle Developer Tools
- tests
	- please do test
	- place tests under `spec/`
	- tests are run by Jasmine
	- running tests
		- press Alt + Cmd + Ctrl + P
		- _or_ select View > Developer > Run Package Specs
		- _or_ from command line: `atom --test spec`
	- created packages have an example suite

### Package: Modifying Text
- command tool to replace text with ascii art
1. generate package from Command Palette and enter package name
2. remove view-related code as this package has no UI
  - one view file in `lib/`
  - one view test file in `spec/`
  - the `styles/` directory
  - all view and modal code lines from the main logic file
3. add a command
	- store a reference to the active text editor
	- test inserting a string
```
convert() {
  const editor = atom.workspace.getActiveTextEditor()
  if (editor) {
    editor.insertText('Hello World!')
  }
}
```
4. reload the latest version of your package
	- "Window: Reload" from Command Palette or Alt + Cmd + Ctrl + L
5. trigger the command
	- add your command to the `activationCommands` in `package.json`
6. add a keybinding
	- open bindings in `keymaps/` and change the keys and the function run
	- note that Atom keymaps are case sensitive
	- strong recommendation: use lowercase and explicitly mark Shift
7. add the dependency for Node `figlet` module
	- this will do the ASCII art conversion
	- add the dependency line to `package.json`: `"figlet": "1.0.8"`
	- run "Update Package Dependencies: Update" from the Command Palette
8. require and use `figlet`
	- note that editor has `.getSelectedText()` and `.insertText()` for you to use
	- pass selected text from editor to figlet
	- declare a font for figlet
	- the figlet function: `figlet(text, { font }, (error, art) => {})`
- why might you want UI-free packages?
	- creating linters
	- creating code checkers

## 4. Behind Atom

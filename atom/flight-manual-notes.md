# Flight manual
https://flight-manual.atom.io/

Skimming QBASIC documentation, revisiting my 8 year old self programming simple adventure games. I wish I could see it then through my eyes now. Oh, that's why I was typing that!

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

### Package: Active Editor Info
- extend UI by adding items to workspace
	- different from Word Count above which showed info in modal
	- drag these to docks on window edges and restore on next project open
- example of creating workspace item displaying info about active editor
1. generate package called `active-editor-info` from Command Palette
2. edit files to show view in workspace
	- register an Atom "opener" with `addOpener` inside `activate()`
		- openers take a URI and return a view
	- add the command to run the view toggle
	- add disposable to destroy views on deactivate package
```
	...
	activate(state) {
		this.subscriptions = new CompositeDisposable(
			atom.workspace.addOpener(uri => {
				if (uri === 'atom://active-editor-info') {
					return new ActiveEditorInfoView();
				}
			}),
			atom.commands.add('atom-workspace', {
				'active-editor-info:toggle': () => this.toggle()
			}),
			new Disposable(() => {
				atom.workspace.getPaneItems().forEach(item => {
					if (item instanceof ActiveEditorInfoView) {
						item.destroy();
					}
				});
			})
		);
	},
	...
```
3. remove the `activeEditorInfoView` and `serialize()`
	- since you can have more than one view instance
	- each can manage its own state and serialization
	- so don't do package-level serialization
4. change `toggle()` to toggle view: `atom.workspace.toggle('atom://active-editor-info')`
5. update view file
	- it's mostly ready to go
	- but add a method to show tab text
```
	getTitle() {
		// Used by Atom for tab text
		return 'Active Editor Info';
	}
```
	- add another to set up that URI
```
	getURI() {
		// Used by Atom to identify the view when toggling.
		return 'atom://active-editor-info';
	}
```
	- consider storing the URI to avoid repeating the string three times
6. reload and run the package command from the Command Palette
	- notice it shows up right in the middle of the workspace
7. constrain item location
	- add view class methods to shape where it opens
	- the default defaults to `'center'`
	- change it to appear on the right by default
	- allow user to override that with one of three other dock positions
```
	getDefaultLocation() {
		return 'right';
	}
	getAllowedLocations() {
		return ['left', 'right', 'bottom'];
	}
```
8. actually displaying the info
	- modify the constructor
	- notice we're just naïvely inserting `innerHTML` using a template string
	- _caution_: sanitize input and go through DOM API in a real production package
```
	this.subscriptions = atom.workspace.getCenter().observeActivePaneItem(item => {
	  if (!atom.workspace.isTextEditor(item)) {
	    message.innerText = 'Open a file to see important information about it.';
	    return;
	  }
	  message.innerHTML = `
	    <h2>${item.getFileName() || 'untitled'}</h2>
	    <ul>
	      <li><b>Soft Wrap:</b> ${item.softWrapped}</li>
	      <li><b>Tab Length:</b> ${item.getTabLength()}</li>
	      <li><b>Encoding:</b> ${item.getEncoding()}</li>
	      <li><b>Line Count:</b> ${item.getLineCount()}</li>
	    </ul>
	  `;
	});
```
9. clean up the subscription in `destroy()`
```
	destroy() {
	  this.element.remove();
	  this.subscriptions.dispose();
	}
```
10. serialize it
	- notice if we reload Atom the item is gone
	- add `serialize()` to the view class
	- since preserved state lives in editor, only need `deserializer` field
		- if item relied on other state would add it to the returned object
```
	serialize() {
		return {
			// This is used to look up the deserializer function. It can be any string, but it needs to be
			// unique across all packages!
			deserializer: 'active-editor-info/ActiveEditorInfoView'
		};
	}
```
11. add a `deserializers` object to `package.json`
	- add a key that matches the string used in `serialize()`
	- add its value as `"deserializeActiveEditorInfoView"`, a method we'll write
```
	{
	  "name": "active-editor-info",
	  ...
	  "deserializers": {
	    "active-editor-info/ActiveEditorInfoView": "deserializeActiveEditorInfoView"
	  }
	}
```
12. write a function to deserialize the view
	- go back to the main logic file
	- add a deserialize function
	- the value returned from `serialize()` will get passed to this function
	- only return a new view instance since serialized object is state free
```
	deserializeActiveEditorInfoView(serialized) {
		return new ActiveEditorInfoView();
	}
```
13. reload and try toggling the view from the Command Palette
- consider what else you could make
	- useful "visual tools for working with code"

### Creating a Theme
- rendered in HTML, styled with Less
- UI themes: tree view, tabs, drop-down lists, status bar
- syntax themes: code, gutter, other editor elements
- change themes in Settings View or Atom > Preferences
- understand [Less](https://speakerdeck.com/danmatthews/less-css), `package.json` and atom.io themes registry
- create a syntax theme
	1. Generate Syntax Theme from the Command Palette
		- give it a name like `motif-syntax`
		- end syntax theme names with `-syntax` and UI ones with `-ui`
	2. check that theme shows in Syntax Theme dropdown under Settings View > Themes
	3. open `styles/colors.less` and change defined color variables
	4. open `styles/base.less` and change selectors
		- for example, make `.gutter` have a red `background-color`
		- caution: declaring a `font-family` in the theme will override Atom settings
	5. reload atom to see changes: Alt + Cmd + Ctrl + L
		- in the future run `atom --dev .` or View > Developer > Open in Dev Mode
- create a UI theme
	1. fork the [template](https://github.com/atom-community/ui-theme-template)
	2. clone the forked repo to local
	3. run Atom in dev mode from the local theme directory
		- command: `atom --dev .`
		- dev mode windows have a little icon in the bottom left
	4. change theme name in `package.json`
		- again, be sure the name ends in `-ui`
	5. run `apm link --dev`
		- this symlinks repo to `~/.atom/dev/packages`
		- now you can always load atom normally to force Atom back to default theme
	6. reload atom
	7. enable the theme in Settings View
	8. work on the UI theme
- theme variables
	- required: provide a `ui-variables.less` or `syntax-variables.less`
		- there is a default fallback for [UI variables](https://github.com/atom/atom/blob/master/static/variables/ui-variables.less)
		- and a default fallback for [syntax variables](https://github.com/atom/atom/blob/master/static/variables/syntax-variables.less)
	- adhere to the [style guide](https://github.com/atom/styleguide)
	- stick to "structural styling"
	- avoid defining colors, padding, absolute pixels (use the theme variables)
- workflow
	- use dev mode (discussed above) for live reload
	- reload all styles at once: Alt + Cmd + Ctrl + L
	- developer tools: Alt + Cmd + I or Toggle Developer Tools
	- open the styleguide: Cmd + Ctrl + Shift + G (or Command Palette "styleguide")
		- styleguide renders every component supported by Atom
		- don't be afraid to open it - very visual and easy to look through
	- side-by-side view
		- open two windows instead of needing to fix everything through reloading
		- example problem: accidentally changing text to same color as background
	- publish your package
		- follow [the steps](https://flight-manual.atom.io/hacking-atom/sections/publishing/) to deploy

### Creating a Grammar
- grammars written in CSON or JSON
- scopes assigned to regex patterns
- scopes turned into targetable CSS classes
- using Oniguruma engine
1. open Command Palette and "Generate Package"
2. name it something, like `language-flight-manual`
	- grammar packages begin with `language-`
3. delete unnecessary folders: `keymaps`, `lib`, `menus`, `styles`
4. remove `activationCommands` from `package.json`
5. create folder `grammars`
6. inside `grammars/` create file like `flight-manual.cson`
7. populate the main grammar with [boilerplate](https://gist.github.com/DamnedScholar/622926bcd222eb1ddc483d12103fd315)
	- `scopeName`: language highlighted by this grammar
	- `name`: display name shown in status bar and grammar selector
	- `fileTypes`: array of file types highlighted by this grammar
	- `patterns`: regex patterns used for tokenizing these files
8. add a basic pattern to tokenize words
	- pattern to tokenize words like "Flight Manual": `'\\bFlight Manual\\b'`
	- give a scope name to this like: `'entity.other.flight-manual'`
	- double escape `\\` since CSON processes the string before Oniguruma!
9. add different scopes with capture groups
	- change the match to include capture groups: `'\\b(Flight) (Manual)\\b'`
	- add a `captures` key after `name` to assign scopes to referenced capture groups
	- now main name is overarching scope but capture names will reference each group
```
'captures':
  '1':
		'name': 'keyword.other.flight.flight-manual'
	'2':
		'name': 'keyword.other.manual.flight-manual'
```
10. add begin/end patterns
	- multiline patterns
	- begin tokenizing from `begin` key and don't stop until `end` key
	- provide punctuation scopes from the start (harder to support later)
	- so try adding (support for note blocks)[https://flight-manual.atom.io/hacking-atom/sections/creating-a-grammar/#beginend-patterns]
	- begin/end patterns only match their own subpatterns (as if its own subgrammar)
11. include inherited markdown patterns: `'include': 'source.gfm'`
	- note that the begin/end blocks still don't highlight
	- instead of duplicating, use the `'include': '$self'` scope in begin/end patterns

### Publishing
- use `apm` to publish and update public registry packages
- prepare a package:
	1. correctly fill out `package.json` fields: `name`, `description`, `repository`
	2. set the `package.json` version to `0.0.0`
	3. check that `package.json` has `engines` entry for Atom
	4. make sure there's a `README.md` in root
	5. package should be a git repo pushed to GitHub
- publish a package:
	1. double check there isn't a name conflict with an existing atom.io package
	2. run `apm publish minor` (version can be `major`, `minor`, `patch`)
		- registers package name on atom.io (first time only)
		- updates `version` in `package.json` and commits
		- creates new [git tag](https://git-scm.com/book/en/v2/Git-Basics-Tagging) for your version
		- pushes tag and current branch to GitHub
		- updates atom.io with your published version
	3. enter GitHub credentials
	4. head over to atom.io packages to see yours among all in registry
- update a package: `apm publish version-type` (to update major.minor.patch number)
	- `version-type` is `major` when you break backwards compatability
	- `version-type` is `minor` when adding new functionality
	- `version-type` is `patch` when changing implementation but not behavior/options

### Iconography
- bundled with Octicons
- see Icons in Styleguide
- add them to styles with a span class `icon icon-whatever-name`
- recommended font size: `16px`
	- use multiples of 16 for clear results
- add text for clarity
	- a text label
	- a [tooltip](https://atom.io/docs/api/v1.27.0/TooltipManager)
	- an attribute `title="label"`

### Debugging
- how to debug Atom and what info to provide in issues
- update version
	- check version: `atom --version`
	- look for [latest stable release](https://github.com/atom/atom/releases/tag/v1.27.0)
	- update in Atom > About and click "Restart and Install Update" if applicable
	- you can also rebuild Atom from source
- safe mode
	- run `atom --safe`
	- does not load packages
	- does not load your init script
	- if this fixes an issue, it's probably in packages or init
	- if this does not help:
		- start Atom in normal mode
		- open Settings View
		- disable packages one by one
		- restart Atom after disabling each package
	- disable or uninstall the problem package
	- contact the package maintainer
- clear saved state across all projects (including unsaved buffers)
	- run `atom --clear-window-state`
	- may also clear all config: `mv ~/.atom ~/.atom-backup` then restart Atom
- linked packages
	- find packages linked to Atom packages or Atom dev packages: `apm links`
	- remove links: `apm unlink` (including `apm unlink all`)
- package settings
	- open Settings View
	- check through settings in Basic Customization
	- check through per-language settings in languages under Packages tab
	- check through the settings for each package
- configuration
	- are these causing unexpected behavior for you?
	- custom styles
	- custom keymaps
	- custom snippets
- keybindings
	- use Keybinding Resolver if the expected commands aren't executing
	- the resolver shows:
		- command
		- CSS selector defining valid context
		- file where keybinding is defined
	- gray keybindings are matched but not executed
	- green keybindings are matched and executed
	- multiple for the same are chosen by selector specificity and load order
		- core loaded first
		- then package keybindings
		- then user-defined ones
	- what causes commands to be listed but not triggered?
		- key combination not used in context defined by keybinding selector
		- _or_ another keybinding had precedence (often conflicting packages)
	- open a package issue if any package's keybindings take precedence over core
- font rendering
	- use Developer Tools > Elements tab to see which fonts render which text
- errors
	- errors log to red notification (for me appearing on right side)
	- check other errors in Console tab of Developer Tools
	- developer tools automatically show in Dev mode
- crash logs
	- check automatic logs in `Console.app`
		- select User Reports
		- find entries starting with `Atom` and ending in `.crash`
		- save dump
- startup performance
	- Timecop gives insights on loading times and behaviors
	- look for any packages that have high loading or activation times
- runtime performance
	- issues are more useful when including Chrome CPU profiler saved profile
		- open Developer Tools
		- collect the JS CPU profile
		- select Start
		- capture a recording of the action
		- select Stop
	- now you can check out the chart view
	- you can save and post the profile data
- profile startup performance
	- create a CPU profile for checking the window load time
	- start atom with `atom --profile-startup .`
	- select Profiles in Developer Tools
	- select "startup" profile
	- then save the startup profile
	- include it in your issue
- build tools
	- is `apm install` causing issues?
	- maybe package depends on native code
	- check that apm can build native code on your machine: `apm install --check`
	- you will need C++ compiler and Python
- GPU issues
	- rendering issues like flickering?
	- run without GPU: `atom --disable-gpu`
	- this may increase draw times and use more CPU energy
	- allow commands to determine how layer tiles (graphics) are drawn: `--enable-gpu-rasterization`
	- make Skia GPU backend draw layer tiles only: `--force-gpu-rasterization`
	- use these Chromium flags after other flags
		- other flags like `--safe` will not be executed after Chromium ones above

### Writing Specs
- Atom tests with Jasmine
- core and packages store tests in `spec/` directory
- files have to end in `-spec`
1. add `describe` method or methods
2. add `it` method or methods
3. add `expect` method or methods (Jasmine [expectations](https://jasmine.github.io/1.3/introduction.html#section-Expectations))
4. consider other matchers besides Jasmine built-ins
	- [jasmine-jquery](https://github.com/velesin/jasmine-jquery)
	- `toBeInstanceOf`: for `instanceof`
	- `toHaveLength`: for `.length`
	- `toExistOnDisk`: file exists on filesystem
	- `toHaveFocus`: if element has focus
	- `toShow`: if element is visible in DOM
- async specs with promises
	- promises and the `waitsForPromise` function
	- use promises in the `describe`, `it`, `beforeEach`, `afterEach`
	- multiple promises: use one new `waitsForPromise` per promise (not nested)
	- optional promise arguments: `shouldReject` (default: `false`), `timeout`, `label` (if times out)
- async specs with wait
	- use `waitsFor` and `runs`
	- more info [from Jasmine](https://jasmine.github.io/1.3/introduction.html#section-Asynchronous_Support)
- run specs
	- use the command `window:run-package-specs` (also for Atom core)
	- that will run all tests in `spec/`
	- CI environments are easily supported
	- command line: `atom --test`
		- optionally add a timeout like `--timeout 60`
		- followed by directories to test
- customizing test runner
	- Atom runs outdated Jasmine 1.3 for compatability
	- add an `atomTestRunner` field to `package.json`
	- this test module must export one function for Atom to run
	- Atom will call the function with [a bunch of parameters](https://flight-manual.atom.io/hacking-atom/sections/writing-specs/#customizing-your-test-runner)
	- the function should return a promise with a resolve code

### Handling URIs
- a package can register itself "to handle special URIs triggered from the system"
- example: `my-package` can handle any URI beginning in `atom://my-package/`
- add a `uriHandler` to `package.json`
	- give it a `method` key to call
	- optionally give it a `deferActivation` key (set to `false` to avoid Atom deferring package activation)
- write the handler method
	- Atom passes parsed URI with query string (arg 1) and a raw URI (arg 2)
	- query strings are still strings and must be converted to native types
- activation deferral
	- by default adding a `uriHandler` means Atom will not activate package until it has a handleable URI
	- once it has the URI Atom activates package and calls the handler
	- disabling avoids this but increases Atom startup time
- no Linux support for URI handlers

### Cross-Platform Compatability
- Electron and Node handle most details across platforms
- symlinks: on Windows specify type `'junction'`
- filenames
	- Windows has [reserved filenames and characters](https://flight-manual.atom.io/hacking-atom/sections/cross-platform-compatibility/#filenames)
	- Mac and Linux require `\` before spaces
- filepaths
	- Windows `\` and only up to 250 characters (avoid deep nesting)
	- Mac and Linux `/`
	- `join` and `normalize` Node functions handle this
- do not parse paths as URLs
	- characters like `?` and `#` are misread
	- Windows drives parse as protocols
	- when URLs are required use `file:` protocol
- `fs.stat`: this returns allocation size of directory, not contents
- `path.relative`: useful on Mac or Linux but Windows allows "multiple absolute roots" (`C:`, `D:`, ...)
- [RimRAF](https://www.npmjs.com/package/rimraf) has retry logic for operating over many files
- line endings
	- `CRLF` on Windows
	- `LF` on Mac, Linux
	- `autocrlf` on Git on Windows (converts between `CRLF`/`LF`)\
	- these "interfere with file lengths, hash codes and direct text comparisons"
	- these also increase Atom selection length by 1 char/line
	- force ending type by [specifying gitattributes](https://flight-manual.atom.io/hacking-atom/sections/cross-platform-compatibility/#line-endings)

### Converting from TextMate
- themes or grammars
- converting a bundle
	- editor preferences, snippets, colorization
	- example converting R: `apm init --package language-r --convert https://github.com/textmate/r.tmbundle`
	- in that example you can go into the `language-r` directory
	- then link your package with `apm link`
	- now try opening `.r` files in Atom
- syntax themes
	- `plist` files vs Atom's Less
	- download the TextMate theme
	- convert the theme: `apm init --theme my-theme --convert ~/Downloads/MyTheme.tmTheme`
	- open Settings View and enable the theme

### Hacking on Atom Core
- for Atom bugs or adding features to core
- run in dev mode with local copy of source
1. fork `atom/atom`
2. clone it to local
3. run the bootstrap script from local directory: `script/bootstrap`
4. run in dev mode
	- set `ATOM_DEV_RESOURCE_PATH` if _not_ cloned to `~/github/atom`
	- use `--dev` parameter when running Atom
	- may be `atom-dev` or `atom-beta` instead depending on source code
	- benefits
		- restart Atom after changes instead of needing to run `script/build`
		- dev packages load instead of regular packages with same name
5. run core tests
 	- make sure the `ATOM_DEV_RESOURCE_PATH` is right
	- go to local and run `atom --test spec`
6. build from source
	- have macOS, Node, npm, Python, and Xcode command line tools
	- see specific [requirements](https://flight-manual.atom.io/hacking-atom/sections/hacking-on-atom-core/#requirements)
	- install using `script/build --install`
	- browse build [options][https://flight-manual.atom.io/hacking-atom/sections/hacking-on-atom-core/#scriptbuild-options] and [errors](https://github.com/atom/atom/search?q=label%3Abuild-error+label%3Amac&type=Issues)

### Contributing to Official Atom Packages
- open an issue
- create a clone
	- for some packages it may be necessary to build Atom core
	- fork then clone
	- go into the directory then run `apm install`
- link your fork instead of the built-in package: `apm link -d`
- now you can run `atom --dev`
- running in dev mode
	- note that you're using Atom to modify Atom
	- only load packages you're building in development mode
	- only develop in stable mode and test packages in dev mode
	- command for "Application: Open Dev" or `atom --dev`
	- create a symlink to your package (automatically done with `apm develop`)
	- `apm link --dev` and `apm unlink --dev` also create and remove dev mode symlinks
- keep dependencies updated: `apm update` after pulling upstream changes

## 4. Behind Atom

### Configuration API
- read global `atom.config` settings
	- useful for packages that need to be configurable
- `atom.config.get` reads value of namespaced key: `@showInvisibles() if atom.config.get "editor.showInvisibles"`
- `atom.config.observe` subscribes to track any view object
	- calls a callback now and then each value change
```
{View} = require 'space-pen'

class MyView extends View
  attached: ->
    @fontSizeObserveSubscription =
      atom.config.observe 'editor.fontSize', (newValue, {previous}) =>
        @adjustFontSize(newValue)

  detached: ->
    @fontSizeObserveSubscription.dispose()
```
- `atom.config.onDidChange` calls callback on each value change (but not now)
- subscriptions return `Disposable` objects for unsubscribing
	- the example shows unsubscribing on view detach
	- group multiple into `CompositeDisposable` and dispose that on view detach
- writing config
	- automatically on startup from the `config.cson`
	- `atom.config.set` to update a config key: `atom.config.set("core.showInvisibles", true)`
- set up a [schema](https://atom.io/docs/api/v1.27.1/Config) to "expos[e] package configuration via specific key paths"

### Keymaps In-Depth
- keymap file
	- CSON/JSON nested hashes
	- apply keystrokes and commands to elements matching a selector
	- can add attribute to selectors: `'atom-text-editor:not([mini])'`
- key combinations
	- modifier keys plus key
	- separated by dash
- commands
	- custom DOM events triggered when presses match bindings
	- UI code listens for named commands, not key presses
	- `atom.commands` is global `CommandRegistry` (these are what show in Command Palette)
	- Command Palette shows commands in its "focus context"
- multiple commands for one keybinding: no support, so just create a new command that does both
- order
	- most specific selector is chosen first
	- otherwise JSON is unordered so just split into multiple files if ordering matters
- grammars
	- limit binding to a grammar with an attribute: `"atom-text-editor[data-grammar='source example']"`
	- only entire grammar, not scopes within it, not subelements of text editor
- unsetting and aborting bindings
	- use `unset!` instead of command to avoid triggering binding
	- unset bindings don't cancel same keybindings on parent selector though
	- use `abort!` instead of command to stop searching for binding
- native Chromium keybindings
	- use `native!` to force native browser keymapping
	- apply selector `.native-key-bindings` to set all keystrokes on an element
	- useful for backspace and arrow keys in components and input elements
- aborting commands and overloading bindings
	- when defining a command use `.abortKeyBinding()` to step out of overloaded keybinding
	- examples include tabbing in snippets, since it should expand if snippet exists otherwise just tab
- consider the [steps Atom takes](https://flight-manual.atom.io/behind-atom/sections/keymaps-in-depth/#step-by-step-how-keydown-events-are-mapped-to-commands) to map a keypress to a command
- getting Atom to recognize keypresses
	- Chromium [is limited](https://blog.atom.io/2016/10/17/the-wonderful-world-of-keyboards.html) in how it reports keypresses
	- there are [ways to get around this](https://flight-manual.atom.io/behind-atom/sections/keymaps-in-depth/#overriding-atoms-keyboard-layout-recognition)

### Scoped Settings, Scopes and Scope Descriptors
- settings specific to a grammar
- a kind of "scoped settings" for "targeting ... a specific syntax token type"
	- example scopes for a JavaScript function: `function` and `name`
- treat scope names like CSS classes
	- example scopes on a JS function's name: `<span class="entity name function js">foo</span>`
- scope selectors
	- target tokens
	- example selecting all JS: `source.js`
	- example selecting all JS function names: `source.js .function.name`
	- example selecting all function names: `.function.name`
- `atom.config.set()` scope selector setting
	- accepts a `scopeSelector` param: `atom.config.set('my-package.my-setting', 'special value', scopeSelector: '.source.js .function.name')`
- scope descriptors
	- array of scopes for a path from root
	- can obtain arrays from editor
		- [root scope descriptor](https://atom.io/docs/api/v1.27.1/TextEditor#instance-getRootScopeDescriptor)
		- [buffer position descriptor](https://atom.io/docs/api/v1.27.1/TextEditor#instance-scopeDescriptorForBufferPosition)
		- [cursor position descriptor](https://atom.io/docs/api/v1.27.1/Cursor#instance-getScopeDescriptor)
	- get the descriptor then `atom.config.get` the value for a setting at that scope

### Serialization in Atom
- useful for reloading/restoring from a previous session
	- window's JSON representation gets deserialized on shutdown
- package serialization hook (`serialize`)
	- optional `serialize` method in main package module
	- return JSON-serializable object: `@myObject.serialize()`
	- this gets passed to `activate` on reload
- serialization methods
	- implement `.serialize()` for the class of any object that should be able to serialize
	- serialize must have a `deserializer` key that can convert the data back into an object
		- it's usually just pointing back to this same class
```
class A
  constructor: (@data) ->
  serialize: -> { deserializer: 'A', data: @data }
```
- deserialization
	- now the package needs to be able to take that serialized object and load it
	- `package.json` should contain a `deserializers` key with a method for that object
	- each of that object's keys should be an object named in `deserializer` and value its deserialize method
	- that method (in main) gets passed the serialized data: `deserializeA: ({ data }) -> new A(data)`
	- alternatively, use `atom.deserializers.add` from within the class (not preferred)
- class-level version can be added to the object: `@version: 2`

### Developing Node Modules
- multiple packages are Node modules rather than Atom packages
	- example: `atom-keymap`
- these require separate way of linking to dev environment
1. clone the module to local
2. navigate to the local directory
3. install the module using `npm install`
4. lin the module using `npm link`
5. change to the directory where Atom is cloned
6. link the specific module to Atom using `npm link module-name`
7. rebuild with Atom Package Manager: `apm rebuild`
8. set environment variable if Atom is not in `~/github/atom`: `export ATOM_DEV_RESOURCE_PATH=Atom Cloned Path`
	- the path should match the directory in step 5
9. run Atom in dev mode: `atom --dev .`
- even after linking you still need to install and rebuild if changing the Node code
1. change to the directory of the Node module
2. `npm install`
3. change to the directory where Atom is cloned
4. `apm rebuild`
5. run `atom --dev .`

### Interacting with Other Packages via Services
- one Atom package can provide services to other packages
	- these are "versioned APIs"
	- register services in `packages.json` under a `providedServices` key
- provide a service by setting version keys and then methods to call
```
{
  "providedServices": {
    "my-service": {
      "description": "What the service does",
      "versions": {
        "1.2.3": "provideMyServiceV1",
        "2.3.4": "provideMyServiceV2"
      }
    }
  }
}
```
- those methods should exist in the main module
- each method "should return a value that implements the service's API"
```
module.exports =
  #...

  provideMyServiceV1: ->
    adaptToLegacyAPI(myService)

  provideMyServiceV2: ->
    myService
```
- consume services by setting up the same kind of syntax in `packages.json`
	- but under a `consumedServices` key
	- again list versions under a service name
	- use [version ranges](https://docs.npmjs.com/misc/semver#ranges) like `^1.2.3` or `>=2.3.4 <2.5`
- the consume methods
	- called whenever active package provides that service
	- are passed the service object returned from provider methods
	- usually require cleanup for when providing package gets deactivated
```
{Disposable} = require 'atom'

module.exports =
  #...

  consumeAnotherServiceV1: (service) ->
    useService(adaptServiceFromLegacyAPI(service))
    new Disposable -> stopUsingService(service)

  consumeAnotherServiceV2: (service) ->
    useService(service)
    new Disposable -> stopUsingService(service)
```

### Maintaining Your Packages
- actions besides publishing
- manually publishing
	- _not_ recommended for anyone but advanced users
	- performing wrong steps may require unpublishing
	- this takes over what `apm` normally does automatically
	- `apm` will still only publish to `atom.io`
	- `apm` will still only list packages hosted on `GitHub`
	1. update your `package.json` version number
	2. commit the version change
	3. make a git tag to ref that commit
		- must match regex `^v\d+\.\d+\.\d+`
		- the number must match the version number in `package.json`
	4. run `git push --follow-tags`
	5. run `apm publish --tag name` where name matches the git tag name
- add collaborators
	- invite them as a [personal collaborator](https://help.github.com/articles/inviting-collaborators-to-a-personal-repository/) on GitHub
	- you can also do this with an [organization](https://help.github.com/articles/collaborating-with-groups-in-organizations/)
		- collaborate with the team that published the package
- transfer ownership
	- [transfer the repo](https://help.github.com/articles/about-repository-transfers/)
	- the new owner can now publish that package
	- this is permanent!
- unpublish a package from atom.io registry: `apm unpublish package-name`
- unpublish a version: `apm unpublish package-name@1.1.1`
- rename a package: `apm publish --rename changed-name`
	- the original name cannot be reused
	- changes name in `package.json`, pushes a new commit tag, publishes renamed package

### How Atom Uses Chromium Snapshots
- Atom uses [V8 snapshots](https://v8project.blogspot.it/2015/09/custom-startup-snapshots.html) to preload
	- core packages
	- core services
- runtime Atom fills in the info not gained from snapshot during compilation
	- 3rd-party packages
	- custom stylesheets
	- settings
	- more
- [electron-link](https://github.com/atom/electron-link) traverses require graph
	- replaces "forbidden `require` calls" (not accessible through snapshot) with functions to call at runtime
	- outputs one script with code for all modules that can be reached from entry point
	- code is supplied to `mksnapshot` to make snapshot blob
	- blob gets copied to app bundle
	- running Atom then automatically gets blob loaded by Electron
- new Atom code should go inside snapshot
	- defer use of DOM APIs
	- defer native modules
	- anything that can be deferred to time when available
- when not possible, add unsupported paths to files excluded from snapshot
	- only exclude unsupported files
	- avoid skipping whole Node modules

## Glossary
- buffer
	- text content held in memory
	- not written to file until saved
- command
	- Atom functionality
	- triggered with keybindings
	- triggered through menu items
- dock
	- pane containers attaching to side of Atom window
	- can attach left, right, below
	- "Tree View" is one
- key combination
	- any number of keys pressed to do task
- key sequence
	- subset of key combination
	- when keys pressed and released sequentially (press... then...)
	- not when all keys pressed and released together
- keybinding
	- map a key combination to an Atom command
- keymap
	- collection of keybindings
	- _or_ file(s) of keybindings for Atom package/core
- package
	- a plugin for Atom
- pane
	- visual part of editor
	- holds one or more pane items
	- each Atom window contains one or more
- pane container
	- Atom UI section with one or more panes
- pane item
	- editor or other item shown in a pane
	- represented as tabs by default
	- not equivalent to "tabs" because tabs can be disabled
	- can display non-files such as Settings View
- panel
	- Atom UI that's outside of the editor
	- for example, there's a Find and Replace panel

## Removing Shadow DOM Styles
- removed from text editors in Atom 1.13
- UI themes/packages
	- remove pseudoselector `::shadow`
	- remove combinator selector `/deep/`
- syntax themes
	- remove pseudoselector `:host`
	- prefix grammar selectors to avoid "accidental style pollution": `syntax--`
	- non-language selectors like `.gutter` or `.cursor` should remain unprefixed
- context-targeted stylesheets
	- previously Atom allowed targeting specific shadow DOM elements
	- this involved adding `.atom-text-editor` to filename
	- remove as no longer necessary
	- if selectors do not work after this change, increase selector specificity
		- try repeating selector class to up specificity: `.my-class.my-class`
- decide [when to migrate](https://flight-manual.atom.io/shadow-dom/sections/removing-shadow-dom-styles/#when-should-i-migrate-my-themepackage) your package

## Upgrading to 1.0 APIs

### Upgrading Your Package
- [what to update](https://flight-manual.atom.io/upgrading-to-1-0-apis/sections/upgrading-your-package/) when switching to 1.0
- mostly achievable just by fixing errors/deprecations that come up
- visit the site above for specific updates and deprecations

### Upgrading Your UI Theme or Package Selectors
- [updating to deal with DOM](https://flight-manual.atom.io/upgrading-to-1-0-apis/sections/upgrading-your-ui-theme-or-package-selectors/)
- DOM style-breaking changes
- as mentioned above shadow DOM removed

### Upgrading Your Syntax Theme
- this section deals with text editor Shadow DOM, no longer applicable

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
	- Atom Flight Manual and associated issues in its repo
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
### 3. Hacking Atom
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

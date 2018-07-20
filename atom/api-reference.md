# Atom API Reference

Notes on what I'm discovering and reinforcing as I make my way through the [API Reference](https://atom.io/docs/api/v1.27.2/) after reading the Flight Manual.

## AtomEnvironment
- global class
- packages, themes, menus, window
- can get to references to specific instances like:
  - `::grammars` for a `GrammarRegistry` - get, add, subscribe, ... to grammars
  - `::commands` for a `CommandRegistry` - more about this below
  - `::views` for a `ViewRegistry` - provide view for a model ("View Provider association")
  - a `StyleManager` for getting or subscribing to style elements
  - a `TooltipManager` for displaying one or more tooltips
  - a `PackageManager` for "coordinating the lifecycle" of packages
  - a `DeserializerManager` for deserializing packages
  - as well as clipboard, themes, menu, keymaps and more
- subscriptions - only for a handful of "extended method" events taking callbacks
  - did beep
  - will throw / did throw error
  - shell environment loaded
    - on load or immediately if is loaded
    - callback only called on unhandled error
- details
  - whether Atom is in dev/safe/spec mode
  - get version, release channel (`dev`/`beta`/`stable`)
  - check if released version
  - load time, load settings
- managing window
  - open, close, get/set size (of window), get/set position (of window)
  - extended methods include focus, show/hide, reload, set/check fullscreen and more
- messaging users
  - beep
  - confirm - alert-like dialog async or sync (recommended callback gets selected index)
- dev tools - only a few "extended methods"
  - open, toggle, execute JS

## Color
- returned from `Config::get`
- method for parsing string or object to color
- methods for converting to hex/rgba

## CommandRegistry
- global `atom.commands`
  - commands here findable in Command Palette
- listeners <-> commands, with CSS selectors
- event handling is jQuery-like "event delegation"
  - custom DOM events
  - on focused element
  - through keybinding or Command Palette
- this is instead of binding listeners to DOM nodes
  - attach to `atom.commands`
  - then constrain using CSS selectors
- pattern: `namespace:action`
  - `namespace` is usually the package name
  - `action` is the command's behavior
  - words separated by `-` and all lowercase
- listener call order (parallels CSS cascade)
  - event bubbles up through DOM
  - listeners invoked by specificity (more to less)
  - ties resolved by invoking most recent listener
  - event context: `this` points to `event.currentTarget`
  - `stopPropagation`/`stopImmediatePropagation` stop bubbling and avoid triggering more listeners
- methods include:
  - add command listener to element
  - find, dispatch, on will/did dispatch

## CompositeDisposable
- collects multiple `Disposable` objects
  - `Disposable` objects are returned when subscribing
  - `Disposable` object has method for checking if disposable
  - `Disposable` objects have constructor and destructor (`dispose()`) methods
  - composites dispose these in a group
- methods for constructor and disposal
- collection methods for adding, removing one `Disposable`
- `delete` is an alias for the remove method
- method for clearing the collection

## Config
- `atom.config` has get and set methods
  - pass `get` the `package.key` to return a value
  - pass `get` the `package.key` and a new value to set
- `atom.config` has an observe method to watch changes
  - `onDidChange` to catch old-to-new value changes only
- values are set as strings but type specified in **schema**
  - this is done in package main
  - use JSON to configure type, default, min/max
  - types are coerced and validated - not set if cannot validate!
  - so below setting `atom.config.set('someInt', 'dog')` will not update the value
```
module.exports =
  config:
    someInt:
      type: 'integer'
      default: 23
      minimum: 1

  activate: (state) -> # ...
```
- types: string, integer, number (real), boolean, array, color, object, enum
  - object must have key with child keys defining `properties`
- settings can have `title` and `description` keys with limited markdown
- you can initialize and set settings outside of package main schema
  - _not_ recommended!
- do not "depend on (or write to) configuration keys outside of your keypath"
  - basically, don't try to manipulate settings for other packages (and more)
- methods include:
  - `observe` and `onDidChange` mentioned above
  - get, set, unset keypaths
  - extended: get all, sources for strings added via set, get schema, suppress observers

## Decoration
- visually represents a `DisplayMarker`
- add CSS to gutter numbers, lines, "selection-line regions around marked ranges of text"
- create with `marker = editor.decorateMarker(range)`
- destroy with `marker.destroy()`
  - only when not needed or not owned
- methods include:
  - destroy
  - checks for property changed, destroyed
  - get id, get marker, check type
  - get/set properties
    - example uses a method called `update` instead
    - change decoration's class

## DisplayMarker
- "buffer annotation"
  - stays put as buffer changes
  - anytime buffer location tracked: cursors, folds, misspelled words, ...
- create through `DisplayMarkerLayer` instead of directly
  - mark buffer range
  - _or_ mark screen range
- head and tail
  - always head, sometimes tail
  - tail stays put
  - head moves, like when mouse moves during selection
  - normal orientation: head > tail
  - reversed orientation: head < tail
- validation
  - markers start off valid
  - some changes to buffer can turn marker invalid
  - based on chosen marker invalidation strategy
    - never: good for selections
    - surround: when changes completely surround marker
    - overlap: (default) when changes surround start or end
    - inside: when changes end up inside the marker
    - touch: any contact with marked region, including up to start or end
- methods
  - destroy, copy
  - subscribe for notifications when changed or destroyed
  - check if marker is valid, destroyed, reversed or exclusive
  - get invalidation strategy
  - get, set properties
  - check if marker matches properties
  - compare marker range to another marker
  - compare marker range and options to another marker
  - get or set buffer or screen range
  - get the screen position of the marker's start or end
- head and tail methods
  - set or get buffer and screen positions
  - start or end buffer positions
  - plant, clear or check for tail

## DisplayMarkerLayer
- collection of related DisplayLayers for markers
- overlays a MarkerLayer
- experimental!
- methods:
  - destroy (the layer), clear (all markers in layer), check destroyed
  - subscribe for notifications on destroy, update, create marker
  - create markers using screen/buffer position/range
  - query for marker/markers, marker count
  - find markers with specific properties
- note that the underlying MarkerLayer is also experimental and can do similar

## Disposable
- handle to disposable resource
- an Emitter's `on` event "returns disposables representing subscriptions"
- methods:
  - check if it's disposable
  - constructor
  - dispose
    - runs disposal action
    - indicates resource no longer needed
    - callable multiple times but action only runs once

## Emitter
- register handles with `on` and evoke with `emit`
- class instances intended to be used with internal classes exposing event API
  - example: create emitter and add an `on` for name change, then `emit` when name changes
- methods:
  - constructor, clear, dispose (see Disposable above)
  - on (whenever event emitted), once (next time event emitted)
  - preempt (run before other handlers existing at subscription time)
  - emit (the actual invocation)

## LayerDecoration
- decoration for every marker on a MarkerLayer
- create with text editor's `.decorateMarkerLayer`
  - create many markers without having to set/adjust every single one
- methods:
  - destroy, check if destroyed
  - get/set properties
  - set properties on a specific marker

## MarkerLayer
- container for a marker set
- experimental
- lifecycle methods:
  - copy, destroy, clear
  - check if destroyed
- query methods:
  - get marker by id
  - get all markers in layer
  - get marker count
  - find markers by property
  - get marker layer role (such as `atom.selection`)
- creation methods:
  - create marker with a certain range
  - create marker with no tail and head at a position
- event subscription methods:
  - update (when layer marker created, updated, destroyed)
  - create (when layer marker created)
  - destroy (when layer is destroyed)

## Notification
- user notification
  - has a message
  - has a type
- event subscription methods:
  - pass callback for when dismissed
  - pass callback for when displayed
- read methods:
  - get type, get message
- other method: `dismiss`

## NotificationManager
- create notifications to send to user
- `atom.notifications` is a globally available instance when Atom is running
- event subscription methods:
  - notification added
  - notifications cleared
- add methods:
  - add a success, info, warning, error or fatal error notification
  - each add method takes a message and optional `options` object
  - options include buttons, description, dismissable, ...
- read methods:
  - get notification
- other method: `clear`

## Point
- single row, column point
  - two-element array
  - `new Point(1,2) === [1, 2]`
- properties: row, column
- construction methods:
  - constructor, convert object to point, copy (same row/col), negate
- comparison methods:
  - `.min` finds the earliest of two points
  - `compare` checks whether this point is earler/later/equal
  - check if equal, less than, less or equal, greater than, greater or equal
- methods returning a new Point:
  - `freeze` makes point immutable
  - `translate` adds to row and col of a point
  - `traverse` gets a new point by traversing typewriter-style across lines
    - me: what's the difference in result? any cases?
- conversion methods:
  - point to array
  - serialize array of row/col
  - string representation of point

## Range
- region between two points
- "range-compatible" arrays like `[[0, 0], [1, 0]]`
- has `start` and `end` properties
- construction methods:
  - constructor (taking two points)
  - range from object
  - copy, negate (both start and end)
- serialize/deserialize methods
- details methods:
  - check if is empty, if is single line
  - read row count
  - read rows (array of all rows)
- methods to operate on range:
  - freeze (return immutable version)
  - union with other range
  - translate (takes point to use as delta)
  - traverse (typewriter style again)
- comparison methods
  - compare (returns if starts before, same, after)
  - check if equal
  - check if covers same rows
  - check if intersects with another range
  - check if contains another range
  - check if contains a point
  - check if intersects a row
  - check if intersects a row range
- conversion method: convert to string representation of range

## TextEditor
- "all essential editing state" for one buffer
  - cursor position
  - selection position
  - folds
  - soft wraps
- used to manipulate editor state
- one buffer can be associated with many editors
  - example: one file in two panes
  - above would be one buffer, two editors
  - in above example, changes in one buffer update two editors
  - _but_ editors have own cursor position, selection position, ...
- access editor instance
  - register callback on `atom.workspace` global using `observeTextEditors`
    - called with current editors
    - called with any future editors
  - example method call with callback param
```
atom.workspace.observeTextEditors(editor => {
  editor.insertText('Hello World')
})
```
- soft-wrap coords: buffer vs screen
  - soft wrapping and folds cause misalignment between screen and buffer
  - wrapped lines display as multiple on screen but are only one in buffer
  - folded lines do not display lines on screen that are counted in buffer
  - consider difference in behavior when extending
    - example: jumping cursor 10 lines for user probably calls for screen coords
    - example: jumping between method definitions may call for buffer coords
  - advice: _default to buffer coords_!
- methods:
  - subscription methods
    - pass callbacks for events
    - including changes to buffer title, path, or changes stopped
    - cursor and selection changes
    - adding, removing gutters
    - buffer saved, buffer destroyed
    - extension methods include changes to wrapping, grammar, add/remove selection, ...
  - info about file
    - read title, path
    - check if file is modified, empty
    - get/set encoding
  - save file and save file as
  - read text
    - read buffer text, read buffer text in range
    - read specific lines
    - read paragraph around cursor
  - change text
    - replace (set), insert, delete, backspace text
    - extension methods include change case, insert lines, delete to various points, ...
  - history methods
    - undo and redo last change
    - extension methods for batching undo/redos and handling checkpoints (pointer in history stack)
  - editor coordinates
    - convert between screen/buffer positions or ranges
    - extended methods for clipping returned point to a valid position in screen/buffer
  - decoration methods
    - add decoration to track a DisplayMarker (including if marker moved, invalidated, destroyed)
    - add decoration to every marker in a DisplayMarkerLayer
    - extended methods to get decorations or get line/highlight/overlay decorations
  - marker methods
    - create marker with a specific buffer/screen range or position
    - find markers
    - create marker layer to group markers
    - retrieve a specific marker layer
    - retrieve default marker layer (all markers not given an explicit layer)
    - read markers, marker by id or marker count
  - cursor (movement) methods
    - various ways to read cursor screen/buffer positions
    - or fetch/add a cursor at a buffer/screen position
    - check if there are multiple cursors
    - many movement methods, everything from up/down to end of screen line
    - extension methods allow even more movement behaviors
    - plus extension methods for fetching last cursor, all cursors or word under cursor
  - selection methods
    - read text selection
    - get/set selected buffer/screen ranges
    - add screen/buffer range selection
    - many selection methods for, like move above, everything from up/down to end of screen line
    - even more extended methods for selection direction/location behaviors
    - plus extended methods for fetching all selections, markers, larger/smaller syntax nodes
    - and a handy extended method for checking if a buffer range intersects a selection
  - find/replace ("Searching and Replacing") methods
    - scan buffer for text matching regex and run iterator function on matches (like `match` or `replace`)
    - scan buffer within a range (otherwise just as above)
    - scan buffer backwards
      - useful for making programmatic changes while iterating
      - invoke instead of `scan` to avoid "tripping over your own changes"!
  - tab methods
    - get/set/toggle soft tabs for the editor
    - get/set tab length
    - extended methods for checking if buffer uses soft tabs and reading tab text (indent level)
  - soft wrap methods
    - check if text is soft wrapped
    - set/toggle editor soft wrapping
    - get column where text will soft wrap
  - indentation methods
    - get/set indentation level for buffer row (depending on soft tabs and tab length)
    - extended methods to indent/outdent specific rows, get line indent level, autoindent rows based on a grammar
  - grammar: read the current grammar for this editor
  - syntax scope methods
    - get the root scope for this editor's current language
    - get the scope descriptor for this position in the buffer
    - extended methods for getting range for scope at cursor position, checking if row is commented out
  - clipboard methods
    - copy/cut/paste text
    - cut text up to end of screen line or buffer line
  - fold methods
    - fold/unfold the current screen/buffer row
    - extended methods for folding lines, folding/unfolding all, toggling folding and checking for folded/foldable
  - gutter methods
    - get gutters, add a gutter
    - get a gutter with a given name (but documentation shows no params for this method)
  - scroll methods
    - scroll to cursor position or a specific screen/buffer position
  - rendering methods
    - read or set a mini editor's "greyed out placeholder" text
      - this text displays when editor does not yet have content

## TooltipManager
- "associates tooltips with HTML elements"
- globally available through `atom.tooltips`
- adding a tooltip returns a disposable
- since routinely grouped, add to a CompositeDisposable
- optionally display keybinding by setting `keyBindingCommand` property
```
const { CompositeDisposable } = require('atom')
const subscriptions = new CompositeDisposable()
subscriptions.add(atom.tooltips.add(document.createElement('div'), {title: 'This is a tooltip'}))
subscriptions.add(atom.tooltips.add(document.createElement('div'), {
  title: 'Another tooltip',
  keyBindingCommand: 'find-and-replace:toggle-case-option',
  keyBindingTarget: this.findEditor.element
}))
subscriptions.dispose()
```
- methods:
  - add an HTML element using various options (title, placement, hover/click/focus trigger)
    - as shown above this returns a Disposable
  - find tooltips applied to a specific element
    - returns array of tooltips on the target element

## ViewRegistry
- associates a model with a view
- class provides view for any model
  - the model/view association has to be registered first
  - models are registered using `addViewProvider`
- building a pane item
  - separate models from views
  - do this for "all but the simplest items"
  - model deals with logic and API calls
  - view deals with presentation
- any object can be a model
  - implement `getTitle` in models if displayed in Pane
- how a view works
  - tells workspace how to present the given model
  - view provider returns DOM node
  - HTML5 elements can implement views
- global access through `atom.views`
- methods:
  - add a view provider
    - optionally pass model constructor to limit view creation to that model
    - pass factory function that returns some HTML
    - adding a view provider returns a disposable
  - get the view for an object in the workspace
    - direct access to the view layer
    - use to manipulate DOM in ways not supported by API
    - performs a [series of checks](https://atom.io/docs/api/v1.28.0/ViewRegistry#instance-getView) to resolve the view and return an associated view

## Workspace
- UI state for the whole window
- globally accessible through `atom.workspace`
- uses:
  - open files
  - get notifications of current/future editors
  - manipulate panes
    - add panel methods allow panel creation
- "items" display in a workspace pane
  - in WorkspaceCenter
  - or in one of three Docks
- workspace requires `getTitle` method
- methods:
  - subscription methods for observing editors/panels, or events on editor/panel changes (add, destroy, ...)
  - opening methods to open/hide/create item for URI
    - add an opener function that runs whenever a URI is opened
    - create new editor
  - panes extended methods for reading panes and pane containers and activating panes
  - pane location methods for getting the center workspace or the left/right/bottom dock
  - panel methods for reading anel items at various locations or adding a panel to them
  - search and replace methods for scanning/replacing across all files in workspace with a regex

## WorkspaceCenter
- the main workspace at center of window
- this is the "workspace center" within the Workspace
  - the Workspace also includes the three Docks (more above)
- methods:
  - subscription methods
    - observe editors and pane items
    - events when pane items added/changed/destroyed
    - event when text editor added
  - pane item methods
    - read array of pane items in the workspace center
    - just the active pane item
    - read all text editors in the workspace center
    - just the current active text editor
  - extended methods for panes
    - read all panes or the active pane
    - activate the previous or next pane


## BufferedNodeProcess
- same as BufferedProcess but takes and runs a Node script
- Windows does not support `#!` shebangs so this method is necessary there
- included with `const {BufferedNodeProcess} = require('atom')`
- constructor method as below - compare BufferedProcess

## BufferedProcess
- Node child process wrapper with error/output line buffering
- an example of creating a buffered process
```
{BufferedProcess} = require('atom')

const command = 'ps'
const args = ['-ef']
const stdout = (output) => console.log(output)
const exit = (code) => console.log("ps -ef exited with #{code}")
const process = new BufferedProcess({command, args, stdout, exit})
```
- methods
  - constructor: as above, takes command, arguments, output and an exit code
  - will throw error callback - method to call when error raised
    - usually called because command isn't available at this path
    - call `handle()` on the object passed to callback to signal error handled
  - kill method to terminate the process

## Clipboard
- representation of the Atom clipboard
  - for copy/paste
  - available globally through `atom.clipboard`
- example use:
```
atom.clipboard.write('Hello World!')
console.log(atom.clipboard.read())
```
- methods:
  - write text to the clipboard
  - read string existing in clipboard
  - read the text and associated metadata
    - metadata is stored by earlier calls to `write`
    - returns an object with both the string and any metadata

## ContextMenuManager
- registry for commands that should appear in a context menu
  - available globally through `atom.contextMenu`
  - reminder: context menus come up on right click in a given context
- expects a specific [CSON format](https://atom.io/docs/api/v1.28.1/ContextMenuManager)
  - select the context key, then associate labels with commands in that context
  - package menu `.cson` then specifies the menu under key `context-menu`
- method to add context menu items
  - pass it CSS selector matching elements that should be the key
  - then value objects have [these keys](https://atom.io/docs/api/v1.28.1/ContextMenuManager#instance-add)
    - `label` string for menu item label
    - `command` string for associated command
    - `enabled` boolean for whether item is clickable
    - `submenu` allows nesting on this item by adding array of label-command objects
    - `created` function called on the item each time right click brings up menu
    - and others
  - returns a Disposable

## Cursor
- Cursor representing place of textual insertion
- visually a blinking vertical line
- belongs to TextEditor
- has DisplayMarker metadata attached to it
- methods:
  - subscribe with a callback for did change position and did destroy events
  - manage cursor position
    - set or get screen or buffer position
    - break down reading into screen/buffer row or column
    - check if cursor is at the beginning or end of a line
  - read cursor position details
    - get the underlying DisplayMarker
    - check if surrounded by whitespace, between word and nonword, is inside word, has preceding characters
    - get the indentation level
    - get the scope descriptor
    - check if it's the last cursor in the editor
  - move the cursor
    - move to or along the movement dimensions found in TextEditor, including up, right, end of word, ...
  - read local positions/ranges
    - get the word, word boundary, line, paragraph and more around a cursor
  - compare buffer position to another cursor's buffer position (passing the other cursor as argument)
  - utility methods
    - clear (deselect) the current selection
    - get the regex used by cursor to determine what a word is
    - get the regex used by cursor to determine what a subword is
      - camelCase and snake_case contain multiple subwords

## DeserializerManager
- manage Deserializers
  - these are used for serialized state as explained in the Flight Manual
  - available globally through `atom.deserializers`
- example of using it to add a Deserializer
```
class MyPackageView extends View
  atom.deserializers.add(this)

  @deserialize: (state) ->
    new MyPackageView(state)

  constructor: (@state) ->

  serialize: ->
    @state
```
- review [serialization](https://flight-manual.atom.io/behind-atom/sections/serialization-in-atom/)
  - the preferred way (instead of through the global `add` above) is to add through `package.json`
- methods:
  - add one or more deserializers
    - added objects must have a `name` and a `deserialize` property
  - deserialize a `state` object and its params

## Directory
- represents a disk directory
- watch the directory for changes
- methods:
  - constructor
    - configure new directory instance, taking string argument for path
  - create a directory at `getPath` if it doesn't exist
    - returns Promise
    - Promise resolves when directory created
  - subscribe to did change event, callback on directory content changes
  - metadata methods:
    - check if is a file/directory/symbolic link
    - check if directory exists (one variation returns a promise, other sync)
    - check if directory is root
  - path management:
    - read the path or the path with symlinks and relative pathing
    - read the directory's basename
    - `relativize` to read the path from current directory
  - traversal:
    - traverse to parent
    - traverse to a specific file
    - traverse to a specific subdirectory
    - read entries from the directory, either passing a callback or sync
    - check if a path is inside this directory

## Dock
- represents containers at editor edges
- not intended for direct creation
  - access workspace docks through Workspace
  - use Workspace methods `getLeftDock`, `getRightDock`, `getBottomDock`
  - _or_ add dock item through Workspace method `open`
- methods:
  - basic ones to add, show, hide
  - toggle visibility, check if visible
  - subscribe for events
    - changed visibility
    - observe visibility (for current and future changes)
    - changed pane items
    - observe pane items (current and future changes)
    - pane add, will/did destroy, or changed active
    - watch pane items, too
  - pane item methods: read all pane items or just the active one
  - pane methods
    - read all panes or just the active one
    - activate the next/previous pane

## File
- representation of a single file
  - this is a file to be watched, read, written
- methods:
  - constructor and create
    - construction configures the file but does not access it
    - `create` returns a Promise resolving once file created
  - subscribe for events
    - callbacks for changed, renamed, deleted
    - callback for watch error invoked after file unsubscribed from watches
  - metadata
    - check if exists (sync or async), or if is file/directory/symbolic link
    - read the SHA (sync or async)
    - read or set the charset encoding
  - path management
    - read the path
    - read the "completely resolved" path (sync or async)
    - read the base filename only (no directory)
  - traversal: read the parent directory
  - read and write
    - read the contents, passing boolean to require direct read or accept cached copy
    - create a read stream or a write stream, returning a stream object
    - write text (sync or async)

## GitRepository
- representation of Atom's git operations
  - not meant to be instantiated
  - access through global `atom.project` with a `getRepositories()` call
  - works only with a repo-backed project
- submodules handled through `path` argument to methods
  - determines underlying repo to use
  - example use below
```
git = atom.project.getRepositories()[0]
git.getShortHead()   # 'master'
git.getShortHead('vendor/path/to/a/submodule')
```
- require the Atom GitRepository class in your package
```
{GitRepository} = require 'atom'
```
- methods:
  - construction/destruction
    - open a path
    - call destroy on the objects or check if it was destroyed
  - subscribe for events
    - callbacks when changed status of one file or statuses of multiple files
      - status changes when file updates, reloads, ...
      - mutliple, for example window focus checks all paths in repo
    - callbacks when destroyed
    - as normal, all return a Disposable for unsubscribing
  - repo details
    - read the type of version control
    - read the path of the repo or just to the working directory
    - check if project is at root
    - `relativize` path to the working directory
    - check if branch exists
    - get short head reference (no `refs/heads`, tags, remotes; no SHA-1 for detached head)
    - check if a path is a submodule in the repo
    - check how many commits ahead/behind branch is from upstream remote
      - also variant for cached commits
    - retrieve configuration value for a configuration key
    - retrieve origin URL
    - retrieve upstream branch for current head
    - read the local and remote references (heads, remotes, tags arrays)
    - read SHA string for a specific reference
  - reading status
    - check if path is new/modified/ignored
    - read the status of a path (variant for cached status) or a directory
    - check if status shows modified
    - check if status shows new path
  - getting diffs
    - diff stats for number of path lines added or removed
    - compare line diffs for head version of path versus passed-in text
  - checking out
    - check out head at a path, just like running git reset and checkout `HEAD` at that path
    - check out a branch in the repo, passing string reference and boolean to create if not exist

## Grammar
- the Atom grammar
  - tokenize text lines
  - not meant to instantiate
  - access from GrammarRegistry using `loadGrammar` (see immediately below)
- methods:
  - subscribe for callback when updated
    - updated means grammar it depends on is added/removed from GrammarRegistry
    - returns a Disposable as expected with event subscriptions
  - tokenization
    - tokenize all lines in passed-in text, returning array of token arrays
    - tokenize a line from passed-in line string, `ruleStack` (from previous line or null if first line)

## GrammarRegistry
- contains at least one grammar
- methods:
  - subscribe with callback for when grammar added, updated, removed
  - management
    - read all grammars, or a specific grammar (sync or async)
    - add grammar, or load grammar and add it (sync or async)
    - remove grammar with passed-in scope name

## Gutter
- representation of a TextEditor gutter
- gutters get created through TextEditor method `addGutter`
  - recall that TextEditor objects accessed through callback
  - register that callback on `atom.workspace` method `observeTextEditors`
  - the callback method fires on current editor instances and whenever any future created editor
- methods:
  - destroy the gutter instance this method is called on
  - subscribe with callback for events
    - when gutter visibility changed
    - when gutter destroyed
  - visibility
    - show/hide the gutter
    - check if the gutter is visible
    - add DisplayMarker decoration to the gutter
      - decoration for the passed-in marker
      - tracks the DisplayMarker when moved/invalidated/destroyed
      - `decorationParams` support all params in TextEditor `decorateMarker`
      - set `.type` to `line-number` for numbered gutters, otherwise to `gutter`
      - returns a Decoration

## HistoryManager
- keep track of projects opened in the past
- available globally through `atom.history`
- powers the "Reopen Project" menu
- methods:
  - read list of previously opened projects
  - clear projects from the history
  - register callback for when list of projects changed
    - returns a Disposable like other subscription methods

## KeymapManager
- associates keystrokes with commands
  - context sensitive
  - globally accessible through `atom.keymaps`
- keybindings are just JS objects
  - CSS selectors for contexts are outer keys
  - keystroke pattern -> command mappings are defined for each selector
- keystroke matching
  - context keystroke sequence matches keybinding
  - custom DOM event with command type dispatches on event target
  - keymap starts looking at target for keyboard event
    - are there keybindings with CSS selectors matching the target?
    - selects the most specific match
    - selects most recent match when matches are equally specific
    - unfound matches then search up to parent selector recursively until top
- found keybindings
  - command dispatched on target of keyboard event
    - this happens even when matched element is higher in DOM
  - `.preventDefault()` called if binding match found
  - `.abortKeyBinding()` can be called on command event object
    - let other bindings match instead
    - example: bind snippet expansion to tab but abort if no snippet found
- multistroke keybindings
  - pending state when keystrokes partially match multikeystroke binding
  - pending state terminates
    - on following keystroke
    - _or_ after milliseconds defined by `partialMatchTimeout`
  - if terminated without matches
    - "longest ambiguous bindings" causing pending state disabled temporarily
    - previous keystrokes replay
    - if unfound on replay this happens again with next longest bindings, ...
- methods:
  - build a keydown DOM event
    - meant to be used for testing
    - pass in a key and options like target element or modifier keys
  - construction/destruction
    - constructor with options to assign to keymap
    - clear registered keybindings and keystroke queue
    - destroy (unwatch paths)
  - subscribe for events with callback
    - when binding matched or partially matched
    - when binding failed to match
    - when failed to read file (failed to load keymap file)
  - build or add bindings
    - build keybindings
      - group by CSS selector
      - pass in a `source` and `bindings` mapping keystrokes to commands
    - add a set of keybindings, passing in same arguments
  - access bindings
    - get current keybindings
    - find keybindings matching keystrokes, command, target
  - manage keymap files
    - load keymaps from a path
    - watch (reload) path on changes
  - manage keyboard events
    - handle (dispatch) custom `'keydown'` type keyboard event
    - turn keydown event into keystroke string
    - add custom keystroke resolver for raw events
      - used to work around Chrome or change how Atom resolves key combinations
    - get partial match timeout (milliseconds before partial pending states terminate)

## MenuManager
- application menu item registry
- globally available through `atom.menu`
- consider the [CSON format](https://atom.io/docs/api/v1.28.2/MenuManager) for menus
  - include within package's menu `.cson` under `menu` key
- methods:
  - add items to menu, passing in a label and optionally submenu items, command
  - update to refresh the menu

## Package
- load and activate package
  - main module
  - resources including stylesheets, keymaps, grammar, menus
- methods:
  - subscribe for events
    - when deactivated (once all packages activated)
  - module compatibility
    - check if compatible (all native module dependencies compiled against current Atom)
    - rebuild native module dependencies
    - read the build failure output (sterr contents)

## PackageManager
- coordinate packages lifecycle
  - load, activate, parse metadata
  - load, activate, parse resources
    - keymaps
    - menus
    - stylesheets
- globally available through `atom.packages`
- activate a package to register resources and call `activate()` on main module
- deactivate a package to unregister resources and call `deactivate()` on main module
- enable or disable a package
  - through `core.disabledPackages` config settings
  - or calling `enablePackage()` or `disablePackage()`
- methods:
  - subscribe with callbacks for events
    - all packages loaded or activated
    - one package loaded/unloaded or activated/deactivated
  - package system data
    - get the apm command path (`core.apmPath` if exists)
    - get the directory path where Atom looks for packages
  - package data
    - resolve named package to path on disk
    - check if named package is bundled with Atom
  - enable and disable package by name, check if disabled
  - access active packages
    - get one named package or all packages
    - check if a package is active
    - check if all packages have activated
  - access loaded packages
    - get one loaded package by name or all loaded packages
    - check if a named package is loaded
    - check if all packages have loaded
  - access available packages
    - get paths to all the available packages
    - get names of all the available packages
    - get metadata for all the available packages

## Pane
- content container at center of Workspace
  - can hold multiple items
  - only one item active at a time (one is active)
  - interface displays view for the active item
  - tabs display for all items (by default)
- pending items
  - pane can have a pending item
  - added pending items replace current pending
  - tab text for pending items displays in italics
- methods
  - subscribe with callback for events
    - flex scale changed values or observed (this and future values)
    - pane activated / destroyed (called before) / will destroy (called before)
    - active changed or observe
    - item added, moved or removed, plus will remove (for calling before removed)
    - observe current and future items, or active item
    - active item changed
    - next/last/done choosing chosen most recently used item
    - before item destroyed
  - item methods
    - activate specific items or read active item
    - various methods for moving, adding, destroying, saving items (including active)
  - lifecycle methods
    - check if pane active or if destroyed
    - activate, destroy pane
  - split methods
    - split pane up/down or left/right

## Panel
- 

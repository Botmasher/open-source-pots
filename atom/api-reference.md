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

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

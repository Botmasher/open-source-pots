# Repo notes: atom/atom

## Overview
- README checks the basic boxes but lacks any path into the code base
  - test shields
  - mentions and links to Electron
  - product description, including links to community and code of conduct
  - links to documentation: Flight Manual and API Reference (see my notes)
  - installation prerequisites and OS steps
  - links to building instructions
  - license
- how to know the code?
  - existing docs don't directly answer that question
- goals of this doc:
  - break down and understand the codebase from a wide view
  - trace a beginning path through the code to gain that understanding

## Basic Branching
- `apm` for Atom Package Manager
  - just an install folder for APM: https://github.com/atom/apm
  - most use cases covered by APM command line tool
  - API info: https://atom.io/docs/api/v1.29.0/PackageManager
  - Flight Manual: https://flight-manual.atom.io/atom-server-side-apis/sections/atom-package-server-api/
  - an [example of how Atom uses apm](https://github.com/atom/settings-view/blob/master/lib/package-manager.coffee)
- `benchmark` for a benchmark runner and large file / long line tests
- `docs` for building documentation
  - focused on platform-specific instructions for building Atom from source
  - this was moved to the Flight Manual
  - `./focus` contains a doc [sharing current plans](https://github.com/atom/atom/tree/master/docs/focus)
- `dot-atom` contains the files and directories that install to `.atom`
  - including the custom stylesheet and keymap files
  - including the custom packages directory
- `exports` requires and exports objects
  - requires and exports a bunch of src objects that all seem to deal with file and buffer load/events
    - including Task and TextEditor when not a child node process (meaning when process type is `'renderer'`)
  - also contains a handful of Electron API shims using [grim.deprecate](https://github.com/atom/grim/blob/master/src/grim.coffee)
- `keymaps`
  - cson keybinding maps for various platforms
  - `base.cson` has selectors for `atom-text-editor` and many `body` default to native
  - movement, selection and many other bindings for `darwin`, `linux`, `win32`
- `menus`
  - three files containing array of menu items for different platforms
  - objects contain a `label` and `submenu` arrays of objects with `label` and `command`
- `resources`
  - png app icons
  - shell script, js and apm shell and command scripts for `win` installation
  - long bundle info plist for `mac`
  - debian and redhat package for `linux`
- `script`
  - a variety of scripts for building Atom
  - linter scripts
  - version and platform sensitive installation, cleaning, transpiling
  - `vsts` subfolder
    - contains its own README
    - scripts for automating Atom releases
    - `yml` files for configuring releases
    - tasks for handling release branch on different platforms
    - uses Visual Studio Team Services
      - [multi-phase jobs](https://github.com/Microsoft/vsts-agent/blob/master/docs/preview/yamlgettingstarted-jobs.md)
      - generate Atom installation packages on all three platforms
      - publish new release on successful build
    - Atom Nightly packages published to GitHub and atom.io
    - version release numbers calculated by `generate-version.js`
      - nightly release number incremented after comparing base version against `package.json`
    - phase templates build Atom simultaneously across platforms
    - successful build release artifacts uploaded to S3 bucket
- `spec`
- `src`
- `static`
  - `.less` files for editor, cursors, docks, syntax and much more
  - stylesheet fallback variables
  - basic html template `index.html`
  - window load and setup script in `index.js`
  - and the octocat spinner!
- `vendor`
  - Jasmine tests for JS and JQuery
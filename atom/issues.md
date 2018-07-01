# Issues

Issues noticed while reading and taking notes on the documentation. This includes both the [API docs](https://atom.io/docs/api/v1.28.0/) and the [Flight Manual](https://flight-manual.atom.io/).

## API Docs

- [ ] documentation for getting `gutterWithName` indicates no parameter for a "given name"
  - where: TextEditor > Gutters
  - uri: https://atom.io/docs/api/v1.28.0/TextEditor#instance-gutterWithName
  - description: (check the code base & test before submitting as issue) the documentation explains that the method "get[s] the gutter with the given name" but lacks info on a parameter for passing that name


## Flight Manual

- [X] conflicting info about default movement and selection keybindings
  - PR (fixed): https://github.com/atom/flight-manual.atom.io/pull/467
  - where: 2 > Moving in Atom > Additional Movement and Selection Commands
  - uri: https://flight-manual.atom.io/using-atom/sections/moving-in-atom/#additional-movement-and-selection-commands
  - description: the text explains that "the command `editor:move-to-beginning-of-screen-line` is available in the command palette, but it's not bound to any key combination", but two fenced blocks below this `editor:move-to-beginning-of-screen-line` is listed as one of the "Movement and Selection Commands that have a keyboard shortcut by default"

- [ ] heading text lacks prescribed title-case
  - where: 2 > GitHub package
  - uri: https://flight-manual.atom.io/using-atom/sections/github-package/
  - description: most multiword headings in this section are not title-cased, which conflicts with the [language guidelines](https://github.com/atom/flight-manual.atom.io/blob/master/CONTRIBUTING.md#language) in CONTRIBUTING.md.

- [ ] heading text lacks prescribed title-case
  - where: 2 > Version Control in Atom
  - uri: https://flight-manual.atom.io/using-atom/sections/version-control-in-atom/
  - description: most multiword headings in this section are not title-cased, which conflicts with the [language guidelines](https://github.com/atom/flight-manual.atom.io/blob/master/CONTRIBUTING.md#language) in CONTRIBUTING.md.

- [ ] text uses "hit" instead of prescribed "press"
  - where: 2 > Writing in Atom
  - uri: https://flight-manual.atom.io/using-atom/sections/writing-in-atom/
  - description: keypress instructions under Spell Checking state to "pull up a menu of possible corrections by _hitting_ <kbd>Cmd+Shift+;</kbd>" (italics mine). Later, the Snippets subsection gives two instructions to "hit `tab`". These conflict with the [language guidelines]( https://github.com/atom/flight-manual.atom.io/blob/master/CONTRIBUTING.md#language) in CONTRIBUTING.md.

- [ ] heading text lacks prescribed title-case
  - where: 3 > Cross-Platform Compatibility
  - uri: https://flight-manual.atom.io/hacking-atom/sections/cross-platform-compatibility/
  - description: most multiword headings in this section are not title-cased, which conflicts with the [language guidelines](https://github.com/atom/flight-manual.atom.io/blob/master/CONTRIBUTING.md#language) in CONTRIBUTING.md.

- [ ] heading text lacks prescribed title-case
  - where: Appendix B
  - uri: https://flight-manual.atom.io/shadow-dom/sections/removing-shadow-dom-styles/
  - description: most multiword headings in this section are not title-cased, including some full sentences, which conflicts with the [language guidelines](https://github.com/atom/flight-manual.atom.io/blob/master/CONTRIBUTING.md#language) in CONTRIBUTING.md.

  - [ ] heading text lacks prescribed title-case
    - where: Appendix D (including both sections)
    - uri: https://flight-manual.atom.io/atom-server-side-apis/
    - description: most multiword headings in this section are not title-cased, including some full sentences, which conflicts with the [language guidelines](https://github.com/atom/flight-manual.atom.io/blob/master/CONTRIBUTING.md#language) in CONTRIBUTING.md.

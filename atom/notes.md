# OpenSourcePot for Atom

Getting into the Atom repo as a programmer who's just starting to use the editor. What would I be contributing to, and how can I contribute? Here's my outline of the project

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
-

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

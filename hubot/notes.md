# OpenSourcePot for Hubot

## Project links
- the main project is here: https://github.com/hubotio/hubot
- the snazzy client site is here: https://hubot.github.com/
- docs are here: https://github.com/hubotio/hubot/tree/master/docs
	- also the documentation page: https://hubot.github.com/docs/
	- breakdown of scripting: https://hubot.github.com/docs/scripting/

## Layout of the project
- nine different repos
- note that the evolution repo is crucial right now for understanding Hubot aspirations, including:
	- the move away from CoffeeScript
	- the aim to support multiple adapters so Hubot tasks can "glue" together "various tools and systems"
	- the Probot merger: https://github.com/probot
- overview of the subprojects:
	- `hubot`: the heavy lifting on chat bots
	- `hubot-redis-brain`: store hubot's data in Redis
	- `hubot-help`: show all of the hubot commands
	- `hubot-diagnostics`: small script for diagnosing hubot (reply to ping, list adapter, tell time or echo text)
	- `evolution`: tracking and proposing progress (more above)
	- `generator-hubot`: Yeoman generator for making hubots
		- docs for the generated bot: https://github.com/hubotio/generator-hubot/tree/master/generators/app/templates
		- see how it makes a bot for your company: https://github.com/hubotio/generator-hubot/blob/master/generators/app/index.js
	- `hubot-for-hubot`: an implementation of hubot for the hubot slack community
	- `hubot-rules`: explains the rules of being a robot, what else?
	- `hubot-scripts-list`: builds a list of all Yeoman generators

## Source code for `hubot`

### General notes
_April 3, 2018_
- get a better sense for the source code
- `/bin` holds scripts for creating and editing a hubot instance in command line
	- `create` is deprecated and you should create a hubot instance through Yeoman
- `/docs` holds all the Markdown for what's shown in site documentation
- `/examples` contains a simple PowerShell startup and a system service script
- `/tests` contains JS tests
	- these are good to analyze before you start hacking your own hubot miniprojects
	- `/fixtures` creates and tests a mock adapter
- `/script` has very little but some `npm` stuff
- `/src` contains the core heavy duty execution going on
	- work your way into `brain.js`, `response.js`, `robot.js`
- consider project dependencies: https://github.com/hubotio/hubot/blob/master/package.json

### Digging into src
_April 4, 2018_
- `user.js` exports a `User` class
	- representation of a chat user
	- just a class with a constructor
	- simply stores a name and optional options
- `message.js` exports various message classes
	- base `Message` models a user chat message
		- constructor stores `user`, `user.room` and if `done`
		- `finish()` sets `done` to ensure no listeners get called
	- `TextMessage` builds on it and stores `text` and `id`
		- `match()` method tests text against a regex
		- `toString()` method gets the text string
	- `EnterMessage` does nothing but extend the base and is for user entrance notification
		- the `text` property is always `null`
	- `LeaveMessage` does nothing but extend the base and is for user exit notification
		- the `text` property is always `null`
	- `LeaveMessage` does nothing but extend the base and is for user changing topic
		- the `text` is the new topic
	- `CatchAllMessage` is for messages "that no matchers matched"
- `middleware.js` exports an async Middleware class
	- goes through the middleware stack and executes in order
	- `context` object passes through whole middleware stack
	- `next(context, done)` is called at end of the stack
	- `done()` is the "completion callback" that can be wrapped by middleware executing this
	- constructor takes in robot and stack array
	- `execute(context, next, done)` does the work outlined above
		- nests a function to execute one piece of middleware
		- nests another function to execute when stack finishes
		- calls `process.nextTick()` to loop through async
			- uses `async.reduce` to execute next in stack https://caolan.github.io/async/docs.html#reduce
	- `register` method adds it to the stack if it contains 4 args (expected for middleware)
		- call with robot, context, next, done
		- it pushes onto stack
_April 5_
- see my update below

## Plan going forward
0. read through the basics
	- get an overview readthrough of the source code
	- read through and take notes on the docs
1. read through source code and take diligent notes
	- focus on the main project
	- give or point to example code
	- give examples of practical input/output scenarios wherever possible
2. use and hack Hubot to get a sense of how this bot works
	- come up with four starter projects and list them here
	- then do them!
3. revisit the source code
	- you're probably doing this along the way
	- but now come at it with the understanding of someone who's implemented stuff
	- refer back to docs looking for things to update
4. wade into Hubot community
	- chat
	- issues
5. interface with issues and messages
	- triage
	- respond
6. propose an alteration you think will make
	- come up with three helpful fixes or enhancements and list them here
	- for each fix, document clearly reproducible steps
	- for each enhancement, give as much context as possible
	- propose working in detail on the most doable or useful one
- **Update**: a credible source tells me Hubot is rather abandoned these days
	- either take a look at Probot, which is being worked on
	- or consider a more active project
	- (if you're specifically looking for a GitHub project, Atom x-ray is one)

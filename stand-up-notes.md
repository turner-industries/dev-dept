# Bi-Weekly Stand up Meeting

## 5-18-17

* SQL Saturday Update
  * Forest - 
  * Matthew - 
* Upcoming Events
  * Matthew will be presenting C# 7 - Wed, May 31st
  * React Native workshop update
* Date Handling
  * Server should always use DateTime.UtcNow
  * Dates should be returned to the client and parsed to local time in JS
* Mediator Pipeline Testing
  * Discuss handler / validation testing
* String Enums
  * Example of validation
  * SQL constraints?
* Build Server
  * Request to up CPU - integration tests on PRs
* CraftTrax NPM

## 5-4-17

* SQL Saturday Update
  * Matthew is almost ready
    * Intro to parallel programming
  * Forest is waiting on Brandon's outline
  * Justin has submitted - React Native
* Upcoming Events
  * .NET User Group
    * Brandon - Intro to .NET core
    * Next Wednesday after work
  * React Training
    * More potential attendees
    * Justin proposed potentially attending another React Training session solo
      * This would allow him to give that training himself around July or Aug
      * We can have the React Training guys give the advanced training
  * Next Workshop - Matthew - C# 7
    * Sending out invite soon

## 4-20-17

* Architectural Discussion
  * Validation vs Business Rules (thinking about them differently and database impacts)
    * Different solutions:
      * No context in validators
        * Keeps context access consistent
        * Pollutes handler unnecessarily
      * Only entities used in handlers, rest in validators (foreign keys, etc.)
        * Inconsistent context access
        * Keeps handlers smaller
      * Duplicate queries in validators and handlers
        * Keeps validation all in one place
        * Can cause performance issues
     * Going to try only entities used in handlers, rest in validators
* SQL Saturday Follow-up
  * Submit your talks
  * Need to come up with a game potentially
* Workshops
  * Feedback on the last format (involvement, dedicated slices)
    * Thumbs up
    * Test approach was good, more tripping up potentially, abstract before jumping in
    * What are the practical uses of this?
  * Future ideas (Creating and Validating Jwt Tokens, Asp.Net Core Versioning, React unit testing)
    * C# 7 - Matthew
* Code Reviews
  * Bi-Weekly
    * Matthew will be in charge of this
    * Organization/Advance notice needed?
    * Format challenges
      * Stay on track
      * Lots of feedback at the same time
  * Pull Requests
    * Cross-team reviews
* Training
  * React training (Envoc can only send 2 - talking to Sparkhound - followup with Ryan)
  * October 18th / 19th - Tentatively
  * Tech Park
  * Partnering with other firms potentially

## 4-6-17

* SQL Saturday Follow-up
  * Submit your talks
  * Need to come up with a game potentially
* Workshops
  * Forest - Destructuring
    * April 11th - 11 AM
    * Interactive
* Training
  * React training
  * October 18th / 19th - Tentatively
  * Tech Park
  * Partnering with other firms potentially
* Starter Kit
  * Baseline for new projects
  * Going to review / contribute as a group
  * https://github.com/turner-industries/web-starter-kit

## 3-23-17

* Standards
  * Need to create a separate md file for these
  * Look at eslint files next week
  * Naming conventions
    * Back End
      * Classes, Methods - Pascal
      * Params, fields - Camel
    * ~Front End~ React
      * Files
        * Components - Pascal
        * All others - Kebab
      * Content
        * Classes / Components - Pascal
        * Variables - Camel
        * Methods
          * Non-lifecycle - _ + Camel
          * Lifecycle - Camel
  * Best Practices
    * Bind class methods explictly in constructor or use @autobind decorator
* JS Workshop Follow-up
  * Went well - learned a lot
  * Maybe add to our Github
  * Add participant exercises
  * Forest volunteered to go next on ES6
    * Around April 11th
* SQL Saturday
  * Jay has submitted his talk
  * Brandon will be submitting today
  * Forest, Matthew, and Justin will be submitting soon
* Logging
  * Forest has an example project set up with logging of our mediator pipeline
  * Need to set up regex matching to cleanse sensitive information
    * Passwords
    * Socials
    * Pay Rates
* Mediator
  * Async mediator only
  * Custom mediator going forward on new projects

## 3-9-17

* DevOps Day Follow-up
  * Potentially do hands on exercises
  * Good idea in general
  * Open it up to the rest of the dev department
  * Justin is up next
    * March 16 - 11 to 1
    * Javascript async stuff
* SQL Saturday
  * May 30th is deadline to register
* Logging
  * WinTake is going to look into the free plan of exceptionless
    * Look in to using Serilog as the logger
  * We'll review the usage at a later date to figure out which version to get
* Potentially run automatic formatting check on CI
  * ReSharper
  * StyleCop
  * HoundCI
  * ESLint
* Standards
  * Avoid JsonIgnore when possible, as well as other prop attributes (required, validation, etc.)
  * Whitespace
    * Horizontal
      * Front end
        * 2 spaces
      * Back end
        * 4 spaces
    * Vertical
      * No more than 1 blank line
      * Break up in to logical blocks
  * Naming conventions
    * Don't use all uppercase abbreviations
  * Commenting
  * Curly braces
    * "{ }" for all if statements
  * Implicit vs explicit variables
  * Regions
    * No regions
  * Enums
  * Private fields
    * Use _ for class level private fields
    
## 2-23-17

* DevOps Day
   * Thursday - March 2nd
   * Potentially 11-1
* Logging solution
   * WinTake is going to try SeriLog (ILogger.Log())
   * Justin is going to look into the free plan of exceptionless
   * We'll review the usage at a later date
   * Potentially stand up another VM or use an existing that Forest knows about later on
* SQL Saturday
   * Ideas
      * Piknik - Parallel programming
      * Jay - Still thinking
      * Justin - React Native
      * Forest - Developing faster - automation / snippets / templates / etc., or Redux
      * Brandon - .NET Core or CI & CD w/ VSTS & Azure IX
* Standards
   * Whitespace
   * Naming conventions
   * Commenting
   * Curly braces - "{ }"
   * Implicit vs explicit variables
   * Regions
   * Enums
   * Private fields - "_"

## 2-9-17

* Code Reviews
    * Going great; going to continue
    * Next code review will be Jay
    * Piknik will schedule this for 2 weeks from now
* Lunch and Learn
    * Justin will schedule this
* SQL Saturday
    * Going to try to get a bunch of sessions this year, potentially our own room
    * Bring a topic you'd like to present to the stand-up next time
* IMediator / Helpers / Handler injections
    * Injecting IRequestHandlers is the way to go
* Hangfire
    * Need to add BackgroundMediator to WinTake - maybe next Friday
* Group Reading / Watching
    * Can do this at lunch if someone presents a topic

-------------

## 1-11-17

* Following through
    * Less ambitious goals = more probable
* Videos at lunch
    * Will start out with 2 per week watching React videos
* Potential improvements
    * Shadowing other team coding sessions
    * Maybe assigning an article or a book to read and reviewing at future stand-ups
    
-------------

## 11-30-16
* Dev Training Days
  * Unit Testing session first
    * Come up with requirements for next meeting
* Commmand / Query Architecture
  * Brandon is looking at speed issues
* Checkmarx coming tomorrow

--------------

## 11-16-16

* Dev Training Days
  * Lunch and learn
  * Add anything you'd like to discuss to Trello card
  * Have a planning session to go over agenda
* Command / Query Architecture
  * Discuss with Donnell and Piknik
  * Consistent response types
* Add ResponseTypes to API methods for Swagger
* Checkmarx
  * Scans code and finds potential security holes
  * Considering purchasing
  * Nightly build (1-3 hours per scan)

--------------

## 11-02-16

* XLabs
  * Jay proposed contributing to the Xamarin community through our own library of components
* Southeastern Endeavors
  * Attend more class presentations
  * Present at 285 / 383
    * How to do MVC, Git, etc.
  * Hammond .NET User Group
    * Look for someone to run it as a student
    * Brandon will ask Dr. Alkadi about this
* Speaking
  * Justin - Activate Conf [Nov 3]
  * Brandon - Dev Days [Nov 4]
  * Some of us should plan on speaking at SQL Saturday Next Year
* Learning Project
  * Coming along - Piknik worked on it this past week
    * Has some ideas about endpoints and structure
* Mediator
  * WinTake is going sprint by sprint
  * Jay is working on Mediator for CraftTrax
* We all learned something none of the others knew

--------------

## 10-19-16

* Unification
  * Sub DTOs - inherit from base DTOs
    * WinTake - todo
* Better identify versions in environments
  * Git tagging for current environments
  * Version info in footer
* Learning project
  * Every other Friday - direction/recap
* Speaking
  * Justin - Activate Conf [Nov 3]
  * Brandon - Dev Days [Nov 4]
  * Other - BRDNUG, Refresh BR, HDNUG
  * Can take over and start back up Hammond .NET User Group

--------------

## 10-05-16

* Unification
  * Sub DTOs - inherit from base DTOs
* Forest
  * Unification
  * Front-end `route/component` structure
  * Mobile team may exist in the future

--------------

## 09-21-16

* Remote constraints
  * Have clear direction on what you want to achieve on remote day
  * Not whole team on same day
  * Black Ops
* Unification
    * Uniform interface architecture
    * {Verb}{Subject}Request
    * NuGet
    * Keyboard shortcuts / IDE settings
    * Push atom settings
* How-to
    * Production Deployments (continued)

#### Done
* Simple Injector
* Octopus

--------------

## 9-07-16

* New Dev Side Project Tutorial
  * Starter: [react-slingshot](https://github.com/coryhouse/react-slingshot)
* Come together on 1 set of [linting rules](js/.eslintrc)
* Improve .gitignore / remove files from repository
* Discuss CI process
* Fix NuGet steps

--------------

## 8-24-16

* People
    * Flood requests/needs
    * Brandon Cornett
    * Designer?

* Unification
    * Simple Injector
    * Uniform interface architecture

* How-to
    * Production Deployments (continued)

#### Ambitions

* New Dev Side Project Tutorial
* Come together on 1 set of R# rules
    * Move to nightly
    * Put in Github
* Come together on 1 set of linting rules
    * Put in Github

--------------

## 8-10-16

* People
    * Matthew Puneky
    * Brandon Cornett
    * Designer?
* Projects
    * WinTake
* Tools
    * Slack
        * VSO - build vs chat
        * Github
    * CI (Turner Infrastructure status)
        * NuGet
        * Build steps
        * Resharper
* Process
    * Service account? (detaching from personal user accounts)
        * GitHub
        * Slack
    * NuGet / GitHub
        * Identify
        * Migrate
* Tech
    * Application Deploy Structure
        * FE & BE
    * Simple Injector
* How-to
    * Production Deployments

#### Ambitions

* Come together on 1 set of linting rules
    * Put in Github

--------------

## 7-27-16

* Good Bye Shawn
* GIT - status
  * CraftTrax hit snag b/c separate projects
* Working on dev [workflows](https://github.com/turner-industries/dev-dept)
* Benefits of GitHub contrib...
* Matthew Puneky - Aug 8?

#### Ambitions

* Turner Infrastructure
  * begin moving GitHub/VSO
  * setup in CI
  * make CI publish to Nuget
  * Jay to figure out how to VSO deploy to Nuget
* CI blog

-----------

## 7-13-16

* What are we talking about???
    * We need to take meeting notes
* Jay - Pair programming
    * Find things to work on with
* Tech Training
    * GIT - Document problems we hit and solutions
* Visual Studio Online
    * CI - Need docs

#### Ambitions

* Start Turner GitHub'in
* Team Learn GIT
* Team learn Webpack

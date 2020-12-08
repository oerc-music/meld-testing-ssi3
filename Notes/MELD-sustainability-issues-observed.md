# MELD sustainability issues observed

Many of these issues are interdependent...

MELD is itself a kind of platform for others to build diverse applications that run in a variety of environments, so suffers from issues of being what Brookes [1] describes as a  "Programming systems product", which (according to Brookes) requires an order of magnitude or more effort to produce (in sustainable form).  As such, the sustainability issues faced by MELD may be an extreme case of issues faced by other DH applications.

[1] Frederick P. Brooks, Jr., _The Mythical Man-Month_, 

@@TODO: for each issue mentioned, link out to more detailed discussion and recommendations.


## Overview

MELD and MELD applications are built using Javascript / ES6 / NodeJS / React / Redux.

The incorporate both browser- and server- based code.


## Issues observed

### Development environment dependencies

Tools needed to process code to runnable form (JSX, Babel, etc.)

Example: building hello-meld, testing hello-meld-1

Be clear about difference between development environment requirements and run-time requirements.

Example: `src/` and `lib/` for node/React apps?


### Run-time environment dependencies

JS version

Node version

Library dependencies

Example: running selectable-score example

Use of obsolete visualization libraries:  where possible isolate dependencies behind a common API, so that support libraries can be replaced without affecting multiple applications (and tests) that use them.

Example: MELD media player (?)


### Testing

Lack of runnable tests with clear pass/fail indications.

(NOTE: automated tests are preferred, but even documented manual tests are better than nothing.)

Example: refactoring graph traversal code.


### (Lack of) Continuous integration

Bugs are easiest to fix when they are detected soon after they are introduced.  CI helps with this, but in turn depends on testing.

Example: refactoring graph traversal code.


### Design for testing

Automated tests are most useful when they can in a simple, easily replicated environment.  Many test frameworks exist that meet this need, but often at the cost of imposing constraints on the code that can be tested.

E.g. testing React components, but not applications.

Example: testing hello-meld-1


### Application code version management

In a dynamic environment, it is very easy to end up with an application that depends on a version of one or more libraries that is not under active development.

Example: many examples seen with MELD.  The MELD dev-meld-2.0 branch was dependent on deprecated modules and an older node version before it was even released.  And many applications


### Documentation

Documentation can be a 2-edged sword.  Out of date documentation can be confusing.  But enough documentation needs to be provided to get a new user up-and-running.

Documentation for setting up a development/test environment is also important.

Simple runnable examples and scripts can often serve also as documentation.

Examples: on first attempt, I was unable to get the selectable-score app running.  Turns out it would only work out-of-the-box on an older version of node.


### Software platform complexity

Building on a complex software platform can bring great benefits, by reducing the amount of specialized code that an application needs.  But the platforms themselves bring their own complexities, including time to learn, development- and run-time dependencies, assumptions, and more.

Example: NodeJS /React / Redux environment takes a while to get the hang of.

Very simple examples can help.

Example: hello-meld-1 (?)


### Code complexity: understanding the codebase

Understanding even a simple application can be difficult if the running environment is unfamiliar.  A trivial example, in the style of "Hello World", can be veruy useful as the number of unfamiliar parts is reduced to a minimum.

Example: selectable-score app running on NodeJS / React / Redux

Also: remove any un-needed dependencies.


### Data preparation

Preparing data to be used by an app can be as challenging as using/maintaining the app itself.

Example: creating an MEI file for a score created using MuseScore, or other tool.

Example: creating graph data to link audio data to MEI data

Consider:

Tool chain documentation/examples

Test fixtures

Scripts


### Supporting tools

What tools are available to support use of a system or application?

Consider creating simple/stand-alone tools to assist with data preparationb?

Example: meld-cli-tool

Example: Verovio web site


### Monoliths

Monoliths can be a big source of confusion, as it can be difficult to tell which partys of an application are connected to which functionality, especially when built using an unfamiliar run-time environment.

Example: selectable-score app built with MELD built with NodeJS / React / Redux

Very simple, minimal, incremental examples can help new developers to see how the pieces fit together, and provide a useful basis for tutorial materials.

NOTE: this need not require much additional effort:  if an application is built incrementally, keep copies of each runnable version, especially early in the development process.



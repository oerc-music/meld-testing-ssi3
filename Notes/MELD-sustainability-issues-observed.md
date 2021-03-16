# MELD sustainability issues observed

Many of these issues are interdependent...

MELD is itself a kind of platform for others to build diverse applications that run in a variety of environments, so suffers from issues of being what Brookes [1] describes as a  "Programming systems product", which (according to Brookes) requires an order of magnitude or more effort to produce (in sustainable form).  As such, the sustainability issues faced by MELD may be an extreme case of issues faced by other DH applications.

[1] Frederick P. Brooks, Jr., _The Mythical Man-Month_, 

@@TODO: for each issue mentioned, link out to more detailed discussion and recommendations.

I've tried to record here all issues encountered when trying to build and run MELD applications.  Some of them are general programming issues rather than specifically sustainability concerns.


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


### Consistent build environment

Some difficult problems with MELD were tracked down to older versions of Node/JS libraries sometimes getting picked up despite explicit version constraints in the `package.json` files.  I don't know why this was happening, but it meant that tests might pass or fail without anything visible being changed.


### Application code version management

In a dynamic environment, it is very easy to end up with an application that depends on a version of one or more libraries that is not under active development.

Example: many examples seen with MELD.  The MELD dev-meld-2.0 branch was dependent on deprecated modules and an older node version before it was even released.  And many applications


### React multiple versions

This is a problem specific to React.  If multiple copies of React are included in a  build, which can happen easily if React is mentioned as a dependency in a library and also in a calling app, the resulting code falls foul of a React Hook rules problem, and fails.  This adds an additional layer of complexity to the problem of code and dependency version management.

The problem is exacerbated by allowing library versions used to become out of date.  The `node` ecosystem rather encourages each module to be tied to a restricted version of a module on which it depends.  When different modules have different version dependencies, multiple copies of the dependee module are typically installed; when this module is `reach`, the application subsequently fails with a hook rule violation.

Working around this problem to get an application working requires playing with the installed libraries in undocumented fashion, which causes failures when trying to run code in a testing environment.  By now, we're seeing a series of cascading problems and workarounds that become very difficult to understand and manage in a sustainable way.


### Project management

It is quite important for a collaborative project to have a governance structure so that all parties know what they can do and what level of stability to expect.  The governance structure need not be complex (e.g. @@ link to MELD governance).

But along with this, try to use existing system affordances to support process to the maximum extent possible.  Such as github pull requests and review structures.


### Documentation

Documentation can be a 2-edged sword.  Out of date documentation can be confusing.  But enough documentation needs to be provided to get a new user up-and-running.

Simple runnable examples and scripts can often serve also as documentation.

Examples: on first attempt, I was unable to get the selectable-score app running.  Turns out it would only work out-of-the-box on an older version of node.


### Operating system environment and command shell differences

The MacOS operating system changed the default shell environment in its "Catalina" release from `bash` to `zsh`.  For existing users, the shell is not affected, but any new users will default to a different shell, which means that some commands may not work as expected.

The broader lesson here is to be aware of different operating system environments and, as far as practical, design scripts and commands to work with all variants, or be specific about the expected operating system environment to be used - this is especially important in areas such as instructions for getting started with a package, such as may be found in a README or similar file.


### Filename case variations

File systems on MacOS (and Windows?) are presented as being case insensitive, where on Linux and Unix they are case sensitive.  This can lead to some strange inconsistencies, especially when working with git and GitGub.  Even when tools treat filenames as case insensitive, the underlying file system still Odd

Filenames in GitHub are case sensitive, and development environments (such as node, etc.) often require filenames to be presented with a particular combination of upper and lower case.  A simple file rename may fail if the host system is trying to reat filenames as case insensitive.

The kind of problems this can throw up should be caught by Continuous Integration.  Otherwise, the important lesson for sustainability is to pay attention to filename case, and especially don't depend on having different files ehose names differ only in the letter case used.


### Document development environment setup

Documentation for setting up a development/test environment is also important.  

Example: could not get Hello MELD app to run using a local, modified copy of meld-clients-core.   The same app was working when built using the published meld-clients-core library.  Whatever I do, there seems to be some dependency missing.

[[Say npm link in your local m-c-c and then npm link meld-clients-core in the app  Unfortunately, this doesn't seem to work when using test environments like Mocha/Jest]]


### Software platform complexity

Building on a complex software platform can bring great benefits, by reducing the amount of specialized code that an application needs.  But the platforms themselves bring their own complexities, including time to learn, development- and run-time dependencies, assumptions, and more.

Example: NodeJS /React / Redux environment takes a while to get the hang of.

Very simple examples can help.

Example: hello-meld-1 (?)


### Code complexity: understanding the codebase

Understanding even a simple application can be difficult if the running environment is unfamiliar.  A trivial example, in the style of "Hello World", can be very useful as the number of unfamiliar parts is reduced to a minimum.

Example: selectable-score app running on NodeJS / React / Redux

Also: remove any un-needed dependencies.


### Defensive coding: expect and report errors

I've encountered situations where a feature is not implemented, but may fail silently if encountered in a running app.  In such cases, it is better to fail noisily than silently - e.g. add a log message or assertion, with enough infrmation to identify the point of failure.

More generally, if a failure or unexpected exception occurs, don't ignore it silently, but generate error messages that will help future developers to know that a problem exists and hopefully to understand where it is coming from.

Related to this, if you are calling an external function that expects some conditions to be satisfied, it may help to check those conditions beforehand as it is oftemn popssible to produce more meaningful diagnostics in tjis way.


Example: Unimplemented features

Example: graph traversal encounters non-JSON RDF resource.


### Governance: process for updating core code

Simple outline of process for adding updates to the main branch.  Need not be complex, but there's an implied governance model here - should that be explicit?

Example: PR for updating graph traversal code; not sure when it's OK to merge; updates to main branch creating conflicts while PR is being developed; how to resolve PR conflicts


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

Monoliths can be a big source of confusion, as it can be difficult to tell which parts of an application are connected to which functionality, especially when built using an unfamiliar run-time environment.

Example: selectable-score app built with MELD built with NodeJS / React / Redux

Very simple, minimal, incremental examples can help new developers to see how the pieces fit together, and provide a useful basis for tutorial materials.

NOTE: this need not require much additional effort:  if an application is built incrementally, keep copies of each runnable version, especially early in the development process.


### Application architecture

(Affect of architecture design choices on testability.)

(Possible recommendation: expose more through network interface?)



### Technical debt

Example: applications not updated to use latest MELD library version.

Example: deprecated environment and/or library dependencies allowed to accumulate.


....

NOTE: warnings about node package versions.

NOTE: By end of project need to articulate an approach (or approaches) to avoiding deprecated library dependencies, as part of SSI3 activity.  Also node version dependencies. (see next)  Articulate problems of technical debt?   Note maintenance load from building on new, fast-moving projects.  Isolate dependencies behind API.  Use MELD as example...

NOTE: for MELD, raise this early.  E.g. raise an issue against meld-client-core.  Also suggest possible solutions? (incl CI.)

Unnecessary dependencies



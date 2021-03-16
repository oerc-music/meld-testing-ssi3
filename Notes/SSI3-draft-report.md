# Sustainability for Digital Humanities research software

# Summary of conclusions

@@ distil out key observations and recommendations, with references to more detailed discussion

@@ 5-7 point checklist for start of project?
@@@@notes?
(Social)
- governance?
(development support environment)
- code management support
- language/tools/platform/framework choice - is they paying their way?
- automatic setup for development/testing environment, isolated from other system components
(architecture)
- components?   Can complex elements be separated?
- how to test?   Establish testing early on
@@@@


# Introduction

The SSI3 "Software Sustainability for Digital Humanities" activity aims to explore software sustainability, with a focus on testing, using the MELD framework as an exemplar.  We aim to provide and document concrete sustainability benefits for the Music Encoding and Linked Data (MELD) project, and suggest how other Digital Humanities (DH) projects might realize similar benefits.

Sustainability is a perennial problem for research software.  The funding of research projects means that there is often little or no resource available beyond the lifetime of a project to keep the software outputs running and available.   When such outputs continue to be available, it is often by virtue of the enthusiasm and dedication of individual researchers, rather than a wider framework in which sustainablity is expected and facilitated.

These concerns are particularly acute with many Digital Humanities (DH) projects.  Funding for such projects is often very thin, and software is often not the most significant academic output.  Yet, increasingly, DH research depends on drawing information from multiple sources, often created by diverse projects, so considerations of sustainability become more important for continued progress of research.

In this activity, through hands-on experience of looking at sustainability of the MELD framework, we aim to identify areas which give rise to sustainability problems, and steps towards sustainability that can be adopted even by projects operating with shoestring software development resources.


## Abbreviations and technical terms used

- DH - Digital Humanities (research)

- MELD - Music Encoding and Linked Data: project and software framework

- SSI - Software Sustainability Institute: see https://www.software.ac.uk/about

- SSI3 - EPSRC-funded phase 3 activity of SSI: see https://gtr.ukri.org/projects?ref=EP%2FS021779%2F1


## What is sustainability?

Oddly, I couldn't find any discussion of this on the [SSI web site]( https://www.software.ac.uk).  There is discussion of software sustainability  but it's hard to find a concrete definition of what it means.  It has been described thus: "Software sustainability covers a broad range of concepts, related to both environmental sustainability, and the longevity of a codebase." [1].

[1] https://arxiv.org/pdf/1903.06039.pdf

For the purposes of this activity, I take "codebase" to extend to data that may underpin research results.  Indeed, longevity of research data may be more important than longevity of any particular piece of code that creates or processes it.

In theory, when a codebase and all its environment dependencies are frozen, the resulting system will keep running indefinitely.  In practice, there are many forces that work against this ideal: one of which is the need to apply a never-ending stream if security updates to any system that is accessible from the public Internet (or a large private intranet).  Over time, the underlying system can change to the point that any software running on it also needs to be updated in order to keep running.

Other disruptive forces include hardware failures that necessitate reinstallation of the entire system.  Even if a complete copy of all the installed software is available, it is possible that there are underlying changes in any replacement hardware that require the operating system to be upgraded, which in turn may be incompatible with the application software.

Similar problems arise when running on virtualized and/or cloud software.  My experience is that a software system can be kept running for up to 5-10 years without being updated, and sometimes less.  For a longer active deployment life, an active process to keep all software components current may be required.  In general, it is easier to upgrade in several small increments rather than wait for a forced upgrade and then have to deal with multiple incompatibilities that may interact in subtle ways.

Finally, upgrading is much easier when there is an extensive test suite in place.  A test suite can help to pinpoint any failures that occur as a result of an upgrade, which is often the biggest problem when it comnes to fixing them.


## Digital Humanities research software

There are some characteristics of DH research software that can make its sustainability a different proposition than for, say, commercial or scientific software.  This may be in the nature of the task the DH software tries to accomplish, or may be the environment in which it is created and used.

Tasks:

- data intensive (as opposed to computation intensive)
- heterogeneous data
- diverse data sources
- non-definitive data - needs context and interpretation
- possibly contradictory data
- supporting human researchers

Environment:

- shoestring funding
- short-term funding (e.g. months rather than years)
- unclear requirements at outset
- written by software non-experts 

Of course, not all DH software is just like this, and some scientific software shares some of these characteristics, but on balance it is my experience that these characteristics are more typical of DH software.

A particular consequence of then environment in which DH software is created is that it may be not possible or unrealistic to fully employ software engineering best practices.  Some particular functionality may be required quickly to support a particular scholarly output, with no immediate requirement or additional resource to ensure sustainability of the software.

Because of the value in DH of capturing context and combining a wide range of data sources, it might be argued that sustainability is even more important for DH software than it is for, say, scientific software.  Once a computed scientific result has been established, there may be limited value in revisiting it later. but humanities research tends (by its nature?) to gain value through the accumulation of understanding and perspectives over time, and as such, access to and integration with previously accumulated data can greatly increase the value accrued by investing in sustainability for DH software, or at least the data that it generates.

But when considering creation of sustainable software for DH, there is a risk of creating inflexible monoliths.  Most software systems tend to ossify over time, becoming harder to adapt and evolve as early design choices become more pervasive though the system implementation.

The previous paragraphs point to a related sustainability issue:  software vs data.  Sustainability is often focused on software programs, and less on the data they manipulate.  Yet it is often the case that the real value in both scientific and humanities software-based research project is in the data.  The scientific community (notably life sciences, astrophysics) have responded by creating a number of domain-specific data repositories to facilitate data sharing.  There are similar initiatives for humanities, but diversity and heterogeneity of subjects and data mean that central repositories with broad utility are challenging to create.

Following a trail blazed by the bioinformatics community, humanities researchers have started looking to linked data and knowledge graph technologies to create sustainable interoperable data.  But even when using standard data models and formats, divergent or inconsistent use of vocabularies (ontologies) can create barriers to re-use.  And maintenance (updating and correction) of substantial datasets can require significant resource.


# MELD: case study

## MELD background

Music notation expresses performance instructions in a way commonly understood by musicians, but is limited to encoding static, a priori knowledge.  Music Encoding and Linked Data (MELD) [@@ref https://ora.ox.ac.uk/objects/uuid:945287f6-5dd3-4424-940c-b919b8ad2768] is a basis for communicating static and dynamic information between musicians, musicologists and others.  MELD is essentially a framework for distributed real-time annotation of digital music scores.  MELD users and software agents create annotations of semantically distinguishable music concepts and relationships. These are associated with musical structure specified by the Music Encoding Initiative schema (MEI).  The use of standard Linked Data [@@RDF] and Web Annotations [@@] allows incorporation of further knowledge from non-musical sources.

The MELD framework isn't entirely typical of the DH software characterization outlined above.  It was conceived from early in its lifetime as a framework that could be used in support of a range of music research applications.  The initial development was conducted under the aegis of a large multi-centre multi-year engineering project.  Yet it does still suffer from some of the environmental characteristics described:  collaboration with other researchers would give rise to requirements that had to be addressed very rapidly without time to consider sustainability issues.  And while the MELD framework itself benefited from the substantial efforts of a large research project, the specific applications under review have been created within the more limited resourcing of humanities projects.

The MELD framework has a number of components, two of which are:

1. [`meld-clients-core`](https://github.com/oerc-music/meld-clients-core): support code for MELD client (browser) applications based on the Javascript React framework.  This is a powerful, popular and widely used framework,m created by Facebook, for implementing Web browser applications.  But with great power comes considerable complexity, and becoming conversant with React can take some considerable effort even for someone already familiar with Javascript.  Further, React makes heavy use of modern Javascript capabilities and patterns that are a considerable departure from traditional programming models.

2. [`meld-web-services`](https://github.com/oerc-music/meld-web-services): a set of web services to support MELD client applications.  Historically, these were custom code to support sessions and annotations, but much of this is being replaced by off-the-shelf [Linked Data Platform (LDP)](https://www.w3.org/TR/ldp/) servers (e.g. [GOLD](https://github.com/linkeddata/gold), and more recently using various deployments of [Solid](https://github.com/solid/specification/).


## Approach

The original intent of this project was to focus on testing the web (HTTP) interfaces between project-specific and generic back-end storage components, using existing work on [SOFA and MELD](https://github.com/oerc-music/nin-remixer-public/blob/master/notes/SOFA-architecture-notes.md) as a starting point.  But, delving into the software, we found that much of the essential logic to be tested was exposed through API calls within browser applications.  This required a rethink of the approach, which has led to us facing several hard lessons about software sustainability.

On closer examination, the MELD functionality in the applications we surveyed for this project, [Lohengrin forbidden question study](https://github.com/oerc-music/ForbiddenQuestion) and [Delius annotation](https://github.com/oerc-music/delius-annotation), was mainly visible only to internal API calls, so a different testing approach was needed.


### Initial plan

The original proposal outline was as follows:

Proposal: (An exemplar of) Best practice for testing in DH software development

- Realised as a testing framework for MELD.
- Building upon command line MELD client developed in the later stages of FAST.
- Extending this as a unit testing approach for MELD app development.
- We have a small but varied ecosystem of music studies and their associated software the testing framework could be validated against:
    - Lohengrin opera analysis;
    - Delius live performance annotation;
    - Historical musicology with mixed media digital archives (Text, audio, image, score from British Library, New York Philharmonic Archives);
    - TROMPA Rehearsal companion.
- Also as a reflective case study of software sustainability in DH projects.  While the MELD framework itself benefited from the substantial research efforts of FAST, the apps above have been created within the more limited resourcing of humanities projects.  This is an opportunity for SSI to reflect on effective software sustainability practice in such situations, which are typical for DH.
- Outputs
    - The software testing framework for MELD (public repo).
    - Unit tests for (some/all of) the above studies/apps (public repo).
    - Documentation and a best practice report (public, online).


### What we actually did

It was revealed early in the project that HTTP-level testing using a command line tool wasn't going to provide sufficient access to exercise key application logic for the Lohengrin and Delius applications.  To test the internal interfaces, we wrote testing code in the applications' implementation language (Javascript), using existing test frameworks.  This required a greater familiarity with the implementation of applications using MELD, and the React framework ecosystem on which the MELD client-side code is built.  In turn, this required us to more immediately address a number of sustainability issues in the MELD codebase.

The main consequences of this revised approach was a deviation from the original plan, with the creation of a series of simple "Hello MELD" applications, designed with testability in mind.  Implementing these applications has yielded several lessons in creating sustainable MELD applications, and also some re-evaluation of the attainable goals of this short project.

Revised goals:

- dropped (for now) the goal of testing at the HTTP interface
- implemented a series of simple "Hello MELD" applications, designed with testability in mind, used for both learning and testing
- implementing some simple tests using existing Javascript test frameworks (Mocha and Jest were trialled, and settled on Jest as it integrates more easily with React).
- shift from testing as lead activity, to recording and documenting observed sustainability issues with the MELD libraries and applications.

As a result, the outputs of this exercise will focus less on specific implementation of sustainability practices, and more on describing sustainability issues and possible mitigations, than was originally envisaged.


### Observations

#### Complexity of supporting software environment

The MELD client support code is implemented using NodeJS, React and Redux.  This environment offers a rich set of features, which have allowed complex functionality to be incorporated into MELD with relatively little initial software development effort.  

But it is a complex, dynamic environment with many supporting tools.  This gives rise to a complex web of dependencies, some of which may be frequently changing.  This can give rise to incompatibilities and failures when the MELD software itself, and any supporting tools used, are not kept up-to-date with the latest React/Redux library modules.  Many of the updates to underlying software are to address security vulnerabilities, so are important to incorporate into a public-facing web application.

#### Undetected problems in MELD code

There were a number of areas where the MELD support code did not behave in ways that were expected.  This would cause problems with existing applications, which might work by virtue of unnoticed changes made to the runtime environment.   When encountered, these errors could be hard to track down and fix.

These are problems that automated tests, and especially continuous integration, are intended to catch.  The "Helo MELD" applications were helpful when it came to isolating and fixing such failures.

Effective testing is a cornerstone of software sustainability.

Retro-fitting tests to existing applications has proved to be challenging, for reasons that range from setting up a test environment that sufficiently mimics the application environment, to accessing internal interfaces that expose application logic to be tested.

#### Browser vs non-browser code

In theory, Javascript code is highly portable, possible to run in a browser, desktop or server computer.  This is very appealing for testing, as it can be tricky to run automated tests in a browser, and continuous integraton platforms are generally server-based.

In practice, there are a number of significant differences: Javascript language variants, run-time support, module loading mechanisms, and more.  We ran into several cases of code that would run as expected in a browser, but would fail when run in a non-browser test environment, due to differences in the ways in which modules were loaded.

The original intent to test the HTTP interface was dependent on a significant amount of application logic being implemented on a server.  When this is not not the case, effective testing requires that it uses code-level APIs, with test code directly invoking APIs in a way that that resembles application code.

Setting up even a very simple test environment proved to be fraught with difficulties.  Theoretically, the testing framework would paper over any differences from the browser environment.  In practice, there were areas where differences would persist, and complex workarounds were adopted to avoid provoking these.

#### Hello MELD series

The  applications were extremely helpful, both for learning the framework and when bringing a new developer onboard, as it allowed the various parts of a complex set of interfaces to be mastered separately.  The "Hello MELD" series also provided fallback options when failures were experienced, allowing code-induced and environment-induced problems to be identified and separated.

It was important that the first of these "Hello MELD" applications was so simple that one might easily think that there was nothing to go wrong.  In practice, things did go wrong, and even a trivially simple application served the important function of proving that a minimum set of runtime requirements had been set up correctly.  Having a trivial application provided a useful platform for exploring testing tools and frameworks without getting bogged down in application code minutiae, and allowed easy experimentation with application design options (using established MELD patterns) that would facilitate testing. 


# Sustainability issues encountered

This section aims to highlight key issues encountered.  Raw observations and notes are contained in a separate document:  MELD-sustainability-issues-observed.md

## Working with a complex and dynamic software ecosystem

We ran into a number of compatibility problems while working with MELD on recent versions of NodeJS, React and Redux.  Attempting to update dependencies to later versions gave rise to failures that proved quite difficult to identify and remedy, and required changes to the MELD software.  Further, MELD itself is undergoing several changes in response to new application requirements, raising further library compatibility concerns.

Keeping application and support code up-to-date with a complex evolving environment like React/Redux requires significant ongoing engineering effort, which can be challenging for a small research project.  This is especially true if the development is split over several research projects that don't necessarily have continuously available development effort - the wider world doesn't wait, and fragmented research activities are easily left behind.

### Mitigations

- before adopting a complex support platform, consider whether the value provided (in terms of easy-to-use features) justifies the additional complexity and maintenance requirements.  (The consensus for MELD is that the features offered enabled results that would otherwise have probably not been achieved.)

- when adopting a complex support platform, consider a strategy or practice to ensure that the underlying dependencies can be safely updated. Ideally, this will involve having some form of automated testing and continuous integration in place.  Try to keep dependencies up-to-date as you go, rather than relying on infrequent updates of multiple dependencies.

- consider how to create a complete snapshot, including all supporting components, of a working version of an application.  For example, as a Docker image?  (Bear in mind that for a web based application, there remains a possible dependency on the browser used.)

## Inconsistent build and runtime environments

We found different developers were using different versions of the runtime environment and build tools.  This led to applications that worked in some environments and failed in others.

Some of the build tools used were not always reliable.  For example, Node Package Manager (npm) is supposed to record package versions used, and thus ensure consistency.  In practice, we found that differences between different development environments were not being corrected, leading to inconsistencies noted.  

Clear identification of supporting build tools used was lacking.  The Javascript/React/Redux environment used in particular relies on a range of tools to compile code to a form that can be run in the base runtime environment.  The tools needed are themselves somewhat dependent on the NodeJS and/or web browser versions used to run the application code.

### Mitigations

- create automated scripts to create clean runtime and development environments from scratch.  Such scripts also serve usefully as documentation.

- establish a continuous integration environment:  this forces the environment to be rebuilt from scratch (or from established and documented resources) whenever the software is updated.

- maintain development/test envi=ronments separately from other aspects of the host system (e.g,. using tools like Node's `nvm` or Python's `venv`).  Provide detailed instructions and/or automated script for setting up a new development/test environment, or resetting an existing one.

## Lack of automated testing and continuous integration

Although MELD has a simple test application, there was no automated test suite of continuous integration set up.  (Continuous integration is used here to mean an environment that automatically installs the software into a clean environment, runs the tests and reports any failures whenever any changes are made to the software.  This allows problems to be detected rapidly, hence easier to fix because the changes made are still fresh in a developer's mind.)

The lack has resulted in uncertainty about the status of the software.  Attempts to install the simple test applications resulted in failures that would probably have been picked up by automated tests and continuous integration.

(Not yet covered: something about setting up test fixtures and/or mocking.)

### Mitigations

- establish an automated testing regime at the outset of a project, or very early in its lifetime.  Much of the value of automated testing can be realized even by very lightweight tests - 90%+ test coverage is great of you can afford it, but even very minimal coverage can provide valuable proof of life, and can be quick to implement.  In practice, once a testing regime is in place, I find it tends to collect coverage-improving tests over time  with very little additional effort.  Expect automated testing to pay back any initial investment of effort within a few weeks of development activity.

- if it can be done quickly and easily, set up continuous integration early in the project.  A prerequisite for this is some form of automated testing, even if it only to ensure that system can be built and installed.  In practice, I've have found that having easy-to-run tests can be almost as valuable (but may be less effective for picking up installation and environment setup problems).

- consider testability when designing code: ensure that important features are easy to invoke and access for testing.

- treat tests as an important source of documentation.

## Application code version management issues

The MELD libraries are being used by different developers in different institutions, who also apply fixes and updates to the libraries themselves.  This has meant that different branches of the MELD code being used in different applications over extended periods of time, exacerbating the consistency issues noted previously.

There was some lack of clarity about the code governance (e.g. the exact requirements for updating the reference version of the software that should be the basis of all new applications (and preferably any that are actively being developed).

### Mitigations

- establish a governance structure early in the project.  It doesn't need to be complex: one of it's main purposes is to let all developers know what is expected in order to update the main codebase.

- try to make use of available affordances to automate governance constraints; e.g. using GitHub reviewer requirements for merging a pull request.


## Application code complexity

The MELD support libraries, and especially the underlying React/Redux framework, present a rich and flexible interface, which in turn forces a degree of complexity onto the application code that uses them.  This has meant that applications are initially difficult for new developers to understand.  We found that, in practice, it took a significant amount of hand-holding from an existing MELD application developer to bring a new developer up to speed using the MELD framework.

### Mitigations

There's no silver bullet for solving this - it's a perennial software problem.  But some things to consider might be:

- create some very simple, easy-to-understand applications/instances with minimal functionality.  These can provide a gentle onramp, helping developers to understand which aspects of a platform or system are providing what kind of functionality (and which aspects might be ignored).

- try to conceive an application as independent modules with separate concerns.  An example of this approach is Unix command line tools that can be used alone, or combined in various ways.   It's not always easy or possible for web applications, but we had some success with SOFA (an earlier MELD application) using Solid (LDP) for data storage, and chainable command line tools for various features.

- don't let technical debt accumulate.

## Run time error detection and reporting

Run time error detection  and reporting in the MELD libraries, and sometimes in the underlying React/Redux libraries, was very patchy.  This would result in applications failing for no apparent reason, or failing for indicated reasons that have no discernible connection with what the application is trying to do.  This can make problem diagnosis and rectification difficult and time-consuming.

### Mitigations

Mitigations here are common software coder practices.

- don't fail silently.  If something is wrong, or if an option is not (yet) implemented, report it.  Such reports don't have to be pretty, just obvious.  If possible, they should refer to application data and concepts, not internal software concepts.

- code defensively!  Don't assume that certain inputs can't happen:  check them and generate a report (or crash with a stack dump) if they aren't satisfied.

- provide debug options to report inputs received, to help identify where any problem might be occurring.

## Data preparation

MELD applications, in common with many Digital Humanities applications, typically work with existing data, which may be created using other tools (e.g. music files), or extracted from existing public resources (e.g. Wikidata).  For an application to be useful, or testable, suitable input data must be available.

The MELD framework does provide data for testing and demonstration use, but it wasn't always easy to find.

For creating a simple stripped-down application, we needed to create a new dataset using tools that are not part of the MNELD framework.  There MELD documentation did not contain pointers to easy-to-use options for creating such data, and this process alone took a few days of investigation and experimentation.

### Mitigations

- for testing, provide ready-to-use datasets.  Ideally. they should be as simple as possible for the feature they are intended to test.

- for users, provide sample instructions for creating input data.  For example, in the case of MELD, an MEI file can be created MuseScore to create a musical score, exported as MusicXML and then use the Verovio web service to convert this into MEI (e.g. as described for [`hello-meld-2`](https://github.com/oerc-music/meld-hello-meld/tree/main/hello-meld-2#use-verovio-to-generate-mei-from-musescore-musicxml))

- provide pointers to file format and vocabulary term descrioptions.

## Accumulated technical debt

This isn't really a separate issue, but a consequence of the other factors reported above.

Technical debt refers here to software additions and patches that are used to get some software running, but which don't represent an optimal way of solving the underlying problem.  It often results in code that has duplicated and/or tortured logic, and fails to maintain a clean separation of concerns between software elements.

When developing software under time-and-resource constraints, it is often expedient to take short-cuts to validate an approach, or to demonstrate a result.  Individually, these shortcuts are often not problematic, but if they are allowed to accumulate in a codebase the resulting software can quickly become harder to understand and update.  And faced with with such difficulty, a developer will often use additional shortcuts or workarounds to avoid having to deal with the difficult code.  In this way, the accumulation of technical debt can appear to be exponential growth, and speed of making changes and fixes will decay accordingly.

### Mitigations

Again, no magic bullets here.  It's mostly common software engineering practice and skills.

- be aware of technical debt; keep notes of where it exists, and ideas for eliminating it.  When a change turns out to be more difficult than it should be, that may be an indication that the code should be refined.

- don't let too much technical debt accumulate.

    How much is too much?  This is hard to say - it's also possible to spend too much time refining code.  A possible rule of thumb is this:  when a piece of code needs to be modified, first see if there are any changes that make the desired changes easier to apply while preserving its existing functionality.

- try to maintain separation of concerns between software components

- try to avoid duplicated logic (also known as DRY: "don't repeat yourself")


# Strategies for sustainability

@@@maybe...

- small pieces, loosely coupled (e.g. free-standing command line tools)

- design for visibility of intermediate results for testability

- partial, minimal testing

- small increments; save working versions (including basic instructions to reproduce)

- when? include sustainability considerations when re-using

- avoid complex UIs if possible?

- separate presentation from processing and retrieval

- carefully consider what using complex support frameworks actually provides


# Discussion



## Resourcing

@@ Note Kevin's comment about value of occasional S/W expert input

- [ ] (from 2021-01-05): to SSI report outline:  discuss DH project structures (resource constrained; discussion to have).  Dig out old notes and circulate?  Start collecting notes about project issues.  Quantify how much effort is required for various sustainability activities.
    - Recognize the difference between long-term infra goals vs immediate requirements (cf. Brooks reference).
    - Combined resourcing of research and service support (e.g,. researcher and library, etc.)  Scholar/institutional aspects.  Failure of linked art 3 funding app.  Research questions driving tech development?  RQs as "predictions"?   Also, science vs creative.
    - Ethnographic study on DH research for wishlist?
    - DH project characterization
        - resource constrained; no budget for pure engineering concerns
        - combining engineering and scholarship?
        - tech people working with non-tech people 
            - collaboration models
        - methodological issues (enginbeers and scholars having different perspectives)  
        - technical vs scholarly requirements (MusicXML vs MEI).





# Further work for this activity?

- More on testing

- Performance evaluation and tuning

- @@ Ethnographic study on DH research for wishlist?




----


# Implementation notes

Note: MELD is implemented using node, React and Redux, so the implementation and tools used are generally specific to this platform.

## Developer onboarding: Hello MELD examples

@@


## Testing

@@ React/redux components (browser client UI generation)

@@ Data management (non-browser client API)

@@



Kevin:

My initial questions, which would be very helpful to inform that discussion if you got a chance beforehand: 

1) what is the best test practice, and/or the most widely used testing framework, for node/react development? It would be helpful if you could survey this (David and I won't know). 

(While I wouldn't entire put it past the Facebooks of this world to not be using anything, my instinct is to assume they must be using *something* for testing) 

2) If there were a solution to move MELD to use such a framework (assuming one exists. perhaps mocha or jest?), how much effort would it be? What changes to MELD would be required? 
 I think the below means you have an answer to Q1. I don't feel I have clear answers to Q2, so in that sense I don't think it's moot? So Q2 still stands; but to put it a different way, are you proposing: 

- we ought to be using Jest, but there are problems with how it interacts with MELD as-is? If so I don't fully understand where the problem lies (beyond the libraries we use??). Or what you're proposing we'd need to do to overcome this? 

or 

- moving MELD to a sustainable footing is impossible, because it's impossible to build a test framework for it? (which intuitively leads back to the previous option?  depending on how absolute impossible is?!?) 

I agree your findings so far indicate it may be a time to review next steps. If you could have a couple of alternative proposals for those next steps, along with pros/cons, that would be great! (that's pretty much what I'm trying to elicit from through those q). 






## Continuous integration

@@

## Performance

@@ especially, profile to help identify bottlenecks; e.g. graph traversal

## Technical Debt

@@ accumulated tech debt from a series of small projects...

@@ multiple projects with different priorities forcing different development branches


## Governance

- [ ] (from 2021-01-05): add reference to governance/update process to sustainability notes
    - need to discuss in the context of DH research project constraints
    - [x] Note added in `MELD-sustainability-issues-observed.md`
    - [x] Check out governance document from MELD.
        - https://docs.google.com/document/d/1_ZBXb0QvDTauPmw1X_5w1vcHqtX6t7sm_BzbeZb6_uc/edit#heading=h.j3fekq3jzeiy
        - where is the list of maintained apps and branches.
            - should dependencies be noted?
        - I don't entirely understand the bit about "must also apply it to all maintained branches"
        - Plan to make minor process update to this - e.g. use GitHub review request.
    - [ ] Also, make ref to older OSS Watch governance recommendations?






## @@others?


on reporting - project report vs advice and guidance ("practice led study") - reflections on MELD as case study - more than characterize problem - also add recommendations
(e.g. "be aware, think about at outset")

DL: "Hello MELD as method of "onboarding" very effective" 

KP: interactions between experience programmer working with digital humanist, in small quantities spread over time, can have a massive multiplier effect.

KP: capture different ways of looking at s/w (HTTP testing vs internal API testing)

KP: try and situate the report against the Lohengrin/Delius app.  E.g., how would testing approach would work/apply to existing app.

KP: different testing approaches - what is effect on technical debt?

DL: noted the value of paying down tech debt away from pressures of immediate project (e.g. SSI3 work for MELD).

KP: in framing report - think about what would be useful for a project like TanC or BitH, and offer recommendations that work towards that.



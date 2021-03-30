# Sustainability for Digital Humanities research software

@@ When initial draft is done, circulate draft to DDeR to ask about definition of sustainability, and more

@@ Add commentary that emphasizes and picks out personal observations.  Use italics?

@@


## Table of contents

- [Sustainability for Digital Humanities research software](#sustainability-for-digital-humanities-research-software)
  * [Table of contents](#table-of-contents)
- [Summary of conclusions](#summary-of-conclusions)
- [1. Introduction](#1-introduction)
  * [1.1 About this report](#11-about-this-report)
  * [1.2 Abbreviations and technical terms used](#12-abbreviations-and-technical-terms-used)
  * [1.3 What is software sustainability?](#13-what-is-software-sustainability-)
  * [2. Characteristics of DH research software](#2-characteristics-of-dh-research-software)
- [3. Case study: Music Encoding and Linked Data](#3-case-study--music-encoding-and-linked-data)
  * [3.1 MELD background](#31-meld-background)
    + [3.1.1 Server and client code](#311-server-and-client-code)
  * [3.2 Approach](#32-approach)
    + [3.2.1 Initial plan](#321-initial-plan)
    + [3.2.2 What we actually did](#322-what-we-actually-did)
    + [3.2.3 Observations](#323-observations)
      - [Complexity of supporting software environment](#complexity-of-supporting-software-environment)
      - [Undetected problems in MELD code](#undetected-problems-in-meld-code)
      - [Browser vs non-browser code](#browser-vs-non-browser-code)
      - [Hello MELD series](#hello-meld-series)
      - [Value of part-time technical expertise](#value-of-part-time-technical-expertise)
- [4. Sustainability issues encountered](#4-sustainability-issues-encountered)
  * [4.1 Working with a complex and dynamic software ecosystem](#41-working-with-a-complex-and-dynamic-software-ecosystem)
    + [Suggested mitigations](#suggested-mitigations)
  * [4.2 Inconsistent build and runtime environments](#42-inconsistent-build-and-runtime-environments)
    + [Suggested mitigations](#suggested-mitigations-1)
  * [4.3 Lack of automated testing and continuous integration](#43-lack-of-automated-testing-and-continuous-integration)
    + [Suggested mitigations](#suggested-mitigations-2)
    + [Test fixtures and mocking](#test-fixtures-and-mocking)
  * [4.4 Application code version management issues](#44-application-code-version-management-issues)
    + [Suggested mitigations](#suggested-mitigations-3)
  * [4.5 Application code complexity](#45-application-code-complexity)
    + [Suggested mitigations](#suggested-mitigations-4)
  * [4.6 Run time error detection and reporting](#46-run-time-error-detection-and-reporting)
    + [Suggested mitigations](#suggested-mitigations-5)
  * [4.7 Data preparation](#47-data-preparation)
    + [Suggested mitigations](#suggested-mitigations-6)
  * [4.8 Accumulated technical debt](#48-accumulated-technical-debt)
    + [Suggested mitigations](#suggested-mitigations-7)
- [5. Lessons in sustainability for DH](#5-lessons-in-sustainability-for-dh)
  * [5.1 Resourcing](#51-resourcing)
  * [5.2 Technical activity planning](#52-technical-activity-planning)
  * [5.3 Technical design choices](#53-technical-design-choices)
- [6. Recommendations for DH software sustainability](#6-recommendations-for-dh-software-sustainability)
  * [6.1 Recommendation 1: Technical architecture](#61-recommendation-1--technical-architecture)
  * [6.2 Recommendation 2: Design for testing](#62-recommendation-2--design-for-testing)
  * [6.3 Recommendation 3: Continuous integration](#63-recommendation-3--continuous-integration)
  * [6.4 Recommendation 4: Incremental development](#64-recommendation-4--incremental-development)
  * [6.5 Recommendation 5: Dealing with technical debt](#65-recommendation-5--dealing-with-technical-debt)
  * [6.6 Recommendation 6: Keep dependencies up to date](#66-recommendation-6--keep-dependencies-up-to-date)
  * [6.7 Recommendation 7: minimal application examples](#67-recommendation-7--minimal-application-examples)
  * [6.8 Summary of conclusions](#68-summary-of-conclusions)
- [7. Further sustainability issues requiring other investigation through the use-case](#7-further-sustainability-issues-requiring-other-investigation-through-the-use-case)
- [8. Acknowledgements](#8-acknowledgements)


# Summary of conclusions

@@ distil out key observations and recommendations, with references to more detailed discussion

@@ add links to observations and discussion

@@ 5-7 point checklist for planning/start of project?
@@@@notes?
(Social)
- governance?
- access to technical experience?
(development support environment)
- code management support
- language/tools/platform/framework choice - is they paying their way?
- automatic setup for development/testing environment, isolated from other system components
(architecture)
- components?   Can complex elements be separated?
- how to test?   Establish testing early on
@@@@


# 1. Introduction

The SSI3 "Software Sustainability for Digital Humanities" activity aims to explore software sustainability, with a focus on testing, using the MELD framework as an exemplar.  We aim to provide and document concrete sustainability benefits for the Music Encoding and Linked Data (MELD) project, and suggest how other Digital Humanities (DH) projects might realize similar benefits.

Sustainability is a perennial problem for research software.  The funding of research projects means that there is often little or no resource available beyond the lifetime of a project to keep the software outputs running and available.   When such outputs continue to be available, it is often by virtue of the enthusiasm and dedication of individual researchers, rather than a wider framework in which sustainability is expected and facilitated.

These concerns are particularly acute with many Digital Humanities (DH) projects.  Funding for such projects is often very thin, and software is often not the most significant academic output.  Yet, increasingly, DH research depends on drawing information from multiple sources, often created by diverse projects, so considerations of sustainability become more important for continued progress of research.

In this activity, through hands-on experience of looking at sustainability of the MELD framework, we aim to identify areas which give rise to sustainability problems, and steps towards sustainability that can be adopted even by projects operating with shoestring software development resources.


## 1.1 About this report

This report includes objectively observed phenomena, subjective personal impressions, andf widely accepted practices about the development and maintenance of a software system.

> Software sustainability arises at the interface between technical environment within which it is performed, and the personal involvement of software developers who create and sustain the software.  As a research software engineer, I feel that it can be useful to highlight subjective issues faced by a working developer.  Such subjective views are highlighted in this document through presentation in the style of this paragraph.

The main body of the report is structured thus:

- Discussion of the characteristics of Digital Humanities research software, and how they may affect sustainability of software outputs.

- Case study observations arising from attempts to test Music Encoding and Linked Data software.  Some of the issues encountered required some revision of the original plan, and as such provide useful insights into the effect of  sustainability issues on software development and maintenance.  Some possible mitigations that are specific to the case study are suggested.

- A discussion of the lessons arising from our case study, and how they may apply more widely to other DH research.

- A distillation of recommendations and considerations for software sustainability, particularly in the context of Digital Humanities research.


## 1.2 Abbreviations and technical terms used

- DH - Digital Humanities (research)

- MELD - Music Encoding and Linked Data: project and software framework

- SSI - Software Sustainability Institute: see https://www.software.ac.uk/about

- SSI3 - EPSRC-funded phase 3 activity of SSI: see https://gtr.ukri.org/projects?ref=EP%2FS021779%2F1



## 1.3 What is software sustainability?

There is much discussion of software sustainability, but it's hard to find a consensus definition of what it means.  It has been described thus: "Software sustainability covers a broad range of concepts, related to both environmental sustainability, and the longevity of a codebase." [1].

[1] [What Makes Research Software Sustainable? An
Interview Study With Research Software Engineers](https://arxiv.org/pdf/1903.06039.pdf).

> Oddly, I couldn't find any discussion of this on the [SSI web site]( https://www.software.ac.uk).  

For the purposes of this activity, "codebase" is taken to include data that may underpin research results.  It may well be that longevity of research data is more important than longevity of the code that creates or processes it.

In theory, when a codebase and all its environment dependencies are frozen, the resulting system will keep running indefinitely.  In practice, there are many forces that work against this ideal: one of which is the need to apply a never-ending stream of security updates to any system that is accessible from the public Internet (or from a large private intranet).  Over time, the underlying system can change to the point that any software running on it also needs to be updated in order to keep running.

Other disruptive forces include hardware failures that necessitate re-installation of the entire system.  Even if a complete copy of all the installed software is available, it is possible that there are underlying changes in any replacement hardware that require the operating system to be upgraded, which in turn may be incompatible with the application software.  Similar problems arise when running on virtualized and/or cloud software.

> My experience is that a software system can be kept running for up to 5-10 years without being updated, and sometimes less.  

For a longer active deployment life, an active process to keep all software components current is likely to be required.  In general, it is easier to upgrade in several small increments rather than wait for a forced upgrade and then have to deal with multiple incompatibilities that may interact in subtle ways.  Upgrading is much easier when there is an extensive test suite in place.  A test suite can help to pinpoint any failures that occur as a result of an upgrade, which is often the biggest problem faced when trying to fix them.


## 2. Characteristics of DH research software

There are some characteristics of DH research software that can make its sustainability a different proposition than for, say, commercial or scientific software.  This may be in the nature of the task the DH software tries to accomplish, or may be the environment in which it is created and used.

Tasks:

- Digital humanities research is often data intensive (as opposed to computation intensive).
- Humanities research typically draws upon and combines diverse data sources.
- Digital humanities research data are often heterogeneous, describing a variety of subjects with variable degrees of detail.
- Valid primary data, such as historical accounts, may also be contradictory.
- Data to be presented or processed may be non-definitive, needing context and human interpretation in order to understand what they convey.
- Data are not primarily a scholarly end in themselves, but used to support analyses by human researchers.

Environment:

- Shoestring funding.  Also, short-term funding (e.g. months rather than years).
- Unclear initial software requirements - the role of software in DH may be to facilitate exploration of data with a view to forming and evaluating scholarly conclusions.  Such software may be written _ad hoc_ by researchers without any software engineering training. 
- Difficulty of obtaining funding for software maintenance that is not directly linked to new research; yet one of the reasons for sustaining software is to allow researchers without software development skills to benefit from existing developments.
- DH scholars and software engineers have diverse perspectives, all of which contribute to achieving useful results.  The requirements for long-lived research software need to address both scholarly needs and engineering constraints.
- Software development is a skill in high demand, and it is common for software developers to leave for better-funded work.  This can result in loss of knowledge that is needed to work on the software.  And software maintenance can be tedious, uninteresting work;  many developers would rather work on new software in which they have a greater sense of ownership.  Loss of developers can be mitigated by attention to good engineering practices, but may needs effort that is not costed in a research proposal.
- Scholars, who may themselves be able to write small programs for a specific purpose, sometimes fail to recognize that "You just can't do things for no money", especially when it comes to writing software to serve multiple users over an extended period of time.  The effects of a lack of engineering resources may then be perceived as lack of competence.

> Of course, not all DH software is like this, and some scientific software shares some of these characteristics, but on balance it is my experience that these characteristics are more typical of DH software.

A particular consequence of the environment in which DH software is created is that it may be not possible or unrealistic to fully employ software engineering best practices.  Some functionality may be required quickly to support a particular scholarly output, with no immediate requirement or additional resource to ensure sustainability of the software.

Because of the value in DH of capturing context and combining a wide range of data sources, it might be argued that sustainability is even more important for DH software than it is for, say, scientific software.  Once a computed scientific result has been established, there may be limited value in revisiting it later.  But humanities research tends (by its nature?) to gain value through the accumulation of understanding and perspectives over time, and as such, access to and integration with previously accumulated data can greatly increase the value accrued by investing in software sustainability, or at least sustainable access to generated data.

The foregoing points to a related sustainability issue:  software vs data.  Sustainability is often focused on software programs, and less on the data they manipulate.  Yet it is often the case that the real value in both scientific and humanities software-based research project is in the data.  The scientific community (notably life sciences, astrophysics) have responded by creating a number of domain-specific data repositories to facilitate data sharing.  There are similar initiatives for humanities, but diversity and heterogeneity of subjects and data mean that central repositories with broad utility are challenging to create.

Following a trail blazed by the bioinformatics community, humanities researchers have started looking to linked data and knowledge graph technologies to create sustainable interoperable data (@@refs: linked pasts, linked art, EMLO, etc).  But even when using standard data models and formats, divergent or inconsistent use of vocabularies (ontologies) can create barriers to re-use.  And maintenance (updating and correction) of substantial datasets can require significant resource.


# 3. Case study: Music Encoding and Linked Data

## 3.1 MELD background

Music notation expresses performance instructions in a way commonly understood by musicians, but is limited to encoding static, a priori knowledge.  Music Encoding and Linked Data (MELD) [@@ref https://ora.ox.ac.uk/objects/uuid:945287f6-5dd3-4424-940c-b919b8ad2768] is a basis for communicating static and dynamic information between musicians, musicologists and others.  MELD is essentially a framework for distributed real-time annotation of digitally encoded music notation (including, but not limited to, musical scores).  MELD users and software agents create annotations of semantically distinguishable music concepts and relationships. These are associated with musical structure specified by the Music Encoding Initiative schema (MEI).  The use of standard Linked Data [@@RDF] and Web Annotations [@@] allows incorporation of further knowledge from non-musical sources.

The MELD framework isn't entirely typical of the DH software characterization outlined above.  It was conceived from early in its lifetime as a framework that could be used in support of a range of music research applications.  The initial development was conducted and funded under the aegis of a large multi-centre multi-year engineering project, which was only later re-targeted for use in DH projects.  Yet it does still suffer from some of the environmental characteristics described:  collaboration with other researchers would give rise to requirements that had to be addressed very rapidly without time to consider sustainability issues.  And while the MELD framework itself benefited from the substantial efforts of a large research project, the specific applications under review have been created within the more limited resourcing of humanities projects.

The MELD framework has a number of components, two of which are:

1. [`meld-clients-core`](https://github.com/oerc-music/meld-clients-core): support code for MELD client (browser) applications based on the Javascript React framework.  This is a powerful, popular and widely used framework,m created by Facebook, for implementing Web browser applications.  But with great power comes considerable complexity, and becoming conversant with React can take some considerable effort even for someone already familiar with Javascript.  Further, React makes heavy use of modern Javascript capabilities and patterns that are a considerable departure from traditional programming models.

2. [`meld-web-services`](https://github.com/oerc-music/meld-web-services): a set of web services to support MELD client applications.  Historically, these were custom code to support sessions and annotations, but much of this is being replaced by off-the-shelf [Linked Data Platform (LDP)](https://www.w3.org/TR/ldp/) servers (e.g. [GOLD](https://github.com/linkeddata/gold), and more recently using various deployments of [Solid](https://github.com/solid/specification/).

### 3.1.1 Server and client code

@@ add discussion of server and client code


## 3.2 Approach

The original intent of this project was to focus on testing the web (HTTP) interfaces between project-specific and generic back-end storage components, using existing work on [SOFA and MELD](https://github.com/oerc-music/nin-remixer-public/blob/master/notes/SOFA-architecture-notes.md) as a starting point.  But, delving into the software, we found that much of the essential logic to be tested was exposed through API calls within browser applications.  This required a rethink of the approach, which has led to us facing and dealing with several specific software sustainability issues.

@@include brief summary of what SOFA is@@

@@relevance to humanities audience: make description less technical (e.g. "data that is passed between servers and web clients".  Also reference to internal browser calls?  Need to introduce "browser application".  Also mention React here -- move up to prev section)

@@who is the audience?  SSI?  Who funds SSI? Also RSEs working in DH. And material to help justify doing the work properly (use of link for educating researchers). Funders? @@

On closer examination, the MELD functionality in the applications we surveyed for this project, [Lohengrin time machine study](https://github.com/oerc-music/ForbiddenQuestion) and [Delius annotation](https://github.com/oerc-music/delius-annotation), was mainly visible only to internal API calls, so a different testing approach was needed.


### 3.2.1 Initial plan

The original plan of work was:

@@ Shorten what follows; describe not quote

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


### 3.2.2 What we actually did

@@ use similar structure to original plan

It was revealed early in the project that HTTP-level testing using a command line tool wasn't going to provide sufficient access to exercise key application logic for the Lohengrin and Delius applications.  To test the internal interfaces, we wrote testing code in the applications' implementation language (Javascript), using existing test frameworks.  This required a greater familiarity with the implementation of applications using MELD, and the React framework ecosystem on which the MELD client-side code is built.  In turn, this required us to more immediately address a number of sustainability issues in the MELD codebase.

The main consequences of this revised approach was a deviation from the original plan, with the creation of a series of simple "Hello MELD" applications, designed with testability in mind.  Implementing these applications has yielded several lessons in creating sustainable MELD applications, and also some re-evaluation of the attainable goals of this short project.

Revised goals:

- dropped (for now) the goal of testing at the HTTP interface
- implemented a series of simple "Hello MELD" applications, designed with testability in mind, and used for both learning and testing
- implementing some simple tests using existing Javascript test frameworks (Mocha and Jest were trialled, and settled on Jest as it integrates more easily with React).
- a partial shift from testing as lead activity, to recording and documenting observed sustainability issues with the MELD libraries and applications.

As a result, the outputs of this exercise will focus less on specific implementation of sustainability practices, and more on describing sustainability issues and possible mitigations, than was originally envisaged.


### 3.2.3 Observations

#### Complexity of supporting software environment

The MELD client support code is implemented using NodeJS, React and Redux.  This environment offers a rich set of features, which have allowed complex functionality to be incorporated into MELD with relatively little initial software development effort.  

But React is a complex, dynamic environment with many supporting tools.  This gives rise to a complex web of dependencies, some of which may be frequently changing.  This can give rise to incompatibilities and failures when the MELD software itself, and any supporting tools used, are not kept up-to-date with the latest React/Redux library modules.  Many of the updates to underlying software are to address security vulnerabilities, so are important to incorporate into a public-facing web application.

#### Undetected problems in MELD code

There were a number of areas where the MELD support code did not behave in ways that were expected.  This would cause problems with existing applications, which might still work only by virtue of unrecorded changes made to the runtime environment, but which would fail when installed in a new environment.   When encountered, these failures are often hard to track down and fix.

These are problems that automated tests, and especially continuous integration, are intended to catch.  The "Hello MELD" applications proved helpful when it came to isolating and fixing such failures.

Effective testing is a cornerstone of software sustainability.

Retro-fitting tests to existing applications has proved to be challenging, for reasons that range from setting up a test environment that sufficiently mimics the application environment, to accessing internal interfaces that expose application logic to be tested.

#### Browser vs non-browser code

In theory, Javascript code is highly portable, possible to run in a browser, desktop or server computer.  This is very appealing for testing, as it can be tricky to run automated tests in a browser, and continuous integration platforms are generally server-based.  (There do exist mechanisms for testing in "headless" browsers, but these can add complexity for a small project)

In practice, there are a number of significant differences: Javascript language variants, run-time support, module loading mechanisms, and more.  We ran into several cases of code that would run as expected in a browser, but would fail when run in a non-browser test environment, due to differences in the ways in which modules were loaded.

The original intent to test the HTTP interface was dependent on a significant amount of application logic being implemented on a server.  When this is not not the case, effective testing requires that it uses code-level APIs, with test code directly invoking APIs in a way that that resembles application code.

Setting up even a very simple test environment for testing browser-based application code turned out to be challenging.  Theoretically, the testing framework would paper over any differences from the browser environment.  In practice, there were areas where differences would persist, and complex workarounds were adopted to avoid provoking these.

#### Hello MELD series

The "Hello MELD" applications were extremely helpful, both for learning the framework when bringing a new developer on board, as it allowed the various parts of a complex set of interfaces to be mastered separately, and creating code that was easier to test .  Also, the "Hello MELD" series of applications, which performed increasingly complex operations using MELD, provided fallback options when failures were encountered, allowing code-induced and environment-induced problems to be identified and separated.

It was important that the first of these "Hello MELD" applications was so simple that one might easily think that there was nothing to go wrong.  In practice, things did go wrong, and even a trivially simple application served the important function of proving that a minimum set of runtime requirements had been set up correctly.  Having a trivial application provided a useful platform for exploring testing tools and frameworks without getting bogged down in application code minutiae, and allowed easy experimentation with application design options (using established MELD patterns) that would facilitate testing. 

#### Value of part-time technical expertise

The availability of occasional access to an experienced software developer was reported to be very valuable for project researchers who were primarily humanists, but engaged in software development.  While it can represent a significant cost to a project to employ a full-time developer, having part-time availability potentially offers many of the benefits of a full-time developer, and potentially facilitates training and technical capacity building for digital humanities projects.

For MELD, the funded developer was at 20% FTE (1 day/week), with the effort split between (a) software maintenance and building testing capabilities (i.e. direct technical work), (b) supporting other project developers (indirect technical work) and (c) project management, SSI3 introspection and and reporting activities.  One day/week is relatively easy to schedule and manage, but the above suggests that 10%/0.5 day/week might be sufficient for technical support aspects.


# 4. Sustainability issues encountered

This section aims to highlight key issues encountered.  Raw observations and notes are contained in a separate document:  [MELD sustainability issues observed](./MELD-sustainability-issues-observed.md).

## 4.1 Working with a complex and dynamic software ecosystem

We ran into a number of compatibility problems while working with MELD on recent versions of NodeJS, React and Redux.  Attempting to update dependencies to later versions gave rise to failures that proved quite difficult to identify and remedy, and required changes to the MELD software.  Further, MELD itself is undergoing several changes in response to new application requirements, raising further library compatibility concerns.

Keeping application and support code up-to-date with a complex evolving environment like React/Redux requires significant ongoing engineering effort, which can be challenging for a small research project.  This is especially true if the development is split over several research projects that don't necessarily have continuously available development effort - the wider world doesn't wait, and fragmented research activities are easily left behind.

### Suggested mitigations

- before adopting a complex support platform, consider whether the value provided (in terms of easy-to-use features) justifies the additional complexity and maintenance requirements.  (The consensus for MELD is that the features offered by tghe React/Redux platform did enable results that otherwise would probably not have been achieved.)

- when adopting a complex support platform, consider a strategy or practice to ensure that the underlying dependencies can be safely updated. Ideally, this will involve having some form of automated testing and continuous integration in place.  Try to keep dependencies up-to-date as you go, rather than relying on infrequent updates of multiple dependencies.

- consider how to create a complete snapshot, including all supporting components, of a working version of an application.  For example, as a Docker image?  (Bear in mind that for a web based application, there remains a possible dependency on the browser used.)

## 4.2 Inconsistent build and runtime environments

We found different developers were using different versions of the runtime environment and build tools.  This led to applications that worked in some environments and failed in others.

Some of the build tools used were not always reliable.  For example, Node Package Manager (npm) is supposed to record package versions used, and thus ensure consistency.  In practice, we found that differences between different development environments were not being corrected, leading to inconsistencies noted.  

Clear identification of supporting build tools used was lacking.  The Javascript/React/Redux environment in particular relies on a range of tools to compile code to a form that can be run in a base runtime environment.  The tools needed are themselves somewhat dependent on the NodeJS and/or web browser versions used to run the application code.

### Suggested mitigations

- create automated scripts to create clean runtime and development environments from scratch.  Such scripts also serve usefully as documentation.

- establish a continuous integration environment:  this forces the environment to be rebuilt from scratch (or from established and documented resources) whenever the software is updated.

- maintain development/test environments separately from other aspects of the host system (e.g., using tools like Node's `nvm` or Python's `venv`).  Provide detailed instructions and/or automated script for setting up a new development/test environment, or resetting an existing one.

## 4.3 Lack of automated testing and continuous integration

Although MELD has a simple test application, there was no automated test suite of continuous integration set up.  (Continuous integration is used here to mean an environment that automatically installs the software into a clean environment, runs the tests and reports any failures, whenever any changes are made to the software.  This allows problems to be detected rapidly, hence easier to fix because the breaking changes made are still fresh in a developer's mind.)

The lack has resulted in uncertainty about the status of the software.  Attempts to install the simple test applications resulted in failures that would probably have been picked up by automated tests and continuous integration.

### Suggested mitigations

- establish an automated testing regime at the outset of a project, or very early in its lifetime.  Much of the value of automated testing can be realized even by very lightweight tests - 90%+ test coverage is great of you can afford it, but even very minimal coverage can provide valuable proof of life, and can be quick to implement.  In practice, once a testing regime is in place, I find it tends to collect coverage-improving tests over time  with very little additional effort.  Expect automated testing to pay back any initial investment of effort within a few weeks of development activity.

- if it can be done quickly and easily, set up continuous integration early in the project.  A prerequisite for this is some form of automated testing, even if it only to ensure that system can be built and installed.  In practice, I've have found that having easy-to-run tests can be almost as valuable (but may be less effective for picking up installation and environment setup problems).

- consider testability when designing code: ensure that important features are easy to invoke and access for testing.

- treat tests as an important source of documentation: some of the effort put into documenting an interface may be better directed toward creating tests that can also be used as examples.

### Test fixtures and mocking

For data-intensive applications, including many DH applications, consideration should also be given to creation of text fixtures (e.g., datasets against which the tests are run), or "mocking" of the interfaces through which such data is accessed.  This aspect has not been explored as part of the project reported here.

See also the section "Data preparation" below.

## 4.4 Application code version management issues

The MELD libraries are being used by different developers in different institutions, who also apply fixes and updates to the libraries themselves.  This has meant that different branches of the MELD code being used in different applications over extended periods of time, exacerbating the consistency issues noted previously.

There was some early lack of clarity about the code governance (e.g. the exact requirements for updating the reference version of the software that should be the basis of all new applications (and preferably any that are actively being developed).

### Suggested mitigations

- establish a governance structure early in the project.  It doesn't need to be complex: one of it's main purposes is to let all developers know what is expected in order to update the main codebase.

- try to make use of available affordances to automate governance constraints; e.g. using GitHub reviewer requirements for merging a pull request.


## 4.5 Application code complexity

The MELD support libraries, and especially the underlying React/Redux framework, present a rich and flexible interface, which in turn forces a degree of complexity onto the application code that uses them.  This has meant that applications are initially difficult for new developers to understand.  We found that, in practice, it took a significant amount of hand-holding from an existing MELD application developer to bring a new developer up to speed using the MELD framework.

### Suggested mitigations

There's no silver bullet for solving this - it's a perennial software problem.  But some things to consider might be:

- create some very simple, easy-to-understand applications/instances with minimal functionality.  These can provide a gentle onramp, helping developers to understand which aspects of a platform or system are providing what kind of functionality (and which aspects might be ignored).

- try to conceive an application as independent modules with separate concerns.  An example of this approach is Unix command line tools that can be used alone, or combined in various ways.   It's not always easy or possible for web applications, but we had some success with SOFA (an earlier MELD application) using Solid (LDP) for data storage, and chainable command line tools for various features.

- don't let too much technical debt accumulate (for some value of "too much").

## 4.6 Run time error detection and reporting

Run time error detection  and reporting in the MELD libraries, and sometimes in the underlying React/Redux libraries, was very patchy.  This would result in applications failing for no apparent reason, or failing for indicated reasons that have no discernible connection with what the application is trying to do.  This can make problem diagnosis and rectification difficult and time-consuming.

### Suggested mitigations

Mitigations here are common software coding practices.

- don't fail silently.  If something is wrong, or if an option is not (yet) implemented, report it.  Such reports don't have to be pretty, just obvious.  If possible, they should refer to application data and concepts, not internal software concepts.

- code defensively!  Don't assume that certain inputs can't happen:  check them and generate a report (or crash with a stack dump) if they aren't satisfied.

- provide debug options to report inputs received, to help identify where any problem might be occurring.

## 4.7 Data preparation

MELD applications, in common with many Digital Humanities applications, typically work with existing data, which may be created using other tools (e.g. music files), or extracted from existing public resources (e.g. Wikidata).  For an application to be useful, or testable, suitable input data must be available.

The MELD framework does provide data for testing and demonstration use, but it wasn't always easy to find.

For creating a simple stripped-down application, we needed to create a new dataset using tools that are not part of the MELD framework.  The MELD documentation did not contain pointers to easy-to-use options for creating such data, and this process alone took a few days of investigation and experimentation.

### Suggested mitigations

- for testing, provide ready-to-use datasets.  Ideally. they should be as simple as possible for the feature they are intended to test.

- for users, provide simple instructions for creating input data.  For example, in the case of MELD, an MEI file can be created from a MuseScore musical score, exported as MusicXML and then processed using the Verovio web service to convert it into MEI (e.g. as described for [`hello-meld-2`](https://github.com/oerc-music/meld-hello-meld/tree/main/hello-meld-2#use-verovio-to-generate-mei-from-musescore-musicxml))

- provide pointers to file format and vocabulary term descriptions.

## 4.8 Accumulated technical debt

This isn't really a separate issue, but more a consequence of the other factors reported above.

Technical debt refers here to software additions and patches that are used to get some software running, but which don't represent an optimal way of solving the underlying problem.  It often results in code that has duplicated and/or tortured logic, and fails to maintain a clean separation of concerns between software elements.

When developing software under time-and-resource constraints, it is often expedient to take short-cuts to validate an approach, or to demonstrate a result.  Individually, these shortcuts are often not problematic, but if they are allowed to accumulate in a codebase the resulting software can quickly become harder to understand and update.  And faced with with such difficulty, a developer will often use additional shortcuts or workarounds to avoid having to deal with the difficult code.  In this way, the accumulation of technical debt can grow exponentially, and the speed of making changes and fixes will decay accordingly.

### Suggested mitigations

Again, no magic bullets here.  It's mostly common software engineering practice and skills.

- be aware of technical debt; keep notes of where it exists, and ideas for eliminating it.  When a change turns out to be more difficult than it should be, that may be an indication that the code first should be refined.

- don't let too much technical debt accumulate.

    How much is too much?  This is hard to say - it's also possible to spend too much time refining code.  A possible rule of thumb is this:  when a piece of code needs to be modified, first see if there are any changes that make the desired changes easier to apply while preserving its existing functionality.

- try to maintain separation of concerns between software components.

- try to avoid duplicated logic (also known as DRY: "don't repeat yourself").


# 5. Lessons in sustainability for DH

Achieving long-lived utility for the software outputs of a digital humanities research activity (or any research activity?) is often a delicate balancing act between getting the work done, and putting in the extra effort to ensure longer term viability (i.e. so that the work done "stays done").

This balancing act affects resource allocation, conduct of the project, and technical design choices, which are discussed below under the headings "Resourcing", "Technical activity" and "Technical design".

The discussion that follows is not unique to DH software, but it is intended to focus on aspects that arise more commonly there.

## 5.1 Resourcing

Considerations of resourcing will necessarily involve trade-offs between immediate requirements and longer term utility and sustainability.  Such considerations are not unique to DH software: in "The Mythical Man-Month" chapter "The Tar Pit", Frederick Brooks discusses near order-of-magnitude costs associated with the evolution of a "program" (solving an immediate problem) into a "programming systems product" (providing long-term utility, and supporting evolution to address new problems).

More recently, such trade-offs are considered by "agile development" practices, which propose that it is futile to expend up-front effort to address future problems, but which also propose that evolution is a fundamental element of software development, and needs to be supported by development practices (such as continuous testing, refactoring, etc.)

## 5.2 Technical activity planning

When planning (and budgeting) a development activity, some factors to consider are:

- code management
- dealing with technical debt
- combating software decay ("bitrot")
- code testing
- automated builds

This is not an exhaustive list, but reflects some key concerns that have been raised by  our work with MELD.

Code management probably needs little discussion, as it seems to be a given for just about any software project.  The existence of a shared repository of source code, which includes a history of all changes made, is very common practice and, among other things, helps to safeguard all the efforts that go into creating some piece of code.  Some popular code management tools are `git`, GitHub and SourceForge.  There are plenty of challenges associated with code management and version control, but these have not proved to be a major issue with MELD, so their existence will simply be assumed.

Technical debt arises when the overall structure of some software is misaligned with the functionality it is required to provide.  It accumulates through the application of quick (or not-so-quick) fixes to add new functionality, without adjusting the software structure to accommodate the changes.  Addressing technical debt requires a level of ongoing effort that does not, of itself, advance the capabilities of the software.  Not attending to technical debt typically results in software that becomes harder to maintain or modify with the result that new features become more difficult and more expensive to add.  But paying too much attention can result in wasted effort.

Where technical debt arises (mainly) from changes to the software itself, decay may arise from changes to the external environment in which it is deployed and/or developed, including operating system, language compilers, runtime libraries, external libraries and utilities used, etc.  In  particular, software may decay even when it is not being changed;  security mitigations are a particular source of such changes.  Combating decay requires a degree of onging engineering effort to recompile and retest software with updated tools, libraries and operating environments, and to apply any software changes that may be needed to ensure its continued operation.

Testing us required in all stages of technical activity.  While manual testing may be deemed sufficient when initially developing some software, automated testing is generally a cornerstone of activities to combat technical debt and decay.  There are many forms of automated testing discussed in software engineering literature, but for the purposes of discussion here it is proably safe to say that _any_ automated testing is better than none at all. 

To be confident of being able to successfully install and use a software system, and automated build and deployment system is extremely valuable.  Many modern programing languages have relatively easy-to-use automatic build systems (e.g. NodeJS has `npm`, Python has `pip` and `setuptools`, Ruby has `gem`, etc.).  Or one might write a shell script to perform the required installation steps.  A key benefit of an automated install system is that, when combined with automated testing, it allows continuous integration, where tests are run every time a code change is submitted to a code repository.


## 5.3 Technical design choices

Sustainability of a codebase is affected by software architecture, and myriad technical design choices.  There is extensive technical literature on this subject, which is not covered here.  Just a few points for consideration are offered.

When considering creation of sustainable software, there is a risk of creating inflexible monoliths.  Most software systems tend to ossify over time, becoming harder to adapt and evolve as early design choices become more pervasive though the system implementation.

Large monolithic applications are often harder to maintain, and hence harder to sustain.  Does an application need to be monolithic?  Sometimes, especially where non-technical users are involved, the answer may be "yes". The alternatives here would be to conceive an application as consisting of a number of smaller, independent pieces that are run separately, and pass information via files or data handling pipelines.  But this can place a greater burden on a non-technical user learning to use the application.  

A monolith can sometimes be decomposed by separating data processing components from data presentation.  This can also facilitate testing, by creating explicit interfaces that can be used as test targets.

In choosing to use a complex dynamic supporting framework (such as React, or any of numerous other web application frameworks), it is possible to incur dependencies that render the resulting software more prone to decay, and in needing additional effort for sustainability.

Complex user interfaces require a lot of effort to build and maintain, and can be a source of sustainability problems.  Sometimes, sophisticated visualizations and flexible interactions are important for research software.  It may alternatively be the case that the important research value of the software can be achieved by simplified data presentation, or generating data in a form that can be consumed and presented by an off-the-shelf application.


# 6. Recommendations for DH software sustainability

@@ Add text pointing out it;s not prescriptive @@

@@ Include discussion of role of "Hello world" type apps

@@ Links back to lessons/observations

@@ Presentation - all this to background of "sunscreen"?? :):):) @@





The following commentary is based on qualitative observations rather than detailed quantitative evidence, and as such offers topics to consider for improving sustainability rather than detailed guidelines.  Generally, all projects are different, and each will need to make it's own trade-offs, but consideration of some common themes will likely help to improve outcomes.


## 6.1 Recommendation 1: Technical architecture

Can the research software be effective if conceived as a set of independent tools that are designed to work together, possibly sharing information via well defined interfaces (such as simple files or data pipelines)?  Smaller, independent programs are usually easier to write, test and maintain.  Consider separating data presentation from data processing and/or retrieval.  As long as the interfaces remain stable, changes to one part of an application suite should not require changes to another part.  The downside may be that multiple independent applications are harder for non-technical users to learn and use.

Avoid creating complex user interfaces, or limit the complexity of any that are needed.  Implementing a sophisticated user interface can require development effort disproportionate to the value provided, and subsequent maintenance effort is similarly magnified.

In choosing to use a supporting application support platform, consider whether the value provided by the platform outweighs the additional dependencies that are introduced, and that may require ongoing maintenance effort to keep up to date.  Note that such platforms can take significant effort to learn, which may lead to difficulties finding future developers to help with software maintenance.


## 6.2 Recommendation 2: Design for testing

When designing and planning a system implementation, think about how it can be tested.  Think about intermediate results that can provide insight into how the software is working, and how they can be made visible for testing.

As the software is developed, implement some tests, however minimal.  These will serve to confirm that the software is (to some degree) testable, and also a simple "I'm alive" kind of test is extremely valuable.  Later, as problems are encountered, additional tests can be added to explore and diagnose the problem, and help to avoid future regression.

Consider budgeting (say) 20% development effort for implementing automatic tests.  This number is arbitrary, without supporting evidence, so a different number might be chosen.  But consider this:  if a piece of software is developed over a week, does spending one day of that week writing some basic tests significantly undermine what can be achieved?  Even if that effort is not recouped within the week of initial development, I estimate from personal experience that the time would be recouped over a further 2-3 weeks of continued development.   Such tests can serve several purposes:  thinking more clearly about how the software should work; confidence that the code is (at least partially) working as expected; examples for new developers to learn about the software; executable specification of how the software is intended to work.  The effort saved in manual testing, documentation and learning can quickly outweigh the effort of writing the tests in the first place.

Software engineering literature discusses the desirability of high levels of test coverage.  Yet even lower levels of test coverage can be very valuable, and with a test framework in place, additional tests are relatively easy to add as proiblems are uncovered.


## 6.3 Recommendation 3: Continuous integration

With automated testing in place, even if very cursory, and assuming that software building and deployment is automated, it should be relatively easy to set up a continuous integration system so that errors can be detected very when code changes are applied.


## 6.4 Recommendation 4: Incremental development

Develop in small increments, with testing at each stage.  When a failure occurs after a small number of changes have been made, it is easier to pinpoint the cause of failure compared with when many changes have been made to add multiple new features.

At each stage, keep a copy of the working code (typically annotated in a version control system) along with instructions for running it (where the instructions can be as simple as identifying a script to use).

When adding a new feature, consider first refactoring the code:  restructuring the code to accommodate the planned changes, without actually changing its functionality.  All existing tests should continue to pass.

Ideally, any set of code changes should be a sufficiently small advance on an existing known working system that they can be thrown away if they don't work.  (This isn't always easy to do in practice, but something to think about.)


## 6.5 Recommendation 5: Dealing with technical debt

Don't allow too much technical debt to accumulate.

This begs a question: how much is too much?  A rule of thumb might be to eliminate technical debt when it becomes an impediment to adding a new feature.  It's usually better to eliminate problem code before changing it's functionality, rather than trying to do so after it has been further modified.

Treat elimination of technical debt separately from adding new functionality, and save changes so that any functional enhancements are made from a recoverable baseine.


## 6.6 Recommendation 6: Keep dependencies up to date

Don't put off updating dependencies.  As with incremental development, it's easier to troubleshoot any problems if relatively few things have changed.


## 6.7 Recommendation 7: minimal application examples


## 6.8 Summary of conclusions

@@@ copy from top of document @@


# 7. Further sustainability issues requiring other investigation through the use-case

@@@ (why now? why these people? what cost if not funded?)

@@ building on lessons of this project (role of ongoing RSE contribution...)

work @@ for MELD sustainability

@@ how to continue the exploration to obtain broader generalized results @@

- More work on MELD testing - produce more examples of React/Redux platform

- Example test fixtures and mocks

- Continuous integration example

- Performance evaluation and tuning  @@ especially, profile to help identify bottlenecks; e.g. graph traversal  (using MELD as case study for more general results)   Without the present project, we wouldn't have understood this requirement (answers why me Q)???

- Solid-based application examples?

- Ethnographic study on DH research for wishlist?


# 8. Acknowledgements

@@ (TROMPA, BiTH, Lohengrin, Delius, FASTm, Un locking musicology)

@@ (inc MELDfest participants)


----


# Additional notes, to be removed

@@ The following points should be covered somewhere in the main document above.

@@ accumulated tech debt from a series of small projects...

@@ multiple projects with different priorities forcing different development branches

@@ who is the audience?  SSI?  Who funds SSI? Also RSEs working in DH. And material to help justify doing the work properly (use of link for educating researchers). Funders? @@


## Governance

- [ ] (from 2021-01-05): add reference to governance/update process to sustainability notes
    - need to discuss in the context of DH research project constraints
    - [x] Check out governance document from MELD.
        - https://docs.google.com/document/d/1_ZBXb0QvDTauPmw1X_5w1vcHqtX6t7sm_BzbeZb6_uc/edit#heading=h.j3fekq3jzeiy
        - where is the list of maintained apps and branches.
            - should dependencies be noted?
        - I don't entirely understand the bit about "must also apply it to all maintained branches"
        - Plan to make minor process update to this - e.g. use GitHub review request.
    - [ ] Also, make ref to older OSS Watch governance recommendations?


KP: try and situate the report against the Lohengrin/Delius app.  E.g., how would testing approach would work/apply to existing app.

KP: in framing report - think about what would be useful for a project like TanC or BitH, and offer recommendations that work towards that.

KP: different testing approaches - what is effect on technical debt? @@@

DL: noted the value of paying down tech debt away from pressures of immediate project (e.g. SSI3 work for MELD).



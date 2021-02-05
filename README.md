# meld-testing-ssi3

Testing framework for MELD, developed as part of SSI3 project for exploring development of sustainable software for digital humanities research.

## Outline plan

The initial plan was to focus immediately on automated testing for the forthcoming MELD 2.0 release, on the basis that this would provide a way to capture API functionality upon which apps would depend, and provide a way to quickly catch regressions as he MELD core is evolved, with the final report focusing on tools and techniques.  In practice, we have found that getting started with MELD was quite a big impediment, and significant effort has gone into creating a series of simple MELD apps (under the banner "Hello MELD") as a way of onboarding a new developers.  In the process, a number of sustainability issues were noted.  My revised plan is to treat this whole exercise as a kind of case study in sustainability issues that have been observed, and to offer suggestions - some with implementations - about how they might be resolved.

Also in the initial plan was to focus on the HTTP interface between MELD applications and the LDP-accessed storage.  Examination of the MELD applications showed that there was a lot of in-browser logic that was sufficiently tricky to need separate testing.  Further, the process of developer-onboarding with MELD takes place without reference to an external store.  So in the revised plan, the initial focus is on the MELD rendering capabilities, especially Verovio rendering of music scores.


### 2020: August - October

- [x] Sort infrastructure issues
- [x] Prepare presentations for MELD discussions
- [x] SOFA review and high-level documentation (to feed MELD discussions)
- [x] Read/reread MELD papers
- [x] Discussions, planning


### 2020: November

- [x] MELD initial software reviewing
- [x] Reading about React/Redux
- [x] Initial "Hello MELD" MELD app - to be used as learning exercise, test bed and maybe a basis for tutorial.


### 2020: December

- [x] Continued "Hello MELD" app implementations, adding further MELD capabilities
- [x] Initial enumeration of sustainability concerns encountered with MELD
- [x] Initial testing framework for MELD (working with "hello-meld-1")
- [x] Initial outline for SSI3 report


### 2021: January

- [ ] Flesh out further "Hello MELD" apps, with testing
- [ ] Organize test suite for meld-clients-core, based initially on the "Hello MELD" tests
- [ ] Review text fixture setup for MELD publication (e.g. Solid and/or mocks?)
- [ ] Updates to SSI3 report


### 2021: February

- [ ] Review Lohengrin MELD app
- [ ] Review Delius MELD app
- [ ] mini MELDfest: present "Hello MELD and initial testing framework"
- [ ] Updates to SSI3 report


### 2021: March

- [ ] Investigate performance/bottleneck monitoring as part of tests
- [ ] Flesh out test plan for graph traversal
- [ ] Expand graph traversal tests per plan
- [ ] Updates to SSI3 report


### 2021: April

- [ ] With tests in place, look at graph traversal refactoring to improve performance
- [ ] Review sustainability lessons to date
- [ ] Articulate possible follow-on work
- [ ] Draft initial SSI3 report with suggestions for further developments


### 2021: May

- [ ] Circulate sustainability report to SSI team
- [ ] Solicit feedback on sustainability report
- [ ] Stabilize refactored graph traversal code
- [ ] Investigate CI options for MELD core


### 2021: June

- [ ] Revise SSI3 report in light of feedback
- [ ] Initial implement/deploy CI for MELD core
- [ ] 


### 2021: July

- [ ] Final SSI3 report
- [ ] Wrap up tutorial and testing code



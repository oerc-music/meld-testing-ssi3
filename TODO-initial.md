This file is an initial list of activities for the MELD testing project.
The intent is to tick these off as they cease to become planned tasks, or move them to the GitHub issue list.


## 2020-07-29

- [x] Prepare SOFA intro for MELDfest (done)

- [x] Prepare command line tool presentation for MELDfest
    - add to SOFA presentation? 
    - https://github.com/oerc-music/nin-remixer-public/blob/master/notes/20201004-SOFA-and-MELD.key.pdf

- [x] TODO: Prepare SSI project premise for MELDfest
    - add to SOFA presentation?  Based on Kevin's proposal
    - https://github.com/oerc-music/nin-remixer-public/blob/master/notes/20201004-SOFA-and-MELD.key.pdf


## 2020-08-22

- [x] Join presentation of MELD to BitH project


## 2020-09-01 and 2020-09-04

- [x] Progress status of antheia (now back online and publicly accessible)

- [x] Progress getting seldom back online (hosts MMM video)


## 2020-10-05

- Note that BitH testing is not a goal for SSI3, but hope that SSI3 work may inform BitH (e.g. via the report)


## 2020-10-13

- [x] go through Kevin's emails and merge SSI3 activities to get on with into a single list
    - currently  nvalt:20201013-SSI3-project-tasks on my laptop (this file)

- [x] Prepare for MELDfest  (see 2020-07-29 above)
    - [x] draft agenda: https://docs.google.com/document/d/17uzZW46vTqXVdcaKiIuX9MSIn4kb0PeJQcOUB6LaAKk/
    - [x] Sofa slides on my laptop at oerc-music/nin-remixer-public/notes/20201004-SOFA-and-MELD.key
        - [x] also https://github.com/oerc-music/nin-remixer-public/blob/master/notes/20201004-SOFA-and-MELD.pdf 
        - [x] seek out copy of SOFA video
        - [x] SOFA SAAM paper: https://dl.acm.org/doi/10.1145/3243907.3243912
        - [x] SOFA SAAM paper: https://ora.ox.ac.uk/objects/uuid:989f8931-ac42-43ed-b6cc-9d6b1386dd3c
    - [x] Prep to talk about SOFA testing / meld-cli-tools
        - https://github.com/oerc-music/meld-cli-tools
    - [x] Prep to talk about SSI3 premise (and focus?)

- [x] Chase JPNP for copy of SOFA video
    - 2020-10-13: sent chasing email (in case there's a later version)
    - 2020-10-13: Found copy Desktop/screencast2-SOFA-SAAM-demo.mkv in email "Fwd: Re: Chillin' on the SOFA" dated 2018-10-08
        - An updated screencast (once I managed to work out audio settings in "simplescreenrecorder").  It now features a fragment being played back before the selection is made.  It doesn't show the whole composition  being played as that bit is not yet functional. 

- [x] TODO:  review Lohengrin app - "page in" some familiarity with it
    - (Links from 2020-10-13 Teams chat log)
    - ‘Lohengrin TimeMachine Digital Companion’ - interactive tablet app, essay, and video
    - Video of app: https://hcommons.org/deposits/item/hc:31989/
    - Introductory video by Prof. Dreyfus, information on app, link to live demo (best effort): https://um.web.ox.ac.uk/lohengrin
    - App: https://lohengrin.linkedmusic.org/

    Connects essay, music score, vocal score, and provides cross-links for playback.  
    Opera timeline is visible at bottom, with colour coding for the different story realms.
    Side by side comparisons of different instances of the motif are possible.

- [ ] TODO:  look at graph traversal code on Lohengrin app
    - Repo for Lohengrin app: Link https://github.com/oerc-music/ForbiddenQuestion , by David Lewis.
    - 2020-10-20: currently stuck finding my way into the app
    - Plan to come back to this when simpler stuff (Hello MELD, selectable score) is nailed

- [ ] TODO: review Delius app
    - delius-annotation is the live quartet annotation on tablet
    - delius-playback is the replay of that capture
    - annotations-bith_meld_intro-2020106-01.odp - Kevin's MELD intro with Climb! and delius
    - ‘Delius in Performance’ - enhanced BL Discovering Music article:
        - Video of app:https://drive.google.com/file/d/1kRHZEdiGXa6vYfPlN1e3X2WIr6yTM4VZ
        - Paper: https://dl.acm.org/doi/10.1145/3273024.3273038
        - Live demo (best effort availability): https://bl.linkedmusic.org/
    - Delius String Quartet live performance annotation
        - Background video: https://drive.google.com/file/d/1zTUz5j4SWQPOTcz9b6pqq_-Jmlc8mu5D
        - Paper: https://dl.acm.org/doi/10.1145/3358664.3358669
        - Live demo (best effort availability): https://delius-annotation.linkedmusic.org/
        - delius_in_performance-enhanced_article-sound.mp4 (drive.google.com)

- [ ] TODO: review Lohengrin, Delius and MELD papers

- [ ] TODO:  think about focusing testing effort on the MELD traversal app.
     - Basically this is a generalised facility to extract information required for a given app.  
         - (note to self - e.g. think SPARQL DESCRIBE issues?)

- [ ] TODO:  think about profiling options to be part of testing framework.  Identify where time is being spent?

 
‘Listening through Time’ - companion to New York Philharmonic Archives podcast
Background information and video demonstration:https://um.web.ox.ac.uk/nyphil
Live demo (best effort availability): https://nyphil.linkedmusic.org/


## 2020-11-04

- [x] TODO: raise simple app in internal SSI3 MELD testing discussion in Oxford.  (see also: minimal selectable-score application, see https://github.com/trompamusic/selectable-score-demo and https://github.com/trompamusic/selectable-score)

- [ ] TODO: consider support for tracking React changes?  (Note: any form of testing should go a way toward this.  More generally, problems of moving dependencies.)  Consider effect on choice of testing framework.

- [ ] TODO: look at changing names of blackList/whiteList in meld core codebase  (dev branch)
    - see issue #1


## 20290-11-04

- [ ] TODO: look into CI for MELD core (see email this date from Kevin)


## 2020-11-10

- [x] TODO: new update on RS work to MMM list

- [ ] TODO: work with David/Mark to get MELD simple annotation example running
    - [x] Simple Help MELD app is running, with basic score rendering.
    - [ ] Add selectable score to "Hello MELD" family
        - see issue #2

- [ ] TODO: investigate toolchain for MEI creation.
    - [x] "Hello MELD" score created in MuseScore
    - [ ] Generate MusicXML and try conversion to MEI.

- [ ] TODO: think about presenting test framework at putative MELDfest in spring 2021

- [ ] TODO: set up project for MELD testing
    - [x] create project on GitHub
    - [ ] pick testing framework; MochaJS? (alt JestJS)
    - [ ] Create simple test suite for "Hello MELD"


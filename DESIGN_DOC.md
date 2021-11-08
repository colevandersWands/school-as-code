# Design Document / Requirements

This is mostly going to be a free-form discussion of what we want to achieve, and how we're planning to achieve it. I expect we'll revise this significantly as we go, particularly after getting feedback from users.

## Overview

As more people get interested in learning to code/program/hack/etc, and more of the collaboration moves to remote/distributed teams and schools, we want to give people a ready-made scaffold for setting up their own learning infrastructure.

Ideally, if there's somebody who wants to teach, and somebody who wants to learn, you've basically got a class. If there are multiple people who want to learn, all the better. In the interest of making such ad-hoc groupings fast and easy to set up, we decided to try and create a very basic School-As-Code (SaC).

Given that GitHub repos and organizations are free and a natural place for new learners to actually practice both source control and code review, setting up that infrastructure in GitHub is an obvious choice.

We want somebody to be able to press one button (after setting their own personal GitHub token), and have a fully set up curriculum ready to go. At the moment, this is going to incorporate some ready-made materials, but in the future, it's very likely that it could incorporate any number of different dynamically generated materials, as well as context, assessments, or anything else that somebody thinks would be useful to have in the process of teaching/learning.


## Proposed Solution

To assist in this, we want the following bare minimum "assets" set up for the class.


### Artifacts

- "home" repository for the organization. This will be the "landing page" for the class, incorporating all the information about the class, as well as links to the different materials, project boards, etc. It will be created from a template for the time being, but would be extendable with custom templates some point in the future.
- "project board" for teachers and students to track progress with cards. These cards will be derived from templates for now, but similar to the static template used above, we could even see cards auto-generated for students at some point dynamically.
- admin repo used for the admin(s) to track admin type stuff. It's possible the there's only one teacher, who is also the admin.
- teachers repo used for more class-oriented needs.
- student repos used for students to store and share their progress on the assignments.


### Scripts

It's our intention that a declarative process will be able to take some form of configuration (eg. the `school.yaml` file in this repo) and turn that into the artifacts listed above. It should also be possible that some of the config gets updated, for example a new student is added to the config for the student repos, and on a subsequent run of the automation, the only change to the artifacts is that the new student also has a repo of their own. We want this automation to enact the config file as what exists, not necessarily specify how it gets created/updated/deleted.


### Language and Frameworks

At the moment, the existing automation uses shell scripts, but it might be that this isn't the best solution, going forward. This will get a wider discussion in the Users section below, but it's highly likely that we end up with a higher-level interpreted language for reasons expanded on in that discussion.

It's also possible that the automation will create artifacts outside of GitHub (eg, discord webhooks), but that still needs to be discussed.


## Users

There are two key user groups that we're interested in: teachers/admins and students.

### Teachers/Admins

This group is likely to be fairly experienced to advanced in terms of tech, and so we don't necessarily need to hide all of the implementation details from them. Obviously, the less they have to worry about, the better...though it's probably not too much to expect them to be able to create a GitHub org as well as a personal access token and set those values in their shell to run the automation.

We also fully expect these types of users to be able to modify and extend this automation for their own purposes, and we'll most likely to try add comments where that would help them in doing so.

In terms of the language choices that we make for the initial version of the automation, it would probably be best to use a higher-level language than shell scripts, mostly because we want to use a simple, clean configuration file format like yaml, and bash doesn't have native support for parsing yaml. I'll propose the following three options, which advantages and disadvantages for each:

- Python

Very common and easy to use, though given that the initial coursework we're looking to re-implement is using node, and configuring python environments is traditionally "not fun" (:tm:), this might introduce more trouble than it's worth. On the other hand, provided that we eventually want this to be behind a web-app or some other type of facade, maybe using python, even if it's not as close to the existing coursework, would be a good step towards where we expect to go eventually.

- NodeJS

This is the language used in the coursework we're trying to replicate, and so it's basically guaranteed to be on the workstation of anybody who wants to teach it. The asynchronous nature of Node could make it a bit trickier (just in terms of forcing synchronous execution with a bunch of extra code) if there are dependencies between artifacts that need to be created in a specific order (eg, we need project boards before we add cards to those boards).

- Docker

This option conveniently side-steps the need to even know anything about either Python or NodeJS (or any implementation language), as we can just run the automation in a container. The only downside is that it requires an installation of Docker and the ability to build/run containers. This is becoming a more common aspect of development (definitely full-stack/backend, and even in some cases frontend) these days, so it might not be entirely unreasonable to expect technical users to be able to use Docker.

---

So it seems in general that we need to decide on what will get us the fastest feedback/development in the short term, given that even if we were to reimplement everything from NodeJS to Python (or vice-versa), it wouldn't be very significant, and so any decision we make now is very easily reversible in the near to medium term anyway.

### Students

To the extent that we're interested in designing for students, they probably don't play a huge role in determining our design, though it would be nice if they have an easy way to modify their participation themselves via automation. This might take the form of creating another repo for themselves or even dynamically generating a new project board task card for themselves if they find their assigned work too easy (self-directed differentiated instruction?).

## Scoping and Timeline

We'd like to reproduce what's currently existing, and so we're aiming to have the above artifacts creatable and updateable by automation by the end of the year. We're planning to tweet out for any prospective users to try out our "beta release" on or soon after Dec 31, 2021.

## Open Questions

It's not at all clear at this point what form this will eventually take, and so the idea is to create something usable as an MVP that we can ask people to try out and see how they like it.

It would be very cool if this stuff could integrate with gitpods or codespaces, or any of the other online learning resources, but that's probably for future iterations.

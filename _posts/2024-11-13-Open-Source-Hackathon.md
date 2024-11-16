---
layout: post
title:  "Attending Open Source Hackathon"
date:   2024-11-16 00:30:00 +0000
tags: Programming Meetups
---

A quick post about [Man Group's Open Source Hackathon](https://www.man.com/london-hackathon-2024) which I joined last Saturday. It was a unique and interesting opportunity, and I talked to plenty of interesting people all around :D

_It might be more of a recount + ramble, but let's hope this style works._

## What is it?

The hackathon was a one-day event organised by Man Group to let different people join and contribute to open source projects. People of different skills and experiences are welcome to apply, and get to work on one of the listed projects with one of the maintainers. This is unlikely anything I've heard before! Usually hackathons are around a topic, but not all of the participants follow it that closely, at least from the ones I joined back in university. Contributing to open source as an initiative isn't new either, e.g. [Hacktoberfest](https://hacktoberfest.com/), but doing it in person and having the maintainers fully onboard? That's new.

Needless to say, I was delighted and went to apply it. After a few weeks, I got an invitation!

## The Day

### Arrival

As I stepped into the building at atround 8:15 am, I was greeted by various volunteers, who gave me a badge, wristband, and stash. Good signs they know what a hackathon is.

Going up to the actual venue, I picked up some breakfast and talked with other participants. They truly came from all walks from life, some were undergrads in their final year worrying about their career prospects, others were data scientists, researchers, or quant researchers. They also came from all over the placve: some from just around the corner in London, others from Cambridge, Warwick, or other cities. I even heard someone arrived from Switzerland that morning! I stopped internally complaining about waking up at 6:15 afterwards.

We then received a welcome from the organisers, and listed the projects which we could pick from (in alphabetical order):

- [airflow](https://github.com/apache/airflow)
- [aiohttp](https://github.com/aio-libs/aiohttp)
- [conda-forge](https://github.com/conda-forge)
- [FastAPI](https://github.com/fastapi/fastapi)
- [finmarketpy](https://github.com/cuemacro/finmarketpy) / [findatapy](https://github.com/cuemacro/findatapy)
- [Jupyter](https://github.com/jupyter/jupyter) / [scipy](https://github.com/scipy/scipy)
- [NATS](https://github.com/nats-io)
- [pandas](https://github.com/pandas-dev/pandas)

Aside from the above libraries, there was also [Starlight](https://www.starlight.org.uk/), a charity for children's play in healthcare. The project was to help them visualise and analyse their services and impact using their (private) datasets. 

I went with airflow, because it was something I've heard of and used at work, and seemed to provide the best chance of me making a code contribution in a familiar language.

### Working

Along with other participants, I joined Jarek and we all introduced ourselves. While we had a range of programming experiences, we were all interested in helping out the project.

Before we made any drastic moves, we had to set up our dev environments. Jarek even said it would be a great boon even if the only thing we accomplished today was getting the environment up and running.

Navigating the numerous documents (which will hopefully be clarified by some PRs), I managed to get it working on my machine. For such a large project, I'm surprised by the new set of tools it uses -- [uv](https://docs.astral.sh/uv/) for setting up the venv, and breeze (an internal tool) to set up the containers to run airflow.

With the environment up, I can now pick some issues to work on. Even the "good first issues" were diverse, especially since airflow has providers to connect to different databases and services. The issues ranged from documentation updates and improvements, to full-blown bugs and feature requests. That said, Jarek also added the project is "PR-based", meaning PRs can be raised without an issues, and the author of PRs help verify whether the change works as intended when a release candidate is made.

I wanted to get something out the door quickly, so started by taking a [linting related issue](https://github.com/apache/airflow/issues/40567). It issue had a long list of checks which were ignored but could be fixed, and specifically called out "PLEASE submit these in many small PRs". Jarek concurred, saying we can even think of the time to review PRs as `O(n^2)`. I fixed it in a handful of files

When picking my second issue, I wanted a bit more challenge, so went scouring again. I found this interesting issue which was in the amazon provider, and even had someone attempt a fix -- only for it to be reverted due to breaking tests. I was about to dig into regex hell when I realised a much simpler fix was available, and happily made another PR.

Side note: while airflow releases quite infrequently, and more so with airflow 3 coming up, providers have a more rapid schedule, so the release candidate is up already!
<!-- I've also been tagged to help verify the changes. I'll... get to it. -->

Near the end of the day, I wanted to bring home an issue other than linting, and found [this interesting issue](https://github.com/apache/airflow/issues/42850). While digging around the history I wasn't sure whether to validate the schema as is or also consider a future proposal. Luckily, Jarek had the context and guided me to focus just on the current format first. It _does_ help to have a maintainer physically in the room while you're thinking on this issue.

_Post hackathon update: the issue seems to be assigned to someone else, and they've arrived at the same conclusion._

### Meanwhile

Between coding and havnig meals (or sweets or tea 🙃), I joined their office tour and also talked with other participants, from university students and researchers, to hedge fund managers and machine learning engineers. Weirdly, a common theme seems to be interest or working in the finance industry, which I guess is reasonable considering Man Group is a "global investment manager".

At my table, I also listened into Jarek's quotes and stories. All interesting considering his point of view as a open source library maintainer, but also just from previous experience. I've summarised some of them here (paraphrased, so all mistakes are mine):

- Open source tools/libraries don't have a set owner, so playing the "let me see the manager" card might mean there's no one for you to see.
- Research before you ask your question: this shows you've spent time learning, and makes it worth for maintainers/contributors to help you.
- Speaking of contributors, 
- They have to intentionally pick things to include or exclude, as to keep the tool focused.
- On uv vs pip, he's undersatnding of why pip has its problems and why uv is fast. As a new tool, uv can afford to not support older tools and libraries, while pip has a long history and _has_ to keep it stable.
- Bisecting is a good way to find problems. Not just [`git bisect`](https://git-scm.com/docs/git-bisect) -- which I've heard good things about -- but just bisection like binary search in general. For example, when you're figuring out where the bug is, literally comment out half the code and see if it shows up or not. You don't even have to be familiar with the code for this to work!
- Apache foundation's voting process is often open for at least 72 hours. This is to account for timezones but also people having different weekends, e.g. [people in Isarel have shorter work days or are off on Fridays and don't work on Saturdays](https://en.wikipedia.org/wiki/Public_holidays_in_Israel).

In addition to Jarek, I also learnt overheard a bit about what others on my table were working on. You can also see our PRs by seacrhing for [PRs with "label:man-hackthon" on the airflow repo.](https://github.com/apache/airflow/pulls?q=is%3Apr+is%3Aclosed+label%3Amans-hackathon)

- updating contribution docs
- update graphviz to make sure it works on new MacOS -- this was only discovered since one of us brought a near-mint Mac!
- a UI fix despite us mostly being Python programmers

There were also issues which went through some investigation but don't have a (only high level descriptions here since I only caught bits here and there):

- a weird interaction with Airflow and Kafka -- a brave idea as someone's first open source contribution. No PRs as far as I'm aware, but they did some thorough investigation.
- an issue with a provider but drilled down to issues with the third party software 
- and an problem where tests took too long to run one a maintainer's machine, but one of us dove into it with flamegraphs and concluded it varies from machine to machine. I guess [not all issues are reproducible](https://xkcd.com/583/)...

### Round Up

At the end of the day, we all rounded up for each project to share their progress. Some tables (like ours) closed down bits and bobs, while others (like pandas) started the day with a plan and reported they made good progress on it. Across the tables, people closed 5-year-old bug tickets, made their first open source contributions, and just as importantly, had fun.

### Pub

Like other PyData events, this ended in a pub, but a different one from the usual.

Throughout the night, I had chats with others with and outside of my teams. I once again learnt the various backgrounds they came from, including their previous contributions to open source (if any). Like me, they overall thought the experience was great, with maintainers being good guides and having made non-trivial contributions, though a few lamented only having worked on documentation. Well, someone's gotta do it, so it might as well be them.

## Overall Thoughts

I think the event delivered on its promise to be a mix between university hackathons and getting people to contribute to open source.

On the one hand, it's definitely a hackthon. Free food, stash, side events (an office tour), and pub afterwards -- what more do you want? Oh, and the participants have varying capacbilities but all did some programming or coding-adjacent things. Each of the tables could be a team, but the Starlight table must have been closest to a whole team working on a project from a sponsor. The overall vibes were great and I was already sold on the event a couple hours in. <!-- I also realise I've only graduated from university for a bit over 2 years. -->

On the other hand, the projects were all open source related. The libraries are used day to day and the contributions do help. Again, it's Hacktoberfest, but all the projects signed up for this, and the participants got guidance on what and how to contribute.

A better improvement might be for the projects to provide guidance _before_ the day, such as where to find contributing docs, or what they are expecting to achieve on the day. Knowing this on the day helps, but it might save people time on the day, so they don't have to install software during those precious hours.

Overall, it was a great day, and I'd happily do it again :D

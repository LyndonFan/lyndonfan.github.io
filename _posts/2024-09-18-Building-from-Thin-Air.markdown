---
layout: post
title:  "Building from Thin Air is Hard"
date:   2024-09-18 23:50:00 +0100
categories: Programming
---

A couple weeks back, I promoted [Aether, a note taking app that includes semantic search](LINK). Here's what happened during the month as I built it, and some other thoughts.

## Week 0

- received cold email from backdrop build
    - inviting me to join their competition / hackathon
- interested
    - not for the founder side of things
    - just the commitment to pushing something out
- still didn't have an idea yet
    - cobbled together application (just a few words about myself)
    - picked a vague name

## Week 1

- actually need to decide on something
- brainstorm
    - smart note taker
    - todo list manager
    - an actually clear dependency board -- the one in Miro had interweaving lines of similar colours, and tasks get moved from sprint to sprint without leaving a reason.
- eventualyl settled on first one
- did a lot of planning, because wanted this to _work_
    - list features of taker
        - front
        - back
    - dates
    - est. time taken

- start on front end
- googled around to find layout ideas
    - design isn't my strong suit
- immediately found a similar design
    - WEBSITE TBC
    - used the CodeMirror component
    - (yoink)
- also used [Simplenote](TBC) as reference
- used AI to help generate React front end
    - use local_storage

## Week 2

- looked at backend options 
- meat of the app: converting markdown text into meaning
- tried with LLMs
    - various models
        - text summary models
        - LLMs
    - task of "Summarize the text below, which may be in markdown. Only output the summary.\n---\n"
- show examples
    - Paris Olympics text from wiki: worked great!
    - AWS EC2 text in markdown
        - text summary models: incomphrensable text
        - LLMs: not bad?
    - failed to run a good bunch of text 
        - went into a tangent on markdown syntax
        - never got anywhere

- frustrated, went onto frontend ahead of schedule
- features
    - import from clipboard
    - create > 1 note
    - import from file

## Week 3

- never got to building a backend
    - luckily, Python is more strong suit
- stack: FastAPI + SQLite
    - FastAPI: wanted to try it out, afaik smoother experience than Flask, but less knobs than Django
    - SQLite: more persistent storage, but odn't want to spin up / manage a DB instance
- _did_ cheat with AI for basic CRUD
- BUT had to make several changes when using websockets
    - only seen it used in a previous company project
    - first time dealing with them myself
        - create a single one, don't close and open whenever user changes which note they're working on
        - send more updates to specify changes
- integrate w/ front end
- start adding search features
    - usual "exact" search (order by no. of occurrences)
    - dip into search by regex
- took time to look at more "traditional" NLP techniques
    - nltk for word semantic meaning
    - led to vector db for text
        - chroma db
        - worked!
- took the weekend off for a trip

## Week 4

- refreshed, started cracking on bugs and polishing
- improve layout, refactor some front end
- removed sqlite and sqlalchemy
    - go "all in" on chromadb
    - just use raw python to do further processing

- "relatively uneventful, until I turned on Github Advanced Security."
    - had it at work, so turned it on for personal project to see what would happen
- picked up on how I implemented regex search
    - "regular expression injection"
- obviously, this was intended, so I gleefully dismissed the alert with a message which I could have never written at work:
    - [use screenshot](https://github.com/LyndonFan/Aether/security/code-scanning/3)
    - > Alt text: This is a feature to allow users to search by regex, so is not a bug.

## Week 5

- dedicated to polish, "packaging", and creating the demo

- "only a few days left (thank goodness for bank holiday)"
- squahsed more bugs
- then look at 
- docker-compose
    - ...but need to build containers first
    - tried figuring out network, but
        - too many things happening?
    - took the "easy" way out and just have everything (including inter-app communication) go through host network

- needed an example for the demo, but not bothered to create enough notes for it
- wanted to use friends' notes
    - record meticulous notes of his readings history on Hong Kong
    - convert into demo-able
        - single docx to files
        - skim and find examples
    - show back to him
        - pretty amzed!
        - found more e.g.s
            - equiv to middleman
            - "Chinese culture"

## Final Thoughts

- yay for pushing something!
- using git for personal projects
    - not just github
    - esp. w/ feature branch help know what you're doing
    - have a history of progress
        - steps
        - dead ends
- building and completing a product is **hard**
    - (thanks captain obvious)
    - coming up with idea
    - debugging
- AI is more helpful than I thought
    - tools used
        - ChatGPT, eps. w/ Grimore GPT
        - Codeium (DOUBLE CHECK with or w/o "e")
    - useful at 
        - repeating stuff
        - writing simple / straightforward code
            - e.g. CRUD app, React app
    - but
        - needs prompting
- more discipline could have helped, but so could more motivation
    - being invested might have pushed me more to
- would I do it again?
    - maybe
    - need an idea going in
    - better plan
    - scope better w.r.t. deadline
        - esp. it's during my free time and life happens
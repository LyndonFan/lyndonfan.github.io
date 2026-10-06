---
layout: post
title:  "Data Engineering Meetup -- September 2026"
date:   2026-10-06 18:30:00 +0100
tags: Programming Meetups
---

_Disclaimer: This was organised by one of my colleagues (hi Chloé!) but all words and observations are my own._

Last month, I went to a [data engineering meetup](https://www.meetup.com/data-engineering-london/). This wasn't Pydata, but was very intriguing. 

## Building Lakehouses

The first talk was on building storage systems by [Robert](https://www.linkedin.com/in/berkeleybob2105/), CEO of [Altinity](https://altinity.com/). It was surprisingly technical!

He motivated better storage by describing AI agents querying databases more and us humans building good systems. He then dove into how analytical databases are built, going through columnar storage, hardware, to lakehouses.

This was a great talk for me! It was detailed, but just enough for someone who vaguely remembers their database classes. Ending on general usage and advice (e.g. compute grows linearly while storage grow exponentially) was a good note.

## AI Turning Storage into Memory

The second talk was about High Performance Compute (HPC), by [Mischa](https://www.linkedin.com/in/mischavankesteren/) at [HPE](https://www.hpe.com/uk/en/home.html), short for Hewlett Packard Enterprise. Yes, the same HP known for printers!<!-- At least they are separate companies/branches? Otherwise I would have asked "why is printer ink so bloody expensive?" -->

The talk began by introducing different types of storage, going from fast (but small) caches and RAMs, to high capacity (but slow) local drives and external storages. The existing model for HPC was built around the these assumptions.

But over time and with AI, the model has changed:
- caches have grown bigger
- memory has gotten more expensive
- storage / databases are faster

This also changed workflows, e.g. saving program state and actual data in storage, and using memory as cache.

This talk felt just as detailed than the first one, though I'm much less familiar with this field so some bits flew over my head :sweat_smile:

## Panel

Next up was a panel with the above speakers, plus [Sergii](https://www.linkedin.com/in/sergii-ivakhno/) from [XR Extreme Reach](https://www.xr.global/). They answered questions about storage, data, and AI:

- How do we determine whether data should be in hot path / cold storage?
- What should we change about our storage architecture? What will we re-invent?
- Do you separate compute and storage costs?
- What's something fashionable to _not_ have?
- Will abstractions allow people to not understand lower levels?

Many of their answers were similar, but came from different angles and varying examples.

At the end was a quickfire round where the panelists just held up yes/no signs. Some prompts were (not in order, paraphrased):

- Would you take fast storage over more storage?
- Would you store everything if storage is free?
- Will vector databases exist in the future?
- Is the lakehouse overhyped?

## Chats & Thoughts

Between the talks and panel there was pizza and chance to talk to others. Most folks were software or data engineers, and a few were curious data analysts. I don't recall talking to any data scientists / machine learning folks.

Overall, this was refreshing! The content was more focused and specific, with live discussion too. The smaller crowd helped me talk some new people. It's missing a bit of wider topics and is less frequent though.

Looking forward to joining this / another meetup!
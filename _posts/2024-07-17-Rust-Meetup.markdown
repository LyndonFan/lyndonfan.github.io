---
layout: post
title:  "Rust Meetup @ Reading"
date:   2024-07-17 17:30:00 +0100
tags: Programming
---

## What are these meetups?

It's a monthly meeting for people who use [Rust](https://www.rust-lang.org/) to do, well, anything! From actually using them on their jobs, to just for hobby projects. From what I can tell, it's more drinks and discussions than a formal meetup.

_Wait, I thought you mostly did Python. How did you get involved with Rust?_

I'm not using it for my job (yet). I first heard of it from the dataframe library [polars](https://pola.rs/), which was written in Rust. Fast forward to a month ago when [Tom]({% link _posts/2024-07-03-gamble-more.md%}) had a idea to port an Apache Spark helper library to polars, and I was sold. It's still a work in progress, but [you can check it out here](https://github.com/TomBurdge/harley). I've written some of the string processing functions in Rust entirely, which made me more comfortable with the language.

## What was there?

### Demos

The only scheduled activity in the evening was a demo session for people to show off what they've built in Rust. I showcased the above repo, which people actually took interest in! Some questions were about polars, which I happily answered; others were about how the plugin mechanism worked, which I gave my guesses. I received some light applause at the end, which was a nice bonus.

This was followed by [paths2html](https://github.com/rustworkshop/paths2html) by Tim, a way to view a list of filepaths by putting them in an html file. The program is blazingly fast, but the code was just around 150 lines -- and a third of them were for tests!

The final demo was [textdb](https://github.com/andy-thomason/textdb) by Amy, a way to query large [Tab-Separated Values files](https://en.wikipedia.org/wiki/Tab-separated_values). The motivation was to process large (over 1TB) TSV files, which were commonplace in biology. The files take a long time to download in the first place, let alone be put into a database. Moreover, most files have a sorted primary key, so she built it to take advantage of this by using binary chopping (a term much cooler than binary search). It was interesting to not just see the product, but also learn about the thought process behind it.

### Chats

The remaining time was just people chatting about various things, often about Rust, but sometimes just programming. People talked about how they got into programming and Rust, as well as what they were working on. I learnt a lot just from listening, but also from asking others about learning Rust, e.g. running `rustup doc --book` will show the "The Rust Programming Language" book in your browser, and it's all offline.

## How was it?

Overall, it was great! I haven't had the chance to attend tech meetups for a while, so this was a nice treat. Moreover, the small size helped make conversations last longer and be more related.

It also felt very refreshing compared to my last PyData experience. PyData had a more formalised schedule, talks, and a wider group of attendees; while this meetup had much fewer people, but was more casual. Their backgrounds were quite diverse: from self-taught to veterans, and their work ranged from aiding genetics research to blockchain technology.

The thing I was most glad about talking to more experienced developers. During the conversations, I was humbled by the deep knowledge they had. Memory maps, LLVMs, and how Windows installers work -- oh my! I definitely have a long road ahead of me, but as they say, "學海無涯苦作舟"<!--definitely not 回頭是岸-->, or "there is no royal road to learning." I suppose I should crack on.
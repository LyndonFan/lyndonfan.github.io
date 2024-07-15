---
layout: post
title:  "Rust Meetup @ Reading"
date:   2024-07-17 22:00:00 +0100
categories: Programming
---

## What are these meetups?

It's a monthly meeting for people who use [Rust](https://www.rust-lang.org/) to do, well, anything! From actually using them on their jobs, to just in hobby projects. From what I can tell it's more drinks and discussions than a formal meetup.

_Wait, I thought you mostly did Python. How did you get involved with Rust?_

I'm not using for my job (yet). I first heard of it from the dataframe library [polars](https://pola.rs/), which was written in Rust. Fast forward to a month ago where [Tom]({% link _posts/2024-07-03-gamble-more.md%}) had a idea to port an Apache Spark helper library to polars, and I was sold. So far it's still a work in progress, but [you can check it out here](https://github.com/TomBurdge/harley). I've written some of the string processing functions in Rust entirely, which has been a good start into getting more comfy with the language.

## What was there?

This was more relaxed / informal, and the only scheduled activity letting people showcase what they've built in Rust.

### Demos

I showcased the above repo, which people actually took interest in! Some questions were about polars, which I happily answered; others were about how the plugin mechanism worked, which I gave my guesses. I received some light applause at the end, which was a nice bonus.

This was followed by [paths2html](https://github.com/rustworkshop/paths2html) by Tim, a way to view list of filepaths by putting them in a html file. The program is blazingly fast, but the code was just around 150 lines -- and a third of them were for tests!

The final demo was [textdb](https://github.com/andy-thomason/textdb) by Amy, a way to query large [Tab-Separated Values files](https://en.wikipedia.org/wiki/Tab-separated_values). The motivation was to process large (over 1TB) TSV files, which were commonplace in biology. The files take a long time to download in the first place, let alone be put into a database. Moreover, most files have a sorted primary key, so she built it to take advantage of this by using binary chopping (a term much cooler than binary search). It was interesting not just see the product, but also learn the thought process behind it.

### Chats

The remaining time was just people chatting about various things, often about Rust, but sometimes just programming. People talked about how they got into programming and Rust, as well as what they were working on. I learnt a lot just from listening, but also through asking others about learning Rust, e.g. running `rustup doc --book` will show the "The Rust Programming Language" book in your browser, and it's all offline.

## How was it?

Overall, it was great! I haven't had the chance to go tech meetups for a while, so this was a nice treat. Moreover, the small size helped make conversations last longer and more related.

It also felt very refreshing compared to my last PyData experience. PyData had a more formalised schedule, talks, and wider group of attendees; while this meetup had much less people and structure. Their backgrounds were quite diverse: from self-taught to veterans, and their work ranged from aiding genetics research to blockchain technology.

The thing I was most glad about talking to a bunch of much more experienced developers. From the conversations, I was humbled by the deep knowledge they had. Memory maps, LLVMs, and how Windows installers work -- oh my! I definitely have a long road ahead of me, but as they say, "學海無涯苦作舟"<!--definitely not 回頭是岸-->, or "there is no royal road to learning". I suppose I should crack on.
---
layout: post
title:  "Pydata London Meetup"
date:   2023-07-05 23:53:00 +0100
tags: Meetups
---

## What is Pydata? What are these meetups?

[Pydata](https://pydata.org/) is a non-profit charity under NumFocus, an organisation that supports many computing languages/libraries such as numpy, scikit-learn, and julia. [Pydata London](https://london.pydata.org/) is one of its many global "branches", where people passionate about data gather and chat about it.

If you want to come along to one of these events, go to the Meetup page below and find an event to RSVP:

[https://www.meetup.com/pydata-london-meetup/](https://www.meetup.com/pydata-london-meetup/)

## The Talks

There were two main talks, followed by two quick ones. I'll summarize them (to the best of my ability, any mistakes are mine) and share my quick two cents.

### Evaluating Distributional Forecasts - [Leonidas Tsaprounis](https://www.linkedin.com/in/leonidas-tsaprounis-302ba882/?originalSubdomain=uk)

Like weather forecasts, time series forecasts are more informative if they include the odds and are more than a single number. Distributional forecasts are what's needed, but this begs the question: how do you measure a predicted distribution? Leo describes the proper scoring rules [introduced here](https://sites.stat.washington.edu/raftery/Research/PDF/Gneiting2007jasa.pdf), giving examples such as the log score (`-log(p(y))`) and the Continous Ranked Probability Score (which is integral of the squared distance between proposed and actual cdf over the real line). He then showcases these metrics in action, comparing them with traditional ones for point forecasts and explaining the differences.

The talk was quite interesting, though a bit more technical than I expected. It's necessary, but around 60% of the people (estimated with a sample size of less than 10) might have been confused. Still, I might keep the ideas around the next time I have to deal with a time series.

P.S.: While finding extra links for this section, I stumbled across his original article -- go check it out!

https://medium.com/trusted-data-science-haleon/metrics-for-distributional-forecasts-60e156c60177

### How To Build Your Own Private ChatGPT Using Streamlit, LangChain & Vicuna - [John Sandall](https://twitter.com/john_sandall)

ChatGPT is undoubtedly powerful, showcasing the peak performance of LLMs. Unfortunately, sometimes companies block access to it, or you just don't put to check sensitive data to Microsoft. What should you do?

As John demonstrates, the answer is to build your own chatbot using the given libraries. [Vicuna](https://huggingface.co/lmsys/vicuna-13b-delta-v1.1) is an open-source LLM trained on top of [LLaMA](https://ai.facebook.com/blog/large-language-model-llama-meta-ai/), ~~Facebook~~Meta's own LLM. [LangChain](https://python.langchain.com/docs/get_started/introduction.html) is a library that abstracts away nitty-gritty details for interacting with LLMs and allows them to remember what you and it said in the past. To top it all off, [Streamlit](https://streamlit.io/) is a Python library that allows you to quickly build web apps to showcase data science/visualisation results with little effort. Combine them all and you get your private ChatGPT clone, all running on your own computer.

The title is obviously eye-catching, and the results did speak for themselves. There was an interesting hiccup where the host's Wi-Fi blocked access to ChatGPT, so some parts of the demo were shown with pre-computed results on GitHub instead. If anything, as John said, it shows the need for hosting the LLM locally. As for the demo, I'm impressed by how clean the webpage looks with Streamlit, as well as the details Langchain helped hide. I might give the LLMs a shot later, but I'll definitely use Streamlit as the frontend of my next data science project.

If you want to play with the code yourself, it's here:

[https://github.com/CoefficientSystems/chat-efficient/](https://github.com/CoefficientSystems/chat-efficient/)

### Open Source is Technically Illegal – [Casper da Costa-Luis](https://github.com/casperdcl)

Before you start looking for lawyers, Casper does start some good news: very rarely have these cases been brought to court. The most prominent and recent one is [a class action lawsuit against big tech companies scraping open source models for training their own models](https://www.theverge.com/2023/1/28/23575919/microsoft-openai-github-dismiss-copilot-ai-copyright-lawsuit).

In general, open source projects don't have any warranty, and it's hard to legally blame someone if it breaks. While one might to say they're just fine, some projects could be crucial to many others (like pandas), while others could only be open-source in name and highly affiliated to another close-source software.

The talk was quite ambitious for a quick talk, but raised a lot of good points. His opinions were something I can get behind: a developer shouldn't be sued for being the only person to maintain an open source project, but to break it without notice is a different story.

<!-- I originally wanted to put in "haha left-pad broke everything funny", but realised npm and a certain message app called Kik were at fault as well -->

### London Data Week - [Jennifer Ding](https://jending.com/)

As one of the organisers, Jennifer presents the first ever(!) [London Data Week](https://www.londondataweek.org/). Running from July 3rd to 9th, it is a London-wide festival, aimed at getting people involved in how data is collected and used. There are a variety of events throughout the week, from biking around London and recording cycling infrastructure, to holding workshops about how AI and art work together.

Overall the concept seems nice, and I would have joined the activities if I were in London during the weekends.

## The Chats

There was a quick break between the main talks, and a pub trip after all four. There was also some time before all talks but I only arrived in time to grab some pizza (yes there was free food).

### Sorry, a pub trip?

Yup, it's an official part of the itinerary, though some people do leave before that.

Walking to a nearby pub called the Banker, we continued chatting about ourselves and our fields. I met people with different backgrounds, all with their own experiences and perspectives. Being in a more casual environment allowed the conversation to flow more freely, where we discussed the tools we use, how one gets started in the field, and many more.

## TL;DR

So there were:

-   intriguing talks
-   interesting people -- during and outside of talks
-   free food
-   bonus (and optional) pub trip

What more could I have asked for?

I'll look forward to what these meetups have in store.

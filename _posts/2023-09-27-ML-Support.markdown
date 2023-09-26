---
layout: post
title:  Machine Learning Needs Support
date:   2023-09-27 22:00:00 +0100
categories: Python
---

I went to [Pydata London again](LINKTOOLDARTICLE) earlier this month, and listened to a wonderful talk by Michael Natusch about 10 ways to mess up a Machine Learning Implementation. After sitting on it for a month, I have come to this bold conclusion:

_(taps mic)_

Machine Learning isn't magic, and needs support from everyone else.

_(crowd gasps audibly)_

Shocking, right? Let me explain.

## Machine Learning isn't Magic

ML is a very powerful tool, and like all tools, it can be mishandled. Some examples of misuse include:

- Trying to predict random noise. For example, it is possible to [train a model to predict a coin flip given previous ones](LINKHERE), but clearly the model will only have a 50% accuracy (in expectation) with new data.
- Developing an unhelpful, or even harmful bias. For example, a simple model for classifying whether a transaction is a fraud could predict everything isn't a fraud. It will have a high accuracy, but will also be garbage for actual use.
- Being an overkill. For example, one could train a deep neural network predict a person's BMI from a picture -- successfully! But it's still no replacement for measuring your own height and weight, then crunching the numbers directly.
- Being used for the wrong purpose. For example, ChatGPT and LLMs in general are great for summarising and generating text, but [terrible at playing chess](LINKTOSTOCKFISHVERSUSCHATGPT).

## Why should support it?

## The Business

(Pt form for now)

- serve as main background context for the problems
- provide requirements
  - data sources
  - targets / desired behaviour
  - metrics (accuracy, possible risk)
  - constraints (e.g. explainable/interpretable, comply w/ regulations etc.)

## Data Engineers / SWE

- might be biased as DE myself
- model on its own does nothing, needs to be integrated into some software / pipeline
- build surrounding infrastructure
  - data fed in?
  - results output to where?
  - model where stored?
  - monitor?
- led to creation of MLOps and associated tools

## Users

## TL;DR

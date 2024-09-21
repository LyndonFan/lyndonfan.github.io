---
layout: post
title:  Machine Learning Needs Support
date:   2023-10-01 13:00:00 +0100
tags: Programming
---

I went to [Pydata London again]({% link _posts/2023-07-05-Pydata-London.markdown %}) in early September and listened to a wonderful talk by [Michael Natusch](https://www.linkedin.com/in/cumulyst/?originalSubdomain=uk) on 10 ways to mess up a Machine Learning Implementation. After sitting on it for a month, I have come to this bold conclusion:

_(taps mic)_

Machine learning isn't magic, and needs support from everyone else.

_(dead silence)_

Shocking, right? Let me explain.

## Machine Learning isn't Magic

ML is a powerful tool, but like all tools, it can be mishandled. Some examples of misuse include:

-   Trying to predict random noise. For example, trying to train a model to predict coin flips.
-   Developing an unhelpful or even harmful bias. For example, a simple model for classifying whether a transaction is a fraud could predict everything isn't a fraud. It will have a high accuracy but also be garbage for actual use.
-   Being an overkill. For example, one could (successfully!) train a deep neural network to predict a person's BMI from their profile picture. But it's still no replacement for measuring their height and weight and crunching the numbers.
-   Being used for the wrong purpose. For example, ChatGPT and LLMs are great at summarising and generating text, but [terrible at playing chess](https://www.reddit.com/r/AnarchyChess/comments/10ydnbb/i_placed_stockfish_white_against_chatgpt_black/).

## What do you mean by "support"?

Context.

For example, a model that classifies images into cats or dogs doesn't know what a "cat" is, it just learns sets of features within the array that represent the image which will make it more likely to be "label 0", or however the classification is performed. I could swap all the labels in the dataset and the model won't bat an eye.

We sometimes say a problem "happened out of nowhere", but it's usually wrong. It occurred at a certain time and place, in a particular situation, preceded by a bunch of decisions and mistakes which led to it. Our job is to provide the surrounding info so the people involved with the model can make better improvements.

## The Business

Unless you're working in research and solely focused on beating state of the art benchmarks, machine learning models built in a company ultimately have to serve the business. They serve as the main background for all of the problems the model has to solve. The business team can help provide or adjust what to expect from the model:

-   What should the model do? This should be a mix of the business goals as well as customer needs.
-   How do we measure its success? Note this may differ from the model's metric for training/evaluation. For example, LLMs may be trained from reinforcement learning which has its own set of metrics, but the business only cares about how natural and factual the model's responses are.
-   What resources do we get? The most important one is the sources of data, but also the time, manpower, and budget.
-   Are there constraints to work around? For example, the model has to be deployed in multiple places, but the database can't be uploaded to a central server due to data protection laws.

## Software Engineers / Data Engineers

I may be a bit biased as a data engineer myself, but a plain old model on its own won't do any good -- it needs to be put into some software to see it in action. Here's where I or other data/software engineers come in:

-   How do we store and process the data for **<u>training</u>** the model? This is usually fine if the model is small, but could be a separate issue for a larger task.
-   Where does the model fit into existing pipelines?
-   How do we store and process the data going into and out of the model?
-   Where is the model stored? How do we deploy it?
-   How should we monitor its performance? Should we re-train it, and if so, when?

The last couple of questions have delved a bit into DevOps, which is also the reason MLOps has become a new title for some engineers.

## Data Scientists / ML Engineers

Of course, we can't forget the people who have to train the model themselves. Usually, it's data scientists / ML engineers, and they are in for a wild ride. They're responsible for:

-   Translating the business needs into the requirements for a model, or determining that a model isn't needed
-   Exploring the data. Or if the data doesn't exist, collect it
-   Building, training, and testing various models
-   Explaining what machine learning is to others, and what its limitations are (comes up more often than you think)
-   Communicating their their needs and requirements to all the stakeholders above

It's a lot!

## TL;DR

-   Machine learning isn't magic and can't solve your problems automatically
-   Machine learning is hard and requires many people to cooperate
-   Care for your local data scientist ❤️

Thank you.

_(polite claps, you know, like when you finish a presentation at school)_

---
layout: post
title:  Machine Learning Needs Support
date:   2023-10-01 22:00:00 +0100
categories: Python
---

I went to [Pydata London again](https://lyndonfan.github.io) early September, and listened to a wonderful talk by Michael Natusch about 10 ways to mess up a Machine Learning Implementation. After sitting on it for a month, I have come to this bold conclusion:

_(taps mic)_

Machine learning isn't magic, and needs support from everyone else.

_(dead silence)_

Shocking, right? Let me explain.

## Machine Learning isn't Magic

ML is a very powerful tool, and like all tools, it can be mishandled. Some examples of misuse include:

-   Trying to predict random noise. For example, it is possible to [train a model to predict a coin flip given previous ones](LINKHERE), but clearly the model will only have a 50% accuracy (in expectation) with new data.
-   Developing an unhelpful, or even harmful bias. For example, a simple model for classifying whether a transaction is a fraud could predict everything isn't a fraud. It will have a high accuracy, but will also be garbage for actual use.
-   Being an overkill. For example, one could train a deep neural network predict a person's BMI from a picture -- successfully! But it's still no replacement for measuring your own height and weight, then crunching the numbers directly.
-   Being used for the wrong purpose. For example, ChatGPT and LLMs in general are great for summarising and generating text, but [terrible at playing chess](LINKTOSTOCKFISHVERSUSCHATGPT).

## What do you mean by "support"?

Context.

For example, a model that classifies images into cats or dogs doesn't know what a "cat" is, it just learns sets of features within the array that represents the image which will make it more likely to be label 0, or however the classification is performed. I could swap all the labels in the dataset and the model won't bat an eye.

We sometimes say a problem "happened out of nowhere", but it's usually wrong. It occured at a certain time and place, in a particular situation, preceeded by a bunch of decisions and mistakes which led to it. Our job is to provide the surrounding info so the people involved with the model can make better improvements.

## The Business

Unless you're working in research and solely focused on beating state of the art benchmarks, machine learning models built ina company ultimately have to serve the business. They serve as the main background for all of the problems the model has to solve. The business team can help provide or adjust what to expect from the model:

-   What should the model do? This should be a mix of the business goals as well as customer needs.
-   How do we measure its success? Note this may differ from the model's metric for training/evaluation. For example, LLMs may be trained from reinforcement learning which has its own set of metrics, but the business only cares about how natural and factual the model's responses are.
-   What resources do we get? The most important one is the sources of data, but also the time, manpower, and budget.
-   Are there constraints to work around? For example, the model has to be deployed in multiple places, but the datause dat can't be uploaded to a central server due to datap rotection laws.

## Software Engineers / Data Engineers

I may be a bit biased as a data engineer myself, but a plain old model on its own won't do any good -- it needs to be put into some software in order to see it in action. Here's where me or other data/software engineers come in:

-   How do we store and process the data for <u>**training**</u> the model? This is usually fine if the model is small, but could be its separate issue for a larger task.
-   Where does the model fit into existing pipelines?
-   How do we store and process the data going into and out of the model?
-   Where is the model stored? How do we deploy it?
-   How should we monitor its performance? Should we re-train it, and if so, when?

The last couple questions have vouyered a bit into DevOps, which is also the reason MLOps has become a new title for some engineers.

## Data Scientists / ML Engineers

Of course, we can't forget the people who have to train the model themselves. Most often it's data scientists / ML engineers, but in general these people are in for a wild ride. They're responsible for:

-   Converting the business needs into a possible model, or determining that a model isn't needed
-   Exploring the data. Or if the data doesn't exist, collect it
-   Building, training, and testing various models
-   Explaining what machine learning is to others, and what its limitations are (comes up more often that you think)
-   What they

## TL;DR

-   Machine learning isn't magic and can't solve your problems automatically
-   Machine learning is hard and requires many people to cooperate
-   Care for your local data scientist :heart:

Thank you.

_(polite claps, you know, like when you finish a presentation at school)_

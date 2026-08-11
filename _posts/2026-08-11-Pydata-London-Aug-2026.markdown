---
layout: post
title:  "Pydata London Meetup -- August 2026"
date:   2026-08-12 23:30:00 +0100
tags: Programming Meetups
---

A quick one about last week's PyData! I'll go over what they are, some thoughts, and other random things.

_All summaries and any mistakes are mine._

## Linking -- together -- information

[Sefik](https://www.linkedin.com/in/serengil/) talked about using Graph RAG to better information retrieval.

Retrieval Augmented Generation (RAG) is a technique of helping LLM get better answers by giving it documents to search for. To do so, you first have to chunk the documents into smaller units (paragraphs / sentences), store them in a vector database. Then when you get a query / prompt, you find related chunks (e.g. by cosine similarity), and feed them to the LLM for more information.

Knowledge graphs, like neo4j (where Sefik works at), can store facts. Each node can be an object, and edges are relationships, e.g. London is the capital of the United Kingdom. 

So how are they related?

A drawback of RAG is when chunking, you lose information on which chunks come from the same document -- chunks 1 of document A can be as _unrelated_ as chunk 2 of document A or chunk 1 of document B. But with graphs, you can do:

1. When chunking the document, extract nodes and relationships from the words. Also link them to the chunk.
2. When querying relevant chunks, also return the nodes and relationships. Use them to construct a knowledge graph.
3. Give the LLM tools to query on the graph.

Volia, you can get more context, and more grounded answers!

This sounds clever and nice in theory! He said it's working well in practice, and [the GraphRAG website](https://graphrag.com/) could be a good place to learn more.

## Training with Privacy

[Rafael](https://www.linkedin.com/in/garcia-dias/) presents his project on [FLIP](github.com/londonaicentre/FLIP), a platform for Federated Learning for healthcare.

Healthcare can benefit a lot from AI models, but there are (thankfully) data privacy laws meaning patient data can't be sent to places ad hoc. So what if instead of sending data, we send the model _to_ the data? That's Federated learning, and FLIP is a platform to do this. It is mainly for medical imaging AI models, while also ensuring privacy since patient data never leaves the hospitals.

He went through the design quickly, but some points that caught my eye were:
- Privacy is ensured by only sending model weights / updates instead of data.
- Putting the idea into production is hard!
- Federated learning isn't fully privacy preserving. He also linked to a case to Gboard (but I can't find quickly).

This looks like a massive and intriguing project, and I hope it goes well.

## "You" is hard in Korean

[Chaerin](https://www.linkedin.com/in/chaerinkwon-09800ab7/) explained why "you" is hard to translate in Korean.

The TL;DR is Korean cares about gender of the speaker, the other person, their relationship, _and_ formality. When translating from English into Korean, often there isn't enough information to do so.

An example I came up with is the below. Passing to [Google translate](https://translate.google.co.uk/?sl=en&tl=ko&text=John%20asked%20his%20brother%20Leo%2C%20%22Brother%2C%20do%20you%20know%20where%20the%20key%20is%3F%22&op=translate), we have:

| English | Korean |
| ------- | ------ |
| John asked his brother(a) Leo, "Brother(b), do you know where the key is?" | 존은 동생(a) 레오에게 "형(b), 열쇠 어디 있는지 알아?"라고 물었다. |

I annotated where "brother" appears in the sentences -- they are different! Since I didn't specify who is older. 형 / hyung is when a male refers to an older male, and 동생 is the other way around.

(This _is_ a bit contrived. If Leo is younger, it might be "야, 열쇠 어디 있는지 알아?" It's changing from "Brother", to "Hey", which is less technically correct but is more realistic.)

As someone who has watched Korean variety TV shows, I never put two and two together! From the shows, I see hierarchy matters a lot. But the subtitles are _from_ Korean to English, and the translators don't translate the words into "you". They keep 형 as hyung and 누나 as noona, which I think better portrays the relationships between the people. Translating well and faithfully still requires care (or at least more context).

<!-- you can tell I'm more invested by the length I put into this! -->

## Emily, where is my...?

[Peter](https://www.linkedin.com/in/peterjbleackley/) walked us through Emily, an information retrieval system.

A main feature was reranking results based on a "cooccurrence index", i.e. "Do two words from the query occur in same sentence with a statistically significant frequency in the target document?"

Re-reading [the associated article](https://petebleackley.github.io/Emily/), it is a good construct to solve a technical problem, and has an easy analogy/intuition.

I must also highlight it uses Polars for ~~sanity~~ performance. 

## Pub!

As usual, there are drinks at the Banker! It was great chatting to others, ranging from first timers and students, to regulars and organisers. I learnt more about their thoughts on the talks, backgrounds, and opinions on UK trains.

## Call to action

No, I'm not telling you to subscribe (but it'll be nice :upside_down_smile:)

More urgently, [Pydata London is looking for sponsors and venues!](https://www.linkedin.com/posts/pydata-london_datascience-python-opensource-activity-7492544535781109760-JcT0)

They are looking for sponsors to fund the meetups, and venues to host the meetings (of >= 150 people).

I only attend them semi-regularly, but each time they have shown me something new, interesting, or even useful at work. Even re-reading [thoughts from my first attendance]({% link _posts/2023-07-05-Pydata-London.markdown %}) I was quite stoked about it. <!-- a bit cliche, I know -->

So... see you in September?
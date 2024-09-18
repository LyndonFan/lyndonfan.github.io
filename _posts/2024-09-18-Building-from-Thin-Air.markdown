---
layout: post
title:  "Building from Thin Air is Hard"
date:   2024-09-18 22:15:00 +0100
categories: Programming
---

<p style="text-align:center">~~or "Ramblings on Building a CRUD+ App"~~</p>

A couple of weeks back, I promoted [Aether, a note taking app that includes semantic search]({% post_url 2024-08-28-Aether-Note-Taker %}). Here's what happened along with some thoughts.

## Week 0

In late July, I received an invitation email out of the blue from [Backdrop Build](https://backdropbuild.com/). Peeking at their website, it was full of people prototyping their apps and products, and seeking for investors. I wasn't (and am not) looking to build my own, but it was a good excuse to commit to creating something. I cobbled together an application, stayed vague about my plans, and picked an abstract product name: "Aether".

## Start

As August rolled around, I was notified of my successful application. Now I _actually_ need to come up with an idea.

My brainstorming session involved various ideas I've scribbled on notes, and seeing what could be done with AI as is the trend. Here are some of them:

- A note taker where users can write their notes in markdown (like I usually do), and the app can generate summaries. It can also search for notes based on the meaning of the user's query.
- A todo list with direct calendar integration that prioritises your tasks based on your behaviour.
- A dependency board that's actually readable, with clickable links in distinct colours, and tracking when notes are moved across sprints.

I evaluated the ideas based on whether I could reasonably build them in a month. I don't really have a clue how to build the todo list: how would I know the user's priorities? The dependency board one was doable, but it felt too much frontend which I wasn't in the mood for. So a note taker it was.

I wanted this project to work out, so I did more planning than usual (which is none). I listed out the features it would have, from frontend to backend. Factoring in my existing schedule, I broke down the month into weeks (hence the format of this post), estimated the time it'll take to build the features, and built a rough timeline for how I should complete the project.

And I can definitely follow it to a tee.

...Right?

## Week 1

The first order of business was to build the app's front end. I googled around for layout ideas since design isn't my strong suit. Fortunately / Unfortunately, I immediately found [takenote.dev](https://takenote.dev/):

![Screenshot of TakeNote.dev Demo](/assets/img/20240918_takenotes_dev.png)

Its design was perfect. _Too perfect._

Curiosity got the better of me anyway, so I peeked into its source code and found it used the [CodeMirror](https://codemirror.net/) component for its markdown. At least I'm not recreating it from scratch. To ~~make myself feel less guilty~~gather more sources of inspiration, I looked to [SimpleNote](https://simplenote.com/), my usual app for taking notes, for its colour scheme and what to include in the sidebars.

Now with a rough sketch of the visuals, I "just" had to code it up. I haven't used React in a while, so I used ChatGPT with [Grimore](https://chatgpt.com/g/g-n7Rs0IK86-grimoire) to set up the files and basics. After integrating with the CodeMirror component and using local storage to save the notes, I had a functional app.

## Week 2

The next week was for making the app "intelligent", i.e. how to extract meaning/summarise the user's **markdown** text.

Naturally, I went to HuggingFace and browsed for models under "text summarisation". I first tried its capabilities with the initial paragraphs of the [2024 Paris Olympics on Wikipedia](https://en.wikipedia.org/wiki/2024_Summer_Olympics), then with markdown notes I took during a PyData London event.

![Screenshot of TakeNote.dev Demo.](/assets/img/20240918_model_results.png)
*Model results on the wiki article and markdown notes. See the full log [here](/assets/20240918_model_test_notes.log)*

They all did alright with the article, but spit gibberish on markdown notes I made.

I also tried some LLMs, but my simple MacBook Pro couldn't handle them. 

Frustrated, I took a break from researching AI and returned to the frontend. I went ahead of schedule and implemented some features, such as importing from the clipboard and multiple files.

## Week 3

This week was set for integrating the frontend and backend, but I still don't have a backend yet!

Luckily, Python is my strong suit, and building a backend for a CRUD app is simple. I picked FastAPI with SQLite: the former as a library I wanted to try out and seemed to have a smoother experience than Flask, the latter as a persistent database without spinning up a MySQL / Postgres instance. I _did_ cheat by using AI for the basic CRUD options, which was helpful with the boilerplate needed for SQLAlchemy.

Nonetheless, I made multiple changes when I wanted the app to save the user changes live and had to use websockets. It was my first time building with them -- I'd only seen it before in a company project. I initially had the `/update/<id>` route create a websocket and recreate it whenever the user switches notes. I don't think it's best practice? This seems like opening and closing too many sockets. So I modified it to create only a single websocket and send messages to update which note to edit.

At last, I could put the halves of the project together!

I also added regular exact search and regex search. Continuing the momentum, I investigated the semantic search feature again, this time landing in NLTK, a more "traditional" NLP library. While it was too clunky, this route led me to vector databases, and I discovered [ChromaDB](https://docs.trychroma.com/). Plugging it in, it just worked!

Finishing this feature, I took the weekend off for a trip (it was planned anyway).

## Week 4

Feeling refreshed after my getaway, I dedicated the week to crushing bugs and polishing the product.

This meant improving the layout and refactoring some frontend code. I was using feature branches throughout the weeks, but they came in handy to stop me from breaking the code and being unable to recover from it. I also removed SQLite and SQLAlchemy and went all in on ChromaDB: it seemed to be using SQLite under the hood anyway, and the data is small enough that looping with raw Python is still performant.

The week remained relatively uneventful until I turned on GitHub Advanced Security out of boredom to see what vulnerabilities it picked up on. It immediately warned me about "regular expression injection", since I was taking user input in the search bar and using it as the regex to search for in text. Obviously, this was intended, so I gleefully dismissed the alert with a message which I could have never written at work:

[![Screenshot of me dismissing the Github security alert, with the message: "This is a feature to allow users to search by regex, so is not a bug."](/assets/img/20240918_github_message.png)](https://github.com/LyndonFan/Aether/security/code-scanning/3)

So basically, "This is a feature, not a bug" 🙃

## Week 5

The last week was dedicated to polishing, packaging up the app, and creating the demo.

I was almost out of time at the end of the month -- thank goodness for the bank holiday. After squashing more bugs, I took a proper look at making this easier to run than just starting two programs in separate terminals. I briefly looked at putting this into an Electron app, but it wouldn't have worked as I still need to spin up the database. Instead, I conceded to just making this work with docker-compose.

This still means I've got to make the containers to build. This process took more effort and time than I thought -- literally, I spent too many minutes waiting for the docker build job to finish. Eventually, I found a better way to repackage the files (especially the frontend), and at least it was bearable. I was less familiar with the networking, so ~~cheated~~conceded and just put everything on the host network.

Finally, I could start making a demo!

Drafting the big bullet points for the script was easy, with the main idea being I would do everything in the app: from demonstrating the features to the actual presentation itself. The main challenge now meant I had to find an example to showcase the usual CRUD features, and also the semantic search, so I needed more than a few notes.

Knowing one of my friends has been reading books on Hong Kong history and jotting meticulous notes, I asked if I could borrow them for my demo. Intrigued, he agreed and sent me a docx file. After separating them into separate markdown files (like how I imagine he would have used this app), I presented it back to him along with a small example I found. He then requested more specific but much more interesting examples:

- "Comprador", or middleman for foreign governments, is worded differently in various sources. The app found all of them, even sorting the posts by relevancy.
- "Chinese culture" is more than it says on the tin. It could refer to specifically how the Chinese government wants to promote its "soft power" i.e. culture worldwide, and not unintentionally through pop stars singing Chinese songs. The app lacked this context so couldn't distinguish them

Going through this rabbit hole reminded another friend of [Semantle](https://semantle.com/), a wordle variant based on how close your guess was to the target based on word2vec embeddings. It was both delightful and hair-tearingly frustrating once we spoiled how close our guesses were. (I'm getting sidetracked by this as I'm writing.)

While I enjoyed the positive response, the example was too niche and foreign for me to explain. So I scoured the net and found something slightly more familiar but just as detailed: notes on AWS services. I found an example of "security" relating to IAM, but the document didn't include the term as often. And after a few trials of recording the video, I finished it in one take, uploaded it, and called it a day.

## Final Thoughts

First of all, yay me for pushing something out to the world! The CRUD app but with AI magic is probably crappy and unoriginal, but at least I came up with the idea myself and worked a good part of it.

This also sold me on the practice of using git for personal projects. Not only can you put it on GitHub, but you can also get the benefits of using version tracking in general, e.g. feature branches so you can focus on one thing at a time. Having the history also means tracking your progress over time, so you can look at what steps you took.

Another obvious lesson was building and completing a project is **hard**. Ideas are cheap, but bringing them alive is a lot of work -- it took me a month just for a relatively straightforward project. That said, I would have benefited from better discipline (I procrastinated a lot throughout the weeks), and some motivation or emotional investment beyond just completing a thing.

Speaking of work put in, I've definitely felt a bit guilty leaning on AI tools more than I like. I'm internally justifying this by (a) there's an external deadline, and (b) I could have written the code albeit with more time. That said, it wasn't all copy/paste, or "tab" all the way. The tools I'm using (ChatGPT and [Codeium, basically a free CoPilot](https://codeium.com/)) were great at repetition with minor variations (think similar functions but for a different field), or sections with simple requirements (e.g. basic CRUD app which has been done to death). The tools need a bit more hint in prompting; when it fails I just write the bloody thing myself.

Overall, I'm glad I made something. Would I do this again? Very likely, but I'd prefer having an idea, a better plan, and more time.

_So... Waterfall?_

That's not what I meant.
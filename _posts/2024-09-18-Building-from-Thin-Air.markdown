---
layout: post
title:  "Building from Thin Air is Hard"
date:   2024-09-18 23:50:00 +0100
categories: Programming
---

<p style="text-align:center">~~or "Ramblings on Building a CRUD+ App"~~</p>

A couple of weeks back, I promoted [Aether, a note taking app that includes semantic search]({% post_url 2024-08-28-Aether-Note-Taker %}). Here's what happened during the month as I built it, and some other thoughts.

## Week 0

In late July, I received a email of out of the blue from [Backdrop Build](https://backdropbuild.com/), saying they were organisaing a hackathon of sorts and I was invited. Intrigued, I peeked at their website: it was full of people prototyping their apps and product ideas, and looking for investors. I wasn't (and am not) looking to start my own product, but it was a good excuse for me to commit to creating something. So I cobbled together an application with a few words about myself, stayed vague on what I was about to build, and picked vague product name, which was "Aether".

## Start

As August rolled around, I was notified of me successfully getting into the competition.

Now I _actually_ need to come up with an idea.

I started brainstorming, coming up with various ideas I've scribbled on pieces of notes, and with AI. Some of them were more useful than others, but here were some of the ideas:

- A note taker where users can write their notes in markdown (like how I usually do), and the app can generate summaries. It can also search for relevant pieces of text based on the meaning of the user's query.
- A todo list manager but it integrates with your calendar and can prioritise your tasks based on your behaviour.
- A dependency board that's actually readable and emulates more than sticky notes on a whiteboard. The links are visible and don't share similar colours, and when a note gets moved across sprints an explanation is left as record.

I looking at these ideas, and evaluated them based on which ones I could reasonably build in a month's time. The second one I don't really have a clue how to start it: the todo list is dead easy, but how would I know the user's priorities? The third one was something I could build, but it felt too front-end and I wasn't in the mood for that. So a note taker it was.

I really wanted this project to work out, so did more planning than usual (which is none). I listed out the features it would have, from front-end to back-end. Factoring in my existing schedule and activities, I broke down the month into weeks (hence the format of this post), estimated the time it'll take to build the features, and buitl a rough timeline for how I should complete the project.

And I could definitely sure I can follow it to a tee.

...Right?

## Week 1

First order of business was to build the app's front end. I googled around for layout ideas, since design isn't my strong suit. Fortunately / Unfortunately, I immediately found [this website](https://takenote.dev/):

SCREENSHOT

Its design was perfect. _Too perfect._

Curiosity got me anyways, so I peeked into its source code and found it used the [CodeMirror](https://codemirror.net/) component for its markdown. At least I'm not recreating it from scratch. To ~~make myself feel less guilty~~gather more sources of inspiration, I looked to [SimpleNote](https://simplenote.com/), my usual app for taking notes, for its colours scheme and what to include in the sidebars.

Now that I have a good idea of what the app should look like, the main thing was to code it up. While I do know React, I haven't used it in a while, so instead I used ChatGPT with the [Grimore](LINK) GPT to help me set up the files and basics. After integrating with the CodeMirror component and using local storage for saving the notes, I had a functional app.

## Week 2

The next week was planned for figuring how how to make the app "intelligent", i.e. how to extract meaning / summarise the user's **markdown** text.

Naturally, I went to huggingface and browsed for models under "text summarisation". I also tried some using some LLMs by giving it the prompt of "Summarize the text below, which may be in markdown. Only output the summary."

I experimented with two pieces of text. One was the initial paragraphs of the [2024 Paris Olympics on Wikipedia](https://en.wikipedia.org/wiki/2024_Summer_Olympics), both of which did alright

EVIDENCE

The other were some notes on AWS EC2 which was in markdown. The text summary models did a terrible job, with LLMs doing just fine or were unable to run on my machine for some reason.

EVIDENCE

So neither of them seemed like good options.

Frustrated, I took a break from researching AI and went back to frontend. I went ahead of schedule and implemented some features, such as importing from clipboard and from multiple files.

## Week 3

This week was originally planned for putting the frontend and backend together, but I still don't have a backend yet!

Luckily, Python is my strong suit, and building a backend for a CRUD app is quite simple. I picked FastAPI with SQLite as my stack: the former as a library which I wanted to try out and seemed to have a smoother experience than Flask, the latter as persistent database but without spinning up a MySQL / Postgres instance. I _did_ cheat by using AI for the basic CRUD options, which was especially helpful since there was plenty of boilerplate to set up with SQLAlchemy.

Nonetheless, I had to make multiple changes when I decided to have the update route to use websockets, since I wanted the app to save the user changes as they are being made live. It was my first time building with websockets -- I'd only seen it before in a company project. I definitely fumbled a bit on them, and initially had the `/update/<id>` route create a websocket, and recreate it whenever the user changes notes. I don't think it's best practice? This seems like . So I modified it to create only a single websocket, and send messages to update which note to edit, which made the experience a bit better.

At last, I could integrate both halves of the project!

I also added some searching features, starting with regular exact search and regex search. I continued the momentum investigated the semantic search feature again, this time landing in more "traditional" NLP such as NLTK. This route led me to vector databases, and I discovered [ChromaDB](https://docs.trychroma.com/). Plugging it in, it just worked!

Finishing this major feature, I took the weekend off for a trip (it was planned anyways).

## Week 4

Feeling refreshsed after my getaway, I dedicated the week to crushing bugs and start polishing the product.

This meant improving the layout and refactoring some frontend code. I was using feature branches throughout the weeks, ubt they came in handy to stop me from breaking the code and unable to recover from it. I also removed SQLite and SQLAlchemy entirely, and went all in on ChromaDB: it seemed to be using SQLite under the hood anyways, and the data is small enough that looping with raw Python is still performant.

The week remained relatively uneventful, until I turned on Github Advanced Security. It was something I had at work, and was curious to see what vulnerabilities it picked up on. It immediately warned me about "regular expression injection", since I was taking user input in the search bar and using it as the regex to search for in text. Obviously, this was intended, so I gleefully dismissed the alert with a message which I could have never written at work:

[![Screenshot of me dismissing the Github security alert, with the message: "This is a feature to allow users to search by regex, so is not a bug."](/assets/img/20240918_github_message.png)](https://github.com/LyndonFan/Aether/security/code-scanning/3)

So basically "this is a feature, not a bug" 🙃

## Week 5

The last week was dedicated to polish, actually packaging up the app, and creating the demo.

It didn't take long to realise I'm almost out of time at the end of the month -- thank goodness for the bank holiday. I tooked the day to squash more bugs, and properly look at making this easier to run than just starting two programs in separate terminals. I briefly looked at putting this into an Electron app, but it wouldn't have worked as I still need to spin up the database. Instead, I conceded to just making this work with docker-compose.

This still means I've got to make the containers to. I Even just building them took more effort and time than I thought -- literally, I spent too many minutes waiting for the docker build job to finish. Eventually I found a better way to repackage the files (especially the front-end), and at least it was bearable. The networking was also something I'm less familiar with, so I ~~cheated~~coneded and just put everything on the host network.

Finally, I could start making a demo!

Draftin the big bullet points for the script were easy, with the main idea being I would do everything in the app itself: from demonstrating the features, to the actual presentation itself. The main challenge now meant I had to find an example to showcase the usual CRUD features, and also the semantic search, so I needed more than a few notes.

Knowing one of my friends has been reading books on Hong Kong history and jotting meticulous notes, I asked if I can borrow them for my demo. Intrigued, he agreed and sent me a docx file. After separating them into separate markdown files (like how I imagine he would have used this app), I presented it back to him along with a small example I found. He then requested more specific but much more interesting examples:

- "comprador", or middlemen for foreign governments, is worded differently in various sources. The app was able to find all of them, and got the relevancy right when posts were sorted by distance.
- "Chinese culture" is more than it says on the tin. It could refers to specifically how the Chinese government wants to promote it "soft power" i.e. culture to around the world, and not unintentionally like through pop stars signing Chinese songs. The app lacked this context, so returned results for both without much distinguishing.

Going through this rabbit hole reminded another friend of [Semantle](https://semantle.com/), a wordle variant but based on how close your guess was to the target based on word2vec embeddings. It was both delightful and hair-tearingly frustrating once we spoiled how close to the answer we got. (I was sidetracked by this as I'm writing up this post.)

While I enjoyed the positive response, the example was a bit too niche and foreign for me to explain. So I scoured the net awnd found something slightly more familiar but just as detailed: notes on AWS services. I split up the single markdown document into sections, came up with an example on "security" relating to IAM but not turning up as much in the document. And after a few trials of recording the video, I finished it in one take, uploaded it, and called it a day.

## Final Thoughts

First of all, yay me for pushing something out to the world! The CRUD app but with AI magic is probably crappy and unoriginal, but at least I came up with the idea myself and worked a good part of it.

This also sold me on the practice of using git for personal projects. Not only can you put it on github, you also get the benefits of using version tracking in general, e.g. feature branches so you can focus on one thing at a time. Having the history also means tracking your progress over time, so it's more fond to look back on (and helped me write this blog 2 weeks after I published the project).

Another obvious lesson I learnt is building and completing a project is **hard**. Ideas are cheap, actually bringing them alive is a lot of work -- it took me a month just for a relatively straightforward project. That said, I would have benefited from better discipline (I procrastinated a lot throughout the weeks), and some motivation or emotional investment beyond just completing a thing.

Speaking of work put it, I've definitely felt a bit of guilt when using the AI tools for this project. It's more than I like, but I'm internally justifying this by (a) there's an external deadline, and (b) I could have written the code though with more time. That doesn't mean ChatGPT spat the whole thing out though, I still had to change bits and bobs. The tools I'm using (ChatGPT and [Codeium, basically a free CoPilot](https://codeium.com/)) were great at repetition with minor variations (think similar functions but for a different field), or common sections with simple requirements (e.g. basic CRUD app which has been done to death). The tools need a bit more hint in prompting, and when it fails I just write the bloody thing myself.

Overall, I'm glad I made something. Would I do this again? Very likely, but I'd prefer having an idea and better plan to begin with, and a longer deadline.

_So... Waterfall?_

That's not what I meant.
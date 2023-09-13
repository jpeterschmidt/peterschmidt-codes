---
title: "The art of reading other people's code"
excerpt: 'Slides & commentary from my talk at the 2023 Indy.Code conference in Indianapolis, IN.'
date: '2023-09-13T05:35:07.322Z'
coverImage: '/assets/blog/decoding/cover.jpg'
author:
  name: Jacqui Peterschmidt
  picture: '/assets/blog/authors/jacqui.png'
ogImage:
  url: '/assets/blog/decoding/cover.jpg'
---

Writing code is hard; reading it is harder. I originally gave this talk at the 2023 Indy.Code conference in Indianapolis, IN. I've made some modifications to the slides and content to make it suitable for a blog post.

![XKCD #1695 - Code Quality](https://imgs.xkcd.com/comics/code_quality_2.png "XKCD #1695 - Code Quality")

## Why bother?

A year ago (as of September, 2023) we would have all agreed that all software engineers need to learn how to read code. It's an essential part of the job, we would have said, and no one would get very far without that skill. Now, of course, the field has changed quite a bit with the advent of AI tools that do at least an okay job of explaining code in plain English.

I'm not saying those tools aren't helpful, because they are; they can be an incredible asset in understanding and working with the very unfamiliar or esoteric. However, they have drawbacks today:
- ▶ they're slow
- ▶ they're not ultra reliable
- ▶ they're not deterministic
- ▶ they hallucinate
- ▶ hey're expensive

Those drawbacks are getting smaller every day, but they're still there. But even as the tools get better, we as humans can get better at asking questions and presenting the right context to the tool to get better answers.

And, call me old fashioned, but I think kids these days should just have to learn to read code, like we did back in the day, uphill both ways in the snow.

## Read the docs (or not)

There's tons of advice and writing out there that suggests that to understand a new library or codebase, you should read the relevant documentation. I do think that documentation has a place, but here's the thing (and we're starting out this blog with a Really Hot Take): if you have any doubt about the quality of the documentation (and you probably should have at least some doubt), skip the docs and read the code.

I'm sure we've all read poorly written, poorly maintained documentation. Most of us have probably _written_ poorly written, poorly maintained documentation. The fact of the matter is that the code is the single source of truth. We as engineers always have to do step one, write the code, and then step two, document the code, is fundamentally optional. And, as we all know, optional things don't always get done.

## Where we're at
The scope of the problem I'm trying to provide some guidance on boils down to "you have to read code," but there are some cases I'm not addressing head on. Those are times when someone who does understand already is willing and able to walk you through the code and answer any questions you have. Reading the code is still helpful in that case, but it's not quite the same task as when you don't have help and help isn't coming.

Maybe...
- ▶ the original author is unavailable
- ▶ the original author is unwilling
- ▶ you're the author and you forgot what you wrote last year
- ▶ you're using an unfamiliar library
- ▶ you're using a familiar library in an unfamiliar way
- ▶ you're new to the codebase and need to get started somewhere
- ▶ some post-apocalyptic scenario has occurred and you're the only one left who knows how to code

So, without further ado, let's get into the strategy.

## Step One: Why are you here?
Really think about what your goal is. Nine times out of ten, you're not going on a nice scenic sightseeing tour of a repository. What do you hope to get out of reading whatever you're looking at? Maybe you have a card or a story to work on, maybe you're fixing a bug, maybe you're trying to get some kind of technical reference so you can copy the prior art, maybe this is you first week or month at the job and you have to start somewhere.

_Know what you're doing, know where you are, and keep that goal in mind._

One of the pitfalls of digging into something unfamiliar is curiosity. If we get too curious about how things work, we get way off track, then have to explain to our product manager why that bug still isn't fixed. Ask me how I know.

## Step Two: Find somewhere to start
It'll make you life easier if you start reading in the right spot. I suggest either somewhere important, like
- ▶ the function you're trying to call
- ▶ the controller for the REST API you're trying to use
- ▶ the file where the exception you're trying to fix is thrown

Or, if you're new to a codebase or concept, try starting somewhere safe, like
- ▶ the core types (if you're using types)
- ▶ the database definition

No one ever went wrong by trying to understand the domain first! Even if you don't understand the code, you can try to understand what it's attempting to do.

## Step Three: Clip out-of-bounds and skip the level
Sometimes, you really don't need to understand what's happening. Sometimes, you can just guess.

If you remember your goal from Step One, it's very rarely going to be "understand completely everything that's going on." Frequently you can skip understanding completely, as long as you can get to working software without understanding.

To do that, I typically read the tests and/or the public method names. I also love searching the codebase for an example of a function in use; that usually gets me closer to working code than simply writing from scratch.

There's no toll or fee for trying something and failing fast, and I can almost guarantee that you'll know more than you did before you tried.

I'll tell a quick story on this front. In my first six weeks at Vendr, I was asked to optimize a function that took entries in accounting books and transformed them into information about what software a customer of ours bought. Something important about me is that I took exactly one accounting class in college and I did not care for it one bit. A coworker gave me a file name, outlined the problem, said "let me know if you need help," then went on vacation.

The problem, as it was laid out to me, was that there was _Something_ that caused the runtime of the function to grow exponentially with the number of accounting events we wanted to process. You can imagine why this is a bad thing. My job was to make that not happen anymore.

This is in direct contrast to what I said in Step Two. For the first week or so, I tried to understand what we wanted to do with accounting events. What happened? What data was the customer interested in? I scoured that code, the documentation, even the product for hints about what was supposed to happen. At the end, I understood everything better, but I wasn't any closer to solving the problem.

When I gave up on understanding what accountants do (witchcraft, probably), I found and fixed the problem fairly quickly by setting breakpoints and stepping through the code until I found the exact line that caused the problem. It was simply not important to know what was happening in the whole context of the file - all that was really important was fixing that one line.

That was a learning experience for me, and one I remember every time I start clicking around in a domain I'm not familiar with.

My hints to skip to working software are:
- ▶ read the tests
- ▶ run the tests
- ▶ write a failing test
- ▶ run the code with a debugger
- ▶ *just try something, even if it's really stupid*

## Step Four: Start reading - most important first
So far we haven't done any reading of code, which is a little weird for a blog post about reading code. But, we set goals, set non-goals, found a place to start, and decided we couldn't skip the whole process. Now, despite our best efforts, we do in fact have to read something. This is the hardest part!

My number one suggestion is to take notes, preferrably in line, in the code, in a comment. Adding comments will help you in the moment and help others in the future; if you don't understand it, it's not unlikely that other people also won't understand it.

Another strategy I use *heavily* at Vendr is what we call "rubber-ducking." The process is explained better [here](https://twitter.com/DuncanMalashock/status/1480548481261613062) by my manager Duncan Malashock. I couldn't live without rubber-ducking now, and I'm not sure how I ever got by without it.

The third big thing is getting comfortable with your IDE. Most (all?) modern IDEs come with the ability to command- or control-click into methods, and it takes you right to the declaration of that method without having to search around manually. My one caution is that when you get comfortable with cmd+click, also get comfortable with the back button. It's easy to get lost in the weeds and forget where you came from.

And fourth, I find that it's just different to say something out loud, or write it down on paper, or make a whole diagram of what calls what and how and when. This isn't typically my first step, but for complicated stuff (especially distributed systems), I'm making a process map sooner rather than later. I also share this process map with anyone who might care, and make an effort to keep it up to date so I don't have to start from scratch next time I (or someone else) is confused.

Always keep in mind your goal. If you're reading and reading and it's just not clicking for you, think back to your goal and ask if you really need to understand this part of the code. Maybe you can safely skip it! Even if you think it might be important, you might be able to come back to it later with more context and knowledge.

## Step Five: Try, fail, retry, re-fail, repeat
What I like to do as I read the code is make progress on my goal as I make progress in the reading. This all goes back to the goal: most of the time, that's working software. When I'm working with a new library, I'm going to have my project open on one side of my screen and the library repo cloned and open on the other side. I'm going to take a wild, definitely wrong guess at how I'm supposed to call a function in the library, and come up with some way to test what I've written. For me, because I'm lucky, that's typically unit testing, but sometimes it's faster or easier or better to take a more manual approach. I end up with very much non-working software, and everything I do, everything I read, everything I try to understand is going to start with an error message or a bit of behavior that I don't want.

The key thing in this approach is to fail fast, fail often, and let those failures point you in the direction of success. I'm from Indiana, and I'm a fan of the Midwestern fall tradition of corn mazes. When I go to a corn maze, I don't expect to walk straight through from beginning to end in the most efficient path. I would actually consider that a failure on the part of the corn maze designer! I also don't expect to explore every inch of the maze before I find the exit. Instead of trying to get to working software with no mistakes, which you can be pretty sure isn't going to happen, embrace the mistakes. Expect to make them, and consider it an opportunity for learning and professional development.

I find that people who embrace mistakes write _way_ better error messages and have _way_ better exception handling skills than people who don't have that mindset. They're also easier to talk to about their code!

## Live demo
Just kidding. There's no live demo in the blog post. Come to Indy.Code, or get in touch with me if you're really interested in getting a demo.

## How to practice yourself
It's tough to take a probably-too-long blog post and say to yourself "my life has been changed, I know exactly what to do and I'm going to be good at it tomorrow, I will now send Jacqui Peterschmidt 2 Bitcoin." Like I've been saying this whole time, I think the key to success is to start small where you can be confident. If you want to get better at reading code, and if you don't you probably should have stopped reading this blog post at least a few hundred words ago, I'd suggest starting with a well-known utility library in your language of choice - or the standard library in your language of choice, if that's pretty complete. Find some piece of functionality that's reasonably complex, read it, and see if you can understand how it works. Then check your work by using that function, or by reading the documentation, because in those libraries usually the docs are pretty trustworthy and up-to-date.

## How to teach others
On the other side of the coin for the more senior engineers reading this, my suggestions for helping the people you mentor learn this skill is first and foremost to let them learn themselves. Let them see your thought process when you're reading code by narrating that thought process out loud.

Instead of making your answers super simple, try including relevant links to code or documentation. Encourage failure and fail-fast. I make tons of mistakes, and I will sometimes go through the process of actually writing out and testing a mistake, even when I know before I put my hands to the keyboard that the code won't work. It's the process that matters when you're teaching. 

Another quick story: my brother is doing his bachelor's in AI right now. We were pairing a while back on some type issue he was having. Then, a few days later I had a very similar problem at work and texted him the example. He responded "I thought you were a senior engineer, you shouldn't be making these mistakes." I told him yes, I am a senior engineer, but that just means that I am ten times faster at fixing mistakes, so I can make ten times as many in the same amount of time.

## Conclusion
I took questions at this point in the talk. You're welcome to email me any questions you have at jpeterschmidt19@gmail.com!
---
title: 'We asked ChatGPT to architect our application and this is what happened'
date: '2023-02-03'
layout: post
permalink: /we-asked-chatgpt-about-architecture
categories:
    - coding
tags:
    - architecture
---

I opened up my laptop Monday morning and saw this email from the CEO:

> We're pivoting. We need a new app, fast! New idea is basically Uber for dogs. If you need a cute dog, quick, this app will get you a dog to rent, FAST. It has to have an excellent native iOS and Android app. Need it by next monday.

After wiping up the coffee I spit out on my desk and gathered my sanity, I was blanking on where to start. Then I remembered that ChatGPT is here to help me get started.

So, I asked ChatGPT to help us get started with our app architecture. This is what happened.

![First Interaction in which ChatGPT actually does a pretty good job architecting an app](https://user-images.githubusercontent.com/392778/215508631-0b20b481-0d16-4962-9d43-1a63622314e2.png){: width="100%" }

Huh, well, I'm a little put off by the level of confidence, but that's actually ... Better than I expected.

I'm not just gonna sit here and let AI take over my job though, I can still do better.

A queue will definitely work, but what about "dogs" screams "queuing" to ChatGPT? I'm not quite sure why dog rentals need a queue based system, but I'm not against it either.

And more importantly, what's the justification for using a NoSQL database over a relational database? That's questionable, in this engineer's opinion!

![Second Interaction, in which ChatGPT defends its decision to use NoSQL](https://user-images.githubusercontent.com/392778/215508744-4547758c-05bd-444e-96ab-43fbea16246a.png){: width="100%" }

Ok well, I definitely need to handle a large number of dog rental requests. This idea is going to take off like crazy so I'm glad NoSQL is webscale.

The rest of this is a load of baloney, ChatGPT!

Who's to say NoSQL will be faster at querying than PostGresSQL!? What if I need to get at my dog rentals by joining user information. What if I want to give free dog rentals on birthdays? That's gonna be hard to do!

And NoSQL "can be more cost effective than relational databases" Phoey! Running my own datacenter _can be_ more effective than using the cloud but I'm not gonna do that!

I also don't think storing my dog rental data in unstructured form is a good thing. This is a mission critical app and I want the utmost data integrity.

Ok, let's move on.

What about those Microservices? That seems like overkill to me.

![Third interaction, where ChatGPT tells us why microservices are better](https://user-images.githubusercontent.com/392778/215509612-744c95a4-06b0-4a17-9a13-76875673a119.png){: width="100%" }

That... Makes a lot of sense. Fair point on the pros and cons, ChatGPT. While I watched this answer type out I was full of judgment but then it ended on that note and I don't know if I could have come up with a better answer myself.  

_Well done, ChatGPT, point for you._

![Reluctantly Cheering for ChatGPT](https://media.giphy.com/media/5gYkTDtYSTqeHajMoQ/giphy.gif){: width="100%" }

Ok, so how about the deployment. Using "cloud services" is pretty vague.  Let's clear this up.

![Fourth Interaction, asking about cloud deployment](https://user-images.githubusercontent.com/392778/215510714-12dd89cf-ff00-4d9e-9d1c-be60c1898c6d.png){: width="100%" }

![Suspicious](https://media.giphy.com/media/so8KXAphERsre/giphy.gif){: width="50%" }

This is all suspiciously vague. This type of answer seems like it was written by a non-technical MBA student in their first year. Not even a mention of containers. Blasphemy! 

Take that point away!

Ok, how about React Native. Sure, we'll save some money and effort not having to code two different apps but the experience won't be as good!

![Fourth Interaction, why React Native?](https://user-images.githubusercontent.com/392778/215512800-98bdcb9b-c176-47fb-b001-e0ca9b5bbb92.png){: width="100%" }

Hmmm... Ok, another good overview.

Point back, ChatGPT. 

![Thanks, ChatGPT](https://user-images.githubusercontent.com/392778/215513184-693e4d96-b640-4688-915f-dab0d2df2768.png){: width="100%" }

Finally, let's ask ChatGPT about the CEO's idea, after all, the best engineers know when NOT to build things, right?

![Would you use uber for dogs?](https://user-images.githubusercontent.com/392778/215513780-c6721e5d-55a7-40d2-82d8-b4cc438003df.png){: width="100%" }

Sounds like a winner to me! Off to build that app...

# What did we learn?

AI is a good actor!

But seriously: despite seeing lots of posts/articles about how great ChatGPT is at various things, it _still_ did quite a bit better than I thought it would with such a vague question about design/architecture. I was not expecting such well-rounded answers, even with caveats about weighing pros and cons.

It also does well with very specific information. You can see how diving into NoSQL gave many more specifics and even gave those nice caveats about when it might not be applicable.

The overly-confident tone it presents is off putting, though. For a new tool that is scraping internet articles for its answers, it sounds _way_ more confident than it should be. And not _all_ of the answers presented caveats about weighing pros and cons.

Using the tool is a great way to get started if you're stuck or just learning. It does a great job of amalgamating information that might take you a while to collect with various internet searches.

However, I do hope that the confident tone can be scaled back before unknowing newbies use the tool to direct their software development efforts, whether it be for code or for architecture and design.

There is a great article I recently read called [ChatGPT and the Enshittening of Knowledge](https://castlebridge.ie/insights/llms-and-the-enshittening-of-knowledge/) that sums this up perhaps better than I can:

> [...] in order to ensure that its answer fits what its model tells it is expected to come next, ChatGPT is also making shit up. So, we have a generic non-committal middle of the road representation of knowledge coupled with what, in a human, we’d class as “A-Grade Bullshitter” levels of self-confidence so they make shit up to support their argument.

> The merit or trust rating of a book or journal or newspaper that cites the A-Grade Bullshit then increases the truthiness of the bullshit, resulting the next iteration of the question to the AI having EVEN MORE CONFIDENCE in their bullshit. [...] This results in a classic data quality spiral where the incorrect data becomes accepted as fact and decisions or outputs that disagree are discounted. The Enshittening of Knowledge gathers momentum.

We're still not even close to the eyes, ears and brain of a discriminating engineer.

You heard it here first: Learning the old fashioned way by trying and failing is still the way to go.

![Why are you so confident, ChatGPT?](https://user-images.githubusercontent.com/392778/215564712-5b64fe74-4ac1-41e7-ae62-859960c10350.png){: width="100%" }


_NOTE: The original version of this post appeared on [DEPT® Agency's Engineering Blog](https://engineering.deptagency.com/p/fb0ab9c7-14a2-4712-84fb-4c012f461135)._
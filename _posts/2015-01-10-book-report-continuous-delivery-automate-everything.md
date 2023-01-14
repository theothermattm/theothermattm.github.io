---
id: 78
title: 'Book Report: &#8220;Continuous Delivery&#8221; &#8211; Automate Everything!'
date: '2015-01-10T21:04:03+00:00'
author: theothermattm
layout: post
guid: 'https://code.theothermattm.com/?p=78'
permalink: /book-report-continuous-delivery-automate-everything/
categories:
    - coding
tags:
    - 'book report'
    - books
    - 'configuration management'
    - 'continuous delivery'
    - devops
    - 'source control'
---

About four years ago, I remember seeing [Continuous Delivery by Jez Humble & David Farley](http://www.amazon.com/Continuous-Delivery-Deployment-Automation-Addison-Wesley/dp/0321601912) sitting on a colleague's desk. I remember thinking how awful it looked. Like a textbook from college, blech!

Then, I heard about it a lot.  A LOT. Developer after developer, blog post after blog post. The book's central idea is to automate (almost) everything in your software development processes.  By doing this, the theory goes, you can deliver value to your software customers faster, with less risk.  

There were many potential uses for these ideas coming up in my day-to-day work, so I finally decided to read the book to see what the hype was about. It was definitely a worthwhile read, and as a way to help me remember some key points from this book, I decided to write up this post. I thought it might spark some interest in others to read it as well.  

# Why read this book?

If you're involved with software in any way: a developer, a sysadmin or a manager, you really should know this stuff. 

Reading it cover to cover may not be necessary, which I'll explain below, but you should at least be familiar with the concepts and techniques talked about in this book.  

Even if you think you know the concepts, you should still read it.  Admittedly, some of it is simply becoming standard knowledge and some of the tools it refers to are dated. But, the details in many of the chapters matter and will be really valuable for you. 

# Before you start reading...

Plan on reading about a chapter or less at a time, then stopping to digest it for a day or so.  Because it's written so well, it's almost deceiving how dense the material is.

As I alluded to before, don't feel like you need to read the whole book.  This is mentioned in the book's introduction.  There are a few essential parts I'll outline below, but otherwise, pick and choose as you see fit.  The authors specifically mention that each chapter should stand on its own.

# Essential Parts

Here are the parts I consider must reads:

## The Preface

This sets the stage for why the book was written and the best way to read it (in the author's opinion). 

## Chapter 1 - "The Problem of Delivering Software' 

This chapter addresses fundamental problems the book sets out to solve and general principles it follows.  It also will define certain key terminology that you'll need to be familiar with throughout the book.

Again, this an especially important read for managers who may have been away from code for a while, or (gasp!) managers who have never coded before.  It will help you understand how manual processes are fraught with errors, cause your developers to tear their hair out and generally make them (and by proxy: you) miserable.

## Chapter 2 - "Configuration Management"

This was a standout chapter for me.  Humble and Farley present why it's important to be able to automate the creation of your software environments *completely*.  Most of this stuff I was already familiar with, but the way this information is laid out is the best I've seen:  Step by step, introducing the problems and high-level solutions. It helped me solidify my existing understanding and give more depth to what I already knew. 

This is the type of information you need to convince others that automation of your environments is of utmost importance and also tell them that "Yes! It's possible!"  

If you're less technical, let your eyes glaze over anything that you might not understand.  You'll still come away from the chapter with plenty of new ideas.  Just be careful about how you present them to your developers if they're not already on board :)

# Stand out areas

## Infrastructure Management (Chapter 11)

This was probably my favorite chapter in the whole book.

For a large, dense textbook, I found myself on the edge of my seat in this chapter.  I could not get enough of the ideas that the book presented to solve common issues with software environments getting out of sync and how to prevent them.  I lost count of how many times I thought: 

> "Wow, this would have helped with [insert huge, time draining issue here]."

There are also some important tips in this chapter regarding keeping your environments maintainable.  Things like logging, change management and monitoring.  

You could almost consider this chapter a crash course in good system administration.  And it applies whether you're running in the cloud, your own datacenter, with one server or thousands.

## Source Control 

A key theme throughout the book is that you should use source control.  I knew source control was important, but after reading the book, I realize how crucial it is to *everything* in software.  Now, I want to Git *all the things!!!*

I do disagree with the authors' assessment of using [feature branches](https://www.atlassian.com/git/tutorials/comparing-workflows/feature-branch-workflow/) with Git.
 
They make it clear they are not fans because feature branches deviate from the main line of code and thus stifle Continuous Integration across developers.  I can see their argument, but having used feature branches very effectively,  I have to wonder if their opinion has this changed since 2008 when the book was written.  Things like [pre-flight builds](http://java.dzone.com/articles/preflight-builds-theres-a-time) help with these sort of issues.

## Testing

One of my key takeaways from the book (from Chapter 8) was that to effectively run Automated Acceptance Tests, you should build a test API layer into your application.  This layer captures important use cases which you can use as building blocks for real tests.  This is lightyears more maintainable that using UI driven tests, which the authors say should be used, but as little as possible.  

This was eye opening and a very useful idea I hope to implement someday soon.

Also, they give good guidelines for how to categorize testing and when it makes sense to automate a test and when it doesn't.  

Chapter 12 - "Managing Data" also gives some really great tips on how to manage test data.

# My Only Criticism: Not enough advice on where to start

My only criticism of this book was that it preached about the ideal state of Continuous Delivery a lot and didn't spend enough time on how to get started if you're in a rut already. Which I'm sure is where most readers are at.

I'm sure I'm not alone in that I desperately want to reach the God-like software state these guys describe, but I have some harsh realities of a large organization I need deal with before I can get there.

# Outstanding questions

What follows is a list of some of the questions the book raised for me, but didn't answer.  My intent here is not to answer these questions, but to highlight some areas know I need to get more information on, and that you might, too.

I would love to hear your comments about any of these:

## Differing levels of enthusiasm 

If you work in a medium to large sized organization developing software, I'm sure you have to deal with a range of enthusiasm for the job. This ranges from older people who are completely content to run a deployment from a Word document, to overly enthusiastic cowboy developers who do what they want when they want, without the faintest whiff of a plan.

How do you herd these cats and get them drinking the automation Kool-Aid?  How do you get people who aren't excited, excited?  And how do you contain the people that want to go crazy [shaving yaks](http://en.wiktionary.org/wiki/yak_shaving) without a good, solid vision and plan to get there?

This is something I've been thinking about a lot since I read the book and have tried a few things out, but that's for another post.

## Politics

With individual contributors moving your digital widgets, a lack of enthusiasm is one thing.  But if you have a lack of enthusiasm in management (especially upper management) this can present a *serious* roadblock to making progress towards these ideas.  Or even worse: What if they're opposed to the costs of implementing automation?
 
I'm still trying to find good ideas for convincing upper management to back these ideas (Without telling them to read a textbook...).  Management wants numbers, and numbers for this stuff are hard to come by.  It seems as though storytelling is more effective, but not everybody buys stories...

Furthermore, what about dealing with upper management who pushes *too* hard on these concepts without really understanding them?  You know, the type who reads about DevOps in an in-flight magazine then lands and puts "Must implement DevOps by June" on your yearly objectives...

## DevOps

The book mentions, or alludes to, [DevOps](http://en.wikipedia.org/wiki/DevOps) a fair bit.  

The authors recommend having a unified team with development, operations and QA all working together in harmony. That's great - at a startup or an organization supporting one large application.

But, what if you work in a large, siloed organization that supports hundreds of medium sized applications?  How can you get these ideas to work?

Side note: DevOps is a buzzword I'm beginning to dislike. I appreciate what it's after, but it seems to have been engulfed in meaninglessness by big companies wanting to make a buck.

## Differing Skillsets 

How do you manage continuous delivery in a team of people who range from right out of college to senior developer.  How do you get newbies started?  How do you teach old dogs new tricks?

# Finally...

If you made it here, I hope you're convinced you should read the book, or at very least add it to your Amazon Wishlist :)  If you end up reading it after seeing this post, come on back and tell me what you think.

If you've read the book prior to reading this post, I'd love to hear your comments/criticisms/ideas in the comments.  

After reading this book, if you're not convinced that moving towards these concepts is worthwhile, you should probably find another profession. 

Now, go forth and automate everything! 
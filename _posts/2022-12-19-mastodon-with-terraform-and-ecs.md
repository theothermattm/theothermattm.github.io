---
title: 'Why and how to create your own Mastodon server on AWS with Terraform'
date: '2022-12-19'
author: theothermattm
layout: post
permalink: /mastodon-with-terraform-and-ecs
background: '/img/clean/bg-post.jpg'
categories:
    - coding
tags:
    - cloud
    - aws
---

*TL;DR:* At my employer, [DEPT® Agency](https://deptagency.com), I updated and open-sourced Terraform scripts to bootstrap your own Mastodon server using AWS, Elastic Container Service, and Fargate.  

It all started with a simple request to see how hard it would be to setup a Mastodon server for my company.

Sure, easy. I'll do it!

So began my journey into creating a Mastodon server.

But, before we get into all that ...

# Why would I create my own Mastodon server? Isn't that why Twitter exists?

It would be hard to avoid hearing about [Mastodon](https://joinmastodon.org/) lately with all that's happening around Elon Musk's takeover of Twitter:  [Journalists being banned](https://www.nytimes.com/2022/12/15/technology/twitter-suspends-journalist-accounts-elon-musk.html), [accounts in violation of policies for links to other services](https://techcrunch.com/2022/12/18/twitter-wont-let-you-post-your-facebook-instagram-and-mastodon-handles/), and constant unpredictability with policies being changed daily.

But, even though you've heard of Mastodon, you may not know how you use it - [here's a good resource](https://buffer.com/resources/mastodon-social/) to learn a little more.  Also, if you're not already signed up: You have to find and choose a Mastodon server to use, a key difference from the centralized approach of Twitter.  To get you going faster, I'd recommend creating an account on [Mastodon.social](Mastodon.social) (one of the biggest servers).  Then, once you get that going, use [Movetodon.org](https://www.movetodon.org/) to import your Twitter following list into Mastodon.

But why on earth would you want to create your own Mastodon server?  I can't state it any better than Julien Deswaef in his article [Your organization should run its own Mastodon server](https://martinfowler.com/articles/your-org-run-mastodon.html):

> ... just as with email, you can send a message to anyone in your organization, and at the same time have a conversation with anyone outside of your organization. All this is transparent and your correspondent’s email address lets you know that you are actually talking to someone as part of a particular organization.

> By using your own domain name, your brand, you are creating a recognizable social presence in the Fediverse, without the need to associate it with facebook.com, twitter.com or instagram.com. No need to worry about someone else squatting your name either. Your domain name is what will get people to trust that the Mastodon accounts under it are legitimate and official.

> And once you run your own corner of a social network, you can decide who you invite there, what are your rules of engagement and code of conduct. At Thoughtworks, we’ve allowed all our employees to open an account on our Mastodon server as this is aligned with our culture and our practice of being quite vocal and transparent about what we are passionate about.

Having some piece of mind for your company's brand to exist on a social network that you control fully is huge.

*Ok, now that we've gotten that out of the way...*

# The Context

Jumping ahead a bit, one thing that I couldn't really find was a nice, introductory article explaining the architecture of Mastodon.  I found myself struggling to set up infrastructure I had no context in. So here's a crash course that will help you understand things a bit better before diving in.

Mastodon is comprised of just a few main components, all within the same [open-source codebase](https://github.com/mastodon/mastodon):

![Mastodon Architecture](/img/mastodon-architecture.webp)

([Diagram Source](https://gist.github.com/theothermattm/0740f982316e3b87e8efc9adbc87b7fe))

**The main web application.** This is a Ruby on Rails application that runs on [Puma](https://puma.io/). This includes web pages themselves as well as APIs.

**Background Job Processing.**  This runs using [Sidekiq](https://sidekiq.org/), a Ruby job processing system. These jobs handle things like handling requests from other Mastodon servers (known as federated servers) for messages and follow requests, handling notifications, and other things.

**The streaming API.**  This is a Node.js application that has Websocket API's for real-time updates for the various parts of Mastodon's data model.  Think of things like getting notifications in the app for new follows.

Another important thing to know is that Mastodon created and implements the [ActivityPub spec](https://docs.joinmastodon.org/spec/activitypub/).  This is a specification managed by the W3C for decentralized social networking.  I won't go into the details about ActivityPub here, but it's super important to know about the concepts in it to help you with setup, more on that later.

# How Federated Servers Communicate

I think perhaps the most confusing thing is understanding the flow between different Mastodon servers.  So, let's take the example of Following someone on a different server:

1. A user clicks "Follow" for someone on a different Mastodon server.
1. The Puma web application will take the federation information and lookup how to contact the other server and send a follow request to that server.  The endpoint that's called on the other end is something like `POST /users/{followed_remote_username}/inbox`.  This puts a message in the remote user's "inbox that they have a follow request".
1. On the remote end, the API endpoint puts a message in the [Sidekiq `ingress` queue](https://docs.joinmastodon.org/admin/scaling/#sidekiq). When the job processor picks up that task, if the user doesn't need to approve follow, the server will respond with a corresponding call back to the original users inbox that the follow request is accepted.

This is all part of the ActivityPub spec but is kind of hard to follow, and if I'm being honest, I'm still not sure I got it completely right (comments welcome!).

The takeaway is that background jobs are really important in Mastodon, and so is the ability to securely connect between servers through HTTPS connections.  This process is fraught with SSL errors and layers you'll need to debug if the connections don't just work.

# The Infrastructure Needed

To host this beast (ha ha) here are a few things that need to be set up, and a couple optional things:

**An online web application container.** Something that can run Ruby processes.  Could be Docker, could be a Virtual Machine.  Pick your poison.

**A PostGresSQL database.** This is the heart of your Mastodon instance storing user accounts, messages, etc.

**A Redis data store.** This is used for the job queueing system described above as well as caching.

**A static file store.** This is used to store uploaded photos/files for posts.  Think AWS S3, or similar.

**A reverse proxy or load balancer.** You really don't want your web application directly hosting requests.  You'll want some way to throttle and distribute those requests and also put in some security checks as well.  You could use something like [NGinx](https://docs.nginx.com/nginx/admin-guide/web-server/reverse-proxy/) for a reverse proxy, or in our case, we used an AWS Application Load Balancer.

**Optional: An ElasticSearch cluster.**  This is used for advanced searching capabilities.  I didn't set this up so it's not included in this write-up. If I end up setting one up I'll update the post.

# Choosing a path

There are affordable hosted Mastodon server offerings, but really part of this exercise was learning more about the architecture of Mastodon and what goes into setting up a server.  After all, we're engineers.  We can do it better ourselves, right? 🫠 Kidding...

If you have the knowledge, it makes a lot of sense to stand up Mastodon yourself on cloud infrastructure instead of relying on a hosting provider: You can be assured you're in control of your data and treat Mastodon just like any other app you develop. It's also advantageous to be able to really tweak your cloud resources to your number of users to save money or scale up.  whereas hosting providers charge for the [price of Mastohost is \$89 a month for approximately 2000 users](https://masto.host/pricing/), and the price for [toot.io's 1000 user plan is \$89 a month](https://toot.io/mastodon_hosting.html#pricing).  Also, as of this writing, Mastohost isn't even accepting new customers according to their page.  That's kinda important especially with the recent influx of Mastodon users.

As for cost, I'm anticipating it will be a wash compared with hosting providers.  The AWS estimate for our first month's bill is about $130 but that includes lots of experimentation and also a NAT Gateway, which can be avoided if you'd like to be slightly less secure (exposing your containers publicly, which is generally a no-no).

I'm most familiar with AWS and a . big fan of containerization, so I thought I might go in that direction.  It helps that Mastodon has [official Docker Images](https://hub.docker.com/r/tootsuite/mastodon)! Finally, I knew if I was going to go with AWS, I wanted to use an Infrastructure as Code (IaC) tool so that I could share the setup with the world, and you, dear reader.  I'm most familiar with [Terraform](https://www.terraform.io/) so I chose that.

With all my preferences, I set out to see what already existed to help me out.  One of the huge benefits of using IaC tools is being able to leverage existing open-source code to do this type of thing.  After some false starts, I landed on a project called [mastodon-terraform by r7kamura](https://github.com/r7kamura/mastodon-terraform). It was a beautifully laid out Terraform project with modules and a great setup for providing configuration variables. It also used Elastic Container Service which is a "just complicated enough" container orchestration engine.

The problem was that it was out of date, having been touched last six years ago.  Mastodon had only released its [first stable release in 2017](https://github.com/mastodon/mastodon/releases/tag/v1.0), so I assumed much had changed between then and now.  It also did not use [Fargate Managed ECS Clusters](https://docs.aws.amazon.com/AmazonECS/latest/userguide/what-is-fargate.html), but instead EC2 servers which can be cumbersome to maintain, even with Terraform.

That was as good a place as any to start!

# The Takeaways

In the course of a couple weeks of off and on work, maybe about 20 hours total, I got a server up and running, ready to scale out should our community take off. My company shared the forked repository for all to use here: [deptagency/mastodon-terraform-aws-ecs](https://github.com/deptagency/mastodon-terraform-aws-ecs) (MIT Licensed).

I hit a couple bumps in the road making changes to container definitions in ECS to be in line with updates to Mastodon since 2017, but other than that [r7kamura](https://github.com/r7kamura)'s work was a fantastic base and I thank them greatly.

I also hit a knowledge wall at one point where messages from other servers were not being received by ours.  I spent a lot of time with networking settings, but in the end, posted a question on the [Mastodon Github discussion forum](https://github.com/mastodon/mastodon/discussions/22310) which was super helpful and pointed me in the right direction.

My biggest challenge was finding high-level information about mastodon's architecture and infrastructure.  Mastodon has all kinds of fantastic documentation about the details of how to set up a server and how to configure it.  They also have great introductory material on how to use and administer Mastodon.  Everything in between is a bit of a black box.  I hope that this post helps someone else fill in those gaps.

_I wrote the original version of this post for my employer, [DEPT® Agency](https://engineering.deptagency.com), you can view it [here](https://engineering.deptagency.com/create-your-own-mastodon-server-on-aws-with-terraform)_

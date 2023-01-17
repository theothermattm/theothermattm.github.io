---
title: 'Scaling Technical Teams'
date: '2021-08-18'
author: theothermattm
layout: post
permalink: /scaling-technical-teams
background: '/img/clean/bg-post.jpg'
categories:
    - management
tags:
    - teams
---

> "How can I move faster?" 

I'm sure we've all heard this many times from bosses and clients. Regardless of team size, industry or maturity, I consistently see teams struggle to scale. Even the ones that do it well have some serious learning to do. In some extreme scenarios I've heard "I am spending more on software, yet, somehow, I're actually seeing less software get released. How the hell is this happening?"

Those of us who've been in software for a while are probably very familiar with [Brooks' Law and The Mythical Man Month](https://en.wikipedia.org/wiki/The_Mythical_Man-Month) which states: 

> Adding manpower to a late software project makes it later. 

This is what you're seeing when the situation described above occurs. It makes it even worse when your team isn't set up to scale.

Why should you care about scaling your team? Here are some symptoms of a team that is struggling with growth:

* **Problems organizing work** - Entire teams stuck waiting on product requirements or on other teams, wasting time.
* Too many hurdles make the **slightest of changes to production take a very long time.** This leads to very frustrated business partners.
* **Paralysis by analysis** - Teams stuck debating on how to implement something, again, wasting time.

In this post, I'll start with the less technical ways of addressing these challenges and end with the more technical ones.

First up, the people!

## The Talent

This will sound cliche, but it all starts with hiring. You want to know how to scale your team _before it's too late_ . Here are some things to consider when hiring your software team.

**Start with strong technical leadership.** Hire this talent first. By "technical leadership” I don't mean "managers." I mean player/coach talent who _do the work while building out a team_ , then move to full time leadership.

When picking your technical leads, it's best to **prioritize communication skill and curiosity over technical ability**. You want a leader who is technically sound, but more importantly, you want a leader who asks good questions, exposes shortcomings, and _knows when they are in unfamiliar territory_. This type of person is also someone who can communicate with you and about your business with ease. Someone who can talk tech and also business is rare, so when you find it the cost may be high but it will be worth it.

If you nail your tech lead(s), you'll have a person who can ask the right questions to get projects started, who knows how to staff them with the right balance of people, and who will also contribute. To vet this person, leverage your personal network of technical people to help. Depending on your plans, you may want to hire a few of these people.

After you hire your technical leaders, you want to make sure you **have a good balance between leaders and individual contributors**. Trust that your tech leads will lead this hiring effort and let them know how much you want to be involved. Don't just hire all senior engineers, or you'll have a situation where you have stalemates on technical decisions all the time as Senior Engineers tend to have strong opinions. 

Ideally, you have a balance of people who will want to lead the charge, and people who are more junior and eager to learn. In all of these roles, curiosity is a key trait to look for. Make sure you hire Senior Engineers who _want_ to mentor and check in with them frequently to make sure their mentoring duties don't interfere with their daily work too much. You'll know you're on the verge of having too many junior developers when your senior engineers say mentoring is taking up too much time.

Finally, make sure you **have a large enough product and/or business team to back up your technical team** to help drive work forward. A technical team without enough product owners and/or designers is as good as not having a software team. Of course you want independent teams who can drive work, but if you don't have the business expertise at your disposal you'll be unpleasantly surprised when your app or platform _looks like it was built by engineers_.

### Organization

After people, the next most important thing is organization - Of people and of work.

The big theme here is: Less is more. Put some basic constraints in place and _trust your teams_ to do the right things. You don't want everyone going wild, so things like making sure everyone's using the same language might be a good constraint. But, determining how individual teams run meetings and processes is only going to create frustration. Again, it's amazing what an empowered software team can do if you give them goals and leave them to their own devices.

As a wise man once said:

> If you want to build a ship, don't drum up the mento gather wood, divide the work, and give orders. Instead, teach them to yearn for the vast and endless sea.

- Antoine de Saint-Exupéry

### Team Structure

You don't have to nail the team structure from the get go. Just **keep in mind that you'll have to create more teams** and have a list of "things to figure out." Make sure you're transparent about what has and hasn't been figured out. Figuring things out as you go and being honest about it is a lot better than trying to anticipate everything at the outset. You'll save everyone's sanity and         time. Share the plans for growth with your teams and ask for their input. Software teams can be extremely proficient at helping with these things. They can let you know what they've seen work and more importantly, _you want them bought in since they'll be living with it every day._

### Inter Team Communication

Figure out a way to communicate across teams. Sometimes this is Slack channels, sometimes this is a [Scrum of Scrums](https://www.atlassian.com/agile/scrum/scrum-of-scrums) with your technical leads.Whatever it is, just **get a basic cross-team communication framework in place** and let the teams adjust it and run with it as they wish.

### Organizing Work

Finally, I should talk about organization of _work_ **Strive to have about a month's worth of work _shovel ready_ for _all_ of your teams.** I use the term shovel ready to describe work that is well-defined enough for teams to go and execute on without being blocked. This is an art, not a science; if you over define the work, you risk alienating your team. However, if you don't define it enough, you'll have a bunch of developers looking at each other wondering what you meant by "I want the software to be _easy to use._"

To get to this point, make sure that your technical leaders and product leaders are working together regularly to help strike the right balance and keep defining the work to keep it ahead of the individual contributors on your teams.

When you have a team who has enough definition and enough freedom to ask questions and try out different solutions when implementing, you'll have a team of engaged, productive people and you'll be pleased with the results. It's also extremely important to have a team that feels collaborative and is part of decision-making when things change. 

If a feature or change isn't working out, the team shouldn't feel like it's a reflection of poor work, but rather that they are working _together_ to make a better product.

# Communication

In the previous section, I discussed what can go wrong when a software team isn't built to scale properly and some strategies around hiring and organization that can help mitigate some of those problems.

Next, I'll tackle another area that's ripe for problems relating to scaling teams: 

**Talking and Stuff. Aka - Communication.**

## The Blessing and Curse of "Tribal Knowledge"

[Tribal Knowledge](https://www.isixsigma.com/dictionary/tribal-knowledge/) is an often-used (and very unwoke) term for the stuff that your team knows but isn't codified yet. When you're starting up, there's no time or need to write those things down or automate them. It'll just slow you down. The fact that this knowledge is _just known_ on the team makes it a well oiled machine.

But as you grow and new people come onto your software team, they only find out about these things by happenstance or even worse when an issue comes up and they're not sure how to fix it. You could consider the process of gaining this knowledge a tax on every new person on your team. In order to become fully productive, they need to absorb all that knowledge and the only way to do it is to give it time.

This knowledge can be simple things like where the link to the deployment system is and what branch production is deployed from. Or, it can be skeletons (as in _skeletons in your closet)._ I often find that companies that grow quickly also have skeletons in their closets that only a handful of "the old guard” know.

In the worst case scenario, the old guard views having this knowledge as a badge of honor and job security and holds onto this knowledge. In the best case scenario they have these issues documented and potential tickets documented knowing that they need to be fixed.

Regardless of where you might fall, it's best to get ahead of this as your team is _starting_ to grow.

Make sure that you reward transparency and information sharing, not [information hoarding](https://bloomfire.com/blog/five-signs-company-information-hoarding-problem/). The default reaction to a team member saving the day during a production incident or fixing a bug should be to ask "How do I make sure that doesn't happen again? Is what you did written down somewhere? Could other developers do that if I needed them to?"

Make sure your technical leads set goals for their teams to codify their knowledge. Simply dumping all the knowledge into a document or wiki is not enough, this needs to be thoughtful and strategic.

Here are some other ideas to consider to get ahead of this as you think about scaling your teams:

* Set aside some time periodically to prioritize automation of tasks that require tribal knowledge. There's nothing better than having workingcode instead of documentation.
* Make sure that code comments which document the "[Why? Rather than the How?](https://blog.codinghorror.com/code-tells-you-how-comments-tell-you-why/)" are prioritized and vetted during code review processes. As the linked article explains, the code is the how, and what's more important is the why. This is especially important when something weird is happening. Keeping this documentation as close as possible to the code itself is much better than distributing it in different places like a document or wiki.
* Have a [Runbook](This knowledge can be simple things like where the link to the deployment system is and what  branch production is deployed from.  Or, it can be skeletons ☠️ (as in skeletons in your closet) .  We often find that companies that grow quickly also have skeletons in their closets that only a  handful of "the old guard" know. 


## Curiosity

In our experience, the best developers on the best teams are very, very curious. They don’t just want to build the thing you ask, they want to know why what they’re building will be valuable. The best developers and teams know that if they understand the core of what a business is after, they can lend their own strengths of their technical discipline to making products better.

Don’t treat a developer with lots of questions as a nuisance; embrace them and foster a culture  where asking those questions is not only rewarded but expected. If your software teams aren't asking lots of questions, consider that a potential red flag. Perhaps your tech leads are hoarding information! 


## Organize around information sharing

Make sure that between you, your technical leads and individual team members there is a clear sense of who makes what decisions, who needs to be informed of them, and who doesn’t care at all. There’s a tool for that! [RACI matrix](https://en.wikipedia.org/wiki/Responsibility_assignment_matrix) anyone? You don’t have to use something as formal  as a RACI matrix, but be inspired by the idea. Strive to make sure your team is able to make as many decisions as possible independently. 

When a team knows they’re empowered to make a decision, they will move much faster. And when they can’t make a decision, if they know exactly who to go to, then that decision will get to the right person a lot quicker. This, of course, assumes that when a decision needs to be made outside the team, that decision-maker makes it a priority to get that decision to them fast. Reward unblocking your teams throughout the organization.

Establishing a RACI matrix (or something like it) _before_ your teams scale and updating it as you go is very low effort and will pay off in spades.

# The Technical Things

## Getting Started

Developers often have a difficult time getting an application running on their local machines. In the worst cases, it can take a developer more than a week to get the full system running so they're ready to start developing.

Making sure that other developers can easily get started running a system locally is something that's often overlooked early in a project. As your team grows and new people have to create this setup (or, existing people get new computers!) this slows you down immensely. Combine this with the amount of "Tribal Knowledge" that a new person needs to be productive and you can see how this will slow down the productivity of your team and their ability to scale.

So how can you get ahead of it? **Prioritize automation.** Whether it be through specific talent like DevOps or Site Reliability Engineers or Release Managers, or just making it one developer's priority to script things - Make sure that a new developer can get started in a few hours at the very most. At the very best, one script does _all_ thesetup for a new developer - they run the script, go have a coffee, and come back to a workstation ready to develop on.


Be mindful that different developers have different preferred platforms they use. Some prefer to use Mac, some prefer to use Windows, others Linux. I are not fans of dictating tooling, so prefer automation methods that work across platforms like using Docker images for development setups, or alternatively provide different scripts for different platforms.

Oftentimes, the effort you put into scripting a local setup can be directly translated into deploying the application to a real environment. Even if the scripts aren't directly used, you can usually translate them into a new language/framework because _everything you need to set up is in code and not just written down._

Which brings us to our next point ...

### Make significant investments in Automation/DevOps

One surefire way to kill the ability to scale your team and software is a lack of investment in automation and "DevOps" (side note: I'm using that word even though I hate what it's become!). I have seen many projects where it's time intensive and risky to get software into production because processes are manual. In the worst cases, nobody fully understands how to get the software into production at all.

In some of these same projects, there is little ongoing investment in their DevOps platforms to solve the problem. Instead, more resources go to the development team to crank out _more software that is risky to be deployed._

Even in the earliest phases of your project, make sure you have a fully automated Continuous Integration / Continuous Delivery (CI/CD) pipeline. Regardless of the solution, it should be as easy as pushing code to a branch or clicking a button to deploy to production. Whatever you do, **don't give into the temptation to manually push code out**! It will come back to haunt you before you even get the software out to users. The investment in a CI/CD pipeline will pay off _even before production releases_ because it will allowyou to get test versions of your software out faster to be QA'd. These days with so many easy to use options for CI/CD pipelines, nobody has a good excuse for not having a good CI/CD pipeline.

In concert with CI/CD pipelines, invest in [Infrastructure as Code (IaC)](https://en.wikipedia.org/wiki/Infrastructure_as_code). I recommend [Terraform](https://www.terraform.io/). These tools let you describe your cloud environment and create it with one command. Using these types of tools not reduces your risk of environments being different, and also gives you the ability to spin up new environments very easily. New environments should ideally be button click. This will allow you to spin up a new testing environment for a new feature set, then easily tear it down, saving on cloud compute costs.

Make sure your environments mirror each other as closely as possible. It takes discipline to make sure that this stays in place. Make investments in anonymizing production data to copy to testing environments to make sure features work with production-like data.


### Testing

This one isn't groundbreaking, but needs to be said: make investments in test automation and integrate tests into your CI/CD pipeline. Do this as early as you possibly can. It's much harder to add tests to an existing application than to adjust them as you go.

I often hear from clients that they invest heavily in automated testing. Yet, when it comes down to it, their strategy is just not working and releases of software still require extensive manual testing.

It's easier to start by saying what _doesn't_ work in this area:

* **Relying solely on unit tests in each individual component of the application.** This often just tests one particular case, but functionality of interdependent features often gets missed here. This will inevitably lead to more manual testing.
* **Having too many automated front end tests**, such as in Selenium or Cypress. No matter how hard you try, these tests will be brittle and will break over time and require a lot of effort to maintain. What's worse than no tests at all? Tests that break all the time and you can't rely on them. Rely on these types of tests for absolutely crucial pieces of your software, main use cases and smoke testing to make sure that an app comes up after a restart. Do not try to test everything with them. I've yet to see an organization succeed at this over time.

What does work is automating API tests. It doesn't matter what you use, but being able to work through key functionality cases using just a series of API calls is a good way of automating tests that strikes a good balance betIen making sure your whole application works together and being _easier_ (note not _easy_) to automate.

Having API level tests that go all the way from login, to storing data in a data database, to logout allows you to test a very wide surface of your application with even one test case. It can be difficult to catch every corner case with these types of tests though, so you have to still rely on unit tests for those situations. A good mix of API tests with unit tests for individual components is what you should strive for.

The challenge with automating API tests is having the necessary infrastructure in place to allow for full application testing in an environment that can be torn down and built up at will. Quick throwback to having CI/CD pipelines and IaC tools. If you have a good CI/CD pipeline in place, this _should_ be easier. You can do this with quicklydeployed databases using Docker containers so you don't even need to set up a whole cloud environment to do these tests, which will make them quicker. The sticky wicket in these tests are third party API's which you don't have control over. Use a network mocking tool like [Mock ServiceWorker](https://mswjs.io/) for Javascript to make your tests as seamless as possible in this regard.

I also suggest having **code coverage targets only for areas of the code with high [cyclomatic complexity](https://en.wikipedia.org/wiki/Cyclomatic_complexity)**. Arbitrary code coverage targets  will ensure your useless methods get covered as much as your most important ones do.  Focus on the complicated ones and let API tests cover the rest.

### Code Organization & Decoupling

One of the most significant (and obvious) pain points on large software teams is when multiple people are doing work on the same piece of code at the same time. The team ends up spending more time fixing source control conflicts and coordinating than actually producing value. There's no perfect way to organize code, but if you give it some effort up front and adjust as you go, it's a pretty easy discipline to get into, especially if it's part of your code review process.

Make sure from the get go that you're not putting all your code in a few modules or files (this one's pretty obvious, but worth saying to reiterate). Once you hit a certain point, separate files will become unsustainable. You will smell the code rot from a mile away.

One other idea to consider is the idea of contract driven services (lowercase s) _within the same application._ Many of the benefits of a Microservice architecture are derived from the concept of [Domain Driven Design](https://en.wikipedia.org/wiki/Domain-driven_design), specifically the idea  of a [Bounded Context](https://www.martinfowler.com/bliki/BoundedContext.html). This is a fancy term for drawing smart boundaries around portions of the system, creating contracts for interacting with them and restricting the communication between boundaries to use that contract.

If you can come up with the right bounded contexts for services within your application, you can then work to define contracts for those contexts using simple code documentation or even going so far as to use things like [OpenAPI](https://www.openapis.org/). Yes, using OpenAPI even if you're not making a separate deployed API yet.

The simple act of being more explicit about the contract of a set of services is the important part. By defining the contract of a bounded context, you can then split work out across teams and make sure that they work with each other's code through those contracts.

Once you hit a certain point, separate files will become unsustainable. You will smell the code rot from a mile away. Once you start to sense this oncoming disaster, it probably makes sense to create a separate API or application for some portion of your system. At that point in time, if you've created the right bounded context and have a well defined contract, this exercise will be a lot easier, and there will likely be a team who is already well versed in that particular service that needs to be split out.

Speaking of services...

### What about Microservices?

The past ten years or so the buzzword "Microservices” thrown around as a panacea to fix many problems in scaling and organizing software. And of course, as with many technical buzzwords, there's no one definition of what a Microservice architecture really is. The most important thing to remember is: **Don't conflate a well organized application with Microservices.** A well organized application with separate domains is always a good idea. Microservices may or may not be a good idea.

There _are_ many situations where a microservice architecture will help, but in order to "Do Microservices Right” you need all kinds of foundational architecture in place. For example, have you addressed these questions?

* How are you going to handle deployments across dozens or perhaps hundreds of services?
* How will you handle transactions betIen services? 
* How will you handle discoverability of services? 

Those are just a few of them. If it's not obvious, you can quickly get in over your head, especially if you haven't done it before. Even the godfather of Microservices himself says "[You must be this tall to use microservices.](https://www.martinfowler.com/bliki/MicroservicePrerequisites.html)"

Start with simpler options like those described above in the "Code Organization" section. It probably does make sense at a certain point to have a system with multiple services or applications. But do you need to do it for every little piece of your application? Probably not.

When you use the ideas I outlined in the Code Organization section, you're effectively forcing your teams to work _as though they were using a Microservice_ but without all the headaches of separate deployments, dealing with cross service transactions, etc. You're getting yourself more than 50% of the way there, but keeping things simpler, and more importantly, letting your teams scale out more.

## Parting Thoughts

You can probably tell by the sheer amount of content given to _non-technical_ topics here, I feel strongly that an organization that can scale is really driven by people, culture and communication, more so than technology. Remember that languages and platforms are tools to use  to get to a goal, not the goal itself.

Good luck growing your team!

_Note: Me and other coworkers wrote a different version of this post for my employer, [DEPT® Agency](https://engineering.deptagency.com), you can view it [here](https://www.deptagency.com/en-us/insight/how-to-scale-your-engineering-team/)_
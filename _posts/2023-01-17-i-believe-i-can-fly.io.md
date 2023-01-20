---
title: 'I believe I can fly.io [A fly.io review]'
date: '2023-01-17'
layout: post
permalink: /a-fly-io-review
categories:
    - coding
    - cloud
tags:
    - cloud
    - aws
---

I remember when I heard that Heroku was getting acquired by Salesforce - To say I was disappointed is an understatement.  Heroku pioneered developer-centric, easy-to-use webapp hosting and they did it very well.  But, as expected, things started slowly going downhill after the Salesforce acquisition - Culminating in them [removing their free hobbyist hosting option](https://blog.heroku.com/next-chapter).

At my employer, we still use Heroku to host an internal project tracking application. Last year their [Github integration broke](https://thestack.technology/heroku-outage-github-breach/) and wasn't fixed for almost two months, with very little communication from the company, that's when I decided I would no longer recommend it or use it as an option for deployments.

A few months ago, a colleague enthusiastically recommended [Fly.io](https://fly.io/) to me.  I trust his opinion a lot, and I gave it a shot and stood up a sample Node.js app on it. I was seriously impressed!  An amazing CLI and I got an app with a database up and running in less than 30 minutes.  AND I could do all the wonderful things I could usually do with Docker containers. Oh, and it still has a free option, unlike some others (looking at you, Heroku!)

I was so impressed, that when we had to host another internal app, I immediately reached for it.  It's worth noting that these types of [Platform as a Service (PaaS)](https://en.wikipedia.org/wiki/Platform_as_a_service) tools are not for every use case, but ours was perfect since it wasn't a mission-critical app.  That said, we still wanted some basic things for it like monitoring and being able to view logs, etc.  Things that Heroku does splendidly.

To make a long story short: My experience was that though Fly.io has amazing promise, it doesn't yet have the necessary features for operational use on a team. I'll outline why in this article.  It is great, however, for hobby projects, single developer applications.  **It's especially great if you just need a (free!) place to start and plan on migrating to a more mature platform at some point soon.** 

This article may come off as critical, but useful software often has critics. I'm one of them. But, let me make it 100% clear: **I am a Fly.io fan!** I wouldn't take the time to write this if I wasn't excited by the promise it has.

Ok, on we go....

# Why Fly is awesome

The whole platform is based on Docker containers.  You can do everything from creation of projects to managing secrets via [command line](https://fly.io/docs/hands-on/install-flyctl/). This means everything can be scripted. It can use Heroku build packs.  It can be as simple or as complicated (within reason, see below) as you want or need it to be.

Think of it as Kubernetes or AWS Elastic Container Service on easy mode.  Though this ease of use brings with it a restriction the customization abilities of your deployments, and, as you'll see as you read, some limitations.

Oh, and did I mention that **it has a free option to get started?**

# Setting up an app

It's very, very easy:
1. Download the CLI.
1. Authenticate with `flyctl auth login`
1. Run `flyctl launch` or `flyctl apps create` and point it to your Dockerfile or Docker image.

That's it! 

# Plaintext Configs

Fly uses a `fly.toml` file that is used to [define your application](https://fly.io/docs/reference/configuration/).  All of your configs are here, at a glance and able to be version controlled. 

You can also provide multiple definitions for different environments or variations as well using the `--config` flag on the CLI.  

One thing to note is that there is no "override" option ([a-la docker-compose](https://docs.docker.com/compose/extends/)), so you will have to copy/paste your definitions for now. No big deal.

# Deploys / Free Builders

One of the nicest things of the developer experience is how easy it is to deploy your app for testing.  You run `flyctl deploy` and it ships off your code to a free builder which builds your container image on their servers and uploads it to a secure private repo, then deploys it.  It's really awesome that they provide this for free, even as you scale beyond the initial free option.  Other CI/CD offerings like Github actions cap you at a certain number of builder minutes a month then charge you afterwards.

# Apps and Organizations

Fly has the concept of organizations, which are collections of individual apps.  Individual apps are essentially Docker containers that can run and scale independently. This is also how you grant access to users to manage your applications.

# Secrets Management / Environment Variables

[Adding and managing secrets](https://open.spotify.com/track/1gVFwk7WUdhdmD7YBSaGxI?si=0bedd9198ce34db4&nd=1) in Fly is very easy and a pleasure to use.  You run a CLI `add` or `update` command and the variable is present to your app's container automatically. That's it.  There's even a very easy way to [define non-sensitive environment variables in your fly.toml](https://fly.io/docs/reference/configuration/#the-env-variables-section). This was probably the easiest and best way of managing environment vars I've used, bar none.

# User Roles and Permissioning

There are NO user permissions beyond having access to an organization.  You have organization access, you have the keys to the kingdom.

Additionally, your API tokens have access to _everything you as an end user have access to_.  There is no way to narrow them down so they only have deploy access to one application.  Most importantly, in order to hook up the command line utility to any deployment pipelines, you have to give your CI/CD tool full access to _everything_ your user has access to 😵, including across organizations if the user is part of more than one. I would really like to see fine grained permissioning so that an access token only has deploy access to one application and nothing else.

To get around this, we created a separate "non production" organization where a whole development team can have permissions to freely configure and deploy the app.  We locked our production organization down to a small subset of trusted committers/deployers.

# Monitoring/Alerting

Fly gives you [Prometheus metrics, displayed via a Grafana instance](https://fly.io/docs/reference/metrics/) out of the box that are super useful.  I really love that they used a well-known, open source tool to do this.  It is a version of Grafana with features disabled, though.

Crucially, _it does not have alerting enabled_.  I had to stand up my own instance of Grafana to be able to set up real-time alerts for errors on my app.

It was fairly easy for me to set up Grafana, and the Prometheus metrics from each app were available to plug into any Grafana instance.

I do like that the metrics are exposed and you can do what you want with it, but I had to sink about half a day into getting Grafana stood up with an SMTP provider.

Also, there is no way I'm aware of to export logs to a more digestible place like Splunk/Elasticsearch.  You can look at logs via the web console or the command line only.

# Databases

Fly does support running [Postgres and other databases](https://fly.io/docs/database-storage-guides/) but they are really just running docker containers on volumes on fly itself.  It doesn't give you out-of-the-box High Availability or monitoring or easy snapshotting and restoring. To set up Postgres, you have to fork one of their repos.

Fly themselves claim that "[Fly Postgres is not managed Postgres](https://fly.io/docs/postgres/getting-started/what-you-should-know/)." Managed means things like replication and automated restores (among other things). They even point you to Heroku for a fully managed version of Postgres.  I commend them for being upfront about it, but it sure would be nice to be able to have a managed database alongside your app without having to worry about this. What they provide is certainly workable though!

# Scheduled Tasks/Jobs

Surprisingly, there is no great way to run a Docker container as a scheduled task.  There is a way to do it using the newer "Machines" API which is a bit buried in their [documentation (search for schedule)](https://fly.io/docs/machines/working-with-machines/), but it only supports limited scheduling options and not cron expressions.

Do a quick [Google search for Fly.io scheduled jobs](https://www.google.com/search?q=fly.io+scheduled+jobs), and you'll see that many users have been rallying for this feature for a while.

# Running Multiple Processes / Sidecar Containers

Related to the Jobs portion, but zooming out a little: Fly does not yet have support for sidecar containers, so running multiple processes requires [adding a process manager into your Docker container](https://fly.io/docs/app-guides/multiple-processes/). The addition of this feature might solve a lot of operational challenges we had!

If we could run a sidecar container, we could probably run our jobs or have a logging exporter to output logs to another service for us. 

# Security

One of the big draws of Fly is its ease of use on the command line.  However, this comes at a cost - Because of the lack of fine-grained permissions noted above, it is incredibly easy to run a command to change the wrong application.  And if your auth token is accidentally shared or exposed to a bad actor, they will have access to change a lot. Rotating keys also becomes problematic because you have to replace just about everything.

Speaking for myself, I would love to have an authentication configuration just for my non-productions applications to make sure I don't inadvertently take down my production app.

# Docker Limitations

Some things you take for granted in Docker may not be available, like for instance the ability to mount multiple volumes in a container.  When I was setting up Grafana for alerting, I needed to mount two volumes to configure the app's secrets securely, but only one volume was supported.  Because of this, I had to work around it by copying files over and doing environment variable substitution.

# Shifting Product Features

Fly is still in the early days, and they are adjusting the product quickly.  This is great because you know that there's a passionate group of developers working on this and making the product better.  It's not so great in that features are shifting and not being supported as well anymore.  For instance, we created a Postgres database, and when we went to create a staging version of it, the option to create it was no longer available.   When we went to the documentation page, we saw this:

![Fly changed their database support and showed this warning](/img/fly-database-warning.png)

When you dive into that, you find out that their new "Apps V2" architecture is supplanting their original Hashicorp Nomad architecture, which everything we built was based on.  We stood up the app in June, and somewhere in the October/November timeframe, this announcement happened.
Unfortunately, at the time of this writing (January 2023), the Machines API is just that: literally a REST API you have to send commands to.  There is no support in their CLI for it or at least minimal support

In the course of writing this, I stumbled on a lot of information about how Fly's Machines-based architecture will likely solve a LOT of these limitations.

One such example is that there is a [Terraform provider for the machines API](https://fly.io/docs/machines/guides-examples/terraform-machines/)!  That's really exciting to see and hear.

# In Summary: Onward and Upwards

I hope I've done a decent job of showing you the pros and cons of using Fly right now. Fly's promise and advantage are that it is obviously run by a team of talented developers working to make a tool that they would want to use.  They don't have the baggage of a large company like Amazon or Salesforce holding them back.  This is really exciting.

Very soon Fly will become a contender to fully replace Heroku for production application and will be much more simple and easy to use than something like Kubernetes or any of AWS's offerings.
Right now, it's pretty much the perfect option for hobbyists or a team with one or two developers.  When you need to get going fast and free, it can't be beat.  Just be aware of the limitations I've mentioned and how they may limit your ability to support your application.

In the meantime, if you need a simple-to-use deployment option for a production application on a team, my opinion is that you should stick with Heroku, or investigate using Elastic Beanstalk on AWS.  Of course, there are other options as well ([Aptible](https://www.aptible.com/) is a very interesting one I've heard about) but these are the ones my employer has experience with. 


_Note: I wrote the original version of this post for my employer, [DEPT® Agency](https://engineering.deptagency.com), you can view it [here](https://engineering.deptagency.com/our-experience-with-fly-io)_
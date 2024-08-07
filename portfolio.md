---
title: Portfolio
date: '2023-01-01'
author: theothermattm
layout: page
background: '/img/skyline.webp'
---
# Things I'm proud to have worked on

I've been in software since 2005. I've worked on lots of things, but I can't show you most of them because they're behind corporate firewalls, require special access, or I'm legally not allowed to share them. 

You can see a more traditional resume [here](/assets/resume-latest.pdf) and on [LinkedIn](https://linkedin.com/u/theothermattm). But, there are a few things that I'm very proud to share with the world, which are outlined here with more context.

## Creating an Engineering-Centric Culture at DEPT® Agency

I've made a lot of software for a lot of clients at [DEPT® Agency](https://deptagency.com) and Rocket Insights, the Boston, MA-based agency I worked for which DEPT® Acquired. Outside of that, I'm most proud of assembling a team of top-notch engineers and creating an engineering-centric culture that attracted and retained good talent.

One of the more public faces of this is DEPT®'s [Engineering Soapboxes](https://github.com/deptagency/engineering-soapboxes).

You can read a [full explanation of what they are here](https://engineering.deptagency.com/introducing-web-blueprint-our-practices-for-web-development), but the gist is an open-source volume of seasoned developers' takes on how to build software. I was responsible for bootstrapping them and getting the team to contribute. I contributed a few articles as well.

You can also read my writing on [DEPT®'s Engineering Blog](https://engineering.deptagency.com/author/matt-merrill).

### Ship It! Podcast

I host and am an interviewer on many episodes of DEPT®'s Engineering podcast, [Ship It!](https://shipit.io). Please give it a listen!

### Open Source 

Wherever possible, I try to open-source my work. Here's one of the open source projects I worked on, [Terraform scripts to setup Mastodon on AWS Elastic Container Service](https://github.com/deptagency/mastodon-terraform-aws-ecs).

## The MITRE@Work Mobile Application

I was an architect and product manager of a mobile application used by the entire workforce of the [MITRE Corporation](https://www.mitre.org/), which at the time had over 11,000 employees.

You can read a bit about it and see a small screenshot at [StepTwo](https://www.steptwo.com.au/award-winner/mitre-corporation-creating-a-mobile-app-to-get-things-done/) and also on [Google Play](https://play.google.com/store/apps/details?id=org.mitre.mobile.arcadia&hl=en_US&gl=US&pli=1).

The application was a native app for iOS (Swift) and Android. It allowed employees to log time and view meetings on personal devices. This doesn't sound impressive; however, due to the sensitive nature of MITRE's work for the Federal Government of the United States, getting this information on any device was unprecedented for the company at the time due to numerous security challenges.

I staffed the team, designed the backend architecture, and contributed a fair amount of Node.js code. This backend system had to navigate those security challenges to safely pass data from an internal data center through the public cloud and onto employees' personal devices.

## Highly Available APIs at Liberty Mutual

Liberty Mutual is not just Car and Home Insurance like the ads you see on YouTube. It has many different business units, including a huge commercial insurance business. You also might not know how big it is: It is a Fortune 100 (yes, 100—not 500) company with over 40 billion dollars in revenue a year and over 40,000 employees when I was working there.

I worked on a team that supported dozens of mission-critical API's for Liberty Mutual.

The APIs we supported were critical to the company's business. They were highly available and cost millions in revenue if they went down for more than a few minutes. And we were a small team. 

I created two services in particular that I'm proud of while I was there:

### Mail Tracking Number API

Did you know that for certified mail in bulk, [assigning tracking numbers isn't as easy as asking the USPS for a number](https://postalpro.usps.com/shipping/bulk-proof-delivery-program)?  

I designed, led, and helped implement an API that assigned mail tracking numbers and allowed auditing of mail as it moved through United States Postal Service delivery. The tracking numbers were often used in court to provide proof of delivery for legal notices.

The system's design had to be highly available and withstand legal scrutiny if the accuracy of tracking was ever challenged in court.

The system's most interesting and challenging part was ensuring that unique tracking numbers could be generated from two disparate, active-active failover databases and that a regular job could run without errors multiple times a day.

The project saved half a million dollars in mail costs every year and is still one of the most interesting projects I've worked on. I'm also particularly proud of the architecture of the system we designed.

### Real-Time Tax Withholding Calculator

This API took various financial inputs and ran complex rules calculations to determine US tax withholdings. The rules were dynamic and could be changed on the fly in an admin interface. 

I led the project and developed much of the back and frontends.

Ultimately, this system consolidated a lot of duplicated logic across multiple organizations at Liberty.


## Open Sourcing Whatever I Can

I love open-source software and contribute whenever I can. With two kids, I don't have much time, but what can I put [my Github profile](https://github.com/theothermattm?tab=repositories&q=&type=&language=&sort=stargazers)

Though I'm not actively involved with any major open-source projects, I try to contribute a bug fix here or there, or I even log an issue to contribute.




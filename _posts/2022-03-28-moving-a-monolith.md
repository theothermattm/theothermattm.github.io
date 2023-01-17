---
title: 'Moving a Monolith'
date: '2022-03-28'
author: theothermattm
layout: post
permalink: /moving-a-monolith
background: '/img/clean/bg-post.jpg'
categories:
    - coding
tags:
    - architecture
    - microservices
---


A while back, I spent about a year working on a team who wanted to move their Java based monolith into the 2020's with a spanking new Microservice / Single Page App (SPA) architecture. Sweet! A greenfield redo! Sound a little too good to be true? Here was the catch:   We needed to keep their old monolith running while we did it, and it was a big, complicated application.

I learned a lot of lessons about what to do and what _not_ to do during this process. 

Why Microservices?
------------------

This particular application team wanted to rearchitect with Microservices but didn't seem to have scale problems. They had a modestly large user base, but nothing that was really stretching the scaling boundaries of their current app.  We found this peculiar.  What was their goal in using Microservices, then?  At this point, everyone likes Microservices, but most are cognizant of the overhead they can incur as well.

It took me and the team a bit of time to figure out that they wanted Microservices to accelerate the pace of development. And they didn’t just want them, they wanted them NOW! Ah, this was a different beast that required a different weapon to take down.

Knowing their primary goal was time to market, we worked towards trying to scope new services with proper domain boundaries rather than simply just creating a new service for each portion of the domain.  Our challenge was to _keep things as simple as possible in a Microservices architecture_.

Mission impossible? Probably. 😃

API first development
---------------------

The desire to quickly deliver a new feature set with this architecture forced our teams to work within an API contract driven development model.  This allowed some developers to run ahead on frontend development by agreeing on what the backend REST API would look like _while_ we built it out.

We documented the API's with [OpenAPI](https://www.openapis.org/) specs from the very beginning.  This was crucial.  Our frontend team used these specs religiously.  Later, we were able to take the open API specs and use tools like [express-openapi-validator](https://www.npmjs.com/package/express-openapi-validator) to do automatic validation and [swagger-to-ts](https://www.npmjs.com/package/@manifoldco/swagger-to-ts) to generate Typescript interfaces from this (if you haven't gathered by now, a bunch of our services were in Node.js and Typescript).  Other teams were using Java, and because OpenAPI is, well... open, the tooling for other languages exists as well and can easily be supported.  Our initial investment in API driven development paid off in spades by avoiding writing a lot of boilerplate code.  This all worked brilliantly and we would repeat it again in a heartbeat.

### Side note about API's:

At the beginning of the project, we had a lot of debate about whether to use REST API's or GraphQL API's.  One thing that jumped out at us was that something like GraphQL didn't _need_ OpenAPI, it was built in!  However, we ended up going with RESTful API's because we were running lean - We needed to get going fast.  We made the decision that the time necessary to setup GraphQL resolvers and other infrastructure was best spent elsewhere.  I'll save the debate about whether or not we were right for another post. 😜

Service boundaries are kinda important
--------------------------------------

Since scaling wasn't our primary goal with this architecture, we set our sights on defining the right service boundaries to choose. Our first feature handled a somewhat broad swath of functionality, so we created a service with a larger set of capabilities than you’d typically see in a microservice.  This was instead of trying to carve off a small niche of functionality to allow it to scale independently.  We had plenty of groundwork to lay, so it was nice that we could start with just one new service and still complete a large feature set.

However, this time to market focused approach of determining service boundaries led to bad decisions (big surprise, huh?).  Because we were under the gun to deliver, we didn't get a chance to fully understand the business domain before we dove in and created a new service.  This led to us (honestly) confusing where various API's should be located:  Should they be in the new service we were creating?  Should we have created multiple services?  Should they be in the legacy application temporarily?  Should they be a completely separate service?

In hindsight:  We should have done more experimentation with service boundaries using _real working code_, and refactored as we went if we needed narrower service boundaries.

Using your cloud provider to the max
------------------------------------

One major factor of the success of our project was staying within one cloud provider's ecosystem.  In our case it was Amazon Web Services (AWS).  This same principle applies for any of the major cloud providers though: Using their utilities whenever we could saved us time and kept things smooth and moving quickly.

Our domain was hosted with Cloudfront.  We used Application Load Balancers (ALB's) to balance traffic.  We used S3 to host our SPA's static assets.

On the API side, we deployed our services into Docker containers using Elastic Container Service (ECS) which stored data in databases managed by Relational Data Service (RDS).  It worked splendidly.  The one place where we strayed from pure AWS was that we configured things with Terraform instead of Cloud Formation.

For running containers, we toyed with the idea of using Kubernetes, but decided that seemed like using a jackhammer when only a hammer was needed.  ECS is "just complex enough" for an orchestration engine, in our experience. A year out, that still seems like the right choice.

Pay attention to the glue
-------------------------

One of the executives described moving off the old system while keeping it running as: “Changing the tires on a bus while it’s moving.”  Which is an apt metaphor. So, how were we going to share data between the systems and gracefully move off over time?

One thing that we did right was picking an API gateway that was endlessly customizable in one central place.  Because we were working in (mostly) a Node.js stack, we chose [express-gateway](https://www.express-gateway.io/) as our entrypoint to our Microservices.

In the year at this client, this has been the biggest question:  “If you’re so bought into AWS, why not just use AWS API Gateway?”  There’s a few reasons:

First, we were excited by the fact that express-gateway was basically just glorified E[xpress.js](https://expressjs.com/) middleware with a pre-existing set of routing policies built in. It was open source, backed by the Linux Foundation and [recommended by Auth0](https://auth0.com/blog/apigateway-microservices-superglue/).  Using it, we could get the best of express along with not having to code our own proxying or filtering logic.  We were able to use existing policies like JWT authentication while also writing our own policies for those strange, unforeseen situations that inevitably come up during a major technical transition like this.

Second, we knew that authentication between the two systems was going to be challenging.  The old system used session based cookie authentication, and our new Single Page App needed token based authentication.  How were we going to bridge that gap while not propagating this issue to each of the underlying services?  We _could_ customize the heck out of AWS API Gateway using Lambdas, but we decided that having a central point where we could keep this code and run pipelines was preferable.  We were able to use express-gateway [custom policies](https://www.express-gateway.io/docs/policies/) to bridge this gap in authentication nicely, all in one place in code, without having a bunch of Lambda code sprinkled around that was hard to track.

Third, we knew that this transition state between an old and new architecture was temporary.  We constantly told everyone that if we did things right in this “semi customized” API Gateway, that someday, when the old system was completely retired, we could completely get rid of it and move to an off the shelf solution like API Gateway. This did end up happening eventually after I left.


Don't kid yourself:  The old Monolith isn't going anywhere soon
---------------------------------------------------------------

The tech team wants a new system, and the product team is smart enough to know (and so are you) that "The Big Rewrite" approach is a recipe for disaster! You want to be lean and move your old system into the new world gradually, delivering value along the way.

But, you can't pretend like your old system is going anywhere soon, even if you really really want it to.  We spent a lot of time setting expectations around this with the client.  In our case, and I'm sure many others, the old system will still be the primary system the business runs on for a good while.  If the old system is shaky enough, it will take down the new system as well.

We spent a good amount of time shoring up the older system, which we had to work hard to justify.  And that _still_ wasn't enough. Moving an application to Microservices doesn't have to be all or nothing.  You can take steps in your existing Monolith to _get it ready to be split up_ into Microservices.  It’s important that your product team understands this for roadmap planning.

Don't short change your investment in the foundation
----------------------------------------------------

A wise colleague once told me:

> You can't fill a garbage truck with cement if you want to lay a foundation.

Along those lines, our primary learning point in this project was around foundational Microservices architecture: You cannot build an "MVP" of Microservices infrastructure.  If you have even the faintest whiff there will be more than one service, you have to go all in and invest in the necessary infrastructure to support many services.

Because of the desire to deliver quickly, our Continuous Integration / Continuous Deployment (CI/CD) pipelines were created using an MVP approach.  This was a mistake.  The minute it became time to create services number 2 and 3, we felt the pain of the missing capabilities we didn’t add.  An example was sharing build artifacts across environments.  These missing capabilities caused each service to take a _long_ time to build, and was repeated for each environment to save a little time at the beginning.  After a couple of services, this was a nightmare.

We didn't have time for things like standards on API formats from the get go, and instead learned in service 2 or 3 that we needed them.

The time spent up front building this foundation would have paid off in spades.

Wrapping up
-----------

*   Microservices are _serious business_.  You can't shortchange the time and effort needed to do them right. of where the product is going and.
*   Use your cloud provider as leverage to move faster
*   Have a deep understanding of the business domain and the roadmap going forward in order to architect service boundaries properly

And of course....

![Paul Allen Yells Devops!](/img/paul-a-devops.jpeg)

Happy microservicing!

_Note: I wrote the original version of this post for my employer, [DEPT® Agency](https://engineering.deptagency.com), you can view it [here](https://engineering.deptagency.com/a-journey-moving-the-monolith-to-microservices)_
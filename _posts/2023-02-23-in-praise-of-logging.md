---
title: 'In Praise Of Logging (A Node.js/Javascript Logging Guide)'
date: '2023-02-23'
layout: post
permalink: /in-praise-of-logging
background: '/img/log.png'
categories:
    - coding
tags:
    - logging
---

I often find myself leaving comments in pull requests about logging. Usually about adding logging where they might be helpful for production troubleshooting, or changing their levels to be more appropriate for production. Understandably, when you're under pressure to hit a deadline, logging can fall to the wayside. I find that most people don't share my love of logging and why it can be great. Done well, it can save you a ton of time and headaches.

In this post, I'm going to outline some good practices I've learned along the way and (hopefully) convince you why and how you should love 🪵 logging 🪵 and why you should consider using a logging framework immediately instead of `console.log` statements.

I'll also show by example what some of the most popular node logging libraries do well and how they stack up against what I'm suggesting.

I am going to be referring to code snippets and output from a sample project I put together called [node-logging-examples](https://github.com/theothermattm/node-logging-examples).  You can run this project over on [Replit](https://replit.com/@theothermattm/node-logging-examples#README.md) by using the "Shell" button at the bottom right.
# Lessons from the Frontline - Aka "Why should I listen to this guy?"

I've supported several server side applications. Including being on call for those applications. When you get woken up in the middle of the night for an alert, you want to make sure that your logs are watertight, otherwise, you'll be water _logged_ (GET IT!?). So watertight you can read them squinting in a dreamy haze! I know what to do, and probably more importantly what not to do.

It's also worth knowing that I spent a lot of my career in the Java world (though not recently). Since this post is targeted at Javascript developers, reserve your judgment when I say: Java does logging _very_ well. There are several battle-tested libraries that allow for out of the box ease, as well as endless customizability.

Okay, on with the show...

## Use a framework built for logging, not `console.log`

Table stakes - The rest of this article will hopefully convince you why! I know some people will disagree, and for really really simple apps you can get away with it, but I can't recall a case in my career where I've started with simple console.log statements and didn't regret it later.
## Separate logging configuration from logging code

Whenever possible, you want the ability to dial logging levels up and down at granular levels without performing a code change. This could be via environment variables, or even through external configuration files or databases.

Either way, during a production incident, or even while debugging, being able to dial up or down logging levels with minimal risk is a lifesaver. 

At a very minimal level, you can simply set a `LOG_LEVEL` environment variable which your logging setup inspects and sets the appropriate level.

Going to the more advanced levels, you might have an external configuration file that defines log levels.  Log4js is the only framework I saw with a specific [option to configure logging with an external source](https://log4js-node.github.io/log4js-node/api.html) right out of the box.

You may be thinking: Of course, we set our log levels outside of code! However, I would ask if you can do it only for the areas of the application which need the logging tweaked?

Read on...

## Configuring logging on a per file/module basis

Being able to dial up logging levels in very specific parts of your application can help reduce a lot of noise when troubleshooting. Of course, modern log ingestion/viewing tools like Elasticsearch or Splunk allow you to do really advanced filtering, but what about when you just want to trace through what's happening for a given user in your application? It'd be nice to see just a bit of logging for the parts of your application that aren't having issues, but a ton for the areas of concern.

A lot of node.js frameworks do not have the ability to do this out of the box, instead relying on you to roll your own configuration.  Which has its drawbacks and benefits.

I've been programming in Node.js for years now, but still, one of the best features of [Log4j](https://logging.apache.org/log4j/2.x/) (the de-facto standard Java logging library) I miss the most while programming in Node is being able to easily customize which modules are logging to what logging level.

As an example, these Java classes:

```java
class Main {
  static final Logger logger = LogManager.getLogger(Main.class.getName());
  
  public static void main(String[] args) {
    logger.info("Begin app ...");

ExternalService.makeACall("test request info");
    FailingService.makeACall("something that will fail");
    InternalBusinessLogic.wickedImportantRules("test input");
    logger.info("Completed app!");
  }
}

public class ExternalService {
  static final Logger logger = LogManager.getLogger(ExternalService.class.getName());
  public static void makeACall(String request) {
    logger.info("Making external service call");
    logger.debug("External Service Call Request {}", request);
  }
}

public class FailingService {
  static final Logger logger = LogManager.getLogger(FailingService.class.getName());
  public static void makeACall(String request) {
    logger.info("Making failing external service call");
    logger.error("Something went wrong in the call!");
  }
}

public class InternalBusinessLogic {
  static final Logger logger = LogManager.getLogger(InternalBusinessLogic.class.getName());

  public static void wickedImportantRules(String arg) {
    logger.info("Running business rules");
    logger.debug("Fixing Johnson rod");
    logger.trace("Arguments for business logic: {}", arg);
    logger.trace("More information here that's really low level");
    logger.debug("Done running business rules");
  }
}
```

Will output like this on the console (configuration dependent):

```
22:20:44.283 [main] INFO  FailingService - Making failing external service call
22:20:44.285 [main] ERROR FailingService - Something went wrong in the call!
22:20:44.286 [main] INFO  InternalBusinessLogic - Running business rules
22:20:44.286 [main] DEBUG InternalBusinessLogic - Fixing Johnson rod
22:20:44.294 [main] TRACE InternalBusinessLogic - Arguments for business logic: test input
22:20:44.294 [main] TRACE InternalBusinessLogic - More information here that's really low level
22:20:44.294 [main] DEBUG InternalBusinessLogic - Done running business rules
```
You can dial logging levels and even separate outputs for individual files/classes. See [this ReplIt](https://replit.com/@theothermattm/JavaLoggingExample) for a working example you can run.

I haven't found any Javascript frameworks that allow this type of customizability, out of the box, however, some logging frameworks have the concept of categories or child loggers that allow you to do something _similar_.

For example, I was able to get Log4js to do this with a little bit of help from their [categories](https://log4js-node.github.io/log4js-node/categories.html) API.

This is the type of output you see with my approach, which results in this type of output where the file name is listed before the message.

```
[2023-02-13T10:58:00.168] [INFO] log4js/example-log4js.js - Syncing clients...
[2023-02-13T10:58:00.172] [DEBUG] log4js/log4jssubfolder/clientservice.js - Getting last run
[2023-02-13T10:58:00.174] [DEBUG] log4js/log4jssubfolder/clientservice.js - Getting clients updated since 2023-02-13T10:58:00
[2023-02-13T10:58:00.174] [INFO] log4js/example-log4js.js - Fetching clients updated since 2023-02-13T10:58:00
[2023-02-13T10:58:00.174] [DEBUG] log4js/log4jssubfolder/clientservice.js - Doing something really complicated
[2023-02-13T10:58:00.174] [ERROR] log4js/example-log4js.js - ERROR Calling Remote Service
[2023-02-13T10:58:00.174] [ERROR] log4js/example-log4js.js - Service Call Result []
[2023-02-13T10:58:00.174] [DEBUG] log4js/log4jssubfolder/anotherservice.js - Doing something really fun!
[2023-02-13T10:58:00.174] [DEBUG] log4js/log4jssubfolder/clientservice.js - Setting last run

```

And you can dial the level up and down with a glob pattern (see the `categories` block):

```javascript
{
  "appenders": { "standard": { "type": "stdout" } },
  "categories": {
    "default": { "appenders": ["standard"], "level": "info" },
    "log4js/log4jssubfolder/**.js": {
      "appenders": ["standard"],
      "level": "debug"
    },
    "perf": { "appenders": ["standard"], "level": "trace"}
  }
}

```

It would be great if this were built into log4js, but it's fairly easy to see how it's done ([source here](https://github.com/theothermattm/node-logging-examples/blob/main/log4js/log4jslogger.js)). Here are the highlights:

```javascript
import { fileURLToPath } from 'url';
import glob from 'glob';


const defaultOptions = {
  ignore : ['default'],
}

function getLog4JOptionsUnGlobbed(globbedLog4JsOptions, globOptions) {

  const mergedGlobOptions = {...defaultOptions, globOptions};
  const unglobbedOptions = JSON.parse(JSON.stringify(globbedLog4JsOptions));
  for (const category in globbedLog4JsOptions.categories ) {
    const matchingFiles = glob.sync(category, mergedGlobOptions);
    if ( matchingFiles && matchingFiles.length > 0 ) {
      matchingFiles.forEach((file) => {
        unglobbedOptions.categories[file] = globbedLog4JsOptions.categories[category];
      })
      delete unglobbedOptions.categories[category]
    }
  }
  return unglobbedOptions;
}
function createModuleLogger(fileName) {
  // you can just use __filename if using commonjs modules! (eg require() format)
  const __filename = fileURLToPath(fileName);
  const relativeModuleName = path.relative('', __filename);
  return log4js.getLogger(relativeModuleName);
}
```

You can create new loggers like this in each of your modules/files:

```javascript
// again, you don't need import.meta.url if you're using commonjs
const logger = createModuleLogger(import.meta.url);
```

After that, we're using log4js categories to add the filename for output and configuration. With a little help with some preprocessing the log4js config file with the [glob](https://www.npmjs.com/package/glob) library, we can dial those levels up and down on a per file (or folder) basis.

## Use low-level log statements instead of comments to document code

You can use logging statements to simultaneously document your code _and_ provide useful logging output that you can turn on or shut off with a logging framework.

Consider this simple example, ignore the details, they're purposely opaque so you can pretend you're in a new codebase:

```javascript
const syncClientsWithoutLoggingFramework = async function() {
  console.log('Syncing clients...');
  const lastRun = await getLastRun();
  const from = lastRun || moment().subtract(3, 'days').format(DATE_FORMAT);
  const timeOfLastFetch = moment().format(DATE_FORMAT)
  const clients = await clientsUpdatedSince(timeOfLastFetch);

  console.log(`Fetching clients updated since ${timeOfLastFetch}`);
  if (clients.length === 0) {
    console.log('No updated clients.')
    return;
  }

  // call out to another service to modify the clients list
  // with some _really important business logic_
  const modifiedClients = await doSomethingReallyComplicatedInAnotherService(clients);
  if ( modifiedClients < 1 || (modifiedClients.errors && modifiedClients.errors.length > 0) ) {
    console.error('ERROR Calling Remote Service')
    console.error(`Service Call Result ${JSON.stringify(modifiedClients, null, 2)}`)
  }

  // Store the last time we updated the clients.
  await setLastRun(moment())
  return modifiedClients;
}
```



That results in this logging output:
```
Syncing clients...
Fetching clients updated since 2023-01-23T17:25:18
ERROR Calling Remote Service
Service Call Result []
```

Well, it's better than nothing! But I'm still kinda confused about what's going on.

If we add a logging framework (here I'm going with [Pino](https://github.com/pinojs/pino)) and use the lowest log level (in this case, `trace`), this is what the code looks like as we run it:

```javascript
const logger = pino({level: 'trace'});

const syncClientsWithLoggingFramework = async function() {
  logger.info('Syncing clients...');
  const CLIENT_LAST_RUN_KEY = 'CLIENT_LAST_RUN_DATE';
  const lastRun = await getLastRun();
  const from = lastRun || moment().subtract(3, 'days').format(DATE_FORMAT);
  const timeOfLastFetch = moment().format(DATE_FORMAT)
  const clients = await clientsUpdatedSince(timeOfLastFetch);

  logger.info("Fetching clients updated since %s", timeOfLastFetch);
  logger.trace("Fetching clients with parameters from: %s and timeOfLastFetch: %s", from, timeOfLastFetch);
  if (clients.length === 0) {
    logger.warn('No updated clients.')
    return;
  }

  logger.debug("Calling out to another service to do some really complicated business logic");
  const modifiedClients = await doSomethingReallyComplicatedInAnotherService(clients);
  if ( modifiedClients < 1 || (modifiedClients.errors && modifiedClients.errors.length > 0) ) {
    logger.error('ERROR Calling Remote Service')
    logger.error(`Service Call Result ${JSON.stringify(modifiedClients, null, 2)}`)
  }
  logger.trace("Done calling out and doing important things.");

  logger.trace("Setting last run date so we know where to pick up next time.")
  await setLastRun(moment())
  return modifiedClients;
}
```

This is what the output looks like, and before you judge, hold up:
```json
{"level":30,"time":1674503301467,"pid":918,"hostname":"f6572868d2b5","msg":"Syncing clients..."}
{"level":30,"time":1674503301469,"pid":918,"hostname":"f6572868d2b5","msg":"Fetching clients updated since 2023-01-23T19:48:21"}
{"level":50,"time":1674503301469,"pid":918,"hostname":"f6572868d2b5","msg":"ERROR Calling Remote Service"}
{"level":50,"time":1674503301469,"pid":918,"hostname":"f6572868d2b5","msg":"Service Call Result []"}
```

You might think "But when I'm developing, this looks awful!" And you're right.

But with a logging framework, you can easily configure your logs to be prettified and colorized for development, along with dialing the logging level up to get more information:

```js
const logger = pino(
  {
  level : 'trace',
  transport: {
    target: 'pino-pretty',
    options: {
      colorize: true
    }
  }
});
```

Alternatively, Pino lets you use `pino-pretty` as a command line utility like this:

```
npm start | pino-pretty
```

You can even put a separate `pino-prettyrc` config to customize the output as desired.

This is what the colorized, prettified output looks like:

![Color output from logger](https://user-images.githubusercontent.com/392778/214134946-4a4fa674-7f9a-4376-afa1-9f89f094ff4b.png)

With the addition of a couple of lines of configuration, you have extremely useful JSON objects for production logging, as well as nicely readable, colorized log statements for my command line development with a timestamp to know when things were logged.

Using environment variables, you can very easily drive these logging configurations for local versus production.  There is an example of this with Pino in my example project [here](https://github.com/theothermattm/node-logging-examples/blob/main/pino/pinologger.js#L8). Using console logging makes this extremely hard.
## Bonus: Having a logging _Adapter_

This one is a nice to have, but a lot of people don't really think about it. Let's go back to Java for a minute to illustrate: Generally, most Java applications reach for the [Log4J Api](https://logging.apache.org/log4j/2.x/manual/api.html) to log information. This is an _interface_ for logging in Java. Meaning that other libraries can implement that interface. In Javascript, we don't have interfaces, in Typescript we kinda do, but the point is to have an adapter that delegates the act of logging to a library you configure.

Why is this nice?  Well, let's say you start with a logging framework that serves you well while your app is small, but then you switch to using Elasticsearch to ingest logs. Maybe there's another framework that provides really easy integration to ingest Elasticsearch logs. You can switch out that implementation without changing your actual code. This also makes it easy for library maintainers to have a consistent way to log output.

(As a side note, most people in the Java world just use the Log4J implementation for everything, but it's nice to have options.)

Compare this approach against Node, where there are a number of methods for logging, but no standard interface or adapter. The best we can do is use the traditional `console.log`, `console.error` (and so on) statements and then use another tool to scrape the console output to ingest in a different way. Certainly nothing wrong with that, but it can get kinda complicated depending on your deployment setup. However, Log4js, one of the logging frameworks I'll go over, does have an [API only library](https://www.npmjs.com/package/@log4js-node/log4js-api) that you can use. 

# Node Logging Frameworks - An selected overview

This is by no means an extensive list of logging frameworks for Node, but rather a specially selected list based on popularity and my own use and interests based on my recommendations above.

For another great analysis, check out [this article](https://geshan.com.np/blog/2021/01/nodejs-logging-library/).

## [Bunyan](https://www.npmjs.com/package//bunyan)

⭐7k, 2 million weekly downloads on NPM

This library has been around since the early days of Node and is well respected. Unfortunately, the GitHub project hasn't been updated in 2 years as of this writing, and it is also somewhat of a laggard in the performance area as you'll see below.  There is no ability to do external configuration that I see, and it would be fairly difficult to roll your own.

For this reason, I don't recommend it.

## [Log4js](https://www.npmjs.com/package/log4js)

⭐5.6k, 3.7 million weekly downloads on NPM

I came into this article never having used this library. Probably because I thought "Why would I want to use a java-like library in node?" As the README itself states:

> Although it's got a similar name to the Java library log4j, thinking that it will behave the same way will only bring you sorrow and confusion.

While it's a bit slow on the performance end of things (which you can see in the bonus section at the end of this article) its out of the box configuration settings are very easy to use and extremely useful. Pino also allows a huge amount of customization and much, much better performance, but since it works with streams it can be harder to reason about. Through writing this article, I became a big fan of log4js, despite its name.

## [LogLevel](https://www.npmjs.com/package/loglevel)

⭐2.4k, 9.3 million weekly downloads on NPM

It's so simple! Definitely worth a look, but I think LogLevel's brilliance lies in having a logging framework that works with your browser's console _or_ your server's console. Out of the box, you can't even get timestamps without some gymnastics.

## [Pino](https://www.npmjs.com/package/pino) 

️⭐10.9k, 4.5million weekly downloads on NPM

This framework is newer than the others, having gone 1.0 in 2016. It has been making the rounds in Node projects at DEPT® recently to good fanfare amongst our developers. As it claims, it is _fast_ and very customizable. To me its best feature is the ability to perform logging on a separate thread asynchronously with the flip of a configuration switch. Very cool. On any project that required some _serious_ log gymnastics and high performance, I'd definitely go with Pino.

## [Winston](https://www.npmjs.com/package/winston) 

⭐21.1k, 12.6million weekly downloads on NPM.

One of the more established and popular options, having been around since 2011, I've used Winston personally in many past projects. My opinion of it before going into this analysis was that it was ... fine. A little wonky to set up, and the `silly` logging level is cute, but leaves me scratching my head why they didn't use `trace` like almost every other logging framework.

Winston allows for good customization of logging transports and the ability to customize formats quite well. Its configuration API is a bit wonky though. I've used it in production applications for years and never really liked it, but rather just accepted its wonkiness. Now that I know there are better options, I don't think I'd go back to it.

## My Takeaways

First, I don't think I would ever start a project with plain console logs again after knowing what kind of benefits you can get from a logging framework over time. I know some will disagree with me, and that's fine. But, I'd ask that you consider it. Logging can save you a _lot_ of headaches in an application that's operational.
 
For Node.js, there are lots of great logging frameworks available. If low level customizability and performance are your utmost concern, go with Pino. Its ability to take streams and log asynchronously out of the box is amazing.

For everything else, I can honestly recommend log4js. I hadn't used it (or even heard of it!) before I started writing this, and after working with it, I really loved its ability to get up and running fast and the ease of customization. As you saw here, it's a bit slower, but for the majority of applications I work on, this isn't much of an issue, especially since most production logging levels are dialed way back to info and above level log output. For my next project, I plan on using log4js if I have the choice.

Go forth and Log!
# What About Logging in the Frontend Client?

By [Jake Rainis](https://www.linkedin.com/in/jakerainis/)

Historically, it could be argued this sort of complexity on the server is far greater than on the client since the front-end has traditionally been more responsible for layout and aesthetic than it has for business logic. However, the behavioral complexity of modern front-end applications continues to increase, particularly in framework-driven front-ends such as React, Angular, or Vue. And depending on the context of how the front-end application operates functionally, logging mechanisms could provide a great deal of insight into behavior resulting in an easier way to identify and isolate bugs.  \

So, where does that leave us on the front-end?

Of course, we all know that we can drop `console.log`s anywhere we please to debug an issue during development, but this isn’t a mature solution. These transient log statements are isolated to a single user’s browser session, and then they’re gone forever. Front-end applications run in the browser and don't have the same luxuries as applications running on a server where events can be captured. \

## What About My NextJS App?

You might be wondering about full-stack frameworks such as a [Next](https://nextjs.org/), [Remix](https://remix.run/), or [Nuxt](https://nuxtjs.org/). These too run on a Node server, so they _do _enable logging solutions… But only on the server-side aspects of the application. 

For example, let’s consider a Next application that uses the [Pino](https://github.com/pinojs/pino) library ([Next recommends Pino](https://nextjs.org/docs/going-to-production#logging)). Dropping a Pino log within a Next API route, a middleware, or even in one of Next’s server helpers such as `[getServerSideProps](https://nextjs.org/docs/basic-features/data-fetching/get-server-side-props)` will result in the type of log we’re after here. On the other hand, dropping a Pino log into one of the application's React components would _only_ result in a log to the browser's console. Once again, this is helpful for debugging throughout the development cycle, but it doesn’t help engineers when the application is running in production since the log is never captured!

## Front-End Logging Solutions

As you’ve likely surmised by now, we can’t effectively capture a log from a client-side application without some additional effort. But it is entirely possible! After all, this is a common conundrum for application developers.

The gist of front-end logging lies in sending the log events _somewhere_ to be captured and this could even be accomplished through a custom hand-rolled solution that sends events to your back-end (though, be cautious of the overhead this might introduce). However, there are also third-party solutions out there to make the job easier. 

[Sentry](https://sentry.io/welcome/) is perhaps the most battle-tested error and performance monitoring service that plugs seamlessly into [any application](https://sentry.io/platforms/) — even front-end applications. But there are others too, like [BugSnag](https://www.bugsnag.com/) and [Logflare](https://logflare.app/) (recently acquired by [Supabase](https://supabase.com/)). 

One aspect of the front-end that we don’t have to worry as much about when monitoring a back-end are the browsers themselves. _What browser/version is the end-user viewing our app from? Oh jeez… what if it’s Internet Explorer? [Just kidding](https://www.wired.com/story/microsoft-internet-explorer-is-finally-really-fully-dead/).  What device are they on? What user flow did they follow to trigger this event/error? _One one the great selling points about the third-party services listed above is that they capture this information. This additional context can make bug reproduction and squashing much easier for application engineers.

These tools will come with a price tag when used at scale, but all have generous free tiers that might be sufficient for smaller teams and applications. And since they also work for back-end technologies, logging for all portions of an application can be captured, evaluated, and triaged in one place. 

## Does Every Front-End Need Logging?

In general, logging and monitoring is a best practice and should not be an afterthought when it comes to a back-end application. After all, it’s simple and quick to implement. 

But is it absolutely necessary on the front-end? Well, any front-end app would benefit, but the right answer is subjective. A larger and/or more complex front-end that manages a good deal of business logic would be a better candidate than a relatively static front-end that doesn’t have much complex functionality — particularly if it’s well-tested. Ultimately, it should be a discussion with the product and engineering team to determine what monitoring solutions make sense for your application. \

Speaking of, we here at DEPT® have quite a bit of experience in the realm of application monitoring. We’d love to chat more about it, so don’t hesitate to reach out!

# BONUS SECTION: Logging Framework Performance Analysis

If you've made it this far, I appreciate you and welcome, fellow logging nerd!

Now how about performance?  Pino claims to be super fast.  Let's find out!

To test, I ran a loop of 100,000 simple statements like this (with varying syntax for each logging system):

```js
  const numberOfLoops = 100000
  let startTime = new Date();
  for(let i = 0; i <= numberOfLoops ; i++) {
    console.info(`Test Log Message ${i}`)
  }
  let endTime = new Date();

  console.info(`Time to execute ${numberOfLoops} log messages: ${(endTime-startTime) / 1000} seconds`);
```

To get a better sample, I ran the node process using the GNU `time` command 20 times each using [a bash script](https://github.com/theothermattm/node-logging-examples/blob/main/scripts/perf-test.sh). I purposely wrote the output to a file so my tty was not the bottleneck. This is a sample of the output of each command in the output files:

```
/usr/bin/time -a -o pino-times-output.txt node index.js -f pino -p >> pino-output.txt
<< snip ... >>
{"level":30,"time":1675893207318,"pid":54614,"hostname":"Matts-16-Macbook-Pro.local","msg":"Time to execute 100000 log messages: 517 ms"}
        0.62 real         0.39 user         0.15 sys
```

This was running on my Intel 2.6ghz 6 core i7 Macbook with 16gb of ram. I used the "real" time for comparison.

Of course, this probably isn't a perfect test, but let's see what happened:

![Results in a Table](https://user-images.githubusercontent.com/392778/218817393-7d7321cd-0e37-4424-8e2d-b8e3096ba272.png)

*And it's console winning the race by a nose!*

Let's take a deeper look.

## The Laggard: Log4js

The laggard in the race was log4js, coming in almost 200ms longer than any other framework at an average of 1.54 seconds.

## The middle tier

Second to last was Bunyan, doing slightly better than Log4js at 1.44 seconds. Third to last was Winston at 1.42 seconds, then LogLevel at 1.38 seconds.

## Runner Up: Pino

As promised, Pino did very well, coming in only about 80 milliseconds slower than console logging. And that's with _synchronous_ logging, not [async logging](https://getpino.io/#/docs/asynchronous), which offloads logging onto worker threads to make logging a non-blocking operation, which is awesome. Of course, the whole command would likely take just as long with the worker thread, but it's nice to know that for an online application your logging won't be blocking your application's threads.

## Console

Probably not a surprise, but good old console.log came in first by about 80 milliseconds. Given that there's nothing else happening with the logs, this makes total sense.  If performance is your concern, console.log can't be beat!  But, since it's not nearly as customizable, there are plenty of tradeoffs to consider.
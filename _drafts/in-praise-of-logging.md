
---
title: 'In Praise Of Logging (A Node.js Logging Guide)'
date: '2023-01-19'
layout: post
permalink: /in-praise-of-logging
categories:
    - coding
tags:
    - logging
---

Even though in my day to day job, I'm not coding as much as I used to, I still _review_ lots of code. One of the things I find myself leaving comments about a **lot** is adding logging statements at various levels. I find that most people don't share my love of logging and why it can be great. 



One thing I've noticed in the node.js world is a general lack of appreciation or knowledge of a good logging framework. This is starting to change with things like [NestJS](https://docs.nestjs.com/techniques/logger) on the backend providing an opinionated way to do this.  

In this post, I'm going to outline some good practices I've learned along the way and (hopefully) convince you why and how you should love 🪵 logging 🪵 and why you should use a logging framework immediately isntead of plain old `console` statements.

# Lessons Learned

I cut my development teeth in the Java world. Since this post is targeted at Javascript developers, please reserve your judgement and believe me when I say: Java does logging well. There are a number of battle tested libraries that allow for out of the box ease, as well as endless customizability.

## Having a logging _Interface_

Generally, most Java applications reach for SLF4J (Simple Logging Facade for Java)[https://www.slf4j.org/] or the [Log4J Api](https://logging.apache.org/log4j/2.x/manual/api.html) to log information. Reserving judgement on the complicated names, they are _interfaces_ for logging in Java. Meaning that other libraries can implement that interface.

This is nice, because it gives you a standard way to log throughout your application, but allows you to change the underlying detail of how the logs actually get generated at deployment time.

Why is this nice?  Well, let's say you start with a logging framework which serves you well while your app is small, but then you switch to use Elasticsearch to ingest logs. Maybe there's another framework which provides really easy integration to ingest Elasticsearch logs. You can switch out that implementation without changing your actual code.

As a side note, most people in the Java world just use the Log4J implementation for everything, but it's nice to have options.

Compare this approach against Node, where there are a number of methods for logging, but no standard interface. The best we can do is use the traditional `console.log`, `console.error` (and so on) statements and then use another tool to scrape the console output to ingest in a different way. Certainly nothing wrong with that, but it can get kinda complicated depending on your deployment setup.

## Configuring logging on a per file/module basis

I've been programming in node for years now, but still, one of the best features of Log4j I miss the most programming in Node is being able to easily customize which modules are logging to what logging level.

As an example, with this configuration:

```xml
<Configuration name="ConfigTest" status="ERROR" monitorInterval="5">
    <Appenders>
        <Console name="ConsoleAppender" target="SYSTEM_OUT">
            <PatternLayout pattern="%d{HH:mm:ss.SSS} [%t] %-5level %logger{36} - %msg%n" />
        </Console>
        <File name="FileAppender" fileName="application-${date:yyyyMMdd}.log" immediateFlush="false" append="true">
            <PatternLayout pattern="%d{yyy-MM-dd HH:mm:ss.SSS} [%t] %-5level %logger{36} - %msg%n"/>
        </File>
    </Appenders>
  <Loggers>
    <Logger name="InternalBusinessLogic" level="TRACE" additivity="false">
      <AppenderRef ref="ConsoleAppender"/>
    </Logger>
    <Logger name="FailingService" level="DEBUG" additivity="false">
      <AppenderRef ref="ConsoleAppender"/>
    </Logger>
    <Root level="WARN">
      <AppenderRef ref="ConsoleAppender"/>
      <AppenderRef ref="FileAppender"/>
    </Root>
  </Loggers>
</Configuration>
```

And these Java classes: 

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

I can get output like this on the console:

```
22:20:44.283 [main] INFO  FailingService - Making failing external service call
22:20:44.285 [main] ERROR FailingService - Something went wrong in the call!
22:20:44.286 [main] INFO  InternalBusinessLogic - Running business rules
22:20:44.286 [main] DEBUG InternalBusinessLogic - Fixing Johnson rod
22:20:44.294 [main] TRACE InternalBusinessLogic - Arguments for business logic: test input
22:20:44.294 [main] TRACE InternalBusinessLogic - More information here that's really low level
22:20:44.294 [main] DEBUG InternalBusinessLogic - Done running business rules
```
And separately I can get my errors in a file that gets written out.

I can dial logging levels and even separate outputs for individual files/classes.

TODO can you do this in any node frameworks?
I haven't found any Javascript frameworks that allow this type of customizability, however, some logging frameworks have the concept of categories or child loggers that allow you to do something _similar_.

Take this example in Pino:

```

```

# General Best Practices

## Use `trace` or `debug` statements instead of comments

In combination with a good, configurable logging framework, you can use logging statements to simultaneously document your code _and_ provide useful logging output.

Consider this simple example, you can find the full, working code snippet for you to Fork and play around with on [ReplIt](https://replit.com/@theothermattm/LoggingExample#index.js)

```javascript
const syncClientsWithoutLoggingFramework = async function() {
  console.log('Syncing clients...');
  const CLIENT_LAST_RUN_KEY = 'CLIENT_LAST_RUN_DATE';
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



That results in thislogging output:
```
Syncing clients...
Fetching clients updated since 2023-01-23T17:25:18
ERROR Calling Remote Service
Service Call Result []
```

If we add a logging framework, this is what the code looks like:

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

Here I'm going to go with [Pino](https://github.com/pinojs/pino), but there are a number of other options you could choose I'll discuss in a bit.

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

This is what the colorized, prettified output looks like:

![Color output from logger](https://user-images.githubusercontent.com/392778/214134946-4a4fa674-7f9a-4376-afa1-9f89f094ff4b.png)

With the addition of a couple of lines of configuration, you have extremely useful JSON objects for production logging, as well as nicely readable, colorized log statements for my command line development with a timestamp to know when things were logged.

Using environment variables, you can very easily drive these logging configurations for local versus production.

The default console logging makes this extremely hard.

# What I Recommend


# Performance

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

This was running on my Intel 2.6ghz 6 core i7 Macbook with 16gb of ram.

Of course, this probably isn't a perfect test, but let's see what happened:

![Results in a Table](https://user-images.githubusercontent.com/392778/218269979-b37fd7bd-88a8-40a2-b73d-7643c14be413.png)

*And it's console winning the race by a nose!*

Let's take a deeper look.

## Log4js

The laggard in the race was log4js, coming in almost 200ms longer than any other framework at an average of 1.63 seconds.

TODO insert some ideas as to why

## Winston

Second to last was winston, doing slightly better than Log4js at 1.44 seconds.

TODO insert some ideas as to why

## Pino

As promised, Pino did very well, coming in only about 40 millseconds faster than console logging. And that's with _syncronous_ logging, not [async logging](https://getpino.io/#/docs/asynchronous), which offloads logging onto worker threads to make logging a non-blocking operation, which is awesome. Of course, the whole command would likely take just as long with the worker thread, but it's nice to know that for an online application your logging won't be blocking your application's threads.

## Console

Probably not a surprise, but good old console.log came in first by about 40 milliseconds. Given that there's nothing else happening with the logs, this makes total sense.  If performance is your concern, console.log can't be beat!  But, since it's not nearly as customizable, there are some tradeoffs.

# What About Logging in the Frontend Client?

By [Jake Rainis](TODO)

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

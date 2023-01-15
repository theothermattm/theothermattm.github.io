---
id: 183
title: 'Where to start with Node.js as a Java developer in 2017'
date: '2017-01-07T21:11:57+00:00'
author: theothermattm
layout: post
guid: 'https://code.theothermattm.com/?p=183'
permalink: /where-to-start-with-node-in-2017/
background: '/img/clean/bg-post.jpg'
categories:
    - coding
tags:
    - java
    - javascript
    - node.js
    - tools
---

<p>Recently a friend of mine who is a veteran Java developer was asking me about Node.js development.  He tried to do some Node development 3 or 4 years ago, and got frustrated with the lack of tools and Javascript callback hell.     I tried to tell him that things have changed greatly since then, but found myself not able to formulate a good response in the moment.  He had also read <a href="https://hackernoon.com/how-it-feels-to-learn-javascript-in-2016-d3a717dd577f">"How It Feels To Learn Javascript in 2016"</a>, which is <em>really</em> funny, but isn't exactly a vote of confidence to try Node again!</p>

<p>Shortly after, I came across <a href="https://dev.to/grahamcox82/why-i-struggle-with-node">"Why I Struggle With Node"</a> by Graham Cox.  It's a great post about why he loves Node.js, but gets frustrated with its ecosystem and prefers that of Java.  </p>

<p>Having been a Java developer for 7 years before switching to do mostly Javascript development, I can say that I agree with many of Graham's sentiments.  The Java ecosystem is much more mature and the tools are great. There are many things I miss. But I don't get frustrated with Javascript's tooling as much.  I actually find the Javascript ecosystem and its tools invigorating, as long as you know where to start...</p>

<p>The post made me think of my conversation with my friend.  I linked to Graham's post as a starting point, added some additional thoughts and sent them to my friend to give him a place to start with Node again.  After I wrote it up, I thought it might be useful to other Java developers who haven't taken the dive into Node.js and would like to, but might be intimidated or not know where to start.</p>

<p>If you're in that boat, I suggest you first <a href="https://dev.to/grahamcox82/why-i-struggle-with-node">read Graham's post</a>. It has a great list of tools to start with that translate well into front end code too.  </p>

<p>After you read that post, here are my notes and addons that I think might be helpful if you are coming into Node development from Java in the beginning of 2017:</p>

<h1 id="learningjavascript">Learning Javascript</h1>

<p>If you don't already have a good grasp on the Javascript language, or consider yourself just good enough to be dangerous, I found <a href="http://www.2ality.com/2013/06/basic-javascript.html">"Basic Javascript for the Impatient Programmer"</a> to be particularly useful in improving my understanding of the language.  There's tons of great books as well out there that you can easily find.</p>

<p>Also, before you dive into creating a full app or start setting up tools, learn the <a href="https://spring.io/understanding/javascript-promises">Promise</a> API and use it from the start in your code.  It will make your code easier to reason about, and will make your life much more pleasant once you get past the initial learning curve.  If you can, use the <a href="http://bluebirdjs.com/">Bluebird</a> library for Promises.  It has lots of great extras on top of the native implementation, and rumor has it that <a href="http://stackoverflow.com/a/34961040/288935">it's even faster</a>.</p>

<h1 id="building">Builds</h1>

<p>Like Graham, I use <a href="http://gruntjs.com/">Grunt</a> for building, too. I haven't tried Gulp, I'm sure it's great, but I've used Grunt for a while and have found it effective.  Though, like Graham, I do miss the fact that Maven takes care of a lot of the gruntwork (ha ha!) for you.  For my team's apps, we have separate Grunt tasks and configurations for building front end and back end code since folder structures are different.</p>

<h1 id="transpilers">Transpilers</h1>

<p>I don't use Babel. If I'm being honest, the whole transpiler thing is a mystery to me. I kinda get it: You want to use new features in the language, especially in a browser environment that adapts rather slowly.  But if you're writing production code that needs to be supported by a team with varying levels of Javascript knowledge, it seems to add unnecessary complexity. It's hard enough keeping up with Javascript, let alone when you do source to source compiling.  It will also make your toolchain a lot easier to configure, and your debugging a lot more straightforward.</p>

<h1 id="codecoverage">Code Coverage</h1>

<p>This is mentioned in the post but I thought I'd provide what I've used:  <a href="https://github.com/taichi/grunt-istanbul">Istanbul through Grunt</a>.  It's nice and simple and does the job well.</p>

<h1 id="logging">Logging</h1>

<p>Graham mentions that Node logging isn't as nice as what he's used to in Java.  That's true, but I've used <a href="https://github.com/winstonjs/winston">Winston</a> quite effectively in production apps.</p> 

<h1 id="debugging">Debugging</h1>

<p>Graham is right, debugging in Node is hard. I attribute some of that to it's asynchronous nature, though. </p>

<p>I've used <a href="https://www.npmjs.com/package/node-debug">node-debug</a> effectively as a debugger. It's quite nice, but a real pain to setup. But, if I'm being honest again, usually I try to use <a href="https://github.com/winstonjs/winston">Winston</a> (mentioned above) to put some nice low-level logging statements in place so I can debug with those, then turn logging levels up and down as needed.  Sometimes those low level statements help when you'd least expect it.</p>

<h1 id="applicationwiring">Application Wiring</h1>

<p>When I first started with Node, we were using a dependency injection framework in Node since my team was used to using Spring in Java.  After a while, we just got frustrated with it and abandoned it.</p>

<p>Javascript is a dynamic language, so it's really easy to stub and mock things out.  Use something like <a href="https://www.npmjs.com/package/proxyquire">Proxyquire</a> to trivially inject mocks in tests.</p>

<h1 id="integrationtesting">Integration Testing</h1>

<p>Graham's comments about integration testing being much easier in Java are spot on. I haven't come across nearly as many drop in, in-memory replacements for connected systems like databases or message queues.  This makes it tough to integration test.  What I've done in the past is to create throwaway Continuous Integration database instances at the beginning of the tests and then clear them out at the end of the tests.  It's not that bad once you set it up once, but I yearn for the ease of using something like HSQL in Java.</p>

<h1 id="developmentenvironmentsanddocker">Team Development Environments and Docker</h1>

<p>My team has half Windows users and half Mac users.  It makes for a challenging environment for everyone to setup their development environment in Node.  </p>

<p>If you're on a team with developers using multiple platforms, and deploy to Linux, what I recommend is to create a Dockerfile which defines your development environment. Then, use a <a href="https://docs.docker.com/engine/tutorials/dockervolumes/">Docker volume mount</a> to put your source code inside the container. (Side note:  Use an <a href="https://hub.docker.com/_/node/">official Node.js Docker image as your base image</a>). This lets you use normal Docker build/run commands to bootstrap your dev environment.  The volume mount lets you dynamically update your code so you can still iterate fast, as if you were working on your local machine.  It works well across platforms (no worrying about exceptions in Windows), and as a bonus, if you ever want to run your app in a Docker container, you've already got it more than halfway there.  I'm hoping to post more about this in the future.</p>

<h1 id="goenjoy">Go enjoy!</h1>

<p>I've been developing in Node for about 3 years now, and I still really love the developer experience.  Sure, I'm probably not using the latest and greatest tools that just came out, but this workflow has gotten me to a good place where I can be very productive and crank out some good quality code.</p>

<p>I hope some of these notes help you do the same.  </p>

<p>What tools/techniques/libraries did I miss that were important for you when you came to Node from Java?</p>
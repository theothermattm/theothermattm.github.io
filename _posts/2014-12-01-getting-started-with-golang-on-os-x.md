---
id: 13
title: 'Getting started with Golang on OS X'
date: '2014-12-01T15:20:15+00:00'
author: theothermattm
layout: post
guid: 'https://code.theothermattm.com/?p=13'
permalink: /getting-started-with-golang-on-os-x/
background: '/img/clean/bg-post.jpg'
categories:
    - coding
tags:
    - golang
    - languages
    - mac
    - 'os x'
---

I've heard a lot of buzz recently about the (relatively) new language [Go](https://golang.org/) (aka Golang) created by Google.  Notably, that the fantastic [Docker](http://www.slideshare.net/jpetazzo/docker-and-go-why-did-we-decide-to-write-docker-in-go) was written in it.

I decided to try learning it recently, so thought I'd share my experience getting setup with a development environment on OS X.   I've been doing a lot of [Node.js](http://nodejs.org/) development recently, and I come from a Java background, so it was a bit of a shift in what I'm used to setting up a development environment.  

My intention with this post is not to teach Go fundamentals (see later on in the post for some resources), but rather to help you get your environment setup so you can start writing and running Go code quickly on your Mac.

Finally, at the end, if you're still reading, I'll share some of my thoughts about how my experience went and about the language itself. 

## Getting started using Homebrew

First off, I use [Homebrew](http://brew.sh/) to install all my developer tools, so I pursued that route to get myself started as opposed to installing from source.  I'll document those steps below and note where they're Homebrew specific, however, you can also [install from the source](https://golang.org/doc/install)

## First, install Mercurial (using Homebrew)

Go needs this to install lots of common packages.

`brew install hg`

## Second, install Go itself (using homebrew)

`brew install go --cross-compile-common`

You'll get some information about exporting some directories to your `$PATH` variable.  More on that below.

The `cross-compile-common` option will come in handy later if and when you want to try creating executables for OS'es other than OS X.  This option will increase your install size, though.  So, if you don't plan on trying cross compilation, don't worry about adding that flag.

## Third, set the right environment variables

Go has a much different directory setup than I'm used to with something like Node.js, where you can simply put a project wherever you want on your filesystem and run a command. 

Go expects you define where your main workspace is, and it will then install packages and compile binaries relative to that path.  It does this by using an environment variable called `$GOPATH`.

Additionally, if you don't install in the ["standard" Go install location](https://golang.org/doc/install), you'll need to tell Go where to find binaries.  You can do this by setting the `$GOROOT` variable.  If you installed with Homebrew, see the output it gives you during the install, but the install location is _most likely_ something like this:

`export GOROOT=/usr/local/opt/go/libexec`

Finally, you'll want to make sure anything you compile you can execute easily on your path.  When you compile, Go will by default put your binary in the root of your Go workspace in the `bin/` directory.  You can use the `$GOPATH` variable to reference it.

Also, because the Homebrew output suggested it, I added the `$GOROOT/bin` location to my `$PATH` variable for good measure:

`export PATH=$PATH:$GOROOT/bin:$GOPATH/bin`

(Note that that last part may not be necessary given that Homebrew does a lot of this for you.  I was a bit confused by the message Homebrew output during the install.  If anybody can clarify, please leave a comment and I will update this post.)

## Fourth, try installing the Go Tour as a test

It should be easy:

`go get code.google.com/p/go-tour/gotour`

This retrieves the source code into your `$GOPATH/src` directory under the right namespace directory.

Then, once that's done:
`go install code.google.com/p/go-tour/gotour`

This will actually compile the code for you in the `$GOPATH/bin` directory.

Hopefully, at that point, you've got no errors.  If you don't, you should be able to simply run the `gotour` command on the command prompt and it will spin up a webserver which runs the official Go tour on your local developer machine. 

## Fifth, go forth and learn!

Finally, grab a coffee, and try out some Go code!  

I wouldn't actually recommend learning Go from the "[official tour](https://tour.golang.org)" (see my thoughts at the end of this post). However, it is a good test to make sure your install is running well, which is why I note it above.

In retrospect, I wish I had tried some of the tutorials found on [Dave Cheney's "Resources for new Go Programmers"](http://dave.cheney.net/resources-for-new-go-programmers), at a glance they appear to be much better.  When I have more time I will try one of them out.

Also:  Here's a quick and easy command line trick to help speed up your learn / code / test process: Use logical operators to make sure you compile then run your program in a one liner:
 
`go install code.google.com/p/go-tour/gotour && gotour`

This will compile and execute your program assuming that the compilation is successful. This is just using the Go tour as an example.  Rinse and repeat as needed for your own package and executable while you learn.

## Sixth (optional): Try cross-compiling

One of the things I wanted to try out while I was learning was cross-compiling.  I wanted to see how easy it was to code on my Mac and then build an executable for some of the Linux (RHEL) servers we have at my organization.

If you used the `--cross-compile-common` flag in the Homebrew install above, you should have many of the common OS and Architecture combinations that you can compile to.

You can cross compile by running a command like this:

`GOOS=linux GOARCH=386 go install code.google.com/p/go-tour/gotour`

This will compile the Go tour executable for Linux on a 386 architecture.  It will put the binary in `$GOPATH/bin/$GOOS_$GOOARCH`.  Whereas your normal compile without flags simply goes into `$GOPATH/bin`

You can replace `$GOOS` and `$GOARCH` with a valid combination* from [the list here (search for "$GOOS and $GOARCH")](https://golang.org/doc/install/source).  And, of course, substitute your own package name for the Go tour :)  

(The "valid combinations" may not work unless it is one of the "common" ones that the Homebrew recipe installs for you.  You can see which ones are installed [in the recipe itself](https://github.com/Homebrew/homebrew/blob/master/Library/Formula/go.rb).  You can also run the Homebrew install with <code>--cross-compile-all</code> and it will install all options for you, but it will take up a lot of space.  [This Coderwall article](https://coderwall.com/p/pnfwxg) was very helpful in figuring this all out)

## Parting thoughts

Finally, if you're interested, here's my thoughts on Go itself and my experience learning it.

## The good:

* Go has built in package management system from the get Go (ha, get it!?)
* You can import packages directly from a Git repository (awesome!)
* It's pretty easy to cross compile, which is nice to create simple executables for different environments
* Concurrency is easy (see "the bad")

## The bad

### The [official language tour](https://tour.golang.org/) is not very good. 
 
Given that this is the first place people will likely start to learn the language, I thought that this tour did not explain concepts very well, leaving them to short code examples that took too long for me to grasp, given how short they were.  Additionally, leaving math exercises to the user is not engaging or effective for me, and I'm assuming the same goes for many developers interested in using Go.

As I mentioned above, try seeing if [Dave Cheney's "Resources for new Go Programmers"](http://dave.cheney.net/resources-for-new-go-programmers) has something better for you.

### Making concurrent function calls is almost *too* easy

I was trying to make some HTTP GET requests with goroutines and very quickly learned that I would overwhelm the machine running it and it would fail with a "too many open files" error. Trying to throttle those concurrent requests to a manageable amount was not nearly as easy as creating all of them. The pattern I came across [using semaphores](http://www.golangpatterns.info/concurrency/parallel-for-loop) seemed a bit convoluted for a language that's supposed to make concurrency "easy." 

### Still need native drivers to connect to Oracle

This is a biggie in the large organization where I work.  We use Oracle heavily, and it would be great to have executables that were able to connect to Oracle without native Oracle drivers installed separately.  Alas, this is nothing new, however, since in the Node.js and Ruby world, this is the standard as well.  It's not the end of the world, but using Oracle in Java is still a heck of a lot easier.
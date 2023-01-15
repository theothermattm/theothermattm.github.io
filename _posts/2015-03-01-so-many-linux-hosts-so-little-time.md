---
id: 131
title: 'So many Linux hosts, so little time'
date: '2015-03-01T11:10:09+00:00'
author: theothermattm
layout: post
guid: 'https://code.theothermattm.com/?p=131'
permalink: /so-many-linux-hosts-so-little-time/
background: 'img/clean/bg-post.jpg'
categories:
    - linux
tags:
    - linux
    - ssh
    - tmux
---

On a given day, I'm interacting with at least 10 different Linux servers.  Whenever I used to shut my laptop, I got frustrated having to reopen ssh connections to every server I connected to.  I also got myself into terminal tab overload, with a separate tab open for every server I connected to.  This is no way to live!

Perhaps you interact with more.  A lot more.  Or even just more than one.  If so, you've probably experienced similar frustration.

I found a pretty good solution to both of these issues using [tmux](http://tmux.sourceforge.net/) and ssh.  In this post, I'll describe my workflow for working with my many Linux servers.  

# The Approach

The overall idea is to pick a primary remote host to use as your "Home Base" and manage all your other connections through it.  This could be referred to as a "[bastion host](http://en.wikipedia.org/wiki/Bastion_host)"), though in this case you're not using it for security, but convenience.

Then, use a [Terminal Multiplexer](http://en.wikipedia.org/wiki/Terminal_multiplexer) to keep your all your SSH sessions open on that host. For this article, I will focus on [tmux](http://tmux.sourceforge.net/).  

By using a multiplexer, you can keep your terminal sessions open perpetually on a remote host. If you get disconnected from your local developer machine, you can just connect back to your "primary" remote host, then pick your connection, and be on your way.  

Your terminal output should even be saved and you can very easily switch between them without multiple tabs or windows open on your local development machine.  Nice!

If you've used tmux before, this should be pretty straightforward.  If you haven't, consider this a nice way to get your feet wet getting started with it.

# The Ingredients

* Lots of Linux servers to connect to
* [Open SSH](http://www.openssh.com/) installed on your developer machine
 * If you're on OS X or Linux, you already have it.  For Windows, [Use Cygwin](http://cygwin.wikia.com/wiki/Sshd))
* A remote Linux server that you can install packages on

# The Recipe

## Pick Your Home Base host

First off, pick your Home Base host.  It can be one of the servers you always connect to.  You need to be able to SSH into this host and it needs to be able to:

* Stay up perpetually (or at least more than you close your laptop)
* Keep SSH connections open indefinitely to other servers
* Connect to all your servers.  Really, it should be inside the same firewall.

You also need the ability to install packages on the host (via apt-get, yum, or 
your favorite package manager).

## Install tmux on your Home Base host

If you don't already have it, install [tmux](http://tmux.sourceforge.net/) on 
your Home Base.  Using apt-get as a package manager, it should be as simple as: 

```
sudo apt-get update && sudo apt-get install tmux
```

(Using Yum should be very similar)

### Can't install packages?

If you don't have the ability to install packages, you can also check to see if you already have [tmux](http://tmux.sourceforge.net/) (run `which tmux`) or (or [GNU Screen](http://www.gnu.org/software/screen/) (`which screen`).  If you already have one of those, you're golden!

If GNU Screen is your only option, this approach will still work, but I will only outline the detailed steps for tmux below.

## Got SSH Keys?

If not, start using them. It is not only secure, but a huge timesaver over remembering and entering passwords 
all the time.  [GitHub has a great reference to get you going](https://help.github.com/articles/generating-ssh-keys/).

Make sure you use a passphrase along with your key for extra security. There are even ways to [cache your passphrase](https://help.github.com/articles/working-with-ssh-key-passphrases/) so you don't need to type it all the time.

## Setup SSH keys on your Home Base

You have some options here:

You can just reuse your same private key on your local developer machine and your Home Base host.  This gives you the flexibility to connect to the same servers with your local developer machine or your Home Base in a pinch.  But, [it could be considered less secure](http://superuser.com/questions/189355/is-it-ok-to-share-private-key-file-between-multiple-computers-services).

If you want extra security, you can generate another ssh keypair for your home base server, in addition to the one you'll use for your local developer machine.  That way, you have an individual key for each machine you'll be connecting with.  The downside is more keys and passphrases to manage and remember.

## Copy around your public keys

Now that you've got an SSH keypair (or keypairs), make sure that you copy your public key to your Home Base host, so you can easily ssh into that.

If you decide to re-use your private key, copy that over to your Home Base server. Otherwise, generate another keypair for your home base server.

This is how I copy around my public SSH key around:

<script src="https://gist.github.com/theothermattm/8786339.js"></script>

Next, repeat those same steps to copy your public key to each and every server
you regularly connect to.

This step is probably the most tedious, but it will pay off in time savings within a day.

## Setup SSH Configs

One snag that I hit early on with ssh was dropped connections due to inactivity.  To keep connections
open, I set the ssh configuration option `ServerAliveInterval` on each host to 60 seconds.  This will periodically
ping the server to keep the connection alive.

Here's what you can put in your `.ssh/config` file in your Home Base to set this
for the server 'myremoteserver1.yourdomain.com':

```
Host myremoteserver1.yourdomain.com
    ServerAliveInterval 60
```

You can also do this for all hosts by doing this:
```
Host *
    ServerAliveInterval 60
```

## Start spinning up sessions with tmux

If you've never used tmux before, [reading about its features](https://robots.thoughtbot.com/a-tmux-crash-course) is a good idea.  For now, I'll just give the basics to get a good multiple server workflow going.

SSH into your Home Base host, and pick one of the servers you frequently connect to and start a tmux session
for it, you'll type the following:

```
tmux new-session -s myremoteserver1
```

You'll then see a nice green bar at the bottom of your screen telling you you're in the comforting arms of a tmux session.

Now, ssh into one of your frequently access hosts in that session. Go ahead and play around a bit.

When you're ready, create another tmux session for another host by opening a new tmux session with a different name related to that server.

You'll do this by triggering the tmux "prefix" keybinding.  This keybinding will put you into tmux's control.  By default, the tmux prefix keybinding is `CTRL+b`.  

To start a new session, the key sequence you'll start with is:  `CTRL+B :`

That's: Hold CTRL and B, followed by a colon (:).

You'll now see a yellow bar at the bottom of your terminal you can type into.

Type the following (should look familiar!): `new-session -s myremoteserver2`

Now you'll be put into a new tmux session named for your second remote server.

Then, SSH into your second server.  Play around.

Repeat these steps for any frequently accessed servers.

## Switch around

Phew, that was a lot of work.  So, what's the big deal?

Well, now you can easily switch around between these different sessions to your heart's content.

And they'll stick around after your laptop drops Wifi!

You can do this by hitting the tmux prefix plus the "s" key.  What that key combo would look like is: `CTRL+B s`

You should now see a menu with the two sessions you created.  Select one of them with the arrow keys and press enter and you should be able to switch back and forth.

Since your sessions are still open, you'll have all your buffer output still there from when you last left it.  And
since your SSH won't time out, you can keep these open as long as you need to.

## Disconnect and try again

Let's say you close your laptop and take a break.  You connect back to wifi and
want to get back onto your remote hosts.  If you ssh into your Home Base again, and type: `tmux at`

You will be put right back into those same tmux sessions (the `at` means "attach").  Everything preserved!

This will stay until you exit your sessions in tmux or tmux is terminated on your Home Base host.

## Don't forget to clean up your toys

Periodically, it's important to go through your tmux sessions and prune them for servers you don't need to actively be connected to.  You can do this by again doing this tmux prefix and a colon: `CTRL+B :`

Then at the yellow prompt, type: `kill-session -t sessionname`

Where the session name is one of the ones you created earlier.

# Next steps

As I wrote this article, I realized this was really a big explanation of a tmux/multiplexer use case.  This workflow is really scratching the surface of what tmux can do. If you find this way of working effective, I would encourage you to learn more about tmux. 

As you learn more, you will see it is *very* configurable and powerful.  People also [share their tmux.conf files](https://gist.github.com/theothermattm/c00449800a9c1cb7193a) and you can learn a lot about what it can do that way, as well.

# Caveats

I have the comparative luxury of connecting to all these machines inside my organization's firewall.  I can't speak for the security implications of leaving open SSH connections like this across the "wild" internet.  I would love to see comments about that situation.
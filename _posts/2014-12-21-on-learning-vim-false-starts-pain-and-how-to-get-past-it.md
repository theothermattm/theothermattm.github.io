---
id: 7
title: 'On Learning Vim: False Starts, Pain, and Overcoming the Hump'
date: '2014-12-21T17:58:57+00:00'
author: theothermattm
layout: post
guid: 'https://code.theothermattm.com/?p=7'
permalink: /on-learning-vim-false-starts-pain-and-how-to-get-past-it/
background: 'img/clean/bg-post.jpg'
categories:
    - coding
tags:
    - editors
    - learning
    - tools
    - vim
---

I'm not going to try convince you to use Vim.

If you're reading post and haven't already closed it, you're probably already convinced that it's useful.

The purpose of this post is to help if you <em>want</em> to learn Vim because you see its promise, but just can't seem to get past the hump of learning it and making it your primary editor. Or, perhaps you don't know where to start.

In my opinion, and probably many others', it is worth getting past the Vim hump, so hang in there!  I'll tell you how I went from IDE lover to Vim lover and hopefully provide some useful tips to get you there as well, <em>if you want to.</em>

<h1>The Beginning</h1>

I got my start developing in Java. So, what was my code editor of choice?  An IDE, of course!  First Eclipse, then IntelliJ IDEA.

As I moved along in my career, I saw presenters at meetups and conferences who used Vim. I watched in awe as they performed text gymnastics during their presentations.  It was like they were playing an instrument, rather than coding.

Occasionally, in a fit of inspiration, I would give Vim a try.  But I could never could get into the swing of things.  It just seemed too hard to learn and I couldn't get past how difficult it was to navigate around a project with a large set of files.

Because of this, I never committed.  I always ended up going back to my IDE (or some other modeless text editor), even for scripting languages like Javascript.  I spent a little time looking for the best of both worlds and found some plugins for my IDE which provided Vim keybindings.  But you still had to use IDE features and shortcuts to navigate around. It never felt quite right.

So, I puttered on, half-effectively using Vim on remote Linux servers where I quickly needed to view or edit a file without downloading it.

<h1>The moment of inspiration</h1>

A few months ago, I had the fortune of working alongside a JavaScript developer who just so happened to be a Vim ninja.  I could watch him code in real time, and more importantly: ask questions.

Usually it went something like this:

<blockquote>
  Vim Ninja: (( tap tap tap )) - ((50 lines show up on screen in the exact spot he wanted))
  Me: Wait! How the hell did you do that!? Show me!
  Vim Ninja: Oh, that's a macro, you just type 'q' then...
</blockquote>

You get the idea.

I didn't get any detailed training from him, per se, but I was able to find out about some useful plugins and configuration options that got me really excited to use it again.

At that point, I decided that the way I was writing my code was not as effective as it could be, and I needed to fix that. Since I was writing lots of Javascript code for a Node.js application, it was a good fit.  My usual IDE features of navigating around types didn't really matter that much in a dynamic language.

So, I spent a handful of hours learning how to configure Vim and installed those plugins that the Vim Ninja recommended.  After I finished, <em>I told myself I was going to force myself to use Vim and only Vim for at least two weeks.</em>

<h1>The Commitment</h1>

Then, shortly after I started, this happened:

<script async class="speakerdeck-embed" data-slide="80" data-id="f77760703dc30132cbf962dbae609302" data-ratio="1.33333333333333" src="//speakerdeck.com/assets/embed.js"></script>

I love this slide from <a href="http://seesparkbox.com/">SparkBox</a>, because it is <em>so</em> true with lots of developer tools, but especially with Vim (and Git - perhaps the topic of another post).

If you're comfortable where you're at coding, when you decide to try a new tool like Vim, there is an immediate, sharp hurt you will feel.  The hurt is usually accompanied by questioning the usefulness of the tool and the strong desire to give up.

You have to be determined to get through that hump, but when you do, you reach places you never thought possible.

<h1>How to learn to stop worrying and love the Vim</h1>

To appreciate Vim and take full advantage of it, you have to want to learn it, and need discipline.  Force yourself to use it as your primary text editor for at least a few days or a week.  Longer if you can.  If you don't, you will simply hit the "Ouch!" stage, get frustrated and never go back.

<h1>Where to begin?</h1>

For people in the same boat I was, or people who are brand spankin' new to Vim, here are a few suggestions to get started on your Vim journey.

<h1>Install the right version</h1>

As of this writing, the latest version is 7.4 and that's the one you should install to get all the latest good stuff.

<h2>On OS X</h2>

On OS X, Vim is installed by default, but chances are it's an older version and you'll want to get a newer one, and update it frequently.

To do this, I would recommend using <a href="http://brew.sh/">Homebrew</a> and installing Vim using that: <code>brew install macvim --override-system-vim</code>

You'll also want to make sure that your fresh new Homebrew install of Vim is the default version you use when you type <code>vim</code> or <code>macvim</code>.  The <code>--override-system-vim</code> flag should do most of this.

If you type <code>which vim</code> you should see <code>/usr/local/bin/vim</code>.  If you hit issues, the default version of vim may be first in your <code>$PATH</code> and may need to be adjusted.  <a href="http://stackoverflow.com/a/21713112">This StackOverflow post</a> is really helpful to get that configured correctly.

<h2>On Windows</h2>

For Windows, the <a href="http://www.vim.org/download.php#pc">Vim installer</a> will give you options to install a gui and console version of Vim to use.

<h1>Learn the basics</h1>

This post isn't about how to do this, but there are tons of resources out there:

<ul>
<li>Use the built in tutor:  type <code>:help tutor</code> in Vim</li>
<li><a href="http://vim-adventures.com/">Play a game</a></li>
<li><a href="https://www.google.com/search?q=vim+tutorial&amp;oq=vim+tutorial">Find another tutorial</a></li>
<li>Or, if you don't mind paying a bit, check out <a href="https://upcase.com/the-art-of-vim">Upcase</a></li>
</ul>

<h1>Go whole hog</h1>

Don't learn a bit of Vim, dip your toe in, then shift back to Sublime Text (without vintage mode on).

Make Vim your default editor, and open up all your text files in it.  Resist any urge to open up anything  else to type into, unless there is no other option!  Do this for at least two or three weeks.

This is a bit like trying to get into an exercise regimen.  It's painful at first, but then gets easier and easier.  Finally, you get to a point where you feel great about it and it starts to become more effortless.

<h1>Find a time when you're not under tight deadlines</h1>

Under the gun to get features or fixes in at the end of a project?  <em>Not</em> the time to be learning a new editor.  Learning Vim will cause you to frequently pause, remember a key combination, and interrupt your flow temporarily.

Find a stretch of time at the beginning of a project, or second to that, in the middle of a project, where you have a bit of leeway to let your brain and your finger muscles get used to the shift in how your new editor works.

If you're not under a lot of pressure, you'll power through the "Ouch!" stage in a much easier fashion.

<h1>Focus on how to properly use <a href="http://usevim.com/2012/05/11/visual/">Visual Mode</a></h1>

Learning how to really use visual mode effectively was my "a-ha" moment. This is why using a <a href="http://unix.stackexchange.com/a/57708">modal editor</a> is great.

Learn the basics of <code>shift+V</code>, <code>shift+v</code> and about yanks and puts.  Once you get the hang of these basics, you will be flying and the (text) world is your oyster.

<h1>Learn about the basics of a .vimrc file</h1>

A .vimrc file in your home directory is how you will configure Vim.  And you'll probably want to set some custom configurations right off the bat.

A great start is showing line numbers.  (Why doesn't Vim show them by default!?)

Just create a file called <code>.vimrc</code> in your home directory which contains this:

<pre><code>set number
</code></pre>

And start vim up.  Viola, you'll have line numbers all the time.

Then, go from there.  There are <em>tons</em> of great resources out there on how to configure Vim.  I would recommend finding some examples of .vimrc files and learning about the configurations you see in them.    Everyone and their brother are starting to post their .vimrc and other <a href="http://dotfiles.github.io/">dotfiles on Github</a>.

You can find my .vimrc, too, <a href="https://gist.github.com/theothermattm/8864384">it's right here</a>.

My favorite resource is to look at <a href="https://github.com/thoughtbot/dotfiles/blob/master/vimrc">ThoughtBot's .vimrc file</a>. They have a good default set of options and the comments do a pretty good job of explaining what certain things are.

<h1>Install Some Good Plugins</h1>

There are some Vim purists who think you should learn Vim without using plugins.  I disagree.  It is useful to know "pure" Vim in case you're out on servers and don't have any plugins.  But doing any significant development in Vim without a few decent plugins is like using a rotary phone - it'll get the job done, and it's kinda hipster-retro-cool, but it ain't fun.

First off, go get <a href="https://github.com/gmarik/Vundle.vim">Vundle</a>.  Vundle is a Vim plugin manager that will make installing new Plugins and updating them a breeze.

Once you get it setup, you can just add lines like this to your <code>.vimrc</code> to add new plugins:

<pre><code>Plugin 'kien/ctrlp.vim'
</code></pre>

In this config, the text you put inside the quotes is a git repo for a Vim plugin.  Then you can run <code>:PluginInstall</code> to install.  You're done.

<h2>A list of plugins that will come in handy regardless of your language</h2>

<h3><a href="https://github.com/kien/ctrlp.vim">CtrlP</a></h3>

This allows you to type <code>ctrl+p</code> and a filename.  It'll do some fuzzy searching on your filesystem to find files that match.  You can then navigate up and down the search results list and open a file in your buffer.

This is really similar to Eclipse's <code>ctrl+shift+r</code> or IntelliJ's <code>ctrl+shift+n</code> functionality.

<h3><a href="https://github.com/scrooloose/nerdtree">NerdTree</a></h3>

This gives you a sidebar with a tree version of your filesystem (think the tree view in Sublime Text).

It's awesome to be able to navigate up and down your file structure and open folders freely with just the keyboard.  This is really great when you can't even start to remember the name of a file.

I also find it really useful to use Vim's <code>/</code> search functionality find a folder name in the tree to quickly navigate to it.

I would recommend mapping the toggle on/off for nerd tree to a key binding like <code>map &lt;C-n&gt; :NERDTreeToggle&lt;CR&gt;</code>.  This maps <code>ctrl+n</code> to turn on the tree.

<h3><a href="https://github.com/Raimondi/delimitMate">DelimitMate</a></h3>

Auto completion of parens, brackets, etc.  Ya need it.

<h3><a href="https://github.com/Xuyuanp/nerdtree-git-plugin">NerdTree Git Plugin</a></h3>

If you use git, this one is great to see visual indicators of what git status your files are in right in a file tree.

<h3><a href="https://github.com/airblade/vim-gitgutter">GitGutter</a></h3>

Again, for you git users, this one is great to visually see added/deleted/changed lines in the left hand side of your buffer.

<h1>Javascript coders, you'll be in plugin heaven</h1>

Javascript has an amazing community of developers making <em>great</em> plugins for Vim.  You'll have a blast trying them all out.

I'll leave the configuration and plugin recommendations to this excellent post to help you get setup for Javascript in Vim: <a href="http://oli.me.uk/2013/06/29/equipping-vim-for-javascript/">Equipping Vim for Javascript</a>

<h1>Java coders, things are lacking...</h1>

Even thought I've been coding a lot recently in Javascript, I still code in Java a decent amount.  I wish I didn't have to say this, but I still find myself going back to IntelliJ IDEA when I need to work in it.

Simply put:  I could not find anything that could really compare with the tight integration and IDE provides for a static language like Java.

There is real power of being able to navigate to static types and methods with a simple keyboard command or ctrl+mouse click.  It's also crucial for me to be able to do type-safe refactorings.  Both of these things are built right into these IDE's and are bound to keyboard shortcuts right out of the gate.

That's not to say there aren't options for Vim:  I tried out <a href="http://eclim.org/">Eclim</a> which requires a headless version of Eclipse, but it was wonky to setup, and didn't run very smoothly.  It just didn't feel worth it.

So, if you have to code in Java and only Java, you might have a tougher time really committing to Vim using the suggestions in this post.

However: You can install <a href="http://vrapper.sourceforge.net/home/">Vrapper for Eclipse</a> or <a href="https://github.com/JetBrains/ideavim">IdeaVim for IDEA</a> which provide nice Vim-like keybindings for those IDE's.

If you learn a combination IDE shortcuts for navigating around files and combine those with Vim's keybindings along with Visual Mode, you're more than halfway there.  That's what I do, and I find it really effective.

If you're new to Vim and this is your only option, just really force yourself to not turn the plugin off if you get frustrated! :)

<h1>Some other thoughts</h1>

If I had unlimited time, I would really love to create an open source project which packages up these essential Vim plugins and installs them with some default configurations.  I spent a pretty good amount of time getting configurations together so Vim felt usable enough for me.  It would help others get over the hump a lot quicker if there were pre-configured "packages" that could be installed.  Maybe something like this already exists and I don't know about it?

Finally, while writing this post, I found <a href="http://antjanus.com/blog/thoughts-and-opinions/use-vim">this really great post from Antonin Januska</a> which shares some very similar opinions of mine and is a really good read.

*Update 2/26/2015:* I Just came across a [fantastic, step by step tutorial to setting up vim with plugins](https://github.com/jez/vim-as-an-ide) by [jez](https://github.com/jez) on github. He uses a great approach of using git commits to walk through the process.  
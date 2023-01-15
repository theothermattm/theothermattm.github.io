---
id: 94
title: 'Breathing New Life Into an Old Macbook with an SSD and OS X Yosemite'
date: '2015-01-02T16:23:11+00:00'
author: theothermattm
layout: post
guid: 'https://code.theothermattm.com/?p=94'
permalink: /breathing-new-life-into-an-old-macbook-with-an-ssd-and-os-x-yosemite/
background: '/img/clean/bg-post.jpg'
categories:
    - hardware
    - mac
tags:
    - laptop
    - mac
    - 'os x'
    - yosemite
---

My Christmas gift to myself this year was to install a new Solid State hard drive (SSD) in my old mid-2009 Macbook Pro.  Wow, what a difference!  The most honkin' app I run, Adobe Lightroom, now takes about 5 seconds to load, compared with what felt like an eternity on my old hard drive.

This post outlines what I did to perform this upgrade, which, all in all, was pretty easy.  I'm writing it down so it might help others and also so I don't forget my own steps if anything goes wrong in the future :)

# Why I decided to upgrade 

My old Macbook has served me well and is in great shape.  A new Macbook would be fantastic, but shelling out $200 versus over $2000 is a no brainer.  It's also maxed out on RAM (8GB), and it was running OS X 10.6 Snow Leopard - four major updates behind.  So, it was really showing signs of age and I was missing out on some of the newer OS X features.

In addition to doing personal coding on my home machine in Node.js, Java and the odd bit of Ruby, I also use my Macbook for:

* Using Adobe Lightroom to process photos
* Using Photoshop Elements to touch up the odd photo
* Using VMWare Fusion to run a Windows 7 Virtual Machine so I can run Quicken for personal finance
 * Side note, this is sad, isn't it?  

So, I do a few things in there that require some power beyond the piddly stuff.

# The Drive

After reading a lot of reviews for Solid State Drives for mac, I chose a [Crucial M500 480GB Drive](http://www.amazon.com/Crucial-2-5-Inch-adapter-Internal-CT480M500SSD1/dp/B00BQ8RHJ2/).  It had the best combination of good reviews as well as price.  I made sure to search the Amazon reviews for it to confirm it worked with my 2009 Macbook pro.  (For my model, [this was a particularly helpful review](http://www.amazon.com/review/R32TXYTU8CWRWH/ref=cm_cr_rdp_perm?ie=UTF8&ASIN=B00BQ8RHJ2)).

I had the drive in my Amazon shopping cart for a long time before getting it.  I eventually setup a [camelcamelcamel.com](http://camelcamelcamel.com) alert for it and got it about $40 cheaper than it usually is.

# What I needed before installation

Besides the hard drive itself, I needed the following

## External hard drive enclosure

I wanted to reuse my old hard drive.  So, I found enclosure that would let me use it as an external USB drive.  I went with a [Sabrent enclosure](http://www.amazon.com/Sabrent-2-5-Inch-Aluminum-Enclosure-EC-TB4P/dp/B005EIGUD4).

This was doubly awesome because I could use this to just pop the old drive in this and convert all my data over using "USB 3.0" (see later on).

One drawback is that it sucks a lot of power (again, see later on).

## A thumb drive 8GB or larger

Since I was going to install a new version of OS X on a new hard drive with no [recovery partition](http://support.apple.com/en-us/HT4718), I needed to make an OS X installer thumb drive.  To do this, I needed a thumb drive 8GB or larger. 

## Tiny-ass screwdrivers

Finally, I made sure I had my trusty set [set of small screwdrivers (pentalobe, etc.)](http://www.amazon.com/gp/product/B009MKGRQA/) to use for the tiny screws in the macbook and the internal hard drive.  Having this little kit around is indispensable if you're going to do anything with a Macbook, iPhone or iPad.
 
# The steps

Here's how I did it

## Find a bunch of free time

Having a 17-month old baby, I had to wait for a day when I would be able to "tend" to the install.  While it's not a lot of work, it does take a while and needs to be checked in on.  Kind of like... A baby?  No, that's not right - a baby IS a lot of work! :)

## Create a bootable USB installer

I decided to go straight to the latest OS X and use 10.10 (Yosemite) .  To start this out, you have to go to the App Store and download the Yosemite installer.  This is a few GB and takes a while to download, so I did that early on.

After you download the installer, I copied the installer out of my Applications folder for safe keeping.  Then I followed [Apple's instructions on how to create a USB installer](http://support.apple.com/kb/HT5856).  They're very good and very easy.

## Virtual Machine Prep

I had a feeling that VMWare Fusion might not play well between Snow Leopard and Yosemite, so, I backed up my important stuff from my Windows 7 image and stored it on my Mac drive.  In my case, this was really just my Quicken files.

## Backups

First things first: I backed up *all the things*!  I use two different external hard drives to run my backups for extra redundancy, swapping the drives every couple of backups.  So, I ran a time machine backup of my old hard drive on both of them for good measure.

## Replace the drive.  

For my mid-2009 Macbook pro, [page 35 of this manual shows how to replace the drive](
http://manuals.info.apple.com/MANUALS/1000/MA1128/en_US/MacBook_Pro_15inch_Mid2009.pdf).  

Side note: It is unfortunate that newer Macbooks can't be modified like this anymore.

## Put the old drive in external enclosure

A couple of screws in the [Sabrent enclosure](http://www.amazon.com/Sabrent-2-5-Inch-Aluminum-Enclosure-EC-TB4P/dp/B005EIGUD4) and that was it.  

It's worth noting that this enclosure needed to be hooked up to a USB hub that had external power.  My old macbook's USB ports couldn't power it.

Additionally, I realized that even though the enclosure supported USB 3.0, my old Macbook did not.  Slower speeds, oh well!

## Format the drive

I plugged in the bootable USB installer into the Macbook, held the Option key, and booted up. 

I opened up Disk Utility and choose the new hard drive then formatted it.

## Install Yosemite

Exit Disk Utility and then install Yosemite.  It took about 45 minutes for mine to complete.

## Migrate data

Once Yosemite was installed, I created a new user to test things out, making sure it had a different username than the one I used to use.  Then, I opened Migration Assistant to move my data and settings over from the old drive to the fresh Yosemite install. You can also Migration Assistant from time machine backup on my existing external USB hard drive.

## Install new XCode

I use [Homebrew](http://brew.sh/) and after the upgrade, I was having some issues with various utilities.  Running `brew doctor` gave me the hint I needed to upgrade Xcode from the app store.  This took about 30 minutes to download and install.

## Fix Homebrew

I ran `brew update` and `brew prune` along with `brew uninstall macvim` then `brew install macvim --override-system-vim`.  I was having some issues there.

## Deal with the VM

As expected, VMWare Fusion didn't work across OS X versions.  Migration Assistant was very clear about that.  I decided to give up on VMware Fusion and just try out using VirtualBox since it's free. 

[This article](http://blog.csanchez.org/2011/08/01/migrating-windows-7-from-vmware-fusion-to-virtualbox/) gave a really good overview of how to convert my VM over from VMWare Fusion to VirtualBox.

I also installed the VirtualBox Guest Additions by using the "Devices -> Insert Guest Additions DVD Image" once my VM was up.  Then I could share folders, resize the screen, etc.

## Time machine backups

One thing I did not anticipate was that I would have issues with my existing Time Machine backups.  When I went to run a new Time Machine backup, I was told I didn't have enough space on my external hard drive.  

It seems like Time Machine has not been able to "reconnect" to my old backups and is trying to basically back everything up again and doesn't know that it should delete older copies of my backups.  [this article from pondini.org](http://pondini.org/TM/B6.html) has some good information about how to reconnect backups if this happens.

<strike>As of now, I have not been able to solve this issue and may just reformat my external hard drive and start a fresh backup with Yosemite.  If I get this fixed, I will update this post.</strike>

Update 1/10/2015: I ended up reformatting my backup drive and just started over.  Using `tmutil inheritbackup` and `tmutil associatedisk` as mentioned in the pondini article and [this article by Simon Heimlicher](http://simon.heimlicher.com/articles/2012/07/10/time-machine-inherit-backup-using-tmutil) unfortunately didn't work for me.

## Enjoy

That's it, now it's like I have a new Macbook!
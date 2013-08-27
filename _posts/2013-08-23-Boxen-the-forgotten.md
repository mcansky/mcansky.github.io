---
layout: post
title: Boxen the forgotten
date: 2013-08-23 16:27:31
disqus: n
---

A few months ago I started to use Boxen (the github tool to setup armies of Mac hardware to a team needs). I wrote a post about it mostly linking a very good article on coderwall. That article disappeared some time later.

As I start a new job and the company provides me with new hardware I resurrected my boxen repository. I thought it would spring back to life and do the job.

As usual things tend to break if you just leave them sitting in a corner. I looked around for alternatives, and updates, turns out there is nothing quite similar to it yet. So I did what was needed : I scrubbed in and stepped into the operating room.

Here is what one would need to know to get going with Boxen.

## Get the boxen

You probably have a computer working with git and a text editor already if not, it's ok, the first step will allow you to get there : jump to the *Step 1 : XCode* and come back here.

Fork the boxen base repository : https://github.com/boxen/our-boxen and clone it to your local computer.

## Ready the beast

The base setup described in the repository cover very basic needs. You need to know a couple of things to be able to customise it.

Check the "Customizing" part of the README : https://github.com/boxen/our-boxen#customizing .

Basically you need to edit 2 files :

* Puppetfile
* manifests/site.pp

The first one defines the modules you want to draw rules from, the second one defines which rules you want to use.

For example, you want to add Redis and Riak. You first go to http://github.com/boxen/ there you do a search for 'redis'. You will find the 'puppet-redis' repository, go check it out. You are interested in the tags so check out their list.

Now we can add the reference to the Redis' puppetfile in the Puppetfile :

{% highlight ruby %}
github "redis",      "1.0.0"
{% endhighlight %}

The "1.0.0" part is the tag you have spotted in Redis' git repository. Using that syntax boxen (and puppet) knows where to get the module from (github, boxen's account).

Yet this is not all, we also need to add a line in the *site.pp* file. You will spot similar lines in the files, just add it at the end.

{% highlight ruby %}
include "redis"
{% endhighlight %}

Commit theses changes in a topic branch and push it to your remote.

## Preparing the new Mac

The first step on the new mac is to download and install Xcode. Use the AppleStore, it's now free. Once it's installed, launch it, go to the preferences, in the Downloads page install the Command Line Tools.

Once it's done git and some build necessities are installed. Open a terminal and use the following to clone locally your newly pushed boxen config.

{% highlight sh %}
sudo mkdir -p /opt/boxen
sudo chown ${USER}:staff /opt/boxen
git clone <location of my new git repository> /opt/boxen/repo
{% endhighlight %}

## Running boxen and getting everything installed

To run boxen and install everything you have to go to the boxen directory and run the boxen script located in the script/ directory :

{% highlight sh %}
cd /opt/boxen/repo
script/boxen
{% endhighlight %}

Boxen will ask you a couple of questions : your github account credentials, your root password to run sudo once or twice. And then it will run and install things.


## Real world usage

Ok, you got it working, great. But there is a cleaner way to customise the base boxen config. By default boxen will look into the *modules/people/manifests* dir for user specific *.pp* files. It will use your github username to track down your file (*modules/people/manifests/mcansky.pp*).

There is a README file in that directory check it out to get a simplistic template to use for yours.

Instead of adding things to the *site.pp* file you should add them in your user's *pp* file. It's cleaner.


## A word of caution

Don't let your boxen repository alone for months, I made that mistake once and I spent several hours tracking down a solution. That solution was to clone a new copy of the fresh boxen repository and copy back my old config into it.

So keep checking out changes in boxen and rebasing yours onto it.

## Warning

XQuartz failed to install for me.

## Epilog

I am still quite happy with Boxen, it saves me countless hours of setup. In the end, setting up the new laptop only took me (from the moment I grabbed a new boxen repository to the moment the laptop could open up my favourite editor to work) took only 3 hours.

## Quick tour

The following two steps, in that order :

### 1. On an already git friendly computer

* fork and clone the *our-boxen* repository
* add sources to the Puppetfile
* add modules you want to be installed in a *<USER>.pp* file

### 2. On the new Mac
* install Xcode and the command line tools
* clone your custom *our-boxen* repository in /opt
* run boxen

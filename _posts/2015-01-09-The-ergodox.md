---
layout: post
title: A bit more about the Ergodox
date: 2015-01-09 08:20:00
disqus: n
---

As briefly said in the previous post I have bought and built an Ergodox Keyboard. If you are interested in mechanical, split keyboard you should have a look at those but also search around for alternatives.

If you are looking for a place to buy one it mostly depends on your location. In Europe Falbatech is probably (at the moment) the best place to buy everything (but the keycaps) in one go. If you are in the US Massdrop is probably a better option for now, if you are not in a hurry.

If you are in the mood of getting most things yourself you should check the [official ergodox hardware list](http://ergodox.org/Hardware.aspx).

## The assembly part

The assembly is usually the part that scares most buyers especially if they don’t have much experience in soldering. Reading the [massdrop assembly guide](https://www.massdrop.com/ext/ergodox/assembly) before buying helps a lot keep those fears at bay. It’s also a good companion during the assembly process of course.
If you are still scared by diodes you should replace those with “regular” diodes (1N4148) that are very common, cheaper and easier to solder.

It will take about 6 hours to assemble everything.

## The caps

The caps are a different matter. It’s quite difficult to source a proper ergodox set (there is a lot of 1.5 keys) so most people end up waiting for group buys on mass drop or pimp my keyboard for specific sets. You can also buy blank sets from WASD keyboards.

## a bit of a warning

People are usually impressed how most developers type blindly on their keyboard. Knowing that most of us have already years of typing behind us when we get our first job probably helps a lot to understand that.

So, if you decide to go with a QWERTY layout for the Ergodox and you already have good habits for your typing you should not have much trouble with switching to an Ergodox with a blank key set.

In any case, using [Massdrop layout configurator](https://www.massdrop.com/ext/ergodox) will also help to get a layout you like easily. You can also print it to have next to you while typing.

## into deeper waters

It would be odd to buy such a (expensive) keyboard with an open source firmware without doing a bit more than just soldering diodes and resistors.
After a quick play with the Massdrop layout configurator to get a couple of layers going I decided to look into using the LED on the keyboard properly. My previous post was about that, but here is a bit more about the topic and what I have found.

For web developers thinking about tweaking a firmware kind be a bit of a nightmare : it’s not our daily walk. Yet the [Ergodox firmware](https://github.com/benblazak/ergodox-firmware) is very simple C and there is little to look into to get the gist of it.
Have a look at the [dependencies](https://github.com/benblazak/ergodox-firmware#dependencies-for-building-from-source) to figure out what you need to be compile the firmware.

The *main.c* file is simple and were the big things are happening. If you look at my previous post and the commit I did to use the LEDs you’ll see I mostly worked in there.

But you can also tweak your layouts directly from [other files](https://github.com/benblazak/ergodox-firmware/tree/master/src/keyboard/ergodox/layout) in that repo, in the end that’s exactly what the Massdrop layout configurator does. Just copy one *.c* and its *.h* sibling and you are good to go. You will need to change things in the makefile though to get it to build (or pass a proper ENV variable).

In the end I found the (basic) firmware hacking simpler than I expected and I am quite happy about the result and what I have gathered to tweak it myself from my favorite editor.

## a bigger epilogue

I quite enjoyed building the keyboard itself and then playing with the firmware so far. It’s a lot of fun. I have another one coming in through Massdrop so I will try to do thing a tad differently for that one.
Yet having typed a few hours with the keyboard I do find it a bit, big. I am waiting to see how it goes with writing code with it (I mostly wrote english emails and posts so far to get used to the layout).
I would try to play with a teensy or an arduino micro to understand a bit more how a keyboard works and then possibly try to do a similar keyboard with fewer keys. There are projects here and there such as the [Atreus](http://atreus.technomancy.us).

Using the split format does not forgive much when you have bad typing habits. Look at your keyboard for a minute. See the T,Y,G,H,B and N keys ? The default QWERTY layout of an ergodox layout is split right on in between each pair. I f you currently have the habit of typing the Y, H, or N with your left index, or T, G or B with your right index you are in for quite a ride.

The location of the modifiers and space keys can be a bit odd but thanks to the ability to customise the layout you can tweak them to your liking.


---
layout: post
title: Here is a gem Oakpot
date: 2013-10-28 10:48:31
disqus: n
---

When [Tau](http://www.tau.so) moved in the new place next to the Garonna river we had to solve a little problem to be able to open the main door of the building using our phones. We did so using a little sinatra app and [Twilio](http://www.twilio.com) as I describe [here](https://coderwall.com/p/q_r7dq).

Since then I have been amazed at how easy Twilio makes the whole calls and sms thing. They recently released an new side of their product to connect seamlessly to a PABX and it looks great too.

As I read a post from GoCardless's Blog : [Rolling your own cloud phone system](https://gocardless.com/blog/rolling-your-own-cloud-phone-system/) last week I was left wondering if we could make triggering a call really simple.

I thought it would be nice to be able to simply call *.call(number)* or *.message(text)* to trigger a call between the object's number and a number or send a message to the object's number.

Armed with that idea and a couple of recent tricks up my sleave I opened up a terminal, fire up bundler to create a gem scaffold and started to write some tests.


## The tests went alright

The tests went alright, it was easy to write something simple to describe what I wanted : a module to extend a random class allowing us to use either a default *phone_number* attribute or a custom one to grab the number and then add two methods to the object to trigger calls.

In the process I also thought it would be nice to be able to check if an object was elegible for calls (if it has a phone number attribute) or not.


## Then I plugged the twilio-ruby gem

Theory is always a bit cleaner than reality. Once I started to look into the twilio-ruby gem I remembered that there is a bit more to it than just passing numbers. Messaging would stay really simple but setting up a call between two numbers require an url endpoint responding with proper TwiML.

I did not want my gem to turn into a web friendly thing, it has to be kept simple. So the gem will request two numbers (one from the object), a TwiML friendly url endpoint and return the call object as is.

## In the end ?

In the end [Oakpot](https://github.com/mcansky/Oakpot) is quite simple indeed. If one got a class with an attribute or method called *phone_number* you can extend it by including Oakpot::Call (and/or) Oakpot::Message. This will add a couple of methods to your class :

* .is_oakpotable? : to check if an object has a phone number attribute or method
* .call : to trigger a call from the object's number to the number passed as first parameter using the TwiML located at the url endpoint passed as second parameter.
* .message : to send a message from the object's number to the number passed as first parameter. The message content has to be passed as second parameter.
* .message_from : to send a message to the object's number from the number passed as first parameter. The message content has to be passed as second parameter.

The three methods return a Twilio-ruby Twilio::REST::Call or Twilio::REST::Message object leaving you the matter of deciding what to do with those. Since you can do a lot of different things with calls and message I did not want to decide anything for you once the call is started or the message sent.

## It was fun

The basic code came up in about 12 hours, it's tested using Ruby Minitest lib so it comes will full colors.

For a long time I thought gems were a bit hard to create, test etc ... But I am happy to be wrong on that one since Bundler is able to scaffold something for you in just one line.

As described in the previous post I used the tests to unravel my thoughts before actually writting the code and adding the real stuff to it. It was quite fun to add the bricks one after the other and see them clicking.

## Enjoy

The gem is under MIT license and I will push it soon to rubygems. In the meantime feel free to check it out comment and even send me suggestions and PRs.

For new comers to the Ruby world or wondering about writting Gems I might suggest you to check the repository as the gem is quite a simple one. You should also check : [Gem creation with bundler](http://net.tutsplus.com/tutorials/ruby/gem-creation-with-bundler/).

Matt Sears' MiniTest [quick reference](http://mattsears.com/articles/2011/12/10/minitest-quick-reference) is starting to become one of the permanently opened link in my browsers.

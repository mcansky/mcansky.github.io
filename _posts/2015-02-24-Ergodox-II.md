# Ergodox, part 3

I have finally received the Massdrop Ergodox. Here is a little account of what has been happening with it and its predecessor.

## Solder

A friend had his own ergodox kit to solder and wanted some help so we did a bit of a marathon.

Knowing how soldering the surface mounted diodes was a pain I had bought 500 1N4148 diodes. And we then went through the whole Massdrop ergodox manual. It took us about 7 hours with a couple of breaks.

## How much experience ?

I went through an Electronics and CS degree at university : I know what a diode, LED, resistor are. I also know the basics of soldering, unsoldering, testing, etc ...

I largely under estimated the amount of base knowledge needed to put an Ergodox together. While certainly not impossible it’s tough if you have very little experience in soldering. I would really suggest to ask your friends or in your local hacker space for a bit of help or some kind of Electronics 101. The experience will be much better for sure.

## The result

I am beginning to think that it’s very hard to get the ergodox wrong. If you follow some basic rules you are safe and should not have any issue.

1. prepare your workspace : check the components, have clean space
2. prepare your run : have a read through the mass drop guide
3. do 1 thing at a time : if you need to do one operation followed by another one and then repeat for 76 other identical components group them : solder the same leg of all the diodes, then solder the other one
4. take breaks when you have finished each step, not in the middle of one

We only had 3 issues in the end, and it was all related to switches that were not soldered properly.

## The firmware

The other reason for this post is a follow up to a question I left on Reddit and that was kind of answered by @__mharrison__ : custom firmware.

After several months using the first ergodox I have grown used to it and I am now looking at improving my experience by using more than just 1 layer.

I had trouble to do so with the ergodox firmware so I left a message on Reddit. I did not get any answer. And then, tweeting about the new ergodox @__mharrison__ asked me what I wanted to do with the handful of red switches I’ve used on specific places on the new ergodox.
Since I was stuck on that very precise thing I asked away and he pointed me towards another firmware : https://github.com/tmk/tmk_keyboard. While I was aware of it I never tried it before.

After a couple of hours playing with it and tweaking things I am much happier about the layers. I still have more work there but already both the way to define the layers and the way to add custom macros are easier than on the other firmware.
I will publish my branch soon once I’ve refactored a couple of bits.

## Epilogue

I now have 2 ergodox. One has all MX Clear switches, the other one has brown and red ones. The former uses a PVC case, the later an acrylic one.
I do prefer the first one : it’s lighter, slimmer and quieter too. Coming from a “regular” keyboard I would suggest you to go for clear or the blue ones which look similar.

The acrylic case is disappointing. The use of metallic screws without any rubber feet render the whole thing even more loud and unstable. It’s quite high too.

So if you want an ergodox : get your components online with a couple of friends and get a PVC case or get one printed or laser cut from wood.

The original time estimation for the whole process can be cut to 4 hours probably if you have  one soldering iron per person and can bend the diodes fast.

## One more thing

After 2 of those done I certainly don't plan to stop yet as I see how easy it is I can't help but think of all the heavy lifting that was done before I arrive here.

Really the Ergodox is an awesome project, the firmwares available are of good quality and a lot of information and help are available across the Internet.

Thanks a lot everyone !

---
title: "DIY Laser Engraver for 80 USD"
date: '2016-05-09'
categories:
  - DIY
  - Projects
tags:
  - Music
  - DIY
  - Floppy Drives
  - MIDI
  - Arduino
---

This is the first real project that I started to work on. During winter break in 12th grade, I decided that I wanted to have a go at building a 3D Printer from scratch. Turned out that this was a fairly expensive thing to do on basically no budget so I decided instead to make a laser engraver by recycling any parts that I could find. The final cost ended up being around 70 dollars, something I was quite happy with. Unfortunately I did not do a very decent job documenting the process, but I do have a few pictures of the build and I will try to explain the process I went through. Anyway, here’s a video of the final product engraving (sped up significantly):

<figure>{{< youtube FGswquf6Id4 >}}</figure>

## Design/Research

I really didn’t have a very organized plan on how the final details were going to work out until basically the very end, when I had all the different pieces that I needed to finish the work. In the end the project became almost a mash-up of features I liked in some peoples’ projects along with changes or improvisations to fit my extremely limited budget. One of the first projects I found that I liked was from [Mario Lukas](http://www.mariolukas.de/) (sorry, his blog is in Dutch). In his [YouTube video](https://www.youtube.com/watch?v=3EZFL9W5wNk) of his 3D printer I really liked his idea of using a scanner for the lower axis of the machine. I ended up finding a very similar scanner for 3 dollars broken at a thrift shop which I took apart and screwed down to the wood base of my engraver.

I’m going to mention [pxlweavr’s](http://pxlweavr.com/) engraver as well because his excellent video helped me gain some insight on how to get all the pieces talking to each other, as I started with zero knowledge of CNC machines and this was going to be my first project involving electronics. While I ended up changing, I really wanted to use the same driver mechanism outlined in his [YouTube video](https://www.youtube.com/watch?v=xxQ33cNIXxU).

The [RepRap Project](http://reprap.org/wiki/Main_Page) ended up being the most important source of information. Even though I was building a laser engraver, I was still going to use [grbl](https://github.com/grbl/grbl) running on an Arduino Nano as the controller.

## Construction

For the laser, I found a DVD-RW drive and harvested the laser diode from it. There are many different videos on YouTube explaining this process, just make sure you use proper ESD safety to avoid breaking the diode, as it is very sensitive. I then bought an Aixiz module to house it and a constant current driver for power. It is important to use a constant current driver as any other source will blow out the diode almost instantly.

{{< figure src="/img/engraver/diode.jpg" caption="Laser diode with penny for scale." >}}

Moving on to the electronics, I ended up using one Arduino Nano, two [Easy Drivers](http://www.schmalzhaus.com/EasyDriver/), two laser end stops, and a relay to power the laser from the Arduino with. Here is the circuit on breadboard. All the pinouts are identical to default grbl settings with the exception of the laser, which is on the feed enable pin (also documented at grbl’s website).

{{< figure src="/img/engraver/breadboardCircuit1.jpg" caption="The initial cercuit before mibration to perfboard." >}}

Which began the process of building. I built this in a few hours and did not think to document the vast majority of the work, so I only have a couple photos. The initial plan called for a different style to drive the upper axis, which did not end up working as the drawer rails I bought did not work very well, so I ended up taking apart an inkjet printer and salvaging the print head assembly for it.

Here’s a video of the initial build which had those rails:

<figure>{{< youtube UkmZC5s1Wsk >}}</figure>

This ended up working really well actually, as it provided a really nice way for me to mount the laser diode on the CD drive parts. I am able to focus the laser by hard focusing it at its lowest position and then sliding it upwards as high as the material stands on the bed surface.

{{< figure src="/img/engraver/initialGantryDesign.jpg" caption="The initial design for the gantry. This changed by the end of it all." >}}

{{< figure src="/img/engraver/strippedScanner.jpg" caption="The scanner stripped down and ready to be turned into an axis of motion." >}}

Which leads me to the final product!

{{< figure src="/img/engraver/finalEngraver.jpg" caption="The final engraver." >}}

## Safety

There are some things that are really important to note about this project from a safety perspective. First, I have yet to build a proper guard around the laser. This is problematic because a 500mW laser it can and will blind somebody in an instant if they are exposed to it directly and can still have equally harmful effects from incident reflections. Safety glasses (the laser kind) are a must and I will be building a ventilated shroud to go over the entire machine while in operation. That said, it can produce some really nice looking work when used correctly.

## Conclusion

In the end I was actually able to create this machine for far less than the cost of buying one new. It isn’t nearly as accurate as a properly made machine but it still does a consistently decent job. Here are a couple samples:

{{< figure src="/img/engraver/ship2.jpg" caption="A ship being engraved into a piece of balsa wood." >}}

{{< figure src="/img/engraver/ship1.jpg" caption="The same ship, nearly completed." >}}

If I were to do it again I would probably spend some more money and use proper parts but that was not really the point of this exercise anyway. It was good fun and I should have a decent tool to use in the future for other fun things!

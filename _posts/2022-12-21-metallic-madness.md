---
title: Metallic Madness
date: 2022-12-23 12:10:00 +1100
---
## Off by One
It has been over a week since my last update and for good reason: it has taken a week to find, buy, deliver and receive the new power supply I had mentioned. Five PCI-e/CPU slots ready to go... with only four cables: two CPU and two PCI-e.

I am honestly baffled by this. Not only are there five slots for
exactly and there is a this purpose, there is also a piece of paper
present that describes the proper procedure for using _three PCI-e
expansions_. Perhaps I should have expected as much, given that the
contents of the box list 4 of "PCE-e 6+2 Pin x2" (which I personally
find a little confusing). But I have another power supply of the same
brand, can I not just use the cables in that product? Unfortunately
not, as I am informed in the manual that I cannot use cables for
different products or units. After having a close look this involves
just moving the wires and ground to different numbers. Okay, so if I am
stuck with a specific cable for a specific unit, why am I unable to
procure additional cables that it _heavily implies it is capable of
doing?_

Needless to say, today has been a large source of frustration as I
learnt this through trial and error. All it needed was one additional
cable and this entire situation would have been avoided. Instead, I
spent a long time trying to troubleshoot all of the options I had
available to me before landing on "contact their technical support". It
has been a week and now I have to wait longer for a new cable to arrive
(their support was surprisingly swift and helpful).

## Blindsight
Theoretically, all of this is to power my graphics card. Nothing
explicitly requires that I have a graphics card in the first place. A
little discovery I made during my CPU and motherboard research was that
CPUs like the Intel Core 13th generation come with integrated grpahics.
Combine that with a motherboard that expects it (i.e. has an HDMI or
some kind of graphical output) and I have a fully functional computer
without a graphics card! There might be some caveats of course (drawing
to the screen might be a little slower), but I can do some setup at the
very least whilst I see how I can get this graphics card working.

That plan has now let me boot this PC! Hooray! BIOS loaded up well and
I got to see a bunch of stats regarding what devices were connected
(and what wasn't). The shell being ready means I can finally put [the
plan from last week][Lightning Tower] into action and give this a soul.

## Welcome, Sage!
I installed Kubuntu 22.04 LTS off of a USB drive given that I do not
have a CD drive and that process went pretty smoothly. I did a
reinstall of it later with the third-party drivers included, but
otherwise the installation was super smooth.

Rather than treating this as a tinker box and having the minimum
possible setup from which to build off of, I approached it from the
perspective of "I want a computer to live with". To this end, I
included the default programs (Calculator, OpenOffice Libre etc.)
and used the guided setup for the NVMe hard drives. I had the option
to use LVM (Logical Volume Management) for the storage management, but
from my brief research it mostly helps with large amounts of storage on
disk drives. This might be useful to consider for NAS solutions in the
future, but I think the simplicity will be more beneficial here.

I tend to give my machines names rather than something descriptive
(for example, I don't call things "Windows 10 PC" or some random
identifier). For this one, I decided to call it "Sage". Sonic Frontiers
has been fresh on my mind and, combined with the previous talk on
souls, felt natural to make the connection.

Once that was all ready, I could start installing programs. Firefox and
Thunderbird were already installed, so that covers the browser and mail
client requirements. I was pleasantly surprised that Thunderbird could
connect to Microsoft services as I feel as though it couldn't
previously. It's not perfect as it rqeuires essentially displaying
mailboxes in the old Outlook interface, but it's better than nothing. I
do have the option of attempting to setup Outlook as a Progressive Web
App, but I might leave that for another day.

The other items on my list were Discord and Steam. Both were incredibly
easy to install on Kubuntu as their app catalog, "Discover", were a
simple search and click. Both required updates after installation, but
even this process was simple. I have not had the opportunity to try
Steam yet, but the Discord client works perfectly so far.

All in all, Kubuntu has been a great introductory experience. If I can
login. Oh, I forgot to mention: the login screen does not appear. I am
still investigating that, so hopefully by the next update I will have
the issue resolved. For those curious, I currently use `Ctrl + Alt +
F2` to swap to a new terminal, login there and use `startx` to manually
load the desktop interface.

## What's next?
The most obvious thing to tackle next is the login screen. Although a
workaround is available, it is an awful experience for my home PC. I
have seen some evidence here and there of people receiving the same
problem but no solutions yet. I have kept track of my progress and will
provide a full update once I get it resolved.

I also want to get a VPN setup on this PC to match my previous one.
Initial attempts were promising, but seem to missing one or two steps
that prevent it from working to its full potential.

Hope everyone has a lovely holiday period!

[Lightning Tower]: {% post_url 2022-12-12-lightning-tower %}

---
title: Lightning Tower
date: 2022-12-12 22:30:00 +1100
---

## The Body
Before I get too deep into the details of the world of Linux, there is
one feature it needs that it shares with Windows: a computer to run on.
My gut reaction for computers to go high-end to give myself the best
gaming opportunities and for the computer to be relevant for as long as
possible.

The latter is probably a logical fallacy given that we get a wide range
of specs per generation of CPU and GPU release, but the former reason
is tough to overlook. It probably stems from my youth back in the days
of DotA 2 and the Tomb Raider reboot where games started requiring
players to _look_ for polygons rather than trying to imagine they were
not there. Add onto this a desire for a smooth framerate and the
minimum requirements were pretty high. At the time, I got the latest
NVIDIA GTX 1080 and to this day, I have not had any major performance
issues with that setup.

That in itself highlights the change in gaming and the computers needed
to run them. It feels like we have stagnated in graphic fidelity, not
out of laziness or style, but simply from the fact that we can only see
so much. What began as an obvious line became two or three lines and so
on until it looks like a curve. We can throw much more processing now
than we could before so we can make a few pixels of this curve
slightly more correct. I don't think it's pointless, but its value has
certainly diminished in my opinion. At the same time, we see many games
utilise style and nostalgia to capitalise on this fact. Pretty close is
close enough as long as it is visually appealing. This tends to have
positive performance impacts too!

This ends up making me consider that perhaps I don't need a high-end
computer anymore; the majority of tasks and games I'm involved them
don't need it. I'd need something new to pull me in, something
revolutionary.

No, VR is not it.

Hardware-enhanced ray-tracing looks pretty cool however...

From my relatively narrow research, I found that the AMD Radeon were
much cheaper than their NVIDIA cousins and competed pretty well in
quality. The RX 6800 was around $1000 whereas anything in that price
range from NVIDIA would have been sold out. Unfortunately, it turns out
that AMD cards sell out too, so I conceded myself to getting an AMD
Radeon RX 6900 XT instead. So much for not being high-end...

I mostly chose the surrounding pieces to match the graphics card's
expectations. This gave me the following:

| Hardware     | Product                  |
| ------------ | ------------------------ |
| CPU          | Intel Core i5 13600K     |
| GPU          | AMD Radeon 6900 XT (MSI) |
| Motherboard  | ASUS Prime Z790-P Wi-Fi  |
| Memory       | 2 x 16GB                 |
| Power Supply | 750W                     |
| Storage      | M.2 1.25 TB              |

This ended up totalling around $3,200 AUD, obviously with quite a bit
of fluff added. For example, a Wi-Fi enabled card seems pointless if
you have a direct ethernet port. But if it includes Bluetooth support,
it means I can use controllers without cables without needing to watch
an expansion slot (which I'll probably never fill, knowing myself). The
M.2 storage is also very new to me, so I cannot wait to see how it
performs.

My last PC's tower was very strange in that the motherboard had to be
installed _upside down_. This had a very weird impact on cabling where
provided cables were sometimes strained to reach things they expected
were nearby. I hope this tower being a little more standard should make
it easier for me in the long run.

I see this as a run-of-the-mill build that's probably a little
indulgent for my own requirements I already laid out. I have not setup
any contact methods yet, but I do have a
[Discussions section][Discussions] on this blog's
[GitHub page][GitHub]. If you have any suggestions or want to talk
about your own experience with building PCs today, I recommend starting
a topic there.

## The Soul
The build itself may be straightforward, but I will not know it works
until I have the operating system setup and ready to go. I want to plan
out all of the bells and whistles of this Linux system, but I want to
be practical at the same time.

My previous computer could dual-boot into [Elementary OS][ElementaryOS]
and although it worked for what I needed it to do, I never used it
outside of those situations; Windows just did what it did better. Part
of this comes from the nature of dual-booting (which I might talk about
later), but I think I need to embrace the fact I'm on a Linux machine
further and go with an operating system (or desktop environment, I
guess) that feels a little closer to the machine.

For this PC, I've chosen to use [Kubuntu][Kubuntu] as my "distribution"
of choice. For those unaware, Linux systems tend to be split into two
pieces: the operating system that handles all of program management and
core functionality, and the desktop environment that provides a user
interface on top of the operating system. Kubuntu uses Ubuntu and KDE
as its respective pieces which is personally familiar in its UI style
its command line toolset.

After this I aim to get the drivers installed for the hardware
components I have including:
* Graphics card drivers
* Motherboard drivers (e.g. for sound and bluetooth)
* Keyboard and mouse drivers

Finally, I want to get some core programs installed: Discord, Steam, a
browser and a mail client. These together should serve as a
confirmation that the computer is ready for a deep dive in its setup.

## What's next?
Well, I can't delay it any longer: I have to build my computer. I
received the parts on the day of writing this blog post and spent the
time since writing these blog posts. The next update will summarise my
build process and any thoughts I had on it and then cover the steps I
took to install the distribution, drivers and programs.

For the purposes of this blog, I want to be as detailed as I can on the
process I took. Not only will this let me rollback changes a little
easier, but I hope it can stand as an example of how a home Linux PC
can be setup and how feasible it can be to the general PC audience.

[GitHub]: https://github.com/TomChapple/linux-machine-at-home
[Discussions]: https://github.com/TomChapple/linux-machine-at-home/discussions
[ElementaryOS]: https://elementary.io/
[Kubuntu]: https://kubuntu.org/

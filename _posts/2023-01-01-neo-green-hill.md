---
title: Neo Green Hill
date: 2023-01-01 15:15:00 +1100
---
## Two Wrongs Don't Make a Right
It has been a while since my last update and for that I apologise.
However, I have some great news: the login issue was resolved! Not only
that, the PCI-e cable I required has arrived and with that, I am able
to power my graphics card. The most interesting part of all of this is
that the login issue I experienced before is directly related to the
graphics card being absent from my computer.

What was the problem? Upon startup I would see my motherboard's logo
during the BIOS and during the startup. However, it would stay there
for a suspiciously long time, especially since I'm using M.2 storage.
After about a minute, I would get an error message that appeared which
either talked about `hdaudio` or some device not being connected; both
of these are unrelated to the problem.

However, my computer was still functioning; I could use `Ctrl + Alt +
F2` to switch to terminal 2, given that the login was being attempted
on terminal 1. I did most of my analysis by logging into that session,
manually starting the UI with `startx` and using my computer as normal
otherwise.

The login screen not appearing led me to two places to search for
issues with the most obvious being `journalctl -b` (`-b` being used
here for the current boot). This is intended to be a system log file
which looks to be a general catch-all for logs if there is no other
natural place for it to go (e.g. `/var/log` is an inappropriate place).

With this, I was able to find evidence of `logind` and `sddm` starting
or being referenced. Both of these are involved during the startup of
the login greeter with Simple Desktop Display Manager (SDDM) being the
process that shows the UI of logging in. However, `journalctl` by
itself only proved that SDDM was starting up, but never showed any
other evidence of functionality, let alone a failure.

After a lot more research and some discussion on the [Kubuntu
Forums][Forums], I eventually found that SDDM itself uses the program
X for display by default. This would eventually point me to a new log
file located at `/var/log/Xorg.0.log`. This had a lot more information
that was relevant to the problem at hand. You can find the log file in
question below.

```
[   117.136] (II) systemd-logind: took control of session /org/freedesktop/login1/session/_32
[   117.139] (--) PCI:*(0@0:2:0) 8086:a780:1043:8882 rev 4, Mem @ 0x6002000000/16777216, 0x4000000000/268435456, I/O @ 0x00006000/64, BIOS @ 0x????????/131072
[   117.139] (II) LoadModule: "glx"
[   117.139] (II) Loading /usr/lib/xorg/modules/extensions/libglx.so
[   117.143] (II) Module glx: vendor="X.Org Foundation"
[   117.143] 	compiled for 1.21.1.3, module version = 1.0.0
[   117.143] 	ABI class: X.Org Server Extension, version 10.0
[   117.143] (==) Matched modesetting as autoconfigured driver 0
[   117.143] (==) Matched fbdev as autoconfigured driver 1
[   117.143] (==) Matched vesa as autoconfigured driver 2
[   117.143] (==) Assigned the driver to the xf86ConfigLayout
[   117.143] (II) LoadModule: "modesetting"
[   117.143] (II) Loading /usr/lib/xorg/modules/drivers/modesetting_drv.so
[   117.144] (II) Module modesetting: vendor="X.Org Foundation"
[   117.144] 	compiled for 1.21.1.3, module version = 1.21.1
[   117.144] 	Module class: X.Org Video Driver
[   117.144] 	ABI class: X.Org Video Driver, version 25.2
[   117.144] (II) LoadModule: "fbdev"
[   117.144] (II) Loading /usr/lib/xorg/modules/drivers/fbdev_drv.so
[   117.144] (II) Module fbdev: vendor="X.Org Foundation"
[   117.144] 	compiled for 1.21.1.3, module version = 0.5.0
[   117.144] 	Module class: X.Org Video Driver
[   117.144] 	ABI class: X.Org Video Driver, version 25.2
[   117.144] (II) LoadModule: "vesa"
[   117.144] (II) Loading /usr/lib/xorg/modules/drivers/vesa_drv.so
[   117.144] (II) Module vesa: vendor="X.Org Foundation"
[   117.144] 	compiled for 1.21.1.3, module version = 2.5.0
[   117.144] 	Module class: X.Org Video Driver
[   117.144] 	ABI class: X.Org Video Driver, version 25.2
[   117.144] (II) modesetting: Driver for Modesetting Kernel Drivers: kms
[   117.144] (II) FBDEV: driver for framebuffer: fbdev
[   117.144] (II) VESA: driver for VESA chipsets: vesa
[   117.145] (EE) open /dev/dri/card0: No such file or directory
[   117.145] (WW) Falling back to old probe method for modesetting
[   117.145] (EE) open /dev/dri/card0: No such file or directory
[   117.145] (II) Loading sub module "fbdevhw"
[   117.145] (II) LoadModule: "fbdevhw"
[   117.145] (II) Loading /usr/lib/xorg/modules/libfbdevhw.so
[   117.145] (II) Module fbdevhw: vendor="X.Org Foundation"
[   117.145] 	compiled for 1.21.1.3, module version = 0.0.2
[   117.145] 	ABI class: X.Org Video Driver, version 25.2
[   117.145] (EE) Unable to find a valid framebuffer device
[   117.145] (WW) Falling back to old probe method for fbdev
[   117.145] (II) Loading sub module "fbdevhw"
[   117.145] (II) LoadModule: "fbdevhw"
[   117.145] (II) Loading /usr/lib/xorg/modules/libfbdevhw.so
[   117.145] (II) Module fbdevhw: vendor="X.Org Foundation"
[   117.145] 	compiled for 1.21.1.3, module version = 0.0.2
[   117.145] 	ABI class: X.Org Video Driver, version 25.2
[   117.145] (II) FBDEV(2): using default device
[   117.145] vesa: Refusing to run on UEFI
[   117.145] (EE) Screen 0 deleted because of no matching config section.
[   117.145] (II) UnloadModule: "modesetting"
[   117.145] (EE) Screen 0 deleted because of no matching config section.
[   117.145] (II) UnloadModule: "fbdev"
[   117.145] (II) UnloadSubModule: "fbdevhw"
```

Of note is the following:
* `logind` definitively is involved with X during startup, so I am
  looking in a relevant place.
* Candidate drivers appear to be loaded—"modesetting", "fbdev" and
  "vesa"—which are used in that order. If an error occurs, it moves to
  the next.
* When all drivers fail, the screen is deleted. This is particularly
  important as I found that when I switched _back_ to terminal 1, I
  would not see the booting screen with my motherboard's logo, but
  instead what appeared to be a list of logs.
* One of the drivers, "modesetting", failed because `/dev/dri/card0`
  was missing.

A lot of this information was built on the research in [this
StackExchange question][SDDM StackExchange] (and links to other
questions in it). At the time, my video card was essentially the
integrated graphics of the Intel Core i5-13600K processor. This leads
me to the theory that none of these drivers could interact with with
the integrated graphics and therefore failed to find a feasible option
for rendering the login UI. After that, it would "delete" the screen,
which had the effect of pausing it in its current state of the
motherboard's logo.

Some members of the Kubuntu forums theorise that this incompatibility
may come from the fact that the 13th generation Intel cores were not
fully supported in Ubuntu 20.04. This problem could be fixed with "hwe"
kernels which aim to allow the LTS version to work with newer hardware.
Despite this, I am not sure if my computer used the CPU for its
rendering or the integrated graphics given this incompatibility, but
otherwise it worked servicably doing non-graphic tasks such as web browsing and using Discord.

The _other_ way to fix this is to introduce a piece of hardware that
the drivers expect. For example: a graphics card.

As soon as I received another PCI-e cable from the power supply
manufacturer (which came surprisingly quickly over the holiday period)
I put my graphics card back in, used the new cable along with the
existing two and it worked immediately. Not only did it power up and
use the HDMI output from that card immediately, the login greeter also
appeared too! I have not checked the logs I mentioned before since
then, but it lines up with the theory that drivers did not exist for
the integrated graphics but would support the standard protocols
(OpenGL etc.) on graphics cards.

What this, I have a lesson that I wish to share with those who want to
embark on this home Linux journey:

> If you are using bleeding edge hardware, make sure your build is
> complete, graphics card and all, before you install Ubuntu-based
> distributions.

The only other thing I needed to do after the graphics card
installation was to ensure my desktop environment was "Plasma/X11".
This is the intended default, but I am certain that it changed off of
it during my prior troubleshooting.

Honestly, since the graphics card has been installed, the use of this
PC has been very smooth.

## Gaming without Windows
With the graphics card installed and the major issue with the login
greeter resolved, my next immediate goal was to give some games a go.
The most direct option for getting games ready is with Steam, so I
started with my existing library there.

I tried out Civilisation VI to begin with. I feel like this serves as
a servicable starting point to validate that the graphics were up to
scratch. It helps that this game has a native Linux (or SteamOS)
version I can use. Upon starting it, I gave the benchmark options a
go and they worked without issue. Even in gameplay, I did not notice a
single issue wiht windowing or strange crashes. The only issue I did
have was the graphics quality defaulting to Low as it did not recognise
my graphics card. This was fixed by jumping into the graphics settings
and simply... setting them to high. It might be awkward to do this for
every game, but I think it is a small price to pay.

The next game I tried was a Windows-only game. Currently, Steam allows
for some Windows-only games to be played using their "Proton" system.
This is absolutely fascinating to me as it (and Wine, which it is based
on) **do not** emulate a Windows machine, but instead intercept calls
to Windows-only services and re-implement those in other standards.
Steam has a curated list of games which work with Proton, but it is
possible to manually specify a game should use it. This is done by
going to the game's properties, moving to the "Compatibility" tab and
checking "Force the use of a specific Steam Play compatibility tool".
You can also do this on a general level, but I feel more comfortable
doing this game-by-game.

The game I tested was Freedom Planet 2, a 2D platformer reminiscent of
momentum-based platformers like Sonic the Hedgehog. This worked right
out of the gate with very minor issues. In particular, I switched it to
full screen and it would drop out _very_ briefly between scenes (e.g.
cutscenes, gameplay and world maps). Otherwise, it picked up my
controller immediately and had zero graphical or gameplay issues. What
an absolute treat!

The other game I tested was Spark the Electric Jester 2. I hadn't tried
this series before now and it was only available on Windows, so this
was definitely a risky buy. However, it worked even better than Freedom
Planet did as there were no windowing problems whatsoever. Overall,
Steam's Proton is working splendidly.

## External Devices
As I mentioned before, I used a controller for these games. In
particular, I'm using an Xbox Series X controller connected via my
motherboard's built-in bluetooth. I have been sick to death of cables
getting caught in chairs and the like, so I wanted this computer to be
the step I take to drop that problem altogether. Outside of needing to
get the antenna properly positioned, this has been a smooth experience!
My controller is automatically picked up not only by Steam games, but
Windows-only games and native Linux programs outside of Steam. No
external drivers were needed!

My headset is a slightly different story though. I found that my
microphone only worked whilst it was plugged in via the cable, but it
turns out this is due to a setting in Kubuntu for the audio device. In
particular, my Arctis Bluetooth 3 is set to a "High Fidelity Playback
(AD2P Sink)" profile by default, but you can change it to the
"Handsfree Head Unit (HFP)" profile to enable the microphone after it
connects. Annoyingly, I must do these steps every time it connects and
I cannot see a way of setting a default. Hopefully there is some kind
of easy alternative I can leverage in the future.

I also use a Logitech G502 as my mouse. In particular, it has the
ability to change the mouse speed via some existing buttons on it. Out
of the box, these do not function. _However_, a program on Kubuntu's
"Discover", [Piper][Piper], is able to handle this functionality
perfectly. This also serves as an example of Discover being a great
way to install programs. Without it, I would have need to install and
configure `ratbagd` on the command line before installing Piper as a
UI interface. With Discover, this dependency was automatically included
and as such worked within a single restart after installing it. With
this, I will report a second lesson:

> Tend towards provided App stores with Linux distributions. It is
> tempting to use the command line and `apt`, but these services are
> built with the express purpose of many installation and use of apps
> on Linux easy and user-friendly.

In situations where there is no other option, `apt` is likely the way
to go, but I feel like it is too easy to use this as the first option
and not explore what these communities have done to make this
accessible nowadays.

## What's next?
This is a big question for me. I essentially have my home PC up and
running; in some ways it is running even better than my old PC.

I mentioned last time that I wanted to get a VPN setup. I have
successfully done this, but I will leave this until the next blog post
as it delves into what to do when compatibility doesn't work quite yet.

With Steam providing me a way to game with my Windows games, I also
want the opportunity to run some others outside of Steam. I have messed
around with programming GameBoy games using BGB as the emulator of
choice, but it turns out that is Windows-only. As such, I want to see
if I can get Wine working on a level similar to that of Steam's Proton
for these kinds of programs.

I also want to take the opportunity early to organise my file system to
be as scalable as possible. In particular, I still don't have my second
M.2 drive formatted for use and I plan to move my home drive there so I
have as much room as possible for personal files.

I also want to delve into what applications I have installed thus fur
for what purposes, as I see that would be a useful reference to have
when attempting to solve a problem (e.g. image manipulation with GIMP).

Otherwise, this is a hopeful year for my home Linux PC. I hope you have
all had a great New Year's for 2023 and I hope this year will be great!

[Forums]: https://www.kubuntuforums.net/
[SDDM StackExchange]: https://unix.stackexchange.com/questions/691378/cant-get-to-sddm-login-screen-startx-works-kde-on-ubuntu-server
[Piper]: https://github.com/libratbag/piper/

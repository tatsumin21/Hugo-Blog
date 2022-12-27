---
title: "Switch to Wayland Desktop"
date: 2022-12-27T21:13:05+01:00
draft: false
showDate: true
tags:
  - Linux
  - InfoTech
---

Merry Christmas and Happy New Year!

Thanks to the Christmas and new year holiday, i have enough free time to do something and decide to have a try with Wayland.

Here are a brief memory of my Wayland Setup

## Sum up List

- Operating System: Arch Linux
- Window Manager: Sway
- Terminal: foot
- Status bar: customized Waybar
- notification: mako
- Screenshot: script with grim + Slurp
- Dynamic Menu: wofi

Most of them are common used native wayland supporting applications. a lot of configurations are available to find and use as a reference. Here i only talk about part of them with special reasons.

### Sway

At the beginning i wanted to use DWL/River, both were inspired by DWM on X11, which is also i mostly used in X11. Unfortunately, the make installation and configuration has some problem and i cannot find the solutions to let them run smoothly. Therefore i decided to choose Sway, which is most famouse compositor on Wayland.

### Screenshot

i've used Flameshot on Wayland.

However the Flameshot has only limited supports for wayland. After searching i decided to use the traditional combination with native wayland CLI application `grim` + `slurp` with help of a simple bash script:

```bash
#!/bin/bash

SCREENSHOTDIR="$HOME/Screenshots"
TIME="$(date +%Y-%m-%d-%H-%M-%S)"
IMGPATH="$SCREENSHOTDIR/img-$TIME.png"

grim -g "$(slurp)" "$IMGPATH"
```

It is a simple script that will let you choose the part of screen and make a screenshot of that and then save the `.png` file, with naming it by time the screenshot made, under `Screenshots` in `$HOME` directory.

## At the End

I did not put all my configuration here just because almost all of them are only basic configs that can be found in the internet or Arch Wiki.

Wayland has been developed alreadz several years but also still in start-up stage.

I will try to mainly use wayland for several weeks or months. If there is not too many significant bugs that will have a great influence on daily usage, I will stay at wayland as daily drive.

X is old enough and it is time to leave it away.


---
title: "ArchLinux Desktop in 2021"
date: 2021-11-01T15:39:11+01:00
draft: false
showDate: true
Tags:
  -  InfoTech
  -  Linux
---

After totally transfer Operating System of my home desktop from Windows back to Arch Linux, it has been about 3 months. Although I have tried to go back to Windows once for gaming experiences, the general experiences in other usage was uncomfortable, so just 2 days after I went back to Arch Linux. 

This post is just for a simple memory.

The applications I am using for daily usage are as below:

## DWM

For window manager i use `dwm`. It's just because of the Stack tilling logic comparing to `i3` and ease of install and configuration comparing to `Xmonad`. Patching `dwm` is the thing I mostly don't want to do for using it. However, thanks to the [bakkeby/dwm-flexipatch](https://github.com/bakkeby/dwm-flexipatch) Repo on Github, this also becomes easy. 

### Status bar

I use the built-in status bar of dwm. Also thanks to the Repo `dwm-flexipatch`, the issue of confliction of `alpha` and `systray` was perfectly solved, and plus the `autostart` patch, it is a just status bar providing enough information and functions for normal use.

## st

The same as `dwm`, for terminal I also use the solution provided by [Suckless.org](https://suckless.org/). However, for st I would like to recommend patching it manually by yourself. What's more, I choose [nord](https://www.nordtheme.com/) color theme for st.

## sum up list

- file manager: ranger
- screenshot: flameshot
- game: Lutris, steam
- browser: librewolf
- text editor: neovim
- shell: zsh
- dynamic menu: dmenu
- notification: dunst
- password manager: pass
- cloud storage & sharing: Nextcloud
- email: neomutt + isync + msmtp + notmuch +abook (easy installationa and configuration with [mutt-wizard](https://github.com/LukeSmithxyz/mutt-wizard) by [Luke Smith](https://lukesmith.xyz/))
- RSS: newsboat

By the way, I don't use a display manager to login into X. I configured the `.zprofile` to let it auto run `startx` after login on tty.

All of them are easy-to-config software, especially if you are a programmer. However, because many of them are terminal-based software, if you are also a non-programmer like me and have any questions, `Arch Wiki` and `Youtube` will help you solve almost all problems. 

Enjoy the Linux!



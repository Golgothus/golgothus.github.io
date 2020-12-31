---
title: "TMUX"
date: 2020-12-25T01:02:30-06:00
draft: false
author: "Golgothus"
description: "TryHackMe.com Beginner Path - TMUX walkthrough room"
summary: "TryHackMe Beginner Path - TMUX write-up."
tags: ["TryHackMe","Linux"]
featuredImage: "/images/city.png"
categories: ["THM - Beginner Path"]
---

# TMUX

TMUX Cheatsheet can be found [here](https://imgur.com/bL9Dn3U)

For another excellent resource on learning tmux, check out IppSec's video: [Link](https://www.youtube.com/watch?v=Lqehvpe_djs)

All TMUX commands start with **control + b**

## Session Management

To detach from a session **control + b + d**

To review all open **sessions** we run:

```bash
tmux ls
```

When you create a tmux session without any set name, it defaults to a number - 0

To attach to this session once it's detached ( tmux + ctrl + b + d) we can do:

```bash
tmux a -t 0
```

## Windows Management

To create a new "Window" in TMUX, create your session, then type:

control + b + c

## Copy Mode

To enter copy mode, we use:

```bash
tmux
control + b + [
```

This will allow us an interactive window to scroll in

Some useful commands to keep in mind while in copy mode:

- ctrl + b + [ (enters copy mode)
- ctrl + b + g (go to top)
- ctrl + b + G (go to bottom)
- ctrl + b + q (quit)

To select a window, make sure you are not in an attached session, to check run:

```bash
tmux ls
```

This will let you know if you are attached, to detach run ctrl + b + d, then we can attach to a new window by running:

ctrl + b + \<window number>

## Pane Management

Useful commands:

- ctrl + b + % (split the panes vertically)
- ctrl + b + " (split the panes horizontally)

> We can also resize these panes by holding down the key combo and pressing the arrow keys, try it out now!
>
> ctrl + b + w (lists all active windows, and allows for selection)
> ctrl + b + & (kills windows)

If a pane becomes unresponsive, run ctrl + b + x

To kill a pane via command, run:

```bash
exit
```

> tmux new -s neat

## Byobu

Download Byobu (another terminal multiplexer) through the following site, [here](https://opensource.com/article/20/2/byobu-ssh)

Useful commands:

- ctrl + a + f2 = new window
- ctrl + a + shift + f2 = horizontal split
- ctrl + a + ctrl + f2 = vertical split
- ctrl + a + ctrl + f6 = kill focused split

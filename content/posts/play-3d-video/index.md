---
title: "Playing 3D Videos on Mac Using ffplay"
date: 2024-12-07T21:41:38+04:00
slug: 'play-3d-video'
draft: false
cover: "https://jiejue.obs.ap-southeast-1.myhuaweicloud.com/20250202030000561.webp"
tags:
  - macos
  - 3d video
  - ffplay
---

Recently, I got a pair of smart glasses that support 3D video viewing. They support SBS (Side-by-Side) format videos, but most 3D video content available is in HSBS (Half-width Side-by-Side) format. I was originally using Bino, a well-known 3D player, but it was developed for Intel chips and often lagged on my M1 Mac.

<!--more-->

Later, I discovered that ffplay (part of the ffmpeg project) could perfectly meet my needs. It has native support for Apple Silicon, excellent playback performance, and is simple to operate.

Installation is straightforward. First, you need to install Homebrew - if you haven't already, just go to [brew.sh](https://brew.sh) and follow the instructions. Then, a single command installs ffmpeg:

```bash
brew install ffmpeg
```

Playing 3D videos is also simple. For example, to convert and play an HSBS format video as SBS:

```bash
ffplay -i video.mp4 -vf "scale=2*iw:ih"
```

If you find entering the command tedious, you can add a function to your fish shell configuration file (~/.config/fish/config.fish):

```bash
function play3d
    ffplay -i $argv -vf "scale=2*iw:ih"
end
```

Then you can simply use `play3d video.mp4` from now on.

Some common ffplay shortcuts:
- w: Show/hide progress bar
- Space: Pause/resume
- Left/Right arrow keys: Rewind/forward 10 seconds
- Up/Down arrow keys: Rewind/forward 1 minute
- q or ESC: Quit

What's particularly nice is that if the video has embedded subtitles, ffplay will automatically display them on both sides without any additional configuration.

Compared to other 3D players, while ffplay's interface might be simpler, it has all the essential features, excellent performance, and is very convenient to use. For those looking to watch 3D videos on Mac, this is a great option.

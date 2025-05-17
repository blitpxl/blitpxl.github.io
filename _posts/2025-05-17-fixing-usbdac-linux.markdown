---
title: "Fixing a Quiet USB DAC on Linux"
date: 2025-05-17
tags: linux audio
---

One of the first issue I noticed when daily driving a Linux machine is that my cheap JA11 confusingly outputting less than its half volume even when setting the master volume and all the mixer sliders to full.

But the I found out that there's another hidden mixer settings that at the time I wasn't aware of; `alsamixer` and `pavucontrol`, choose either, according to the current sound server you're currently running. alsamixer if you're running on **pipewire** and pavucontrol if you're running on **PulseAudio**.

In my case, I'm running pipewire, so just type alsamixer on your console and you should be greeted with a modest TUI:

![](/assets/blog/25_05_17/alsamixer_1.png)

Navigate to your active DAC

![](/assets/blog/25_05_17/alsamixer_2.png)

![](/assets/blog/25_05_17/alsamixer_3.png)

And there we have it! The PCM level was set to 30 (for unknown reason.)

CRANK IT TO 100!
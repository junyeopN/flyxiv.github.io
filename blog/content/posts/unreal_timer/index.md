---
title: <Unreal> Adding Timer to Unreal Engine
date: 2025-11-11T18:33:20Z
lastmod: 2025-11-17T18:33:20Z
author: Jun Yeop(Johnny) Na
avatar: /images/favicon.svg
# authorlink: https://author.site
cover: cover.png
category: Game Dev
categories:
  - unreal
  - c++
tags:
  - unreal
  - c++
# nolastmod: true
draft: true
---

I'm trying to make a real-time turn-based system for UE5.

The first step is implementing a timer. Every controllable character will get this timer component and notify the game mode that their turn is here when the timer expires.

# 1. Create an ActorComponent named TurnTimer

![turntimer](./turntimer.png)

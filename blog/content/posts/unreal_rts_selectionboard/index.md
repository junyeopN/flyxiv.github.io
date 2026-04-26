---
title: <Unreal> Creating RTS-Style Selection UI Board
date: 2025-12-01T18:33:20Z
lastmod: 2025-12-01T18:33:20Z
author: Jun Yeop(Johnny) Na
avatar: /images/favicon.svg
# authorlink: https://author.site
cover: cover.png
categories:
  - unreal
  - c++
tags:
  - unreal
  - c++
  - UI
# nolastmod: true
draft: true
---

Good UI is Crucial for an RTS-Style game. I'm going to create a RTS-style unit selection board so that it is easy for the player to check each selected unit's status.

# Architecture

![architecture](./architecture.png)

We need to use nested Widget architecture for reusability.

## 1. UnitSelectionInfo

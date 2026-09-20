---
title: <Unreal> Implement RPG-like Character Stats
date: 2025-06-29T18:33:20Z
lastmod: 2025-06-29T18:33:20Z
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

I needed to add common stats such as HP, movement, attack for my tactics game.

The simplest way is to put them directly in the `Character` Class, but there is a better way: `Actor Component`

## Modularity - Why Actor Component is a Better Choice

Putting stats directly in character will make it hard to reuse these stats in other classes.

- We might have classes other than characters that want to use this class, but if we have these stats in Character, we “HAVE TO DECLARE THEM AS CHARACTER”, which means we have to load all other heavy implementations the Character class has, slowing down the game.

![modularity](./modularity.png)

# Creating UnitBaseStats

Tools -> Create New C++ Class -> `ActorComponent`

![modularity](./modularity.png)

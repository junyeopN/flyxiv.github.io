---
title: <Unreal> Adding Animation To Character
date: 2025-11-11T18:33:20Z
lastmod: 2025-11-17T18:33:20Z
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
# nolastmod: true
draft: true
---

I defined a Gameplay Character but when I give the character a skeletal mesh it is in its stiff, default motion.

![stiff](./stiff.png)

I'm going to implement simple movement animation blueprint for the character.

# Character Asset

https://www.fab.com/listings/dc21702b-7f1e-4aa5-a747-78d519f5fb51

# 1. Create Animation Blueprint

![anim](./anim_blueprint.png)

Set skeleton to `Yin`

![anim2](./anim_blueprint2.png)

Open the animation blueprint.

# 2. Set Animation Blueprint Variable "Speed"

- We're going to set animation to `idle` if the character's speed is 0, and `jogging` if speed is > 0.
- The animation state machine **cannot use the given pawn's status by itself, because it works in a different thread.**

  - We need to set a `Speed` Variable in the ABP(Animation Blueprint) and set it at event graph.

- We get the pawn's actor and save it's 2D speed to Speed

![anim3](./anim_blueprint3.png)

# 3. Add New State Machine

Now we add the animation state machine that will give us `Idle` or `Jog` Animation based on the `Speed` Variable we set.

![anim4](./anim_blueprint4.png)

Name the state machine anything (I named it `WalkJog`), create a `Walk` and `Jog` State and their transitions

![anim5](./anim_blueprint5.png)

We drag the idle animation to the `Idle Combat` State and connect it with the result pose. Same for Jog.

![anim6](./anim_blueprint6.png)

After doing this to jog to, we go to the state transitions and add Speed conditions to the transition.

![anim7](./anim_blueprint7.png)

Same for Jog -> Idle.

Now our animation blueprint is ready. Set it to our character.

![anim8](./anim_blueprint8.png)

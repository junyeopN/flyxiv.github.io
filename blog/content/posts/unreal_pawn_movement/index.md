---
title: <Unreal> Methods For Setting Pawn Movement
date: 2025-12-06T18:33:20Z
lastmod: 2025-12-06T18:33:20Z
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
draft: false
---

# 1. FloatingPawnMovement

```cpp
// in header file,
UPROPERTY(VisibleAnywhere, BlueprintReadOnly, Category = "Movement", meta = (AllowPrivateAccess = "true"))
class UFloatingPawnMovement* FloatingPawnMovement;


// source: MaxSpeed and Acceleration
FloatingPawnMovement = CreateDefaultSubobject<UFloatingPawnMovement>(TEXT("FloatingPawnMovement"));
FloatingPawnMovement->MaxSpeed = CharacterSpeed;
FloatingPawnMovement->Acceleration = 2000.0f;
```

# 2. AddActorWorldOffset

```cpp
const float Speed = 1200.f; // units/sec
const float Dt = GetWorld()->GetDeltaSeconds();

const FRotator YawRot(0.f, GetControlRotation().Yaw, 0.f);
const FVector Forward = FRotationMatrix(YawRot).GetUnitAxis(EAxis::X);
const FVector Right   = FRotationMatrix(YawRot).GetUnitAxis(EAxis::Y);

const FVector Delta = (Forward * Input.Y + Right * Input.X) * Speed * Dt;

Pawn->AddActorWorldOffset(Delta, true); // sweep=true if you want collision
```

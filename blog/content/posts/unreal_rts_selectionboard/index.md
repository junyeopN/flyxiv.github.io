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

```cpp
// Fill out your copyright notice in the Description page of Project Settings.

#pragma once

#include "CoreMinimal.h"
#include "Blueprint/UserWidget.h"
#include "UnitSelectionPanel.generated.h"

/**
 *
 */
UCLASS()
class FORBIDDENTACTICS_API UUnitSelectionPanel : public UUserWidget
{
	GENERATED_BODY()

private:
	UPROPERTY(EditDefaultsOnly, BlueprintReadWrite, Category = "UI", meta = (AllowPrivateAccess = "true"))
	float TotalComponentLength;

	UPROPERTY(EditDefaultsOnly, BlueprintReadWrite, Category = "UI", meta = (AllowPrivateAccess = "true"))
	float BoxLength;

	TArray<TObjectPtr<class ATacticsBaseUnit>> SelectedUnits;
};
```

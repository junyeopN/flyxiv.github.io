---
title: Optimizing Vision AI Model Serving to Scale For 25 Million Requests Per Day
date: 2025-11-20T11:47:20Z
lastmod: 2025-11-20T11:47:20Z
author: Jun Yeop(Johnny) Na
avatar: /images/favicon.svg
# authorlink: https://author.site
cover: cover.png
categories:
  - Work
  - Model Optimization
  - Model Serving
tags:
  - Quantization
  - Triton
  - Dynamic Batching
  - Distillation

# nolastmod: true
draft: false
---

Naver's biggest asset is the rich amount of Korean reviews for all kinds of movies/products/restaurants users generate in their Naver blogs.
I had to recently work in

Limitations:

### No "fraction of GPU" per pod

Our Service GPUs(V100) didn't support MIG, so our inference service pods needed to be assigned a whole GPU.

### Limited GPU Instances

Our team has 8 Nodes of V100 GPU, which was more than enough for a few years.

However, as the services our team provided kept increasing, we were running out of available V100 GPUs and it became apparent
that we have to use our limited resource more efficiently. While some of our models were in the Billion-parameter range,
even those models weren't big enough to utilize one V100 to maximum capacity. A lot of computation power was being wasted assigning one model for each GPU.

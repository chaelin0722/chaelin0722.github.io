---
title:  "[논문정리📃] Inception-v4 Inception-ResNet and the Impact of Residual Connections on Learning"
excerpt: "Week4 -Inception-v4, Inception-ResNet-"

categories:
  - CNN
  - paperReview
tags: [CNN, paperReview]

last_modified_at: 2021-06-29T08:06:00-05:00
classes: wide
---

## Inception-v4 Inception-ResNet and the Impact of Residual Connections on Learning
#### -Inception-v4, Inception-ResNet)-

[논문원본](https://arxiv.org/abs/1602.07261)😙


## 0. 요약

residual connection와의 학습은 Inception networks의 학습속도를 가속화하는데 중요하다. 

## 1. Introduction

이 논문에서는 residual connection(잔여연결)과 가장최근 개정된 버전의 Inception구조에 대해 설명한다.

- residual connections : 매우 깊은 구조를 학습하기 위한 것.

- inception 구조 : inception 네트워크가 아주 깊어지면서 inception 구조의 연속 스테이지가 residual connection으로 대체되었다. 
> 이것으로 Inception은 연산 효율성을 유지하면서 residual(잔여적)접근의 장점을 취할 수 있게되었다. 즉 연산량은 그대로이면서도 residual connection이 주는 장점을 더하게 되었다.
> 또한, inception 신경망을 좀 더 효과적으로 넓고 깊게 하기 위해 나온 것이 inception-v4이다. inception-v4는 inception-v3보다 단순하고 획일화된 구조와 더 많은 inception module을 사용한다. 


이 논문에서는 Inception-v4와 Inception-ResNet 두 가지 방법을 설명하고 있다. 

- Inception-ResNet은 Inception-v4에 residual connection을 결합한 구조이며 학습 속도가 빠르다.


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
#### -Inception-v4, Inception-ResNet-

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

<br>

## Related Work

Convolutional networks는 규모가 큰 이미지 인식에서 매우 유망해지고 있다. 그 다음으로 중요하게 주목된 것이 Network-In-Network 구조였다. 

residual connection은 He et al. 에 의해 소개되었으며 , **residual connection**은..

  (1) 이미지 인식과 특히 객체인식에 효과적이라는 것을 이론적으로, 실질적인 증거로 확신되고있다. 

  (2) residual connections은 매우 깊은 컨볼루션 모델을 학습하는데 필요하다고 주장한다. 
  
  (3) 학습속도를 매우 빠르게 향상시킨다.
  
  
 
## Architecural Choices

다음은 Inception-v4의 전체적인구조와 부분적 구조들이다.

![inceptionv4](https://user-images.githubusercontent.com/53431568/123669887-8dffee00-d877-11eb-86a3-72f9d64064b8.JPG)

Inception-v4는 이전 버전에서의 단점을 개선하고, inception block을 균일하게 획일화 하였다.


다음은 Inception-Resnet의 전체적 구조와 부분적 구조들이다.

![inceptionresnet](https://user-images.githubusercontent.com/53431568/123669882-8ccec100-d877-11eb-942a-16f3c01b6cad.JPG)

Inception-Resnet은 Inception network와 residual block을 결합한 형태이다. Inception-ResNet은 v1버전과 v2버전이 있으며 이 둘의 전체 구조는 같지만, 각 모듈에 차이가 있다!

`stem과 각 inception module에서 사용하는 filter수가 다르다`는 것이 그 차이이다. 

(inception-resnet-v1은 inception-v3와 연산량이 비슷하고, inception-resnet-v2는 inception-v4와 연상량이 비슷하다.)




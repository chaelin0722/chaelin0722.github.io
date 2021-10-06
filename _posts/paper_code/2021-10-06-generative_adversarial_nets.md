---
title:  "[논문정리📃] Generative Adversarial Nets"
excerpt: "-GAN-"

categories:
  - paperReview
tags: [CNN, paperReview, GAN]
use_math: true

last_modified_at: 2021-10-06T08:06:00-05:00
classes: wide
---

## Generative Adversarial Nets
#### - GAN - 

[논문원본](https://arxiv.org/pdf/1406.2661.pdf)😙


이번 논문은 GAN 의 시초가 되는 이안 굿펠로의 논문을 리뷰하겠습니다~. 

`Generative Adversarial Nets` 를 직역해 보면 생성적인 경쟁 네트워크 이다. 논문을 읽어보면 이해가 가겠지만 정말 GAN 을 정확히 표현하는 단어같다. 

<br>

## 🌟 Abstract

이 논문에서는 **경쟁 프로세스**를 통한 생성모델을 예측하는 새로운 프레임워크를 제안합니다. 여기서 **경쟁 프로세스**는 생성모델 G(Generator)는 데이터 분포를 포착하고, 
구분 모델 D(Discriminator)는 샘플이 G(생성된 모델) 보다 학습 데이터에서 생성된건지의 확률값을 예측하는 것을 말합니다. G의 목표는 D의 실수할 확률을 최대화 시키는 것이다.
(한마디로 구분자 D가 G가 만들어낸 모델인지 실제 모델인지 잘 못 맞추게 해야한다는 것이다.)

임의의 함수 G와 D의 공간에서는 한 개의 솔루션이 나오는데, `G는 학습데이터 분포 처럼 분포가되며, D는 어디든 $\frac{1}{2}$ 값을 갖는 것`이다. 이 말이 무슨뜻인지는 [아래]()에 나와있다. 이 말 중요하니까 잘 알아두자!⭐️⭐️


































## Introduction



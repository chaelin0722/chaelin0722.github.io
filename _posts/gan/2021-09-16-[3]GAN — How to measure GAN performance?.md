---
title:  "[GAN study] How to measure GAN performance?- GAN을 측정하는 방법"
excerpt: ""

categories:
  - gan

tags: [Deeplearning, study, gan]

classes: wide

use_math : true

last_modified_at: 2021-09-16T08:06:00-05:00

---


Gan 학습 로드맵 세 번째 시간입니다!

오늘은 GAN의 퍼포먼스를 어떻게 측정하는지 알아보는 시간을 가지려고 합니다.

이 글은 [Jonathan Hui의 포스팅](https://jonathan-hui.medium.com/gan-how-to-measure-gan-performance-64b988c47732)을 참고해 정리한 내용입니다!

<br>

![0_t2b9BdAmHlPj4PUS (1)](https://user-images.githubusercontent.com/53431568/133721856-1a72a581-9082-47e8-afe2-6d88ebd9239f.jpg)



GAN에서는, 생성자(Generator)와 구분자(Discriminator)의 객관적 함수는 각 네트워크가 상대편에 비해 얼마나 잘 하고있는지를 측정합니다. 생성자가 구분자를 얼마나 잘 속이는지 측정하는 것을 예로 들 수 있습니다. 하지만, 이것은 이미지의 품질과 다양성을 측정하기에는 좋은 지표는 아닙니다.

GAN 시리즈의 일원으로 각기 다른 GAN 모델들의 결과를 어떻게 비교하는지 <u>Inception Score(IS)</u>과 <u>Frechet Inception Distance(FID)</u>을 토대로 살펴보고자 합니다. 

<br>

## Inception Score (IS)
IS는 GAN의 수행능력을 두가지 기준으로 측정합니다. 

> - 생성된 이미지들의 품질
> 
> - 생성된 이미지들의 다양성


엔트로피는 랜덤하다고 볼 수 있습니다. 만약 랜덤변수 x가 예측이 쉽다면 낮은 엔트로피를 갖고 있다고 말할 수 있습니다. 반대로, 예측하기 어렵다면 그것은 높은 엔트로피를 갖고있다는 말이 됩니다.

아래 그림에 두 가지 확률분포 $P(x)$ 가 있습니다. $P_2$는 $P_1$ 보다 높은 엔트로피를 가지고 있는데 그 이유는, $P_2$는 균일한분포를 갖고 있고 이 때문에 $x$가 무엇인지 예측하기 어려워지기 때문입니다.

![1_RdIYRsqXxRAKwcjtxg6_kw](https://user-images.githubusercontent.com/53431568/133730812-56324b4b-8750-4aa5-adf5-4c20d02c6bea.jpeg)

GAN 에서는 조건부 확률 $P(y|x)$ 가 예측이 쉬워야(낮은 엔트로피) 합니다. (예를 들어 이미지가 주어졌을 때, 그 객체의 타입을 쉽게 알아야 한다.)

따라서, 생성된 이미지들을 분류하고 $P(y|x)$($y$는 레이블값, $x$는 생성된 데이터)를 예측하기 위해 <u>Inception network</u>를 사용하는 것입니다. 











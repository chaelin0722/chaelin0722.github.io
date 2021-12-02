---
title:  "[GAN study] Why it is so hard to train Generative Adversarial Networks!"
excerpt: ""

categories:
  - gan

tags: [Deeplearning, study, gan]

classes: wide

use_math : true

last_modified_at: 2021-12-03T08:06:00-05:00

---

Gan 학습 로드맵 여섯 번째 시간! 벌써 시간이 이렇게..! 오랜만의 GAN 포스팅!! 😮😮

영어 원문 글은 [여기](https://jonathan-hui.medium.com/gan-why-it-is-so-hard-to-train-generative-advisory-networks-819a86b3750b)에서 참고하기~


<br>

GAN - gan 학습은 왜이렇게 어려울까!

모네 그림을 그리는 것 보다 모네의 그림을 인식하는 것이 훨씬 쉽다. Generative model ( 데이터를 생성하는 것) 은 discriminative model( 데이터를 처리하는 것)
과 비교하는 것보다 훨씬 어려운 일이다. 마찬가지로 GAN 을 학습하는 것은 어렵다. 이번 글은 gan 시리즈의 부분으로 학습이 왜 이리 피해가는지 조사해보았다.

이번 공부를 통해 많은 연구자들의 방향을 이끄는 몇 가지 근본적인 문제를 이해할 것입니다. 우리는 연구가 어디로 향할지 알 수 있도록 몇 가지 의견 불일치를 조사할 것 입니다.
문제를 살펴보기 전, GAN 방정식 중 일부를 간단히 요약해 보겠습니다.


### GAN

GAN 은 정규 분포 또는 균일 분포를 사용하여 잡음 z 를 샘플링 하고 심층 네트워크 생성기 G 를 사용하여 이미지 x(x=G(z))를 생성 합니다.

GAN에서는 판별자 입력이 실제인지 생성되었는지 구별하기 위해 판별자를 추가합니다. 입력이 실제일 확률을 추정하기 위해 값 D(x) 를 출력합니다 .

<br>

### 목적 함수와 기울기

GAN은 다음과 같은 목적 함수를 갖는 minmax game 으로 정의됩니다.

아래 다이어그램은 해당 기울기를 사용하여 판별자와 생성기를 훈련하는 방법을 요약합니다.

## GAN 문제

많은 GAN 모델은 다음과 같은 주요 문제를 겪고 있다.을

> Non-convergence : 모델 매개변수가 진동하고 불안정하며 수렴하지 않는다.
> Mode collapse : 제한된 종류의 샘플을 생성하는 생성자가 붕괴된다.
> Diminished gradient : 구분자(discriminator)가 매우 성공적이어서 생성자의 기울기가 감소되어버려 아무것도 학습하지 못한다.
> genarator과 discriminator의 균형이 맞지 않아 오버피팅을 야기하고 하이퍼파라미터의 값에 따라 매우 예민하다.

### Mode

실제 데이터 분포는 multimodal 하다. 예를 들어, MNIST 에서는 10개의 숫자 '0' 에서 숫자 '9' 라는 10개의 메이저 모드가 있다.
아래 샘플은 두가지 다른 GAN에 의해 생성되었다. 위의 행은 모든 10 MODE를 생성하며 그 동안 두 번째 행은 한개의 모드만을 생성한다.(숫자 "6")
이렇게 데이터의 적은 모드만을 생성할 때, `mode collapse` 문제 라고 부른다. 


### Nash equilibrium (내쉬 균형)

GAN 은 zero-sum non-cooperative game에 기반하고 있따. 요약하자면, 한명이 이기면 다른 한명은 지는것! 제로섬 게임은 minimax 라고도 부른다. 
상대방은 자신의 행동을 최대화하기를 원하고 당신의 행동은 그것을 최소화하는 것입니다. 게임 이론에서 GAN 모델은 판별자와 생성자가 
내쉬 평형에 도달할 때 수렴합니다. 이것은 아래의 minimax 방정식의 최적의 포인트 이다.



양쪽 모두 상대방을 약화시키려고 하기 때문에 내쉬 균형은 상대방이 무엇을 하든 한 플레이어가 자신의 행동을 바꾸지 않을 때 발생한다.
 x 와 y 의 값을 각각 제어하는 두 명의 플레이어 A 와 B 를 고려하십시오 . 플레이어 A 는 xy 값을 최대화하려고 하고 B 는 최소화하려고 합니다.




---작성중--



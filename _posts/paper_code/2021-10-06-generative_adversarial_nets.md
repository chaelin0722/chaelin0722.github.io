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

임의의 함수 G와 D의 공간에서는 한 개의 솔루션이 나오는데, `G는 학습데이터 분포모양으로 회복되며, D는 어디든 `'$1 \over 2$'` 값을 갖는 것`이다.


<img width="689" alt="무제" src="https://user-images.githubusercontent.com/53431568/136160746-9dfb22c3-c1c5-48c9-937a-937923f64bde.png">

위의 그림처럼 학습할 수록 점점 generator은 원래 데이터 분포 양상처럼 변하고, discriminator 모델은 모든 $x$구간에 대해 $1 \over 2$ 값을 갖는 직선의 형태로 존재한다. 이 그래프가 무엇을 뜻하는지 [3. adversary net]()에 나와있다. 이 말 중요하니까 잘 알아두자!⭐️⭐️

G와 D는 다층 퍼셉트론으로 정의되어있어서 역전파로 전체 시스템을 학습할 수 있다. 더 이상 학습을 하거나 샘플을 생성할 때 마르코프 체인이나 unrolled approximate 추론 네트워크는 필요없다. 실험은 생성된 샘플들의 질적이고 양적인 평가를 통한 프레임워크의 잠재성을 나타낸다.

<details markdown="1">
<summary>마르코프 체인📚</summary>

#### 마르코프 체인(Markov chain)이란?
마르코프 체인의 정의란 마르코프 성질을 가진 이산 확률과정을 뜻한다. 마르코프 성질은 ‘특정 상태의 확률은 오직 과거의 상태에 의존한다’라는 것이다. 대표적인 문제로는 어제와 오늘의 날씨로 내일의 날씨에 대해 확률적으로 표현하는 것이 그 예이다. 이러한 연속적인 현상을 단순히 표현할 때는 마르코프 체인을 가정하고 쓸 수 있고, 종종 단순한 모델이 강력한 효과를 발휘할 때도 있다.

<br>

</details>

<br>

## 1. Introduction
딥러닝의 가능성은 자연적 이미지, 스피치를 포함한 오디오 파형, 자연언어 말뭉치의 상징과 같은 AI applications이 마주하는 데이터 종류의 확률분포를 대표하는 풍부하고 계층적인 모델을 발견하는 것이다. 더 나아가 딥러닝에 있어서 가장 큰 성공에 고차원의 풍부한 sensory input을 클래스 라벨에 매핑하는 discriminative model이 포함되어 있다. 이 큰 성공은 특정 부분에 잘 적용되는 기울기(gradient)를 가지는 piecewise linear units를 사용하는 역전파와 드롭아웃 알고리즘에 기반되어 있다.

deep generative 모델은 최대가능도함수(maximum likelihood) 예측과 관련 전략으로 인해 일어나는 많은 interactive한 확률적 계산들을 추론하는 데에 어려움이 있어 그 영향이 작았다. 또, 생성적 문맥(generative context)에서 piecewise linear unit의 이익을 높이는 것의 어려움 때문이라는 이유도 있다. 따라서 이 논문에서는 앞의 어려움들을 회피하는 새로운 생성모델 예측 프로시저를 제안하고 있다.


<details markdown="1">
<summary>piecewise linear unit📚</summary>

#### piecewise linear unit (조각별 선형 유닛)이란?

<br>

</details>

제안하는 adversarial net(경쟁 네트워크) framework에서는 생성 모델이 적대 관계의 적과 경쟁한다 : 식별자/구분자(D)는 샘플이 모델 분포(생성된 가짜)에서 나왔는지 아니면 데이터 분포(원본)에서 나왔는지를 결정하는 것을 학습한다. 비유를 해보자면, 생성 모델은 지폐 위조자에 비유되며 탐지없이 위조지폐를 생성한다(오로지 생성만 하는 네트워크 구조를 가짐). 경찰에 비유되는 식별모델은 생성자가 위조지폐를 생성하는 동안 위조지폐를 탐지한다. 이 게임에서의 경쟁은 위조지폐와 실제 지폐가 구분이 되지 않을 때 까지 진행되어 두 팀으로 하여금 각각의 방법을 향상시키게 한다. (경쟁하면서 두 네트워크의 성능이 향상됨)










<br>

## 2. Relative works



## 3.




## 4.




<br>


#### 참고

[1] [마르코프체인](https://www.puzzledata.com/blog190423/)


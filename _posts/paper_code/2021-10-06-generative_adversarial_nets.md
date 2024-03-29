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

임의의 함수 G와 D의 공간에서는 한 개의 솔루션이 나오는데, `G는 학습데이터 분포모양으로 회복되며, D는 어디든 1/2 값을 갖는 것`이다.


<img width="689" alt="무제" src="https://user-images.githubusercontent.com/53431568/136160746-9dfb22c3-c1c5-48c9-937a-937923f64bde.png">

위의 그림처럼 학습할 수록 점점 generator은 원래 데이터 분포 양상처럼 변하고, discriminator 모델은 모든 $x$구간에 대해 1/2 값을 갖는 직선의 형태로 존재한다. 이 그래프가 무엇을 뜻하는지 [3. Adversary nets](#3-adversarial-nets)에 나와있다. 

이 말 중요하니까 잘 알아두자!⭐️⭐️

G와 D는 다층 퍼셉트론으로 정의되어있어서 역전파로 전체 시스템을 학습할 수 있다. 더 이상 학습을 하거나 샘플을 생성할 때 마르코프 체인이나 unrolled approximate inference(근사 추론) 네트워크는 필요없다. 실험은 생성된 샘플들의 질적이고 양적인 평가를 통한 프레임워크의 잠재성을 나타낸다.

<br>

<details markdown="1">
<summary>마르코프 체인과 approximate inference📚</summary>

#### 마르코프 체인(Markov chain)이란?
마르코프 체인의 정의란 마르코프 성질을 가진 이산 확률과정을 뜻한다. 마르코프 성질은 ‘특정 상태의 확률은 오직 과거의 상태에 의존한다’라는 것이다. 대표적인 문제로는 어제와 오늘의 날씨로 내일의 날씨에 대해 확률적으로 표현하는 것이 그 예이다. 이러한 연속적인 현상을 단순히 표현할 때는 마르코프 체인을 가정하고 쓸 수 있고, 종종 단순한 모델이 강력한 효과를 발휘할 때도 있다.

<br>

#### 근사 추론(approximate inference)이란?
여기서 말하는 inference는 어떤 현상을 설명하기 위한 Model이 완성된 이후 이 Model을 근거로 다른 질문을 던진다는 뜻이다. Inference는 우리가 만든 Model로부터 직접적인 답(확률값)을 도출하는 경우에 사용된다. 

** 여기서 말하고자 하는 것은 생성된 모델에서 확률값을 바로 도출하는 프로세스가 필요 없다는 것 같다. GAN 구조에서는 확률값을 바로 도출하는것이 아니라 두 네트워크를 가지고 경쟁을 시켜 확률값을 만드는 것이기 때문이라고 해석해 보았다.😲

<br>

</details>

<br>

## 1. Introduction
딥러닝의 가능성은 자연적 이미지, 스피치를 포함한 오디오 파형, 자연언어 말뭉치의 상징과 같은 AI applications이 마주하는 데이터 종류의 확률분포를 대표하는 풍부하고 계층적인 모델을 발견하는 것이다. 더 나아가 딥러닝에 있어서 가장 큰 성공에 고차원의 풍부한 sensory input을 클래스 라벨에 매핑하는 discriminative model이 포함되어 있다. 이 큰 성공은 특정 부분에 잘 적용되는 기울기(gradient)를 가지는 piecewise linear units를 사용하는 역전파와 드롭아웃 알고리즘에 기반되어 있다.

deep generative 모델은 최대가능도함수(maximum likelihood) 예측과 관련 전략으로 인해 일어나는 많은 interactive한 확률적 계산들을 추론하는 데에 어려움이 있어 그 영향이 작았다. 또, 생성적 문맥(generative context)에서 piecewise linear unit의 이익을 높이는 것의 어려움 때문이라는 이유도 있다. 따라서 이 논문에서는 앞의 어려움들을 회피하는 새로운 생성모델 예측 프로시저를 제안하고 있다.


제안하는 adversarial net(경쟁 네트워크) framework에서는 생성 모델이 적대 관계의 적과 경쟁한다 : 식별자/구분자(D)는 샘플이 모델 분포(생성된 가짜)에서 나왔는지 아니면 데이터 분포(원본)에서 나왔는지를 결정하는 것을 학습한다. 비유를 해보자면, 생성 모델은 지폐 위조자에 비유되며 탐지없이 위조지폐를 생성한다(오로지 생성만 하는 네트워크 구조를 가짐). 경찰에 비유되는 식별모델은 생성자가 위조지폐를 생성하는 동안 위조지폐를 탐지한다. 이 게임에서의 경쟁은 위조지폐와 실제 지폐가 구분이 되지 않을 때 까지 진행되어 두 팀으로 하여금 각각의 방법을 향상시키게 한다. (경쟁하면서 두 네트워크의 성능이 향상됨)


이 프레임워크는 많은 종류의 모델과 최적화 알고리즘에 대한 특정 학습 알고리즘을 생성할 수 있다. 본 논문에서 생성 모델이 다층퍼셉트론을 통해 랜덤 노이즈를 통과하며 샘플을 생성하는 특별한 경우를 탐험하며 식별자 모델 또한 다층퍼셉트론 구조이다. 이 특별한 경우를 `adversary nets`라고 한다. 이 경우에 있어서 두 모델을 성공적인 `역전파`와 `드롭아웃 알고리즘만` 사용해 학습할 수 있으며 생성 모델에서 나온 샘플은 forward propagation 만 사용하게 된다. 근사 추론이나 마르코프 체인은 더이상 필요하지 않다. => 다른 복잡한 네트워크 필요 없이 오직 forward propagation/ back propagation / dropout algorithm으로 학습 가능하다는 것을 강조하고 있다. 구조가 더 심플해졌다. 그럼 시간 성능도 올랐을 듯


👉 GAN의 핵심은 각각의 역할을 가진 두 모델을 통해 적대적 학습을 하면서 ‘진짜 같은 가짜’를 생성해내는 능력을 높이는 것이라고 볼 수 있겠네요



<br>

## 2. Relative works

-- 생략 --

<br>

## 3. Adversarial nets

adversarial modeling framework는 모델들이 모두 다층퍼셉트론 구조이면 적용하기가 아주 간단하다. 생성자의 분포인 $p_g$에 있는 데이터 $x$를 학습하기 위해,
generator의 input으로 들어갈 noise variables $p_z(z)$에 대한 prior를 정의한다

그리고 $G$는 $\theta_g$를 파라미터로 가지고 있는 다층퍼셉트론에 의해 미분 가능한 함수라고 했을 때, 데이터 공간을 $G(z;\theta_g)$로 매핑해 표현할 수 있다. 

한편, 두번째 다층 퍼셉트론인 Discriminator은 $D(x;\theta_d)$로 나타내며 output은 single scalar(스칼라) 값이 나온다(확률값). $D(x)$는 $x$가 $p_g$(생성데이터분포-가짜)가 아닌 데이터 분포로부터 나올 확률을 나타낸다.


따라서, 이를 수식으로 정리하면 다음과 같은 value function $V(G,D)$에 대한 minimax problem을 푸는 것과 같아진다. (real data라고 판단하면 1 반환, fake data로 판단하면 0 반환)

<br>

$\displaystyle \min_{G} \max_{D}V(D,G)=E_{x\sim P_{data}(x)}[logD(x)]+ E_{x\sim p_{z}(z)}[log(1-D(G(z)))]]$

<br>

$E_{x\sim P_{data}(x)}[logD(x)]$ : 원본(real)data $x$를 discriminator 에 넣었을 때 나오는 결과를 log로 취했을 때 얻는 기댓값

$E_{x\sim p_{z}(z)}[log(1-D(G(z)))]]$ : 생성된(fake)data $z$를 generator에 넣었을 때 나오는 결과를 discriminator에 넣었을 때 그 결과를 $log(1-$결과$)$했을 때 얻는 기댓값

<br>

#### 📚 D 와 G 에 대해서 각각 살펴보자!

먼저 D가 아주 뛰어난 성능을 가져 가짜를 잘 판별한다고 가정하고 value function인 $V(D,G)$으로 살펴보자!

먼저 D가 판별하려는 데이터가 원본(real)data 에서 온 샘플일 경우, $D(x)$가 1이 되어 $logD(x)$은 0이 되어 사라지고 $G(z)$가 생성한 가짜 이미지를 구분할 수 있으므로 $D(G(z)) = 0$ 이 되어 $log(1-D(G(z)))$는 $log(1-0)=log1=0$이 되어 전체 식 $V(D,G) = 0$이 된다. 

`따라서 D가 유도할 수 있는 최댓값은 0 임을 확인할 수 있습니다.` 

<br>

이번에는 반대로 G가 D를 속일만큼 실제같은 가짜 이미지를 생성한다고 생각해보자. 그렇다면 $E_{x\sim P_{data}(x)}[logD(x)]$ 는 D의 식이므로 G성능과는 상관이 없으므로 보지 않아도 된다. 

대신 $E_{x\sim p_{z}(z)}[log(1-D(G(z)))]]$ 에서 G가 생성한 이미지는 D가 진짜라고 판단해야 하므로 1을 반환한다. 

그러므로 $D(G(z)) =1$ 가 되고, $log(1-1)=log0= -\infty$ 으로 마이너스 무한대가 나온다. 

정리하자면, `D는 V(D,G) 확률을 최대화시키기 위해 학습하고, G는 V(D,G)를 최소화시키는 방향으로 학습된다.`

<br>

하지만 실제론, 위의 minmax 방정식이 G가 학습하기 좋은 충분히 큰 기울기를 제공해주진 않는다. 학습 초반엔 G의 성능이 저조해 G가 만든 샘플이 실제와 너무 달라 D가 샘플을 너무 잘 구분해버린다. 이런 경우에는 $log(1-D(G(z)))$이 일정 이상 증가하지 못하고 빨리 수렴해버리는 saturate 현상이 발생한다. 

이를 해결하기 위한 방법으로 $log(1-D(G(z)))$ 를 최소화하는 식으로 G를 학습하기 보다는 <u>$log(D(G(z)))$</u> 를 최대화하는 식으로 G를 학습한다. 이렇게 하면 초기 학습에 훨씬 더 강력한 기울기를 제공한다.

이게 무슨 뜻이냐면.. log함수 특성상 $log(1-\alpha)$의 미분값이 유의미한 값이 될 수 없다. 따라서 $log(1-\alpha)$의 반대인 $log(\alpha)$의 max 값을 찾는 식을 유도한 것이다. 아래 그림으로 참고해보자!

<img src="https://user-images.githubusercontent.com/53431568/136425096-d9c6cb08-6c0a-46d2-bbaa-8d8fb6664af5.jpeg" width="450" height="400"/>
<br>



아래 Fig.1을 보면 

> 파란색 점선 : discriminative 분포
>  
> 검은색 점선 : real 데이터 분포     (진짜)
> 
> 초록색 실선 : generative 분포    (가짜)

<img width="689" alt="무제" src="https://user-images.githubusercontent.com/53431568/136160746-9dfb22c3-c1c5-48c9-937a-937923f64bde.png">



GAN의 학습과정을 이 그림을 통해 확인해보면, 

> (a): 학습초기에는 real과 fake의 분포가 전혀 다르며 D의 성능도 또한 저조 
> 
> (b): D가 (a)보다는 안정적으로 real과 fake를 판별해내고 있음을 확인. 즉, D의 성능이 올라감
>  
> (c): D의 학습이 어느정도 이루어지면, G는 실제 데이터의 분포를 모사하며 D가 구별하기 힘든 방향으로 학습
> 
> (d): 이 과정을 반복하면 real과 fake의 분포가 거의 비슷해져 구분할 수 없을 만큼 G가 학습되고, 마침내 D가 이 둘을 구분할 수 없게 되어 1/2의 확률로 계산


#### 이 과정을 통해 진짜와 가짜 이미지를 구별할 수 없을 만한 데이터를 G가 생성해내고 이것이 `GAN의 최종 결과`라고 볼 수 있다.


<br>

## 4. Theoretical Results

G는 $z \sim p_z$일때 얻어지는 샘플들의 확률분포 $G(z)$로써 $p_g$ 를 암묵적으로 정의한다. 그러므로 아래에서 살펴보는 알고리즘이 $p_{data}$에 대한 좋은 estimator(추정량)로 수렴되도록 하는 것이 논문의 목표이다.


해당 value function을 최적화해주는 방식은, 아래와 같이 Discriminator에 대한 weight를 먼저 업데이트 해주고, Generator에 대해서도 weight update를 번갈아가며 해준다. G의 경우 $logD(x^{(i)})$엔 관여하지 않으므로 식에서 제외되었다. 

![image](https://user-images.githubusercontent.com/53431568/136427992-59e665fc-3a24-4751-bf06-156deae29622.png)

1 epoch마다 아래의 과정을 반복한다.

아래를 k번 반복 (논문에서 k = 1로 실험)

1. m개의 노이즈 샘플을 $p_g(z)$로부터 샘플링
2. m개의 실제 데이터샘플을 $p_{data}(x)$로부터 샘플링
3. 경사상승법을 이용해 $V(G,D)$식 전체를 <u>최대화하도록</u> discriminator 파라미터 업데이트!


그 다음엔,

1. m개의 노이즈 샘플을 $p_g(z)$로부터 샘플링
2. $V(G, D)$에서 $log(1-D(G(z)))$를 <u>최소화하도록</u> 경사하강법을 이용해 generator 파라미터 업데이트!

<br>

논문에서 목표하는 것은, Generator를 통해 나오는 $G(z)$ 값이 $P_{data}$ 의 좋은 estimator가 되기를 기대하며, 해당 조건이 충족하면서 수렴하는지 확인해야 한다. 이에 대한 optimality를 이어서 확인해 봅시다!
 
<br>

### 4.1 Global Optimality of $p_g = p_{data}$

모든 가능한 G에 대해 최적의 discriminator D를 구해보자.

![image](https://user-images.githubusercontent.com/53431568/136430566-5baf32fe-5ef7-4c67-8a49-a0f1ada73d9a.png)

(G가 진짜 같은 가짜를 잘 생성했다면 $p_g(x) = p_{data}(x)$가 될 것이므로 $1 \over 2$일 때 G가 좋은 모델이다. 


증명은 다음과 같다. 모든 가능한 G에 대해 D를 위한 학습기준은 $V(G, D)$를 최대화 시키는 것이다. 따라서 아래의 식을 $D(x)$에 대해 편미분하고 결과값을 0이라고 두면 optimal한 D는 위의 **Proposition 1.** 와 같이 유도된다.

![image](https://user-images.githubusercontent.com/53431568/136431038-0bcefc94-7163-45e0-938d-55df1006c335.png)


증명 유도식은 아래와 같다.

![KakaoTalk_Photo_2021-10-09-13-40-58](https://user-images.githubusercontent.com/53431568/136644302-99ca43cc-61a6-4a67-8008-36676f8145b7.jpeg)


<br>

이렇게 얻은 optimal D를 원래의 목적함수 식에 대입하여 G에 대한 Virtual Training Criterion C(G)를 다음과 같이 유도할 수 있다.

![image](https://user-images.githubusercontent.com/53431568/136431397-2e0c00db-d879-45c3-876b-71f377d3f408.png)

위의 C(G)는 generator가 최소화하고자 하는 기준이 되며, 이것의 global minimum은 오직 $p_g(x) = p_{data}(x)$일때 달성된다. 그 점에서의 C(G)값은 $log{1 \over 2} + log{1 \over 2} = -log4$가 된다

위의 수식을 천천히 풀어서 증명해본 식은 아래를 참고하면 된다. 역시 직접 풀어보는게 이해에 빠른 것 같다.


![IMG_8E3C2FDB30B1-1](https://user-images.githubusercontent.com/53431568/136645248-8a9f0743-d9c5-4250-adda-8f0f5807bec8.jpeg)


=> 이 과정에서 나온 kl-divergence 나 js-divergence에 대한 자세한 내용은 [KL-divergence & JS-divergence & Maximum Likelihood Estimation와 개념정리](https://chaelin0722.github.io/gan/KL_divergence&JS_divergence) 이곳에서 다루었으니 참고!✌️✌️

<br>

### 4.2 Convergence of Algorithm 1

G와 D가 충분한 capacity를 가지며, algorithm 1의 각 스텝에서 discriminator가 주어진 G에 대해 최적점에 도달하는게 가능함과 동시에 
$p_g$ 가 위에서 제시한 criterion을 향상시키도록 업데이트 되는 한, $p_g$는 $p_{data}$ 에 수렴한다.


하지만, adversarial nets는 함수 $G(z;\theta)$를 통해 분포 $p_g$의 제한된 family만을 나타내게 되며, 논문에서 수행하는 최적화는 사실 $p_g$를 직접 최적화 하는게 아닌 $\theta_g$를 최적화하는 것이다. 그래서 앞서 한 증명이 적용되지 않으며 최적의 모델로의 수렴이 보장되지 않는다.

그러나 실무에서 MLP가 보여주는 훌륭한 퍼포먼스는 위와 같은 이론적 gurantee 에도 불구하고 사용할 수 있는 합리적인 모델이라는 사실을 말해준다. 

<br>

## 5. Experiments

> MNIST, TFD(Toronto Face Database), CIFAR-10에 대해 훈련.
> 
> generator에서는 ReLU, sigmoid activation을 섞어 사용.
>  
> discriminator에서는 maxout activation만을 사용. 
>  
>  discriminator 훈련시 드랍아웃 사용.

(저자들이 제안하는 프레임워크는 generator의 중간 레이어들에 dropout과 noise 추가를 이론적으로 허용하지만, 오직 generator의 최하단 레이어에만 노이즈를 추가했다고 함)

Gaussian Parzen window를 G에 의해 생성된 샘플들에 fitting하고 이렇게 추정된 분포 하에 얻어진 log-likelihood를 확인함으로써 $p_g$ 하에서 test set 데이터의 확률을 추정하였다. 이때 정규분포의 분산 파라미터는 교차검증을 통해 얻었다고 한다. 사실 이러한 방식으로 likelihood를 추정하는 것은 다소 분산이 크며 고차원 데이터 공간상에서 품질이 좋지 못하지만 최선의 method여서 이용했다고 한다. 

아래는 위의 방법으로 평가를 진행했을 때, GAN의 우수함을 보여주는 표이다.

<img width="794" alt="무제 2" src="https://user-images.githubusercontent.com/53431568/136645800-c09a1cf4-1d1f-43b9-9730-d427b6bb7e55.png">

학습 후 generator에 의해 생성된 이미지들은 아래와 같다.

<img width="698" alt="무제" src="https://user-images.githubusercontent.com/53431568/136645608-a659afa6-bb8a-4236-ac03-e6283eae045d.png">


## Advantages and disadvantages

#### disadvantages 

  - $p_g(x)$에 대한 명시적인 표현이 없다.
  - 훈련동안 D와 G의 균형을 잘 맞춰서 학습해야 한다. 
  - 최적해 수렴에 있어 이론적 보장이 부족하다.


#### advantages
  - Markov chain 과 inference 의 불필요
  - 모델에 다양한 함수들이 통합될 수 있음
  - generator network가 데이터로부터 직접적으로 업데이트 되지 않고 오직 discriminator 로 부터 흘러들어오는 gradient만을 이용해 학습될 수 있다.






<br>


#### 참고

<br>

[1] [마르코프체인](https://www.puzzledata.com/blog190423/)

[2] [approximate inference](https://kim-hjun.medium.com/approximate-inference%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80-35653b963546)

[3] [https://tobigs.gitbook.io/tobigs/deep-learning/computer-vision/gan-generative-adversarial-network](https://tobigs.gitbook.io/tobigs/deep-learning/computer-vision/gan-generative-adversarial-network)

[4] [https://velog.io/@changdaeoh/Generative-Adversarial-Nets-GAN](https://velog.io/@changdaeoh/Generative-Adversarial-Nets-GAN)

[5] [https://techy8855.tistory.com/m/13](https://techy8855.tistory.com/m/13)

[6] [최적해 유도식](https://di-bigdata-study.tistory.com/6)



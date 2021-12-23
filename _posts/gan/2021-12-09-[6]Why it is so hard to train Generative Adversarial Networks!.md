---
title:  "[GAN study] Why it is so hard to train Generative Adversarial Networks!"
excerpt: ""

categories:
  - gan

tags: [Deeplearning, study, gan]

classes: wide

use_math : true

last_modified_at: 2021-12-23T08:06:00-05:00

---

Gan 학습 로드맵 여섯 번째 시간! 벌써 시간이 이렇게..! 오랜만의 GAN 포스팅!! 😮😮

이번에는 GAN의 개념을 다시 한번 짚고, 학습이 어려운 이유에 대해서 개념정리를 하는 시간이었다. 

영어 원문 글은 [여기](https://jonathan-hui.medium.com/gan-why-it-is-so-hard-to-train-generative-advisory-networks-819a86b3750b)에서 참고하기~


<br>

GAN - gan 학습은 왜이렇게 어려울까!

![image](https://user-images.githubusercontent.com/53431568/147204227-b9e9a5b2-72de-4e73-99e9-2d810e5fd20a.png)


모네 그림을 그리는 것 보다 모네의 그림을 인식하는 것이 훨씬 쉽다. Generative model (데이터를 생성하는 것) 은 discriminative model(데이터를 처리하는 것)
과 비교하는 것보다 훨씬 어려운 일이다. 마찬가지로 GAN 을 학습하는 것은 어렵습니다. 이번 글은 GAN의 학습이 왜 이리 피해(학습이 잘 안되는지)가는지 조사해보았다.

이번 공부를 통해 많은 연구자들의 방향을 이끄는 몇 가지 근본적인 문제를 이해할 것입니다. 우리는 연구가 어디로 향할지 알 수 있도록 몇 가지 의견 불일치를 조사할 것입니다.
문제를 살펴보기 전, GAN 방정식 중 일부를 간단히 요약해 보겠습니다.

<br>

### GAN

GAN 은 정규 분포 또는 균일 분포를 사용하여 잡음 z 를 샘플링 하고 심층 네트워크 생성기 $G$ 를 사용하여 이미지 $x(x=G(z))$를 생성 합니다.

![image](https://user-images.githubusercontent.com/53431568/145149170-77bc65bb-3007-4d70-b1e6-effb20b51753.png)

GAN에서는 판별자 입력이 실제인지 생성되었는지 구별하기 위해 판별자를 추가합니다. 입력이 실제일 확률을 추정하기 위해 값 D(x)을 출력합니다 .

![image](https://user-images.githubusercontent.com/53431568/145149471-7347262c-0899-42e3-831d-3a7109bc6846.png)



<br>

### 목적 함수와 기울기

GAN은 다음과 같은 목적 함수를 갖는 minmax game 으로 정의됩니다.

$\displaystyle \min_{G} \max_{D}V(D,G)=E_{x\sim P_{data}(x)}[logD(x)]+ E_{x\sim p_{z}(z)}[log(1-D(G(z)))]]$


아래 다이어그램은 해당 기울기를 사용하여 판별자와 생성기를 훈련하는 방법을 요약합니다.

![image](https://user-images.githubusercontent.com/53431568/145149663-dfabfb09-9ae2-44fb-bac0-8dbad0e9a7a0.png)

<br>

## GAN 문제

많은 GAN 모델은 다음과 같은 주요 문제를 겪고 있다.

> Non-convergence : 모델 매개변수가 진동하고 불안정하며 수렴하지 않는다.
> Mode collapse : 제한된 종류의 샘플을 생성하는 생성자가 붕괴된다.
> Diminished gradient : 구분자(discriminator)가 매우 성공적이어서 생성자의 기울기가 감소되어버려 아무것도 학습하지 못한다.
> genarator과 discriminator의 균형이 맞지 않아 오버피팅을 야기하고 하이퍼파라미터의 값에 따라 매우 예민하다.

<br>

### Mode

실제 데이터 분포는 multimodal 하다. 예를 들어, MNIST 에서는 10개의 숫자 '0'~'9'를 모두 생성하기를 바랄 것이다. 하지만, 아래 샘플을 보면 각 GAN이 생성한 이미지가 다르다. 위의 GAN이 만든 이미지에서는 10가지의 숫자(10개 MODE)를 모두 생성하고 있지만,  두 번째 행의 다른 GAN은 한개의 숫자(모드)만을 생성하고 있다..(숫자 "6")

이는 생성기가 단 하나의 클래스에 대해서만 확률분포를 학습하고 있는 것이며, 데이터의 적은 모드만을 생성할 때, `mode collapse`(모드붕괴) 문제 라고 부른다. 

![image](https://user-images.githubusercontent.com/53431568/145149914-309ba7a3-bf97-45ee-abae-b742c2bcad1b.png)

따라서 임의의 시드 범위가 신경망의 서로 다른 부분을 골고루 통과하면서 0부터 9까지의 모든 가중치가 조화롭게 잘 작동하도록 훈련해야 다양한 확률분포를 동시에 배우는 것에 오류가 없을 것이다.

<br>

### Nash equilibrium (내쉬 균형)

GAN 은 zero-sum non-cooperative game에 기반하고 있다. 요약하자면, 한명이 이기면 다른 한명은 지는것! (제로섬 게임은 minimax 라고도 함) 

따라서 상대방은 자신의 행동을 최대화하기를 원하고 내 행동은 그것을 최소화하는 것이다. 게임 이론에서 GAN 모델은 판별자와 생성자가 내쉬 평형에 도달할 때 수렴하게 된다.

이것은 아래의 minimax 방정식의 최적의 포인트 이다.

$\displaystyle \min_{G} \max_{D}V(D,G)=E_{x\sim P_{data}(x)}[logD(x)]+ E_{x\sim p_{z}(z)}[log(1-D(G(z)))]]$

양쪽 모두 상대방을 약화시키려고 하기 때문에 내쉬 균형은 상대방이 무엇을 하든 한 플레이어가 자신의 행동을 바꾸지 않을 때 발생한다.

x 와 y 의 값을 각각 제어하는 두 명의 플레이어 A 와 B 가 있다고 해보자. 플레이어 A 는 xy 값을 최대화하려고 하고 B 는 최소화하려고 할 것이다. 

$\displaystyle \min_{B} \max_{A}V(D,G)=xy$


내쉬균형은 $x=y=0$ 이다. 이것은 상대방이 무엇을 하든 상관 없을때에만 나타나는 상태이다. 어떤 상대방의 행동이 게임의 결과에 아무런 영향을 미치지 않을 때에만 있는 현상이다.

이제 경사하강법을 사용하여 내쉬 균형을 쉽게 찾을 수 있는지 한번 봅시다! 기울기의 파라미터 $x$와 $y$는 value function $V$의 기울기에 기반해 업데이트 된다.

![image](https://user-images.githubusercontent.com/53431568/145150792-5d0295ff-6e67-4dfe-971e-484589d6152a.png)

$\alpha$는 학습률이며, 학습 iterations이 진행되는 동안 $x,y$ 와 $xy$를 그려본 결과는 아래와 같으며, 제시한 한 솔루션이 수렴하지 않았다는 것을 알 수 있습니다.

![image](https://user-images.githubusercontent.com/53431568/145150942-d3569496-9522-45d2-8395-bccf1e97de6d.png)


만약 학습률을 높이거나 모델의 학습을 더 오래하면 파라미터 $x,y$는 큰 스윙을 그리며 불안정한 모습을 보이는 것을 확인할 수 있다.

![image](https://user-images.githubusercontent.com/53431568/145150957-02e651ad-1c5b-4af4-b99d-e0e530d95cea.png)

위의 예시들은 몇몇의 비용함수(cost function)는 경사하강기법으로 수렵하지 않는다는 것을 보여주는 훌륭한 케이스이다. (특히 볼록하지 않은 상황에서 더욱 수렴하지 않음)
우리는 이 문제에 대해 직관적 방식으로 살펴볼 수 있다 : `상대방은 언제나 나의 행동에 대응하며, 이는 모델을 수렴하기 어렵게 만든다.`

> minmax 게임에서 경사하강법을 사용하는 것은 비용함수를 수렴하는데 도움이 되지 않을 수 있다.

<br>

블로그에 쓰인 글 만으로는 이해가 어려워서 `GAN 첫걸음` 이라는 책을 참고하여서 부연설명하고자 한다.

신경망을 훈련할 때 흔히 경사하강법(gradient descent)를 이용한다. 이 경우 손실 함수가 학습 파라미터의 조합을 찾아 오차를 최소화 한다. 

하지만 GAN의 작동원리는 단순한 신경망과 다르다. 생성자와 판별자는 서로 반대되는 목적으로 훈련이 되며 MIN-MAX 게임과 같은 방식이다. 따라서 경사 하강법이 MIN-MAX 원리의 GAN에 잘 맞을지 알아보아야 합니다.

간단한 목적함수를 가지고 설명하겠습니다.

$f = x \cdot y$ 가 있다고 할때, 한 플레이어는 x에 대한 통제력이 있고 목표값 f를 최대화하려고 한다. 다른 플레이어는 y에 대한 통제력이 있고 목표값 f 를 최소화 하려고 한다. 이 함수를 시각화 하면 아래와 같다. ( [함수 시각화된 사이트 여기!](https://www.math3d.org/wz85eIlP) )

![image](https://user-images.githubusercontent.com/53431568/147206377-22039697-2dcb-4e36-ac49-50c29a142e7c.png)


$f = x \cdot y$ 의 표면이 마치 말에 얹어진 안장모양이다. 한쪽 방향으로는 값이 커지다가 다시 떨어지고 다른 방향에서는 값이 떨어지다가 다시 올라가는 그런 모양이다. 

직관적으로 위의 함수를 보며 균형적인 해결책을 생각해보면 그나마 나은 것은 안장 그래프의 정중앙 $(x,y)=(0,0)$ 인 지점이다. 이 때, 한 플레이어가 $x$를 0으로 두면 두 번째 플레이어는 $y$를 어떤 값으로 설정해도 $f$ 에 영향을 미치지 못한다. 이때의 $f$값이 제일 나은 타협안이라고 볼 수 있는 것이다..!

슬슬 감이 온다..!

서로 이기려는 두 플레이어 모두에 대해 경사 하강법 계산을 해보면서 결과를 살펴보자!

목적 함수의 기울기에 따라 파라미터들이 조금씩 바뀌게 된다. 

$x -> x + lr*$ $\delta f \over \delta x$

$y -> y - lr*$ $\delta f \over \delta y$


(두 식의 부호가 반대인 이유는 $x$는 경사를 올라가는 과정에서 $f$를 최대화하려고하고 $y$ 는 경사를 내려오는 과정에서 $f$를 최소화 하려고 하기 때문이다.)

$f=xy$ 이므로 이 수식을 다시 짜면


$x \rightarrow x + lr * y$

$y \rightarrow y - lr * x$

이 두 값을 계속 갱신 규칙에 적용하게 되면 수렴하지 않고 진폭이 커지며 진동하는 현상을 볼 수 있다. 다른 초깃값을 넣어도 같은 현상이 나타나는데 이는 상당히 좋지 않은 결과입니다. 경사 하강법이 간단한 min-max 게임에 대해 좋은 해결을 찾지 못할 뿐더러 수렴의 반대인 발산으로 나타나기 때문입니다.


$(x,y)$ 궤적이 $(0,0)$을 중심으로 고정된 원을 그리면서 가까워지지도 않는 것이 수학적으로 가장 좋은 시나리오지만, 이는 업데이트 혹이 무한소처럼 굉장히 작을 때나 가능한 이야기이다. 유한한 업데이트 폭을 설정하고 업데이트를 계속하면 결국 발산하게 된다. 


경사 하강법은 일반적인 환경에서 global minima는 아니더라도 local minima 같은 값을 하나는 찾도록 보장이 되어있는 함수이다. 이 함수가 GAN에 대해 잘 작동하는 경우(유의미한 데이터가 있는 현실적인 GAN은 손실 표면이 훨신 복잡할 것이고 이것이 발산으로 빠질 가능성을 줄여줄 수 는 있다)도 있지만, 근본적으로 경사하강볍은 GAN에는 맞지 않는 방법이다. GAN에 딱 맞는 최적화 방법에 대해서는 현재도 활발하게 논의중에 있다. 


<br>


## Generative model with KL-Divergence
GAN의 수렴 문제를 이해하기 위해서는 KL-Divergence와 JS-divergence를 먼저 공부해야 한다. GAN 이전에, 많은 생성 모델들이 최대우도법(MLE)을 최대화시키기 위해 모델$\theta$를 만들었다. (ex.학습 데이터에 가장 잘 맞는 최적의 모델 파라미터를 찾는 것)

$\hat{\theta}=arg \max_{\theta}\prod^{N}_{i=1}p(x_i|\theta)$

이 방법은 KL-divergence를 최소화 시키는 것과 같다. KL-divergence는 확률분포 $q$ (측정된 분포)가 예상되는 확률분포 $p$ (실제 분포)로부터 어떻게 발산하는지 측정하는 방법이다. 

$D_{KL}(p\||q)=\int_xp(x)log p(x)\over{q(x)}dx$

KL-divergence는 비대칭적이다.

$D_{KL}(p\||q) \neq D_{KL}(q\||p)$

$KL(x)$은 0까지 떨어진다. 

## JS-Divergence

## Vanishing gradients in JS-Divergence

이 주제에 대해서는 자세히 설명한 포스팅이 있으니 [ KL-divergence & JS-divergence & Maximum Likelihood Estimation와 개념정리](https://chaelin0722.github.io/gan/KL_divergence&JS_divergence/) 참고하자!


## Unstable gradients

## Why mode collapse in GAN?

## Implicit Maximum Likelihood Estimation (IMLE)


--(작성중)--

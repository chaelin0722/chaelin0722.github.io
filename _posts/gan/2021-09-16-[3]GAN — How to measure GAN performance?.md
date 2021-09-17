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

GAN 시리즈의 일원으로 각기 다른 GAN 모델들의 결과를 어떻게 비교하는지 **Inception Score(IS)**과 **Frechet Inception Distance(FID)**을 토대로 살펴보고자 합니다. 

<br>

## Inception Score (IS)
IS는 GAN의 수행능력을 두가지 기준으로 측정합니다. 

> - 생성된 이미지들의 품질
> 
> - 생성된 이미지들의 다양성


엔트로피는 랜덤하다고 볼 수 있습니다. 만약 랜덤변수 x가 예측이 쉽다면 낮은 엔트로피를 갖고 있다고 말할 수 있습니다. 반대로, 예측하기 어렵다면 그것은 높은 엔트로피를 갖고있다는 말이 됩니다.

아래 그림에 두 가지 확률분포 $P(x)$ 가 있습니다. $P_2$는 $P_1$ 보다 높은 엔트로피를 가지고 있는데 그 이유는, $P_2$는 균일한분포를 갖고 있고 이 때문에 $x$가 무엇인지 예측하기 어려워지기 때문입니다.

![1_RdIYRsqXxRAKwcjtxg6_kw](https://user-images.githubusercontent.com/53431568/133730812-56324b4b-8750-4aa5-adf5-4c20d02c6bea.jpeg)


GAN 에서는 조건부 확률 $P(y\mid x)$ 의 예측이 쉬워야(낮은 엔트로피) 합니다. (예를 들어 이미지가 주어졌을 때, 그 객체의 타입을 쉽게 알아야 한다.)

따라서, 생성된 이미지들을 분류하고 $P(y\mid x)$($y$는 레이블값, $x$는 생성된 데이터)를 예측하기 위해 **Inception network**를 사용하는 것입니다. 이 방법을 사용해 이미지의 `품질`을 알 수 있습니다.


다음으로는 이미지의 `다양성`을 측정하는 방법에 대해 알아봅시다.

$P(y)$는 주변확률(marginal probability)로 아래 수식과 같이 계산됩니다.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $\int_z p(y\mid x= G(z))dz$

만약 생성된 이미지가 분산된다면, $y$를 위한 데이터 분포는 균일(높은 엔트로피)해야 합니다. 


아래 그림은 이러한 개념을 시각화해서 보여주고 있습니다.

![1_ZkN2nkdtsqvhtKB3QAbfCQ](https://user-images.githubusercontent.com/53431568/133731725-f669e3f9-5409-46cd-87ba-387822f22931.png)



위에서 배운 품질 측정법과 다양성 측정법의 두 기준을 합쳐 `KL-divergence`를 계산하고 아래 수식을 사용해 IS를 계산합니다.

$IS(G) = exp(E_{x\sim p_a}D_{KL}(p(y\mid X) \parallel p(y)))$

단, IS는 클래스 하나당 딱 하나의 이미지만 생성하는 경우 성능을 잘못 대표한다는 단점이 있다. $p(y)$ 는 다양성이 낮더라도 여전히 균일할 것이다. 

<br>

## Fréchet Inception Distance (FID)
FID는 Inception network를 이용해 하나의 중간 레이어에서 특징들을 추출합니다. 그 다음엔 추출된 특징에서 평균 $\mu$와 $\sum$을 갖는 다변수 가우시안 분포를 이용해 데이터 분포를 모델링합니다. 실제 이미지 $x$와 생성된 이미지 $g$ 사이의 FID는 다음과 같이 계산됩니다.

$FID(x,g) = \| \mu_x- \mu_g \|_2^2 + Tr(\sum_x+\sum_g-2(\sum_x\sum_g)^{\frac{1}{2}})$

여기서 $Tr$은 모든 대각선 요소를 합산한다는 뜻입니다.

> 낮은 FID 값은 더 나은 이미지 품질과 다양성을 의미합니다.

FID는 모드 축소에 민감한데, 아래 그림과 같이 시뮬레이션된 누락 모드에 따라 거리가 증가하는 것을 볼 수 있습니다.


![1_8PzOnrzIeuM0E1unrFKLfg](https://user-images.githubusercontent.com/53431568/133784864-fab4e3f7-09fd-46d4-813c-69b8fb60f6e1.png)

FID 는 IS 보다 노이즈에 강합니다. 만약 모델이 한 클래스당 하나의 이미지만 생성한다면 그 거리는 매우 클 것입니다. 따라서 FID는 이미지 다양성에 있어서 좀 더 나은 측정방법입니다. FID 는 높은 편향(high bias)를 갖지만 분산(low diverse)은 낮습니다. 훈련데이터셋과 테스트 데이터셋 간의 FID를 계산하면 둘 다 실제 이미지이므로 FID가 0이 될 것을 예상하게 됩니다. 하지만 훈련 샘플의 다른 배치로 테스트를 실행하면 예상한 것과 달리 0 FID 가 나오지 않습니다.

![1_D-XiZT9FdCWaA9jnyomsVw](https://user-images.githubusercontent.com/53431568/133785691-72fe89d7-5059-4736-ad59-11126ae355b2.png)

또한, FID 와 IS 는 모두 특징추출(특징의 유무)를 기반으로 합니다. 공간적 관계가 유지되지 않는다면, 과연 생성자(Generator)는 같은 점수를 내게 될까요? (아니요~ ㅎㅎ 공간특성이 유지 되지 않는다면.. 특징이 좀 달라지지 않을까요? 그렇다면 점수는 달라질 것 같습니다.)

![1_o0lGo8Jnfw_gmXQd9sUCmg](https://user-images.githubusercontent.com/53431568/133785779-291f0659-e5bc-4391-9a6e-971da967302b.jpeg)

<br>

## Precision, Recall and F1 Score (정밀도, 재현률, F1 점수)

만약, 생성된 이미지가 실제 이미지와 평균적으로 비슷하다면, 정밀도는 높을 것입니다. 높은 재현률은 생성자가 훈련 데이터셋에서 찾은 모든 샘플이을 생성할 수 있음을 의미합니다. F1 점수는 정밀도와 재현률의 조화 평균을 나타낸다.

Google Brain 연구 논문인 "Are GANs created equal"에서는 다양한 GAN 모델의 정밀도와 재현율을 측정하기 위해 삼각형 데이터 세트를 사용한 장난감 실험이 생성되었습니다.

![1_0qc9oLuZxjeAqt4JBzPw2A](https://user-images.githubusercontent.com/53431568/133786990-e39b5bea-b3a7-4f53-99e0-419f2e167fcd.png)


이 토이 데이터셋은 각기 다른 GAN 모델들의 성능을 측정할 수 있으며 이것을 사용해 다른 비용함수의 장점을 측정할 수 있습니다. 예를 들면, 새로운 함수가 커버리지가 좋은 고퀄리티의 삼각형을 잘 생성할 수 있을까요? 

<br>

GAN의 성능을 측정하는 지표에 대해서 알아보았는데요, 각 모델의 측정 지표를 고안하는 사람들도 참 똑똑하다는 생각을 하며.. 오늘도 머리를 싸매는 하루가 되었습니다. ㅎㅎ

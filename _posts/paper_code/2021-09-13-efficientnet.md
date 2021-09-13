---
title:  "[논문정리📃] Rethinking Model Scaling for Convolutional Neural Networks"
excerpt: "Week8 -EfficientNet-"

categories:
  - paperReview
tags: [CNN, paperReview]
use_math: true

last_modified_at: 2021-09-13T08:06:00-05:00
classes: wide
---

## Rethinking Model Scaling for Convolutional Neural Networks
#### - EfficientNet - 

[논문원본](https://arxiv.org/pdf/1905.11946.pdf)😙

<br>

## 0. Abstract
CNN은 한정된 자원에서 개발되어왔으며 더 많은 자원이 가능해지면 더 좋은 성능을 위해 크기를 키워나가는 방향으로 발전되어왔다.

이 논문에서는 model scaling에 대해 이야기 하며 network의 depth, width, resolution 사이의 관계에 대한 균형을 맞춰야 더 나은 성능을 보인다는 것을 보여준다.

depth, width, resolution의 차원들을 간단하면서도 높은 효율을 보이는 새로운 scaling방법인 `'compound coefficient'`를 제안하며, MobileNet과 ResNet에 이 방법을 적용해 효율성을 테스트한다.

더 나아가, 'Neural Architecture Search(NAS)'를 사용해 baseline network를 설계했으며 이 baseline network를 scale up 해 가족 모델인 EfficientNet을 설계하였다.  (NAS는 강화학습을 기반으로 최적의 network를 찾는 방법인데 이것에 대한 자세한 설명은 => [여기](https://chaelin0722.github.io/paperreview/nasnet/))

특히, EfficientNet-B7은 ImageNet dataset에 대해 84.4%(top-1 acc)/97.1%(top-5 acc)를 얻었을 정도로 매우 좋은 성능을 보이며 이는 convNet보다 8.4배 작으며 6.1배 빠른 성능을 가진다.

또한, CIFAR-100(91.7%), Flowers(98.8%) 와 다른 3개의 학습 데이터셋에 전이학습을 시켜도 SOTA 성능을 보인다.

<br>

## 1. Introduction

Scaling up 방법은 ConvNet의 성능향상에 자주 사용되는 방법입니다. ResNet의 경우에도 ResNet-18에서 ResNet-200으로 layer수를 늘림으써 성능이 향상되고, 최근에는 GPipe가 baseline model을 4배 scaling up 하여 ImageNet에 대해 84.3%(top-1 acc)을 얻었다. 하지만 ConvNet의 효율적인 scaling up을 하는 과정에 대해서는 여전히 잘 알려진 바가 없다.

<br>


<details markdown="1">
<summary>GPipe🔎</summary>

### GPipe 란?

GPipe는 Google Brain에서 발표한 학습기법으로 메모리를 많이 차지하는 큰 모델을 효율적으로 학습시키는데 유용하다. Google이 공개한 논문의 벤치마크에 따르면 기준보다 8배 많은 장치(TPU)로 25배 큰 모델을 학습시킬 수 있고, 기준보다 4배 많은 장치에서 3.5배 빨리 학습시킬 수 있다고 한다.

Google은 GPipe를 이용해 5.6억개의 파라미터를 가지는 AmoebaNet-B 모델을 학습시켰다. 이 모델은 ImageNet에서 84.3%(top-1 acc)을 얻고 97%(top-5 acc)로 SOTA를 기록했다.

Gpipe는 Pipeline Parallelism과 Checkpointing, 이 두 방법으로 가능한 큰 모델을 학습시킨다.

#### - Pipeline Parallelism

GPipe는 모델을 여러 파티션으로 나눠 각각 서로 다른 장치에 배치해 더 많은 메모리를 사용할 수 있게 한다. 그리고 여러 파티션이 최재한 병렬적으로 작동할 수 있도록, 모델에 입력되는 미니배치를 여러 마이크로배치로 나눠 모델에 흘려보낸다.

#### - Checkpointing

각 파티션엔 체크포인트를 만들어 메모리 가용량을 극대화한다. 순전파(forward propagation)때 파티션 경계의 입출력만 기억하고 내부의 hidden layer는 휘발시킨다. 휘발된 hidden layer은 역전파(back propagation) 때 다시 계산된다.

<br>

</details>

<br>

그 동안의 ConvNet의 scaling 방법에 대해서는 depth, width, resolution 이 셋 중 하나의 dimension만을 조정하는 방식으로 사용되어왔다. 이 중 두 가지 이상을 조정하는 방법도 고려될 수 있지만, 미세하게 조정해줘야 하는 작업들이 많이 필요하며 최적의 결과를 잘 나타내지 못했다.


따라서 이 논문에서는 간단하면서 효율적인 `'compound scaling method'`를 제안하며 이 방법의 핵심은 `network의 width, depth, resolution 사이의 균형을 맞추는 것은 성능향상에 매우 중요`하며 이들간의 군형은 간단한 상수의 비(constant ratio)로 구해질 수 있다는 것이다.


예를 들어 우리가 $2^N$배 큰 모델을 디자인하고 싶다면 baseline network의 depth를 단순히 $\alpha^N$, width를 $\beta^N$, image size를 $\gamma^N$해서 작은 grid search를 통해 위의 조건을 만족하는 $\alpha, \beta, \gamma$값을 찾게 된다.

<br>

아래 이미지와 같이 적은 parameter수로 엄청난 성능을 낼 수 있다. (significantly out-perform other convnets 라고 써져있다. 압도적인 parameter 적은 수로 엄청난 성능을 낸다...라고 강조함)

<img width="492" alt="무제" src="https://user-images.githubusercontent.com/53431568/133041668-aca3a27a-64f4-4384-8c27-e2029878910b.png">


근데 진짜.. 대단한 성능인 것 같다.


<br>

다음은 3가지 방법의 scale up 을 나타내는 그림이다. 

<img width="731" alt="무제 2" src="https://user-images.githubusercontent.com/53431568/133042458-26766c62-2917-4006-b2ba-af92a6aaba69.png">

(a)의 baseline network를 토대로 (b)~(d) 는 width, depth, resolution을 scaling up 했을 때의 구조를 나타낸다. 

결국 마지막 (e)의 compound scaling 을 잘 하는 것이 이 논문의 목표이다. 



논문에서는 MobileNet과 ResNet을 이용해 이를 확인하고 있으며, Model scaling에 의한 성능 향상은 baseline network에 매우 의존적이기 때문에, baseline network를 설정하는데 있어서 `neural architecture search(NAS)`를 사용한다.

<br>

## 2. Compound Model Scaling

이번 장에서는 scaling problem에 대한 다른 접근법들을 살펴보고 새로운 scaling method를 제안한다.

### 2.1. Problem Formulation

하나의 ConvNet Layer $i$는 $Y_i = F_i(X_i)$로 정의 된다. 

- $F_i$는 연산자, $Y_i$는 output tensor, $X_i$는 input tensor을 의미

- $X_i$의 크기는 $<H_i, W_i, C_i>$이며, 각각 $H_i, W_i$ 는 공간적 차원 $C_i$는 channel 차원을 의미한다. 



하나의 convNet$N$은 $N = F_k\bigodot... \bigodot F_2 \bigodot F_1(X_1)$ 로 표시한다.

하지만 실제로 ConvNet layers 는 여러 stage로 나눠지며 각각의 stage는 같은 구조를 공유한다. (예를 들어 ResNet의 경우 5개의 stage가 있고 down-sampling을 수행하는 맨 처음 레이어를 제외하고는 각 stage의 모든 레이어들은 같은 convolutional 타입을 가진다.)

따라서 우리는 ConvNet을 다음과 같이 정의할 수 있다.

$N = \bigodot\limits_{i=1...s} F_i^{L_i}(X_{<H_i, W_i, C_i>})$ 


$F_i^{L_i}$ 는 $F_i$ 레이어가 $i$ stage에서 $L_i$번 반복, $<H_i, W_i, C_i>$ 는 레이어 $i$의 input tensor X 값을 나타낸다. 

우리의 목표는 최적화 문제로 귀결되는 어떤 제한된 자원이 주어져도 모델 정확도를 최대화 하는 것이다.
이제 논문이 얻고자 하는 최종 목표를 간단한 식으로 정리하면 다음과 같다.

$\max\limits_{d,w,r}\,\,\,\, Accuracy(N(d,w,r))$

$$s.t. N(d,w,r) = \bigodot\limits_{i=1...s}\hat{F\,}_i^{d\cdot\hat{L}_i} (X_{r\cdot\hat{H},r\cdot\hat{W},w\cdot\hat{C}_i})$$

$Memory(N) \leq target \, memory$

$FLOPS(N) \leq target\,flops$

$w,d,r$ 은 network 의 width, depth, resolution을 scaling 하기 위한 상수값이며, $\hat{F}_i,\hat{L}_i,\hat{H}_i,\hat{W}_i,\hat{C}_i$ 는 baseline network에 미리 정해져 있는 파라미터 값이다. 

즉 ,

> 변동되는 상수값 :  $w,d,r$
> 
> 고정값 : $\hat{F}_i,\hat{L}_i,\hat{H}_i,\hat{W}_i,\hat{C}_i$


<br>

### 2.2. Scaling Dimension

중요한 문제는, 최적의 $d,w,r$ coefficient 들은 서로 연관되어있다는 것과 서로 다른 제한적 자원에 놓여있다는 것이다. 따라서 널리 사용된 ConvNet들은 다음의 dimension 중 하나만 선택해 scaling 해왔다. 

1. Depth ($d$)       -> 깊은 레이어

2. Width ($w$)       -> channel의 수

3. Resolution ($r$)  -> input image size


아래 그래프는 baseline model은 width, depth, resolution coefficients 에 따라 scaling up 한 결과를 보여준다.


<img width="938" alt="무제 3" src="https://user-images.githubusercontent.com/53431568/133053805-96f5e915-1e36-41bf-ae46-2eb82bfb8eb8.png">

각각 성능이 좋아짐을 알 수 있지만 acc가 약 80%가 되는 시점에서 급하게 saturate 되는 것을 볼 수 있다.

`네트워크의 depth, width, resolution 의 차원 중 한가지 만을 scaling up 하는 것은 성능을 향상시키지만 더 큰 모델에 있어서는 정확도가 줄어든다.`

<br>

###  2.3 Compound Scaling

직관적으로 생각해보면  `각 요소들은 의존적`이다. 생각해 보자, input image(resolution)가 커진다면 network가 더 넓은 영역을 수용할 수 있는 receptive field를 확보(depth)해야 하며, 더욱 많은 channel(width)을 통해 정제된 pattern을 추출해야 할 것이다. 

아래 그래프는 depth 와 resolution 크기를 고정한 채로 width 값을 변화시키면서 테스트한 결과이다.

동일한 FLOPS에서 width/depth/resolution 조합을 찾아내야 한다. 

<img width="393" alt="무제 4" src="https://user-images.githubusercontent.com/53431568/133053882-7cce71be-9dd2-4716-909e-f16340dd1beb.png">


이 결과를 통해 `ConvNet scaling을 하는 동안 더 나은 성능과 효율성을 추구하기 위해서는 network의 모든 dimensions의 균형을 잡는 것이 중요하다`는 것을 알 수 있다. 


이 논문에서 제안하는 새로운 방식의 compound scaling method는 다음과 같다. compound coefficient 인 $\phi$ 로 network의 width, depth, resolution을 scale한다.

#### compound scaling 방법에 사용되는 notation

depth: $d\,=\,\alpha^\phi$

width: $w\,=\,\beta^\phi$

resolution: $r\,=\,\gamma^\phi$

$s.t. \alpha\cdot \beta^2 \approx 2$

$\alpha \geq 1, \beta \geq 1, \gamma \geq 1$



각 $\alpha, \beta, \gamma$는 small grid search 에 의해 정해질 변수들이며 $\phi$는 얼마나 많은 resource를 사용할지에 대해 사용자가 정할 coefficient 이다. 

Convolution operation의 FLOPS는 $d, w^2, r^2$ 각각에 대해 비례해 증감하는 성질을 갖고 있다. 여기서 width와 resolution에 제곱이 들어간 이유는 depth는 2배 키워주면 FLOPS도 비례해서 2배 증가하지만 width 와 resolution은 가로 세로가 각각 곱해져 제곱 배 증가하기 때문이다.


위의 식에서 ``$\alpha\cdot \beta^2 \approx 2$`` 에서도 알 수 있듯 값을 2로 제한시켰으므로 총 FLOPS는 대략 $2^\phi$에 비례해 증감한다.

<br>

<details markdown="1">
<summary>grid search🔎</summary>

### grid search 란?

Grid search(격자 탐색)은 모델 하이퍼파라미터에 넣을 수 있는 값들을 순차적으로 입력한뒤에 가장 높은 성능을 보이는 하이퍼파라미터들을 찾는 탐색 방법이다. 

즉, 모델을 학습하기 위한 여러 방법이 있는데 이 중 어떤 특정 방법이 이 모델에 적합한지 판단한다.

* 하이퍼파라미터(hyper parameter, 초매개변수)
모델 생성시 사용자가 직접 설정하는 변수로, 만약 랜덤 포레스트 모델을 만든다고 하면 트리의 개수를 몇개까지 할 것인지, 트리의 깊이, 딥러닝 모델에서는 layer의 갯수, 학습횟수 등이 이에 해당한다. 반면, 파라미터(parameter)는 학습 과정에서 생성되는 변수이다. 

<br>
</details>

<br>


## 3. EfficientNet 구조

위의 실험들을 통해 3가지 scaling factor를 동시에 고려하는 것이 좋다는 것을 입증하였다. 

이제, 최적의 비율을 찾아 실제 모델에 적용해 다른 모델들과 성능을 비교하는 과정을 설명하겠다.

이 논문에서는 모델(F)를 고정하고 depth, width, resolution 3가지를 조절하는 방법을 제안하는데 고정하는 모델 (F)를 좋은 모델로 선정하는 것이 아주 중요하다. 아무리 scaling factor을 조절해도 초기 모델 자체의 성능이 낮다면 임계 성능도 낮기 때문이다. 이 논문에서는 MnasNet과 거의 동일한 search space하에서 AutoML을 통해 모델을 탐색하였고, 이 과정을 통해 찾은 작은 모델을 EfficientNet-B0 라고 한다. 


<img width="630" alt="무제 5" src="https://user-images.githubusercontent.com/53431568/133077869-28060f5a-c2e0-4444-b3b4-c8ad0da13c69.png">


모델 구조는 MnasNet과 거의 유사하며 위의 표와 같은 구조로 구성되어있다.

EfficientNet의 $\alpha, \beta, \gamma$ 값은 간단한 grid search로 구해지며, 본 논문에서는

> $\alpha = 1.2$
> 
> $\beta = 1.1$
> 
> $\gamma = 1.15$

를 사용하고 있으며 이 세 값들은 고정한 뒤 $\phi$ 값을 키우며 모델 사이즈를 키우고 있다.


<br>


## 4. Experiments

기존 사람이 디자인한 ConvNet, AutoML을 통해 찾은 ConvNet들과 비교한 결과는 아래 표에 나와있다. 

<img width="820" alt="무제 6" src="https://user-images.githubusercontent.com/53431568/133079384-a9bce793-ca4a-47fb-aff5-7cdbc8e21a71.png">


기존 ConvNet들에 비해 비슷한 정확도를 보이며 parameter 수와 FLOPS 수를 많이 절약할 수 있는 것을 알 수 있다. 또, 기존에 ImageNet 데이터셋 에서 가장 높은 정확도를 달성했던 GPipe보다 더 높은 정확도를 달성하는 것을 확인할 수 있다. 


#### 그 외 다양한 실험 결과들

<img width="820" alt="무제 6" src="https://user-images.githubusercontent.com/53431568/133079438-d8ea79a6-5640-43ac-8631-90b7cee722b6.png">

위의 이미지는 모델이 이미지를 분류할 때 이미지의 어느 영역에 집중했는지 확인할 수 있는 Class Activation Map (CAM) 을 뽑은 결과인데, 3개의 scaling factor을 각각 고려할 때 보다 동시해 고려하였을 때 더 정교한 CAM을 얻을 수 있다는 것을 보여준다.


<img width="544" alt="무제 8" src="https://user-images.githubusercontent.com/53431568/133080185-31f2fc6d-f814-48fe-b22b-e042d316750d.png">

위의 표는 Fig.7 에서 활용된 실험 network depth, width, resolution 조건별 FLOPS와 Top-1 accuracy를 나타내는 표이다. compound scaling을 적용한 경우가 비슷한 FLOPS임에도 더 좋은 성능을 보여줌을 알 수 있다.




### 느낀점..

NAS를 효율적으로 발전시킨 논문인 것에 의의가 있다고 생각한다. 









#### 참고

  [1] [https://bellzero.tistory.com/17](https://bellzero.tistory.com/17)

  [2] [https://norman3.github.io/papers/docs/efficient_net.html](https://norman3.github.io/papers/docs/efficient_net.html)
  
  [3] []()

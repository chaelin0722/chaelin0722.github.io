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


GPipe에 대해 자세히 알고싶다면 '더보기🔎'를 참고!

<details markdown="1">
<summary>더보기🔎</summary>

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

직관적으로 생각해보면 compound scale up을 잘 만들기 위한 손쉬운 방법은 depth/width/resolution을 모두 크게 키우는 것이다. input image가 커진다면 network가 더 넓은 영역을 수용할 수 있는 receptive field를 확보해야 하며, 더욱 많은 channel을 통해 정제된 pattern을 추출해야하기 때문이다. 




논문에서는 MobileNet과 ResNet을 이용해 이를 확인하고 있으며, Model scaling에 의한 성능 향상은 baseline network에 매우 의존적이기 때문에, baseline network를 설정하는데 있어서 `neural architecture search(NAS)`를 사용한다.

<br>

## 2. Compound Model Scaling













#### 참고

  [1] [https://bellzero.tistory.com/17](https://bellzero.tistory.com/17)

  [2] [https://norman3.github.io/papers/docs/efficient_net.html](https://norman3.github.io/papers/docs/efficient_net.html)
  
  [3] []()

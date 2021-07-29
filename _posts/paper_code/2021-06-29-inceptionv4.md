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
#### - Inception-v4, Inception-ResNet-v2 -

[논문원본](https://arxiv.org/abs/1602.07261)😙

이번 논문은 inveption-v2와 resnet-v2의 네트워크 구조들을 살펴볼 수 있는 내용이었다. 네트워크 구조와 residual connection이라는 개념에 대한 핵심만 짚었기 때문에 많은 시간이 소요되는 논문은 아니었다. 


Inception-v4 코드구현 페이지. => [Inception-v4](https://chaelin0722.github.io/deeplearning/cnn/code/inception_v4_code/)


## 0. 요약

residual connection으로 인해 Inception networks의 학습속도가 가속화 된다.

## 1. Introduction

이 논문에서는 residual connection(잔여연결)과 가장최근 개정된 버전의 Inception구조에 대해 설명한다.

- residual connections : 매우 깊은 구조를 학습하기 위한 것.

- inception 구조 : inception 네트워크가 아주 깊어지면서 inception 구조의 연속 스테이지가 residual connection으로 대체되었다. 
  > 이것으로 Inception은 연산 효율성을 유지하면서 residual(잔여적)접근의 장점을 취할 수 있게되었다. 즉 연산량은 그대로이면서도 residual connection이 주는 장점을 더하게 되었다.
  > 또한, inception 신경망을 좀 더 효과적으로 넓고 깊게 하기 위해 나온 것이 inception-v4이다. inception-v4는 inception-v3보다 단순하고 획일화된 구조와 더 많은 inception module을 사용한다. 


이 논문에서는 Inception-v4와 Inception-ResNet 두 가지 방법을 설명하고 있다. 

  - Inception-ResNet은 Inception-v4에 residual connection을 결합한 구조이며 학습 속도가 빠르다.

<br>

## 2. Related Work

Convolutional networks는 규모가 큰 이미지 인식에서 매우 유망해지고 있다. 그 다음으로 중요하게 주목된 것이 Network-In-Network 구조였다. 

residual connection은 He et al. 에 의해 소개되었으며 , **residual connection**은..

  (1) 이미지 인식과 특히 객체인식에 효과적이라는 것을 이론적으로, 실질적인 증거로 확신되고있다. 

  (2) residual connections은 매우 깊은 컨볼루션 모델을 학습하는데 필요하다고 주장한다. 
  
  (3) 학습속도를 매우 빠르게 향상시킨다.
  
  
<br> 

## 3. Architecural Choices

### 1. Pure Inception blocks
예전 Inception 모델은 분할 학습되곤 했다. (메모리에 전체 모델이 맞춰지기 위해서 각각의 복제품이 여러개의 하위 네트워크(sub-networks)로 분할되어 학습되었다는 뜻이다.) 하지만, inception 구조는 조율이 가능하다. 이 말은 즉, fully-trained network의 퀄리티에 영향을 미치지 않는 다양한 레이어들에 많은 필터들을 변화시킬 수 있는 여러 가능성이 있다는 것이다.

학습 속도를 일반화시키기 위해서 다양한 모델 하위 네트워크들 간의 연산의 균형을 위해 우리는 레이어 사이즈를 조율하곤 했다. 반대로 tensorflow의 등장으로 가장 최근의 모델은 복제품을 분할하지 않고 학습이 가능해졌다. 

이것이 어떻게 가능한지 자세히 말하자면! **기울기 연산과 구조화하는 연산에 어떤 텐서가 사용되는지 고려하여 특정 텐서들을 줄임으로써 backpropagation에 의해 사용되는 메모리의 최적화**가 가능해 진 것이다.


Inception v4를 위한 새로운 실험은 불필요한 짐(예전에 사용하던 네트워크 구조를 뜻하는 것 같다, 나머지 네트워크는 놔둔 채 네트워크 컴포넌트를 다양하게 분리하는 제한적인 실험방식)을 배제하고 각 그리드 사이즈를 위해 inception block에 대해 획일화된 선택을 하였다.

<br>

다음은 Inception-v4의 전체적인구조와 부분적 구조들이다.

![inceptionv4](https://user-images.githubusercontent.com/53431568/123669887-8dffee00-d877-11eb-86a3-72f9d64064b8.JPG)

Inception-v4는 이전 버전에서의 단점을 개선하고, inception block을 균일하게 획일화 하였다.


다음은 Inception-Resnet의 전체적 구조와 부분적 구조들이다.

![inceptionresnet](https://user-images.githubusercontent.com/53431568/123669882-8ccec100-d877-11eb-942a-16f3c01b6cad.JPG)

Inception-Resnet은 Inception network와 residual block을 결합한 형태이다. Inception-ResNet은 v1버전과 v2버전이 있으며 이 둘의 전체 구조는 같지만, 각 모듈에 차이가 있다!

`stem과 각 inception module에서 사용하는 filter수가 다르다`는 것이 그 차이이다. 

<br><br>

### 2. Residual Inception Blocks
inception 네트워크의 residual 버전에서는 original incetpion 보다 가벼운 inception block을 사용한다.

각 inception block 다음에는 input값의 깊이에 맞춰지기 위한 addition전에 필터의 차원을 커지게 하는 filter-expansion layer (1 x 1 convolution without activation) 이 뒤따라온다. 이것은 inception block으로 인해 차원이 줄어든 것을 보충하기 위해 필요하다.

우리는 다양한 Inception의 residual version을 시도하였고 다음 두 가지 사실을 알아내었다.

바로, inception-resnet-v1은 inception-v3와 연산량이 비슷하고, inception-resnet-v2는 inception-v4와 연상량이 비슷하다는 것이다.


우리의 residual 과 non-residual Inception variants 의 작은 기술적인 차이는 Inception-ResNet에 있다. 우리는 batch-normalization을 오직 전통적 레이어의 위에 사용하였고 summation의 위에는 사용하지 않았다.

batch-normalization을 통해 장점을 기대하는 것은 매우 합리적이게 보이나 우리는 각 모델의 복제품을 하나의 GPU에 학습시키고 싶었다. 큰 활성화 사이즈를 가진 레이어의 메모리 footprint 는 적절치 못한 양의 GPU 메모리를 소모한다는 것을 알 수 있었다. 따라서 batch-normalization을 이 레이어들의 상위에서 제거함으로써 Inception blocks의 전반적 숫자를 늘릴 수 있었다.  

 
<br> 

#### Inception-v4를 코드로 구현한것을 정리한 페이지이다. =>  [Inception-v4](https://chaelin0722.github.io/deeplearning/cnn/code/inception_v4_code/)

<br>

### 참고
[1] [https://deep-learning-study.tistory.com/525](https://deep-learning-study.tistory.com/525)


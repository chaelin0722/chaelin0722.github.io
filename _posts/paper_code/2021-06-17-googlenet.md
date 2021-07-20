---
title:  "[논문정리📃] Going Deeper with Convolutions"
excerpt: "Week3 -GoogLeNet-"

categories:
  - CNN
  - paperReview
tags: [CNN, paperReview]

use_math: true

last_modified_at: 2021-06-23T08:06:00-05:00
classes: wide
---

## Going deeper with convolutions
#### -GoogLeNet(Inception-v1)-

[논문원본](https://arxiv.org/abs/1409.4842)😙

GoogLeNet 코드구현 페이지. => [GoogLeNet](https://chaelin0722.github.io/deeplearning/cnn/code/googlenet_code/)

## 0. 요약
- “inception” 이라는 deep convolution neural network를 제안한다. 이 inception의 특징은 네트워크 안에서 컴퓨터 리소스의 활용을 향상시킨다. 이것으로 계산 비용은 유지하면서 네트워크의 깊이와 넓이를 증가시킨다.
- ILSVRC 2014에 제출 된 모델은 `22-layer로 이루어진 GoogLeNet`으로, classification과 detection 분야에서 그 성능을 평가했다.

## 1. 개요
- 최근 딥러닝과 convolutional networks의 발전으로 이미지 인식과 물체인식의 성능이 엄청나게 향상되었으며 이것은 (1)새로운 아이디어나 (2)새로운 알고리즘, (3)개선된 네트워크 구조로부터 얻어진 결과들이다. 

- inception은 NIN논문과 함께, “we need to go deeper”라는 유명한 인터넷 밈에서 유래한 이름이다. 이 논문에서는 “deep”이라는 단어를 (1) Inception module이라는 새로운 구조의 도입 이라는 의미와 (2) Network의 depth가 늘어난 것 이라는 의미로 2가지의 뜻으로 사용한다.
 
- 일반적으로 Inception 모델은 NIN의 논리로부터 영감을 얻었으며, Arora의 이론적 연구가 지침이 되었다. Inception 구조의 이점은 ILSVRC 2014 classification 및 detection 분야에서 실험적으로 검증됐으며, 당시의 state-of-the-art보다 훨씬 뛰어난 성능을 보였다.

  🔸 NIN(Network-in-Network)


## 2. Related Work

이 페이지에서는 세 가지 주목할 내용은 아래와 같다. 

#### “CNN의 전형적인 구조” 
- Convolution layer다음에 contrast normalization이나 max-pooling layer가 선택적으로 뒤따르며, 하나 이상의 FC layer가 나오는 형태이다.
- 이 구조를 변형한 모델들은 이미지 분류분야에 널리 사용되며 MNIST, CIFAR, ImageNet 분류 챌린지에서 좋은 성과를 얻었다.

#### “ImageNet과 같은 큰 dataset의 경우”
- layer 수와 layer-size 늘리면서 dropout을 사용해서 overfitting을 피하는 것이 최근의 추세였다. 또한, Maxpooling layer가 공간정보의 정확성을 손실시킨다는 걱정에도 AlexNet의 CNN구조는 localization, object detection, human pose estimation분야에서 성공적인 성능을 보였다.

#### “NIN”은 
- 신경망의 표현력을 높이기 위해 Lin et al. 이 제안한 방식이다.  GoogLeNet의 경우에는 Inception layer가 여러 번 반복되어 22-layer deep model로 구현된다. 이 모델은 1x1 conv layer가 네트워크에 추가되어 depth를 증가시킨다.  1x1 convolution이 가지는 목적은 다음 두 가지가 있다.

  1. 병목현상을 제거하기 위한 차원의 축소
  2. 큰 성능의 저하없이 네트워크의 width와 depth를 증가시키기 위해

1x1 convolution과 병목현상(bottleneck) 에 대한 자세한 내용을 아래 더보기🔎 참고!!
<details markdown="1">
<summary>더보기🔎</summary>

먼저, **병목현상**이란?

![image](https://user-images.githubusercontent.com/53431568/123389559-f8ddca80-d5d4-11eb-9867-708f6d967453.png)

여러 현상에서 쓰이는 단어로 컴퓨터쪽 용어로는, 병의 목 부분처럼 넓은 길이 좁아짐으로써 컴퓨터 성능이 저하되는 현상을 말한다. 쉽게 말하자면, 수용가능한 메모리 공간은 작은데 한꺼번에 많은 양의 데이터가 유입 됨으로써 컴퓨터가 느려지는 현상을 말한다.

이 현상은 CPU와 GPU로부터 발생한다. 각 현상에 대해 알아보자!

**1. CPU bottleneck**

cpu bottlenec 현상은 프로세서가 충분히 빠르게 데이터를 처리하지 못하거나 전송하지 못할때 발생한다. 즉, 데이터의 전송속도를 CPU의 처리속도가 따라가지 못하는 경우에 병목현상이 발생한다.

**2. GPU bottleneck**

gpu bottleneck 현상은 entry-level의 그래픽카드를 빠른 프로세서를 접목해 사용하면 일어난다고 하는 주장도 있고, 기본적으로 CPU 처리능력이 부족하여 GPU 로드율이 90%이하로 떨어지고 프레임이 급격히 하락하는 정도의 상황이 발생하는 정도가 되야 병목현상이 발생했다고 할 수 있다고 보는 사람도 있다.

#### 그렇다면 1x1 convolution은 위의 병목현상을 어떻게 제거할 수 있을까?

먼저, 컨볼루션 연산을 계산하는 공식은 다음과 같다. (파라미터 수는 결과값 크기를 의미한다)

> Convolution Parameters = Kernel Size x Kernel Size x Input Channel x Output Channel

이렇게 Channel 값이 많아지는 경우 연산에 걸리는 속도도 그만큼 증가할 수 밖에 없는데, 이때 Channel(차원)을 축소하는 개념이 Bottleneck layer 이다.

![image](https://user-images.githubusercontent.com/53431568/123392257-ced9d780-d5d7-11eb-9738-d0f330ecce63.png)

위 그림과 같이 중간에 1x1 convolution을 추가함으로써 channel의 수가 줄면서 연산량이 확연히 감소하는 모습을 볼 수 있다. channel수 조절은 직접적인 연산량 감소로 이어지면서 네트워크를 구성할 때 더 깊게 구성할 수 있도록 도움을 준다.(1x1 conv 두번째 목적 큰 성능의 저하없이 네트워크의 width와 depth를 증가시키기 위해)
또, 이렇게 channel수를 줄였다가 다시 늘리는 부분을 bottleneck 구조라고 표현하기도 한다. 

</details>


## 3. Motivation and High Level Consideration!

GoogLeNet이 나오게 된 배경

- DNN을 향상시키기 위한 가장 손쉬운 방법은 “크기”를 늘리는 것이다. 이 방법은 쉽고 간단하나 두 가지 문제가 있다.

(1) 네트워크의 크기가 커져 parameter가 늘어나면서 학습데이터가 적은 경우 overfitting이 쉽게 일어난다.

(2) 컴퓨터 자원의 사용량이 늘어난다. -> 컴퓨터 자원은 유한하기 때문에 네트워크 사이즈를 늘리는 것보다는 컴퓨팅 자원을 효율적으로 분배하는 것이 더 중요하다. 

👉 이 두 가지 문제를 해결하기 위해서는 **`fully connected에서 sparsely connected 구조로 변경`** 하는 것이다.

하지만, 현재의 하드웨어로는 sparse한 매트릭스 연산에 비효율적이다. 따라서 많은 문헌에서는 sparse 행렬을 클러스터링해 `상대적으로 밀도가 높은 하위 dense 행렬로(submatrix)만드는 것을 제안`(=> 이 부분에 주목하자! 💡) 하며 이것은 좋은 성능을 냈다.


## 4. Architectural Details

  Inception구조의 주 아이디어는 CNN에서 각 요소를 최적의 local sparce structure로 근사화하고, 이를 dense component로 바꾸는 방법을 찾는 것이다.   

  `앞서 말한 것과 같다! -> sparse 행렬을 클러스터링하여 상대적으로 밀도가 높은 하위 dense 행렬로(submatrix)만드는 것!`

  Inception 구조는 1x1, 3x3, 5x5로 제한했으며 module은 아래와 같다.


  ![image](https://user-images.githubusercontent.com/53431568/123112301-91ab0380-d478-11eb-9a56-7b6d0af6c4c2.png)

  먼저 (a)를 보면 5x5 convolution이라도 많은 filter가 쌓인다면 계산 비용이 커진다는 단점이 있다. 

  따라서 (b)와 같이 1x1을 통해 차원을 축소하였다. 1x1 convolution은 3x3과 5x5 convolution이전에 사용해 연산 량을 감소시킨다.

  👉 이 구조의 이점은 연산 량을 크게 늘리지 않으면서 네트워크의 크기를 늘릴 수 있고 convolution 연산 이후의   ReLU를 통해 비선형적 특징을 추가할 수 있다.

## 5. GoogLeNet

![image](https://user-images.githubusercontent.com/53431568/123108804-b18cf800-d475-11eb-89ed-9f5e77657320.png)

> 전체 구조의 scheme view 
> - 차원 축소를 위한 1x1 convolution과 ReLU함수
> - 1024유닛을 가진 fully-connected layer과 ReLU함수
> - Dropout layer는 drop된 결과물의 70%의 비율을 갖는다.
> - 분류기로는 softmax를 사용하는 선형 layer을 사용한다.

Inception module 내부를 포함한 모든 Convolution layer에는 ReLU가 적용되어 있다. 또한 receptive field의 크기는 224 x 224로 RGB 컬러 채널을 가지며, mean subtraction을 적용한다.
 
이제 googlenet의 구조를 부분적으로 알아보자! 크게 3가지의 구조로 분석해 볼 수 있다.


### (1) architecture part 1

![image](https://user-images.githubusercontent.com/53431568/123109113-ee58ef00-d475-11eb-98c7-8eb7a120bb3c.png)

낮은 레이어가 위치한 부분으로 Inception module이 사용되지 않음.

**효율적인 메모리 사용을 위해 낮은 layer에서는 기본적인 CNN 모델을 적용하고, 높은 layer에서 Inception module을 사용한다.** 


### (2) architecture part 2

![image](https://user-images.githubusercontent.com/53431568/123109378-2829f580-d476-11eb-8f1c-e7087d7494e0.png)

Inception module이며 다양한 특징을 추출하기 위해 1x1, 3x3, 5x5 convolution이 병렬적으로 연결 되어 있으며 3x3과 5x5 convolution 이전에 1x1을 통해 차원을 축소하는 구조이다. 
Architectural details에서 언급한 것과 같다.

### (3) architecture part 3

![image](https://user-images.githubusercontent.com/53431568/123109541-48f24b00-d476-11eb-8053-1810d11b6a0c.png)

#### auxiliary classifier가 적용된 부분이다.
모델의 레이어가 많아지면 역전파를 수행 할 때 **기울기가 0으로 수렴하는 gradient vanishing 문제**가 발생할 수 있다.
 
이를 해결하기 위해 **중간 layer에 auxiliary classifier를 추가**하여, 중간중간에 결과를 출력해 **추가적인 역전파를 일으켜 gradient가 전달**될 수 있게 하면서도 정규화 효과가 나타나도록 하였다.

지나치게 영향을 주는 것을 막기 위해 auxiliary classifier의 loss에 0.3을 곱하였고, 실제 테스트 시에는 auxiliary classifier를 제거 후, 제일 끝단의 softmax만을 사용한다.
 

### (4) architecture part 4


![image](https://user-images.githubusercontent.com/53431568/123109733-70e1ae80-d476-11eb-9799-3fc545806f92.png)

Output이 나오는 구간이다. 구조를 보면 최종 classifier이전에 average pooling layer를 사용하고 있는데 이는 GAP (Global Average Pooling)가 적용된 것이다.

GAP는 이전 layer에서 추출된 feature map을 각각 평균 낸 것을 이어 1차원 벡터로 만들어 준다. (1차원 벡터로 만들어줘야 최종적으로 이미지 분류를 위한 softmax layer와 연결할 수 있기 때문이다.)
 
 <br>
 
## 6. Training Methodology
 이 논문저자는 asynchronous stochastic gradient descent(SGD)를 사용하였으며 momentum = 0.9, learning rate는 매 8번의 epoch 마다 4% 씩 감소하는 고정 스케쥴을 가진다.  
 
 또, Polyak averaging이 inference time에 사용되는 final model을 만드는데 사용되었다.
 
<details markdown="1">
<summary>Polyak averaging🔎</summary>
Polyak averaging는  최적화 알고리즘이 횡단하는 파라미터 공간의 여러 포인트들을 평균화시킨것을 포함한다. 따라서 만약 최적화 도중
알고리즘이 $\theta(1), \theta(2), ...$를 만나게 되면 Polyak averagingdml 결과는 다음과 같다.

$\hat{\theta}^(t) = \fraq{1}_t \sum_i \theta^(i)$

</detail>

<br>
<hr>
## 왜이럴까~
<br>

<details markdown="1">
<summary>inference time🔎</summary>
직역하자면 추론 시간이라는 것인데, **`하나의 frame을 detection하는데 까지 걸리는 시간을 inference time`**이라고 한다.

영상은 image들의 연속적인 집합이다. FPS란 초당 detection하는 비율을 의미한다. 만약, 초당 20개의 frame에 대해 detection을 수행하면 20fps 라고 한다.
사람들이 자연스럽게 인식하는 영상의 fps는 30fps이다. 따라서 초당 연속적인 frame을 30개 이상 처리할 수 있으면 끊기지 않는 자연스러운 영상이라고 인식하게 되는 것이다.

따라서 Object Detection모델의 성능을 평가할 때 (m)AP개념도 중요하지만 inference time도 중요하게 생각한다. 그래서 여기서 마지막 모델로 inference time을 측정하나보다!
</detail>
 
 
 
 
 
 
 
 
 
 
<br> 
GoogLeNet을 코드로 구현한것을 정리한 페이지이다. => [GoogLeNet](https://chaelin0722.github.io/deeplearning/cnn/code/googlenet_code/)

<br>

### 참고 
 
  [1] [https://sike6054.github.io/blog/paper/second-post/](https://sike6054.github.io/blog/paper/second-post/)

  [2] [https://leedakyeong.tistory.com/entry/%EB%85%BC%EB%AC%B8-GoogleNet-Inception-%EB%A6%AC%EB%B7%B0-Going-deeper-with-convolutions-1](https://leedakyeong.tistory.com/entry/%EB%85%BC%EB%AC%B8-GoogleNet-Inception-%EB%A6%AC%EB%B7%B0-Going-deeper-with-convolutions-1)

  [3] [https://phil-baek.tistory.com/entry/3-GoogLeNet-Going-deeper-with-convolutions-%EB%85%BC%EB%AC%B8-%EB%A6%AC%EB%B7%B0](https://phil-baek.tistory.com/entry/3-GoogLeNet-Going-deeper-with-convolutions-%EB%85%BC%EB%AC%B8-%EB%A6%AC%EB%B7%B0)

  [4] [https://89douner.tistory.com/80](https://89douner.tistory.com/80)

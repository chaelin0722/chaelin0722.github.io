---
title:  "[논문정리📃] EfficientDet: Scalable and Efficient Object Detection"
excerpt: "-EfficientDet-"

categories:
  - paperReview
tags: [CNN, paperReview]
use_math: true

last_modified_at: 2022-01-27T08:06:00-05:00
classes: wide
---

##  EfficientDet: Scalable and Efficient Object Detection
#### - EfficientDet -

[논문원본](https://arxiv.org/abs/1911.09070)😙

Detection 코드는 많이 읽어보았지만 포스팅은 처음인 것 같다. 아무래도 classification에 비해 어려워서 미루다 보니까 detection 쪽은 피하게 된다. 하지만 교수님이 피하기만 하면 안되고 끝까지 마무으리를 지어야 한다고 하셔서 마무리! 를 해보려고 한다.

EfficientDet은 이름에서부터 알 수 있듯이 EfficientNet을 기반으로 detection을 한 모델이다. 이 논문은 [EfficientNet을 먼저 읽으면 이해하기 쉬우니 한 번 읽어보고](https://chaelin0722.github.io/paperreview/efficientnet/) 오는 것을 추천한다

<br>


이 논문에서 알아야 할 핵심 내용 2가지는 BiFPN과 compound scaling method 입니다. 

#### 1) BiFPN 

   논문을 보면, bi-directional feature pyramid network, allows easy and fast multi-scale feature fusion 라고 적혀있는데, 뒤에서도 자세히 다루지만 Feature Pyramid Network를 더 효율적이게 개조한 방식이라고
   생각하면 된다.

#### 2) Compound scaling method 

    EfficientNet을 Backbone network로 사용하다보니 이에 맞추기 위해 compound scaling method를 사용합니다.



아래 이미지를 보면, 연산량은 기존 SOTA 모델과 비교했을때 현저히 줄었지만, 성능은 월등한 것을 확인할 수 있습니다. EfficientNet과 비슷한 양상을 보이네요!

![image](https://user-images.githubusercontent.com/53431568/151330466-cd67a609-e163-4508-874f-5e4ca067d40e.png)



### EfficientDet architecture

전체적인 구조는 다음과 같습니다. backbone으로부터 각각 $1 over 2^i$ 만큼씩 scale 한 feature들을 pyramid 처럼 쭉 나열하여서 3번째 레이어부터 7번째 레이어들의 피쳐들로 계산을 해줍니다.

논문에는 나와있지 않지만 3번째 feature 부터 연산에 고려하는 이유는 아마 이 때부터 유의미한 특징점들을 가진 feature가 생성되기 때문에 3번째부터 하지 않았나 싶네요..!

또, 5개의 feature 들을 BiFPN 연산을 n 번 반복하게 하여 마지막 feature들을 concatenate 하여 각각 class prediction과 box prediction network로 계산해줍니다.

![image](https://user-images.githubusercontent.com/53431568/151333118-e07a3fd0-168e-46ce-907a-a7f3c0a957ba.png)

<br>


## FPN development process

FPN을 처음 들어보았다면 이 개념이 무엇인가 당황스러웠을 텐데! 그게 바로 접니다 ㅎㅎ

![image](https://user-images.githubusercontent.com/53431568/151341751-e933d696-319d-4e7f-8b51-25cdd60b262b.png)

위의 이미지는 FPN의 발전 양상을 보여주고 있는데요, (a)의 FPN은 가장 오리지널 형태로 다양한 scale의 이미지들을 fusion 시키면 성능이 더 좋게 나올것이다! 라는 직관에서 나온 개념입니다. 보면 TOP-DOWN 방식으로
각 feature들을 upsampling(이미지 스케일을 크게하여 합치는것! 더해준다) 하여 합치게 됩니다. 이렇게 해서 성능을 높였지만, 정보가 한쪽으로 흐르는 것에 대한 한계를 지적하며 나온것이 (b)의 PANet입니다.
TOP-DOWN 후 BOTTOM-UP을 한 번 더 추가한 것을 볼 수 있습니다! 그리고 (c)는 NAS의 개념을 FPN에 도입한 것인데, 최적의 성능을 내는 최적의 architecture을 찾도록 하는 것입니다. [NASNET 논문리뷰](https://chaelin0722.github.io/paperreview/nasnet/)
를 참고하면 어떤 방식인지 알수 있습니다! 이렇게 best architecture을 찾으려다보니 GPU 연산이 느려지는 문제가 생겼다고 합니다. 그리고 오히려 PANet의 성능이 더 좋다고 한다.


![image](https://user-images.githubusercontent.com/53431568/151344418-7aa262b1-aaba-4cff-b116-e94ad7270c62.png)![image](https://user-images.githubusercontent.com/53431568/151347204-dd8755d0-9af0-432c-bc19-6408b235ad68.png)


그리고! 여기서 새로나온 BiFPN은 PANet에서 개선시킨 방식입니다. 의 PANet에서 input node가 1개인 노란색으로 하이라이트된 노드를 제외시킨 것을 볼 수 있습니다. 이는 기여도가 적은 feature은 연산을
줄이기 위해서 제거하였다고 합니다. 그리고, input node를 다시한번 마지막 노드인 ouput 노드에 더해줍니다. 이는 더 많은 feature을 fusion 시켜 성능을 좋게 한다고 합니다. 그리고 이 블럭을 n번 반복! 논문에선 3번 반복한다고 함



<br>

## BiFPN accuracy

![image](https://user-images.githubusercontent.com/53431568/151347365-a24e1002-27df-451d-8217-9db32cb4da74.png)

위의 이미지는 FNP들의 성능 비교입니다. BiFPN이 연산량도 작은데 성능은 가장 좋고 또, weight를 추가하면 더 좋아진다고 합니다.


<br>

## BiFPN : Weighted feature fusion

feature fusion을 시킬 때 weight를 주면 성능이 더 좋게 나오는 것을 위의 표에서 확인을 하였습니다. 바로 Fast normalized fusion을 사용하여 성능을 향상시켰습니다.

![image](https://user-images.githubusercontent.com/53431568/151344886-d5309eba-7159-4190-a1af-fb83a30599b7.png)

먼저 unbounded fusion은 단순히 스칼라 값을 input feature에 곱해주는 방식인데 기본적인 방식이죠! 하지만 이렇게 되면 스칼라 값이 너무 크거나 작으면 전체적으로 불균형한 값이 나와버려서 성능이 좋지 않습니다.
그리고 softmax fusion은 어느정도 성능을 달성하지만 연산하는데 시간이 오래걸려서 (아마 자연상수로 계산하기 때문인 것 같습니다.) 비슷한 성능을 보이지만 빠르기 면에서 많이 향상된 Fast normalized fusion을 사용합니다.


Fastnormalized의 방식에서 weight는 Relu 함수를 거쳐나온 값으로 0~1의 값을 보장하면서 분모가 0으로 수렴하는 것을 막기 위해 0.0001의 아주 작은 값을 추가로 더해줍니다(수식에서의 입실론).

아래 코드로 살펴보면 더 와닿습니다.

![image](https://user-images.githubusercontent.com/53431568/151346120-18b1c9de-5608-4b9d-85da-98ecffe27d39.png)

<br>

### Comparision of different feature fusion

![image](https://user-images.githubusercontent.com/53431568/151346017-64c57b4e-8337-4149-9630-0c5761236f90.png)



<br>

## 연산과정 도식화


![image](https://user-images.githubusercontent.com/53431568/151346166-45110395-1220-459d-8566-c4954d89d524.png)

<br>

## Compound scaling

scaling 하는 방식은 아래의 수식대로 처리해줍니다. 이 스케일링 방식은 EfficientNet에 맞춰주기 위해서 고려되었습니다.

![image](https://user-images.githubusercontent.com/53431568/151346272-22fd3fff-c5e6-43bc-8c6c-9c4222e66748.png)


![image](https://user-images.githubusercontent.com/53431568/151346253-9fc7812b-85c1-4613-b945-ad93f868ff4b.png)


### Comparision of different scaling

![image](https://user-images.githubusercontent.com/53431568/151346360-a4b4b502-12d0-41cc-99b5-fd4054b84133.png)


<br>

## SOTA for COCO Dataset

![image](https://user-images.githubusercontent.com/53431568/151346464-d0ef3e39-fd91-4a7e-bbfe-94de8c68283d.png)


<br>

오늘 세미나도 무사히 완료!🥰🥰

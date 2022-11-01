---
title:  "[DL] Detectron Mask-RCNN with BAM(Bottleneck attention Module)"
excerpt: "mask rcnn 에 attention 붙여 성능 높이기"

categories:
  - CNN
tags: [Deeplearning, CNN, study]

last_modified_at: 2022-11-01T08:06:00-05:00
---

mask rcnn 에 attention 을 추가하여 최종 detection 성능을 높이고자 구현해 보았다. 

mask rcnn은 detectron2 에 구현되어있는 파이토치 코드, attention은 BAM 이라는 attention 모듈을 붙였다. 

### 1. Detectron2 설치

설치 방법은 아래 내용 참고!

[Detectron Implementation](https://chaelin0722.github.io/cnn/detectron_implementation/)

<br>

### 2. Detectron 코드 분석

detectron 을 설치하였다면, 여기서 사용할 mask rcnn 코드 부분을 수정해주어야 한다.

mask rcnn 모델은 resnet 기반으로 feature를 총 4 stage 에 거쳐서 뽑기 때문에 3의 attention 모듈을 추가해주었다. 

resnet.py 코드는 "/detectron2/detectron2/modeling/backbone/resnet.py" 경로에 있다. 
 
<script src="https://gist.github.com/chaelin0722/a26d15bf467f87762ce0ebf690d87723.js"></script>
 
 
원래 mask rcnn의 기본구조는 다음과 같다, 각 residual block 에서 나온 feature 가  c2, c3, c4, c5 로 나와있음을 알 수 있다.


![image](https://user-images.githubusercontent.com/53431568/199165818-f3361349-e0d7-4edf-8362-87e5922b6730.png)
 
여기서 각 c2, c3, c4 에 대해 attention을 취해준 후 그 값을 다음 feature에 넘겨주게 되는데 대략 아래의 그림과 같다. 

![image](https://user-images.githubusercontent.com/53431568/199165766-2def8707-951c-4072-91b8-eeebc2b7811e.png)



<br>

### 3. BAM(Bottleneck Attention Module) 코드
 
bam 코드는 official pytorch 코드를 사용하였고, resnet.py와 같은 경로에 추가하였다.
 
따라서 resnet.py 맨 위에 bam 모듈 import는 다음과 같이 추가해주었다. 
 
~~~
from .BAM import * 
~~~
 
 BAM.py 코드를 보면, 입력값을 GAP(Global Average Pooling) 하여 channel 축에 대해서 attention을 한번 해주고, channel 축을 제외한 축들에 대하여 spatial attention을 사용하여 이 두 attention 값을 더해준 후 sigmoid 를 취하여 계산해준다.
 
<script src="https://gist.github.com/chaelin0722/4cfaf8328609e339960f73bde59c09a1.js"></script>
 
 
 따라서 각 축에 대한 attention을 따로 한 후 합친 값을 최종 attention 값으로 사용해준다. 
 
<br> 


### 4. 성능 향상

큰 기대는 안했지만, attention 이라 그런지 확실한 성능 향상을 보이고 있다. 나는 지금 하고있는 과제인 칫솔의 crack 에 대한 detection 부분인데, 확실히 더 잘잡는 것을 보이고 있다. 


#### Mask RCNN 

|Mask RCNN||
|------|---|
|True Positive|38|
|True Negative|81|
|Accuracy|91.54 %|

#### Mask RCNN with BAM 

|Mask RCNN with BAM||
|------|---|
|True Positive|41|
|True Negative|81|
|Accuracy|93.85 %|



다음엔 feature map 을 출력하여 그 차이를 보이고자 한다.
 
### reference
 
 [1] [BAM](https://arxiv.org/abs/1807.06514)
 
 [2] [Detecton2](https://github.com/facebookresearch/detectron2)
 
 
 

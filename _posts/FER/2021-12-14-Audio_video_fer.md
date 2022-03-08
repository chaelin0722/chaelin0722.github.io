---
title:  "[Paper Review📃] Exploring Emotion Features and Fusion Strategies for Audio-Video Emotion Recognition"
excerpt: "-Audio-Video Emotion Recognition-"

categories:
  - fer
tags: [fer,CNN, paperReview]
use_math: true

last_modified_at: 2021-12-14T08:06:00-05:00
classes: wide
---

## Exploring Emotion Features and Fusion Strategies for Audio-Video Emotion Recognition
#### - Audio-Video Emotion Recognition - 

[Paper](https://arxiv.org/pdf/2012.13912.pdf)😙 2021년 12월 14일 기준, AFEW 데이터로 SOTA 성능 달성

#### 이 논문은 audio 와 video 두 가지 modal 을 fusion(혼합) 하여 SOTA 성능을 달성하였다.

이 논문의 메인 CONTRIBUTIONS 를 살펴보면,

1) faceCNN의 전이학습에 알맞은 데이터셋과 알맞은 모델을 사용하는 것이 중요하다 (모든 FER 논문에서도 말하지만,, 얼굴 인식 모델을 잘 pre-train 시키는 것이 성능올리는 것의 핵심이라고 강조하네요)

2) visual feature과 audio feature의 혼합 기법을 위해 세가지 attention 매커니즘을 고안하였다.
 
3) cross-modal feature 혼합을 위해 factorized bilinear Pooling(FBP)기법을 적용하였다. (cross-modal 이라는 것은 audio modal 과 video modal 을 혼합하는 것을 뜻한다고 생각하면 될 것 같다.)

<br>

### Pipeline of audio-video emotion recognition

논문에서 제시하는 구조는 아래와 같습니다.

![image](https://user-images.githubusercontent.com/53431568/146122054-6baeb1fe-4bd2-4828-8aa6-c1013ef014ac.png)

살펴보면, 각 audio 와 video 이미지에 대해 전처리를 수행한 후 각각의 CNN 모델을 사용해 feature을 뽑은 다음에 attention 매커니즘을 사용해 나온 값을 fusion 을 하여 최종 feature 을 뽑게 됩니다.

각 프로세스 별로 자세히 살펴봅시다..!
<br>

### Preprocessing

오디오와 이미지 전처리에 있어서는 간략하게 논문 내용을 그대로 쓰겠습니다.

- AUDIO

> Speech spectrogram -> use hamming window with 40msec window size, 10msec shift
>  
> Log mel-spectrogram -> calculate its deltas and delta-deltas

- VIDEO 

> use dlib toolbox for face detection and alignment
> 
> Extend face bbox with ratio of 30%
>  
> Crop face and scale to 224 x 224

 (한 프래임 내에서 얼굴이 검출되지 않는다면 모든 프레임들을 사용하지 않는다.)

<br>

### Feature Extraction

<img width="288" alt="무제" src="https://user-images.githubusercontent.com/53431568/146126478-e05ec970-0197-49a9-86df-d4c070e00f78.png">

전체 구조에서 봤듯이, audio 와 visual 이미지에 대해 각기 다른 CNN 모델을 사용하여 feature 를 뽑고 있습니다. 먼저, audio 의 경우 AlexNet의 마지막 pooling layer에서 feature 값을 뽑아서 HxWxC 의 피쳐 맵 형태로 추출이 됩니다.

한편, visual의 경우, 마지막 FC레이어에서 나온 값을 피쳐로 뽑기 때문에 n개의 벡터 형태로 값이 나오게 됩니다. (논문에서는 VGGFace, ResNet18, IR50 세 가지 모델에 대해서 실험을 하였고 그중 IR50이 가장 좋은 성능을 내어서 최종 구조 이미지에는 IR로 작성한 것 같습니다.)


### Three Attention machanism

이제 프로세스의 feature 을 혼합하는 모듈에서 intra-modal 부분을 살펴봅시다. 여기도 audio 와 video 는 각각 계산하고 있습니다. 아직까지 둘을 합치지는 않네요.

attention의 수식, 왜 저렇게 가중치와 다음 에 나올것이라 예상되는 FC 레이어(아래 수식에서는 $W$)를 내적하여 유사도를 구하는 지는 [FAN 논문 리뷰에서도 리뷰했으므로 참고하기!](https://chaelin0722.github.io/fer/FAN/)

![image](https://user-images.githubusercontent.com/53431568/146122320-46987964-2bac-40dc-8457-72d267ca94ea.png)

self-attention과 relation-attention 외에 transformer-attention 이라는 개념이 추가되었다! 새로운 개념 알아가는거 너무 재밌네요😆😆

trasnformer-attention은 기존 attention과 마찬가지의 개념을 갖고가는데, feature의 차원을 줄이기 위해 다음과 같은 linear function 과정을 거쳐서 차원을 감소시킨 후, 유사도를 구하고 있습니다.

$W_{m \times d}$를 피쳐값과 곱해주어 차원을 줄인 값을 $f^{`}_i$ 로 설정하여 그 값으로 내적시킨 값을 제곱으로 취하여 가중치로 사용하고 있습니다.


<br>

### FBP(Factorized Bilinear Pooling) module

![image](https://user-images.githubusercontent.com/53431568/146133144-30b4bde3-611e-4ffb-939e-2e2c6d52996c.png)

위의 개념을 이해하기 전, Bilinear Pooling이 무엇인지를 먼저 살펴보겠습니다.

**Bilinear pooling** 은 speech 와 visual과 같이 서로 다른 modal 을 (차원이 다른 벡터 계산) 모든 feature들이 상호작용하여 계산할 수 있도록 하는 방법입니다. 

다음과 같이 audio feature vector $a$ 는 m 차원에 존재하고, video feature vector 은 $n$ 차원에 존재합니다. 두 다른 차원에서 계산을 하려면 어떻게 해야할까요? 여기서는 projection matrix를 사용합니다.

![image](https://user-images.githubusercontent.com/53431568/146133216-a4bd7428-59a1-4d71-95cc-190d8cadfe2e.png)

아래 수식과 같이 a와 v 를 내적하기 위해 가운데 mxn 차원에 존재하는 projection matrix 인 $W$ 를 추가하여서 계산을 해주고 있습니다.

![image](https://user-images.githubusercontent.com/53431568/146133384-c580227a-77f4-4eaa-bcbf-a615512825ba.png)

이렇게 두 벡터의 모든 요소에 대해서 상호작용하며 계산할 수 있어서 모든 element간의 상호 관계성을 파악할 수 있다는 장점이 있습니다. 하지만, 그림에서도 보이는 z 의 면적! 매우 큰 것을 알 수 있죠! 연산의 비용이 매우 크고 overfitting을 초래할 위험이 있습니다.

따라서 이 문제점을 해결하기 위해 제시된 방법 중 하나가 FBP 입니다.

아래는 논문의 수식을 정리해 본것인데요.. 이 논문에 있어서 FBP 의 내용은 간략하게 나와서 [다음 논문을 참고하여 작성하였습니다.](https://arxiv.org/pdf/1901.04889.pdf)

![image](https://user-images.githubusercontent.com/53431568/146133724-059c4e38-4441-4df8-b1f3-83f945d645c2.png)

위의 수식은 복잡해보이지만 단순히 생각해보면, Bilinear pooling의 단점 때문에, matrix factorization tricks 을 사용해 the projection matrix $W_i$ 를 위와 같이 두개의 low- rank의 행렬로 바꾸어 계산하는 것입니다.




### Exploration of Emotion Features

Table 1 은 VGGFace, ResNet18, IR50 세 CNN 모델과 FER+, RAF-DB, AffectNet 데이터들에 대해 각각 학습해 실험한 결과, IR50을 AffectNet 데이터셋으로 전이학습한 모델의 성능이 가장 좋다는 것을 보여줍니다.

![image](https://user-images.githubusercontent.com/53431568/146134108-3d75cf92-8ce6-48e3-9dd6-38db074a657d.png)
 
Table 2 는 Audio와 Visual에 대해서 각 self, relation, transformer attention을 조합해 사용한 결과들 중, 가장 높은 성능을 보이는 것은 두 modal에 있어서 transformer 을 사용하는 것임을 보여줍니다.

![image](https://user-images.githubusercontent.com/53431568/146134304-e3694a02-6289-4a8d-8fe1-82d5e5cd034c.png)


이번주 세미나도 무사히 완료..! 😽







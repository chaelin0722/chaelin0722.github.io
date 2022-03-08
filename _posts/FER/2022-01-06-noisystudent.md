---
title:  "[Paper Review📃] Noisy Student Training using Body Language Dataset Improves Facial Expression Recognition"
excerpt: "-Noisy Student FER-"

categories:
  - fer
  
tags: [fer,CNN, paperReview]
use_math: true

last_modified_at: 2022-01-06T08:06:00-05:00
classes: wide
---

## Noisy Student Training using Body Language Dataset Improves Facial Expression Recognition
#### -Noisy Student FER-

[Paper](https://arxiv.org/pdf/2008.02655.pdf)😙 

이번엔, Papers with code 기준, AFEW 데이터로 SOTA성능을 달성한 FER 논문을 리뷰하려고 한다.


참 많이도 사용하였다. 3가지 attention 기법, 얼굴 이미지를 3가지로 나누어서 분석, noisy student으로 학습, extra dataset 사용.. 이렇게 다 사용해도 audio를 사용한 multi-modal model 보다는 아래에 랭크되어있다.


`**아래 이미지는 AFEW 데이터셋 기준으로 모델 순위이다. 6위를 차지하는 중`

![화면 캡처 2022-01-05 234023](https://user-images.githubusercontent.com/53431568/148239068-d13bd014-69be-456a-ad1f-c405249d6c4a.png)


## Introduction

- Propose an efficient model addresses the challenges posed by videos in the wild while tackling the issue of labelled data inadequacy

- Previous video-based emotion recognition used visual cues but a fusion of 5 different architectures with more than 300 million parameters. `However`, this model proposed method uses a single model with approximately 25million parameters and comparable performance

- Use SOTA pre-trained deep learning model (Enlighten-GAN) for preprocessing (because, previous methods tend to amplify noise, tone distortion, and other artefacts)

- Use three-level attention mechanism (spatial-attention block, channel-attention block, frame-attention block)

요약하자면, 싱글모델을 사용하면서, 이미지 전처리에 GAN, Backbone에서는 3번의 attention, unlabelled dataset(extra data)을 사용하여 SOTA 성능을 달성하였다.


<br>

## Pre-processing

이미지 전처리 과정을 도식화 해보았다.

![화면 캡처 2022-01-06 202517](https://user-images.githubusercontent.com/53431568/148375825-ae5e6eaf-a3b7-4fe4-8e2a-d6eb5c3659cf.png)

모든 FER이 그러하듯, 비디오 데이터를 프레임처리하고, 각 프레임에서 MTCNN을 사용해 얼굴을 찾고 CROP한 후 각도를 맞춰주는 작업을 한다. (MTCNN 논문은 읽는중이다. 추후 포스팅하겠다..!)

여기서 추가되는게 Enlighten-GAN인데, 위의 그림에서도 볼 수 있듯이 어두운 화면에서 얼굴의 특징점을 찾기 힘들기 때문에 GAN을 사용하여서 이미지를 밝게 하는 처리를 해주었다.

기존에는 전처리 알고리즘(gamma correction, difference of Gaussians, histogram equalization 등)을 사용했었는데, 노이즈를 상세화시키고 톤이나 다른 인공물들을 왜곡시키는 문제가 있어 GAN을 사용했다고 한다.

이렇게 밝기 처리해준 얼굴 이미지를 다시 MTCNN을 통해 양쪽 눈과 양쪽 입술을 랜드마크를 찾아서 눈에서 half lower crop!, 입에서 half upper 까지 crop! 해준 후 다시 224x224로 resize해준다.


<br>

## Backbone Network with Spatial-Attention

다음은 ResNet18 backbone 네트워크 구조이다. 논문에서 제시하는 이미지가 헷갈려서 다시 수정해서 정리해보았다.

![image](https://user-images.githubusercontent.com/53431568/148376953-423d2542-68bb-4914-870e-d3f23add2266.png)


input으로는 224x224x9 로 이미지 전처리에서 얻은 face, eyes, mouth 세 장의 이미지를 입력한다. 이 구조는 group-convolution을 사용하여 독립적인 연산을 수행한다고 한다. (이미지로는 3가지 모델을 사용하는 것 처럼 보이지만 하나의 모델로 독립적인 연산을 수행하는 것..!) 보면, 연두색 테두리의 BOX가 Residual block이고, 각 Residual block에서 SA(Spatial Attention) 를 수행하여 1차원의 feature를 뽑아서 4개의 block에서 뽑은 feature들을 concat 시켜서 960개의 feature vector을 뽑아낸다. 이렇게 뽑은 feature는 이미지에서 어느 부분이 중요한지에 대한 정보를 가지고 있다.

### - Spatial Attention 연산

![image](https://user-images.githubusercontent.com/53431568/148377565-6bd267cc-9f5d-41a8-9adf-a0f703180803.png)

$W_sl$과 $W_s2$는 각각 가중치 행렬과 벡터이다. $L$은 2D-tensor로 차원을 변경해준 channel이라고 보면 된다. 자세한건 더 공부해야겠지만, 수식을 보면 attention 구하는 수식과 동일한데 좀 다른 것을 알 수 있다. attention은 본래, fc 레이어에서 연산을 해주었던 반면, 여기서는 행렬곱으로 연산을 한다. 누가 자세히 아시는분 있으면 연락좀.. 주셔요

- [attention is all you need 논문 참고](https://chaelin0722.github.io/paperreview/attention/) 😆
- [FAN](https://chaelin0722.github.io/fer/FAN/)😆
- [Audio-Video emotion recognition](https://chaelin0722.github.io/fer/Audio_video_fer/)😆


<br>

## channel Attention

![image](https://user-images.githubusercontent.com/53431568/148377763-19a17b3d-9067-4c72-838f-a9bb53bea686.png)

각 face, eyes, mouth에서 뽑은 960feature 로 attention연산을 통해 평균을 낸 하나의 960feature을 뽑게 된다. 이를 다시 512 feature로 줄이면 이 feature가 `한 프레임의 feature`이 된다


이렇게 각 프레임에 대한 평균 feature들을 계산해 구해내어 512 feature를 만들게 된는 것이다.

<br>

## Frame Attention

![image](https://user-images.githubusercontent.com/53431568/148379183-1fb6cee9-7974-4e66-8cf0-67236f075917.png)


각 프레임에 대한 512 feature 에 대해서 또 다시 attention! 그리고 최종적으로 7개의 label에 대해 classification 해줍니다.


## Noisy student training

![image](https://user-images.githubusercontent.com/53431568/148379263-f5966ab2-dbb2-41b3-91e1-88aa57facda0.png)

AFEW 데이터를 확인해보니 확실히 양이 적었다. 이걸로만 학습하면 성능이 잘 안나오긴 할것 같다 ㅎㅎ 그래서 이 논문에서 제시한게, Unlabelled된 데이터를 가져와서 pseudo label을 한 후 학습시킨 모델로 사용하는 것(noisy student 개념) 인데, 여기서는 unlabel 된 데이터를 BoLD(BodyLanguage Dataset)을 사용하였다. 

순서는 다음과 같다.

1. AFEW8.0으로 학습한다.
2. 학습한 것 중 가장 best model로 BoLD 데이터에 pseudo label을 진행한다.
3. AFEW8.0에 더하여 label이 생성된 BoLD 데이터를 합한 데이터(최종데이터셋)로 학습을 시키는데 여기에 + noise를 추가해준다
4. 최종 데이터셋으로 3번 4번 반복한다.

(사용한 noise에는 dropout(0.5), contrast, brightness, translation, sharpness, flips 등을 랜덤으로 사용함)

😆 => [Noisy Student 참고](https://chaelin0722.github.io/paperreview/noisy_student/)

<br>

## result

![image](https://user-images.githubusercontent.com/53431568/148380117-3aa18be1-bb5b-45d1-85b9-26426772674a.png)

각 face, mouth, eyes 에 대해서 afew8.0으로 성능측정한 결과인데요, 감정표현에 있어서 눈이 많이 쓰이는 sad는 역시 eyes 에서 성능이 좋고, happy와 angry 같이 입의 표현이 중요한 감정은 mouth에서 성능이 좋게 나온 것을 확인하였습니다. 하지만 best 는 이 세가지를 모두 합해서 사용하는 것! 이라고 주장하네요

<br>

다음은 iteration을 반복하면서 성능이 향상됨 + 균형맞춘 unlabelled 데이터의 중요성을 보여주는 정도로 해석하면 될 것 같음 

![image](https://user-images.githubusercontent.com/53431568/148380630-9e5fa58e-d4ec-431d-b26e-dc6179619562.png)

<br>

CK+데이터에선 99.69%, AFEW 에서는 55.17%의 성능을 달성하였다는 것을 알 수 있습니다.

![image](https://user-images.githubusercontent.com/53431568/148380729-bed6ae8e-fff7-44e1-99ad-b500ccdd46b6.png)



component importance를 보여줍니다. 역시 이것저것 다 붙이고 training도 많이 반복한게 성능이 좋게 나오네요. 

![image](https://user-images.githubusercontent.com/53431568/148380836-3a1fd208-7ae3-43c1-95db-91dacf7ab065.png)

그럼에도 AFEW8.0 데이터 기준으로 6등이라는게.. 이렇게 다 붙여도 결국 multi-modal model을 이길 수 없다니... 공부하면서 많은 생각을 하게 하는 논문이었습니다. 최신 기법들(GAN과 attention 3가지)을 모두 다 갖다 쓰고 심지어 extra dataset까지 사용하였는데 AUDIO를 같이 사용한 model과 10% 씩이나 차이가 난다. visual 만으로는 한계가 있는 것인지 아니면 새로운 알고리즘을 제시하고 방향을 전환하는 것을 고민해봐야겠다..!

오늘도 무사히 세미나 완료!😽 


---
title:  "[논문정리📃] Exploring Emotion Features and Fusion Strategies for Audio-Video Emotion Recognition"
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

[논문원본](https://arxiv.org/pdf/2012.13912.pdf)😙 2021년 12월 14일 기준, AFEW 데이터로 SOTA 성능 달성

#### 이 논문은 audio 와 video 두 가지 modal 을 fusion(혼합) 하여 SOTA 성능을 달성하였다.

이 논문의 메인 CONTRIBUTIONS 를 살펴보면,

1) faceCNN의 전이학습에 알맞은 데이터셋과 알맞은 모델을 사용하는 것이 중요하다 (모든 FER 논문에서도 말하지만,, 얼굴 인식 모델을 잘 pre-train 시키는 것이 성능올리는 것의 핵심이라고 강조하네요)

2) visual feature과 audio feature의 혼합 기법을 위해 세가지 attention 매커니즘을 고안하였다.
 
3) cross-modal feature 혼합을 위해 factorized bilinear Pooling(FBP)기법을 적용하였다. (cross-modal 이라는 것은 audio modal 과 video modal 을 혼합하는 것을 뜻한다고 생각하면 될 것 같다.)


### Pipeline of audio-video emotion recognition

논문에서 제시하는 구조는 아래와 같습니다.

![image](https://user-images.githubusercontent.com/53431568/146122054-6baeb1fe-4bd2-4828-8aa6-c1013ef014ac.png)

살펴보면, 각 audio 와 video 이미지에 대해 전처리를 수행한 후 각각의 CNN 모델을 사용해 feature을 뽑은 다음에 attention 매커니즘을 사용해 나온 값을 fusion 을 하여 최종 feature 을 뽑게 됩니다.



### Three Attention machanism

attention의 수식, 왜 저렇게 가중치와 다음 에 나올것이라 예상되는 FC 레이어(아래 수식에서는 $W$)를 내적하여 유사도를 구하는 지는 [FAN 논문 리뷰에서도 리뷰했으므로 참고하기!](https://chaelin0722.github.io/fer/FAN/)

![image](https://user-images.githubusercontent.com/53431568/146122320-46987964-2bac-40dc-8457-72d267ca94ea.png)






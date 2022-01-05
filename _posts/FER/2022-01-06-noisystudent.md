---
title:  "[논문정리📃] Noisy Student Training using Body Language Dataset Improves Facial Expression Recognition"
excerpt: "-Noisy Student FER-"

categories:
  - fer
  - 
tags: [fer,CNN, paperReview]
use_math: true

last_modified_at: 2022-01-06T08:06:00-05:00
classes: wide
---

## Noisy Student Training using Body Language Dataset Improves Facial Expression Recognition
#### -Noisy Student FER-

[논문원본](https://arxiv.org/pdf/2008.02655.pdf)😙 

이번엔, Papers with code 기준, AFEW 데이터로 SOTA성능을 달성한 FER 논문을 리뷰하려고 한다.


참 많이도 사용하였다. 3가지 attention 기법, 얼굴 이미지를 3가지로 나누어서 분석, noisy student으로 학습, extra dataset 사용.. 이렇게 다 사용해도 audio를 사용한 multi-modal model 보다는 아래에 랭크되어있다.


`**아래 이미지는 AFEW 데이터셋 기준으로 모델 순위이다. 6위를 차지하는 중`

![화면 캡처 2022-01-05 234023](https://user-images.githubusercontent.com/53431568/148239068-d13bd014-69be-456a-ad1f-c405249d6c4a.png)



---
title:  "[논문정리📃] Facial expression and attributes recognition based on multi-task learning of lightweight neural networks"
excerpt: "-Multi-task EfficientNet-B2-"

categories:
  - paperReview
tags: [FER,CNN, paperReview]
use_math: true

last_modified_at: 2021-11-17T08:06:00-05:00
classes: wide
---

## Facial expression and attributes recognition based on multi-task learning of lightweight neural networks
#### - FAN - 

[논문원본](https://arxiv.org/abs/2103.17107)😙


이 논문은 비디오(영상)을 프레임을 여러개 받아 각 프레임마다의 mean, std, min, max 값을 계산하여 얼굴표정을 인식하는 방법을 제시하고 있습니다.


Studied for face identification and classification of facial attributes(age, gender, ethnicity) trained on cropped faces without margins
Necessity to fine-tune these networks to predict facial expressions is highlighted

MobileNet, EfficientNet, RexNet architecture


![image](https://user-images.githubusercontent.com/53431568/142206134-276f1826-64a5-4e2a-93b9-8d0173e95673.png)


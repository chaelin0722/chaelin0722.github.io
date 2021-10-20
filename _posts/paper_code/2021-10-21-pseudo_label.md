---
title:  "[논문정리📃] Pseudo-Labeling and Confirmation Bias in DeepSemi-Supervised Learning"
excerpt: "-Pseudo Label-"

categories:
  - paperReview
tags: [CNN, paperReview]
use_math: true

last_modified_at: 2021-10-21T08:06:00-05:00
classes: wide
---

## Pseudo-Labeling and Confirmation Bias in Deep Semi-Supervised Learning
#### - Pseudo label - 

[논문원본](https://arxiv.org/pdf/1908.02983.pdf)😙

이 논문은 semi supervised learning의 측면에서 pseudo label을 하는 이유와 pseudo label를 어떻게 구성하여 성능을 올릴 수 있었는지에 대해 설명하고 있습니다.

저는 이 논문에 대해서는 자세히보다는 pseudo label이 어떻게 이루어 지는지를 loss function과 구조적 측면에서 확인해 보며 어떤식으로 성능향상에 도움이 되는지에 대해서만 정리하고자 합니다!⭐️

<br>

먼저, 간략하게 pseudo label 을 하는 이유에 대해 적어보자면,

image classification 분야에 있어서 많은 데이터셋이 더 좋은 성능을 내는 것을 보고, 더 많은 데이터셋으로 학습시키고자 하였습니다. 

그래서 나온 방법이, label이 되지 않은 더 많은 데이터셋을 끌어와서 임의로 labeling을 해준 후 학습에 데이터를 추가하는 방식입니다. 이 방식은 지도학습과 비지도 학습을 섞은 형식이므로 semi-supervised learning이라고 합니다.

여기서 임의로 labeling 하는 것은, 먼저 모델을 imagenet 등의 레이블이 있는 데이터셋으로 한번 학습을 시켜준 후, 그 모델로 label이 없는 데이터를 임의로 labeling을 진행합니다. 이때 생긴 임의의 label을 `pseudo label` 이라고 명명합니다. 

pseudo label은 아직 제대로 레이블 되었는지 어땠는지는 모릅니다. 따라서 이 데이터를 추가하여 학습한 결과도 좋은 SOTA성능을 보인다는데!! 여기서 든 의문은 그럼 `pseudo label에서 잘못된 label이 생성되어 학습될 경우
더 안 좋은 결과를 낼 텐데.. 어떤방식으로 labeling 학습을 했는지가 궁금`😲😲하여 이 논문을 읽게 되었습니다.

이제 어떻게 labeling 을 잘 하도록 학습시켰는지 알아보도록 하겠습니다.




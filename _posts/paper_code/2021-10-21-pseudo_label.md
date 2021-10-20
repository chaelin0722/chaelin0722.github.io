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

<br>

### Pseudo label 종류

#### <li> hard-pseudo-label
  네트워크의 예측값을 라벨로 사용하는 방식으로 one-hot vector을 생각하면 됩니다. 예를 들어 어떤 이미지에 대한 레이블 값이 고양이! 이렇게 나오면 그 이미지는 그대로 고양이로 레이블 됩니다.
  

#### <li> soft-pseudo-label
  반면 soft-label 방식은 softmax prediction값을 사용합니다. 즉, continuous distribution 한 label 을 뜻하며 각 클래스로 예측될 확률 값이 들어가 있습니다.
  예를 들어 어떤 이미지에 대해서 고양이일 확률 70% 호랑이일 확률 20% 강아지일 확률 10% 이런식으로 나오게 되는 것입니다.
  

이 논문에서는 soft 방식을 사용하며 이 전에 리뷰했던 noisy student 모델에서도 soft한 방식을 사용합니다.

<br>
  
  
### Pseudo label의 손실함수(loss function)
  
  
CNN 파라미터 $\theta$는 categorical cross-entropy 공식으로 optimize 됩니다. 그 수식은 아래와 같다. 
  
$l^*(\theta) = -\sum_{i=1}^N \tilde{y}^T_ilog(h_\theta(x_i))$  
  
$h_\theta(x)$\는 softmax 함수를 거쳐 나온 확률 값을 의미하며 여기에 $log$를 취하는 것은 element-wise 하기 위함입니다. 
  
다시 자세히 뜯어 보자면 
  
- unlabel된 sample : $N_u$
  
- unlabel set : $D_u = \{x_i\}^{N_u}_{i=1}$
 
- labeled set : $D_l = \{x_i,y_i\}^{N_l}_{i=1}$
  
그리고, one-hot encoding을 위해 $y_i$는 $C$ 클래스들을 모든 데이터 ($N = N_l + N_u$)에 원핫 인코딩 해주며, 그 수식은 $y_i=\{0,1\}^C$, 이렇게 표현할 수 있습니다. 
  
여기서 핵심은 어떻게 레이블 되지 않은 샘플들 ($N_u$) 로부터 pseudo-labels ($\tilde{y}$)를 만들어 내는 것인데요..!
  

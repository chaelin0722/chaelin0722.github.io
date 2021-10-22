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

이 논문은 semi supervised learning(SSL)의 측면에서 pseudo label을 하는 이유와 pseudo label를 어떻게 구성하여 성능을 올릴 수 있었는지에 대해 설명하고 있습니다.

저는 이 논문에 대해서는 자세히보다는 pseudo label이 어떻게 이루어 지는지를 loss function과 구조적 측면에서 확인해 보며 어떤식으로 성능향상에 도움이 되는지에 대해서만 정리하고자 합니다!⭐️

<br>

먼저, 간략하게 pseudo label 을 하는 이유에 대해 적어보자면,

image classification 분야에 있어서 많은 데이터셋이 더 좋은 성능을 내는 것을 보고, 개발자들은 더 많은 데이터셋으로 학습시키고자 하였습니다. 

그래서 나온 방법이, label이 되지 않은 더 많은 데이터셋을 끌어와서 임의로 labeling을 해준 후 학습에 데이터를 추가하는 방식입니다. 이 방식은 지도학습과 비지도 학습을 섞은 형식이므로 semi-supervised learning(SSL)이라고 합니다.

여기서 임의로 labeling 하는 것은, 먼저 모델을 imagenet 등의 레이블이 있는 데이터셋으로 한번 학습을 시켜준 후, 그 모델로 label이 없는 데이터를 임의로 labeling을 진행합니다. 이때 생긴 임의의 label을 `pseudo label` 이라고 명명합니다. 

pseudo label로 이루어진 데이터를 추가하여 학습한 결과도 좋은 SOTA성능을 보인다는데!! 여기서 든 의문은 그럼 `pseudo label에서 잘못된 label이 생성되어 학습될 경우
더 안 좋은 결과를 낼 텐데.. 어떤방식으로 labeling 학습을 했는지가 궁금`😲😲하여 이 논문을 읽게 되었습니다.

이제 어떻게 labeling 을 잘 하도록 학습시켰는지 알아보도록 하겠습니다.

<br>

### Pseudo label 종류

#### 🌟 hard-pseudo-label
  네트워크의 예측값을 라벨로 사용하는 방식으로 one-hot vector을 생각하면 됩니다. 예를 들어 어떤 이미지에 대한 레이블 값이 고양이! 이렇게 나오면 그 이미지는 그대로 고양이로 레이블 됩니다.
  

#### 🌟 soft-pseudo-label
  반면 soft-label 방식은 softmax prediction값을 사용합니다. 즉, continuous distribution 한 label 을 뜻하며 각 클래스로 예측될 확률 값이 들어가 있습니다.
  예를 들어 어떤 이미지에 대해서 고양이일 확률 70% 호랑이일 확률 20% 강아지일 확률 10% 이런식으로 나오게 되는 것입니다.
  

이 논문에서는 soft 방식을 사용하며 이 전에 리뷰했던 noisy student 모델에서도 soft한 방식을 사용합니다.

<br>
  
  
## Pseudo label의 손실함수(loss function)
  
  
CNN 파라미터 $\theta$는 categorical cross-entropy 공식으로 optimize 됩니다. 그 수식은 아래와 같다. 
  
$l^*(\theta) = -\sum_{i=1}^N \tilde{y}^T_ilog(h_\theta(x_i))$  
  
$h_\theta(x)$는 softmax 함수를 거쳐 나온 확률 값을 의미하며 여기에 $log$를 취하는 것은 element-wise 하기 위함입니다. 
  
다시 자세히 뜯어 보자면 
  
- unlabel된 sample : $N_u$
  
- unlabel set : $D_u = \lbrace x_i \rbrace^{N_u}_{i=1}$
 
- labeled set : $D_l = \lbrace ( x_i,y_i ) \rbrace^{N_l}_{i=1}$
  
그리고, one-hot encoding을 위해 $y_i$는 $C$ 클래스들을 모든 데이터 ($N = N_l + N_u$)에 원핫 인코딩 해주며, 그 수식은 $y_i={\lbrace 0,1 \rbrace}^C$, 이렇게 표현할 수 있습니다. 
  
또, pseudo label 된 데이터를 다음과 같이 표현하며, 

$\tilde{D} = \lbrace ( x_i, \tilde{y_i} ) \rbrace^N_{i=1}$ 

이것을 가지고 레이블된 샘플들인 $N_l$에 있어서 $\tilde{y}=y$ 가 될 수 있는 것입니다.
  
  
여기서 핵심은 어떻게 레이블 되지 않은 샘플들 ($N_u$) 로부터 pseudo-labels ($\tilde{y}$)를 만들어 내는 것인데요..!

이전 연구는  one-hot encoding을 사용하는 hard 방식을 사용했습니다. 하지만 softmax를 사용하는 soft 방식이 더 좋은 성능을 내는 것을 발견하였고, 이전 방식에 soft-pseudo labeling 하는 방식을 적용하여 사용하였다고 합니다. 또한, `두 가지 정규화`를 추가적으로 사용하여 pseudo label이 더 잘 되도록 만들었다고 합니다.
  
두 가지 정규화를 적용하여 나온 최종 loss 수식은 일단 다음과 같습니다. 하나하나 따져보기 전에 전체적인 그림을 보고자 함입니다.
  
 $l=l^*+\lambda_AR_A+\lambda_HR_H$

  
<br>
  
그럼 두 정규화를 살펴보도록 하겠습니다.
  
## 📚 첫 번째 정규화 
  
pseudo label을 생성하기 시작하는 학습 초기에는 거의 부정확한 결과를 낸다고 합니다. 그 이유는 CNN은 loss를 줄이기 위해 같은 클래스로 예측해버리는 경향이 있기 때문입니다. 예를 들어 모든 데이터에 대해서 제각각인 클래스로 분류하기 보다는 같은 클래스로 주는 것이 loss가 더 적게 나오기 때문입니다. 시험을 볼 때 랜덤하게 찍는 것 보단, 같은 답으로 줄을 세워 찍으면 더 잘맞는 것과 같은 원리겠지요?
  
따라서 이 논문에서는 아래 공식을 추가하여 각 클래스들의 모든 샘플들의 영향력을 작게 합니다. 
  
  $R_A=\sum_{c=1}^Cp_clog({p_c \over \bar{h_c}})$
  
  
  $p_c$는 이전 class $c$에 대한 이전 확률 분포, $\bar{h}_c$는 class $c$를 dataset의 모든 샘플들에 대한 softmax 확률값들의 평균입니다.
  
  따라서 $p_c = {1 \over C}$ 입니다. 
  
 따라서 이전에 예측한 값에 전체 클래스 분의 예측한 값에 로그를 취한 값을 곱해서 값을 작게 업데이트를 시켜주게 되는 것 같습니다. 
  
  
 <br>
  
## 📚 두 번째 정규화 
 
다음 정규화는 약한 가이던스(부정확한 값들) 때문에 local minima에 빠질 것을 염려해 개별 class에 대한 soft-pseudo-label의 각 확률 분포에  `집중`하도록 하는 방법을 추가하게 됩니다.

정규화 식은 다음과 같습니다.
  
$R_H=-{1\over N}\sum_{i=1}^N\sum_{c=1}^Ch_\theta^c(x_i)log(h_\theta^c(x_i))$  
  
살펴보면, $h_\theta^c(x_i)$는 softmax의 output인 $h_\theta(x)$의 c class value를 의미하며, 이것으로 entropy를 구하는 공식을 취해주어서 각 샘플들에 대한 엔트로피값의 평균을 구하게 됩니다. 그리고 이렇게 나온 엔트로피들은 마이너스 값을 가지므로 맨 앞에 마이너스를 한번 더 취해주어 양수로 만들게 됩니다. 
  
따라서 이 것으로 나오는 값은 뻔히 예상되는 값(예측이 쉬운 값)일 수록 작은값, 결과 예측이 힘들수록 큰 값을 도출하게 됩니다. 그러면 결과 예측이 힘들수록 전체의 loss가 올라가게 되는 거겠죠!
  
  <br>
  
따라서 이 두 정규화를 합친 전체적인 loss 수식이 아래와 같이 되는 것입니다.
  
 
 $l=l^*+\lambda_AR_A+\lambda_HR_H$  
  
  여기서 $l^*$은, 제일 hard-pseudo-label방식에 softmax를 추가한 초기 공식으로, 
  
$l^*(\theta) = -\sum_{i=1}^N \tilde{y}^T_ilog(h_\theta(x_i))$  
  
  다시 풀어서 쓰면, 다음과 같은 아주 긴 함수가 되는 것입니다.

 $l=-\sum_{i=1}^N \tilde{y}^T_ilog(h_\theta(x_i))+\lambda_A \sum_{c=1}^Cp_clog({p_c \over \bar{h_c}})+ -\lambda_H{1\over N}\sum_{i=1}^N\sum_{c=1}^Ch_\theta^c(x_i)log(h_\theta^c(x_i))$  
  
  
  
  그런데 여기서 끝이 아닙니다..ㅎㅎ  `confirmation bias`의 문제를 해결하기 위해 mixup 이라는 개념을 또 추가하게 되었는데요, 
  
## 📚 Confirmation bias (확증 편향)
### mixup
  
  틀린 pseudo-label로 오버피팅되는 것을 확증편향 이라고 합니다. 또, pseudo-label을 하면서 잘못된 레이블로 학습을 계속 하게 되는 딜레마를 극복하기 위해 mixup이라는 개념을 도입하게 됩니다. 
  
이 개념은, `안정적인 모델이라면 특정 벡터의 선형결합에 대한 예측값이 레이블의 선형결합방식이 되어야 한다`는 개념에서 나오게 됩니다. 수식으로 설명드리자면, 
  
  random한 $(x_p, y_p), (x_q, y_q)$에 대해서 
  
  $x = \delta x_p + (1-\delta)x_q $
  
  $y = \delta y_p + (1-\delta)y_q $      
  
  가 성립된다는 것입니다. 
  
  특정 입력 벡터들을 선형결합한 벡터 $x$는 그 레이블 $y$ 또한 똑같은 방식으로 선형결합했을 때, 그 결과 또한 같이 매칭되어야 한다는 것입니다!
  
  이런식으로 레이블이 없는 데이터들에 대한 mixup 모델을 이용해 레이블을 예측한 후 이를 이용해 mixup을 진행하는 방식으로 이루어 집니다.
  
  위의 수식은 
  
$l^* = \deltal^*_p + (1 - \delta)l^*_q$

$l^* = \deltal$  
  
로 될 수 있고 따라서 loss $l^*$에 대해 재정의 하면
  
$l^* = -\sum_{i=1}^N\delta \lbrack\tilde{y}^T_{i,p}log(h_\theta(x_i))\rbrack+(1-\delta)\lbrack\tilde{y}^T_{i,q}log(h_\theta(x_i))\rbrack$ 가 됩니다.
  
  따라서 다음의 최종 수식 
  
 $l=l^*+\lambda_AR_A+\lambda_HR_H$

 
에서 $l^{*}$만 바뀌게 되겠죠!
  
  
  <br>
  
  여기까지 달려와 보았는데요, self-training 을 통한 noisy student 가 pseudo label 을 통해 학습을 하는데 대체 이 pseudo labeling은 어떻게 진행되는 것인지.. 제대로 레이블이 된 데이터가 추가되는게 맞는지 의문투성이었는데 이 논문을 보니 이해가 되었네요👍 
  
  이상하게 pseudo label 논문을 리뷰한 블로그가 거의 없어서 논문을 파고들어 공부하느라 힘이 들었지만.. 
  
  여기까지 파본 나에게 박수😭😭
  
  다음 포스팅에서 만나요🌱🌱
  
  
  
  
  
  
  
  
  

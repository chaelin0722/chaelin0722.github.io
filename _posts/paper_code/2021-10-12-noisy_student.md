---
title:  "[논문정리📃] Self-training with Noisy Student improves ImageNet classification"
excerpt: "-Noisy Student-"

categories:
  - paperReview
tags: [CNN, paperReview]
use_math: true

last_modified_at: 2021-10-13T08:06:00-05:00
classes: wide
---

## Self-training with Noisy Student improves ImageNet classification
#### - Noisy Student - 

[논문원본](https://arxiv.org/pdf/1911.04252.pdf)😙


이번 논문은 EfficientNet 에서 더 많은 데이터셋으로 학습시켜 새로운 SOTA 성능을 보이는 Noisy student에 대해 리뷰하도록 하겠습니다.

<br>

## 🌟 Introduction

그 동안 딥러닝은 이미지 인식에 있어서 눈부신 성공을 보였습니다. 하지만 SOTA(state-of-the-art) 성능을 내기 위해선 아주 큰 데이터셋이 필요하다는 한계에 부딪혔습니다.
이 문제를 보완하고자 나온것이 레이블 되지 않은 데이터셋(unlabeled-dataset)을 추가해 semi-supervised learning 을 사용하는 방식입니다. 베이스 모델은 EfficientNet 모델을 사용하였으며, 각 Efficient-B0~B7 모델로 학습한 결과 성능이 더 향상된 것을 볼 수 있습니다.

<img width="482" alt="무제 3" src="https://user-images.githubusercontent.com/53431568/136979401-4e69ff2c-a865-4a39-9465-49a351c3a823.png">

레이블되지 않은 데이터셋은 JFT-300M 데이터 셋을 사용하였으며 구글 내에서 만든 데이터셋이라고 합니다. 약 3억개의 이미지가 있어 약 125만개 정도되는 imageNet 데이터셋에 비하면 엄청난 크기입니다. 
JFT-300M 데이터셋에 대한 설명은 [JFT-300M 데이터셋이란?](https://chaelin0722.github.io/etc/JFT-300M/)포스팅한 글을 참고하세요!👍

#### 학습 방식은 다음과 같습니다.

먼저, (1) 선생님 모델을 레이블된 데이터셋(imageNet dataset)으로 학습시킨 후 (2) 학습된 선생님 모델로 레이블되지 않은 데이터셋(JFT-300M)의 수도 레이블(pseudo-label)을 생성해냅니다. 
그 후 (3) 학생 모델이 (1)과 (2)가 합쳐진 데이터셋으로 학습을 하게 됩니다. 그렇게 되면 선생님 모델과 같거나 더 큰 모델이 생성됩니다. 이후에 (4) step(2)와 (3)을 반복하면서 모델 성능을 더 높이도록 학습합니다.

논문에서 제시한 이미지는 아래와 같습니다.

![image](https://user-images.githubusercontent.com/53431568/136979502-9819e69e-21d9-4cc0-96c1-87efb47698a9.png)


#### 정리하자면, 

### Noisy student Training process

<img width="1052" alt="무제" src="https://user-images.githubusercontent.com/53431568/136976139-b73c4d9e-73cb-4fe5-b195-25d85e49c8b6.png">


단, 여기서 의문인 것은 pseudo label이 초기에는 불명확할 텐데 어떻게 해서 반복하면서 학습하면 성능이 좋아지는지 그 부분에 대한 설명이 좀 부족하다. soft labeled 를 사용하였다고 짧게 한줄 설명이 있는데, 이 부분에 대해서는 이 논문의 첫번째 레퍼런스인 [Pseudo-Labeling and Confirmation Bias in Deep
Semi-Supervised Learning](https://arxiv.org/pdf/1908.02983.pdf)을 참고하면 도움이 된다. 

### 📚 이 pseudo label 논문에 대해서 정리한 글은 [[논문 정리]Pseudo-Labeling and Confirmation Bias in Deep Semi-Supervised Learning](https://chaelin0722.github.io/paperreview/pseudo_label/)에 정리해 두었으니 필요하다면 참고!☘️


이렇게 레이블 되지않은 JFT-300M 데이터셋을 추가해 imageNet 에 대한 `SOTA 성능을 향상시켰으며 동시에 견고한 모델`이 생성되었습니다. 여기서 견고하다는 것은 논문에서 'robustness'라고 표현하였는데,
robustness한 모델을 만들었다는 것은 모델에 들어오는 데이터가 처음보는 데이터이거나 원래 학습했었던 범주와 좀 동떨어진 범주의 이미지도 잘 분류할 수 있다는 뜻입니다.

아래는 noisy student방식으로 학습한 모델이 ImageNet dataset들의 SOTA 성능을 보이는 것을 나타내는 지표입니다.

<img width="1001" alt="무제 2" src="https://user-images.githubusercontent.com/53431568/136976306-a001d73b-af0c-4abc-bd3b-6627fc5547ab.png">

표의 맨 왼쪽은 ImageNet 데이터셋이며 차례대로 데이터셋과 성능지표에 대한 설명을 하자면,

> ImageNet-A : 구분하기 어려운 200 classes의 이미지들로 구성된 dataset
>  
> ImageNet-C : 15가지 corruption으로 실험된 데이터셋이며 변화가 아주큰 데이터셋이다. mCE(mean corruption error)를 성능지표로 사용하고 있다. 
> 
> ImageNet-P : ImageNet-C 에 비하면 약간의 노이즈나 기울어짐 정도로 augmentation된 데이터셋이다. 적은 변화의 데이터셋이라고 생각하면 될 것 같다. mFR(mean flip rate)을 성능지표로 사용하고 있다.
> 
> mCE(mean corruption error) : image noise에 얼마나 강한 모델인지를 보는 지표로 noise가 있는 데이터도 잘 예측해야하므로 수치가 작을 수록 좋은 성능을 나타낸다.
> 
> mFR(mean flip rate) : perturbation(작은 변화)이 바뀔 때 top-1 prediction이 바뀔 확률을 나타내며 변화가 일어난 데이터의 예측에 변화할 확률이 적어야 좋은 성능이므로 수치가 적을 수록 좋은 성능을 나타낸다.



### 👑 iterative training 을 통해 학습한 최고 성능 모델

iterative training 을 통해 학습한 최고 성능 모델은 다음과 같다. 처음 teacher과 student 모델을 EfficientNet-B7으로 학습시킨 후 그 이후로는 EfficientNet-L2로 3번의 반복을 통해 얻은 모델이 SOTA성능을 보인다고 한다.
여기서 EfficientNet-L2 는 efficientNet의 파라미터값의 scaling을 크게 하여 생성한 모델이다. 자세한 scaling 방법에 대해서는 [EfficientNet 논문리뷰](https://chaelin0722.github.io/paperreview/efficientnet/)에서 확인할 수 있다.

보면 label 데이터셋과 unlabel 데이터셋의 batch 비중을 다르게 해주었는데 unlabel 데이터셋을 더 많이 학습하도록 해주었고 그 결과 더 좋은 성능을 보인다고 한다. 

<img width="548" alt="무제 4" src="https://user-images.githubusercontent.com/53431568/136986507-027a7c02-58b1-44ed-aab1-c9b17818a39e.png">

<br>

### 👑 noise 가 중요한 이유

training을 할 때, student 모델은 최종 데이터셋에 noise를 준 (augmentation을 가한) 데이터셋으로 학습을 하는데, 이때의 noisesms SD(stochastic depth), dropout, data-augmentation 이 세 방식을 사용한다.

아래 표를 분석해보면 기본 EfficientNet-B5 모델(노란색)보다 Noisy student training을 한 모델(빨간색)의 성능이 더 높은것을 확인할 수 있다. 또한, 그에 비해 augmentation을 하지 않은 모델들은(빨간색) 성능이 더 낮아짐을 볼 수 있다.

그리고 한편, teacher 모델에 augmentation을 주면 반대로 또 성능이 낮아지는 것을 알 수 있다. 하지만 전체적으로 noisy student training 을 거친 모델들은 augmentation 여부와 상관없이 원래 베이스 efficientNet 모델보다 더 좋은 성능을 보이는데 
이것은 논문에서도 아마 training 때 stochastic gradient descent 를 사용하기 때문에 더 높아진 것이라고 가설을 세웠다.

<img width="818" alt="무제 5" src="https://user-images.githubusercontent.com/53431568/136988078-05388d16-6044-4084-848f-b35c2450d8d8.png">


<br>

## 🌎 Experiments Result

이번 논문은 이전 SOTA 성능 모델보다 더 적은 파라미터와 extra data 수로 더 높은 성능을 달성했다는 점에서 의미가 있다.

<img width="649" alt="무제 6" src="https://user-images.githubusercontent.com/53431568/136989373-b9ddcc86-de0b-4b7f-baa5-a05f779a2fb1.png">




<br>

#### 참고

[1] [https://deep-learning-study.tistory.com/554](https://deep-learning-study.tistory.com/554)

[2] [https://hoya012.github.io/blog/Self-training-with-Noisy-Student-improves-ImageNet-classification-Review/](https://hoya012.github.io/blog/Self-training-with-Noisy-Student-improves-ImageNet-classification-Review/)




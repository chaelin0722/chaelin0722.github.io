---
title:  "[Paper Review] Prototypical Networks for Few-shot Learning"
excerpt: ""

categories:
  - fer
  - few-shot
tags: [fer, fewshot, paperReview]
last_modified_at: 2022-05-06T08:06:00-05:00
---

## Prototypical Networks for Few-shot Learning

오늘 리뷰할 논문은! [논문 원본🌼](https://arxiv.org/abs/1703.05175)

Recently, I've been studying Few shot learning (FSL). Even though I had difficulty in fully understanding what really fsl means, I figured out that this method is interesting. So, I am trying to apply this concept to Facial Expression Recognition problem, which are known as fine-grained classification. I beliebe that fsl will solve the problems of FER has.

얼굴감정인식 문제를 해결하기 위해 Few-shot learning 개념에 대해 공부해보고 있다. 이 개념을 온전히 이해하기에 많은 어려움들이 있었지만 ㅎㅎ(논문에는 완벽히 FSL이 무엇인지 기술되어있기 보다는 이 방법론을 여러 논문에서 다루는 형태가 다 다르기 때문에 논문들을 몇개씩 보면서 정리를 해보는 시간이 필요하였다.)

먼저, 이 논문에 들어가기 앞서, FSL 이 가지는 목표를 살펴보면서 왜, FSL 방법론이 나왔고, 이것을 통해 무엇을 하고싶은것인지 정리를 해보자!

FSL이 가지는 목표는 두 가지로 나누어서 살펴볼 수 있다. 일단 few-shot learning 에서 few-shot 은 저번 포스팅에서도 다뤘듯이, 한번 학습할때 하나의 클래스당 적은 갯수(few-shot)의 이미지를 사용하게 된다. 왜 이렇게 사용하는가?! 바로 데이터가 적은 classification 문제에 대하여 오버피팅을 막기 위해 학습하는 방법을 학습하게 하게 하기 위함이다. 

즉, overfitting이 안되려면 적은 데이터로 학습하여도 다른 새로운 테스트 데이터가 들어왔을 때 잘! 일반화가 되어야한다. 따라서 새로운 클래스(레이블)데이터가 들어왔을때도 잘 분류할 수 있게, 일반화를 잘 하는 것을 목표로 삼는다. 

#### 정확히는 1) 분류기(classifier)가 한클래스당 적은 데이터로 학습하여도 잘 일반화가 되게 하는 것 2) 분류기가 테스트 환경에서 아주 새로운 클래스가 나와도 잘 일반화 되게 하는 것! 

또한, FSL의 overfitting 문제를 해결하기 위해 meta learning 방법론을 사용하는데, 큰 개념은 test 환경과 train 환경을 유사하게 조성하여서 일반화를 잘 되게 하는 것으로 이해하면 될 것이다. 

### Purpose of FSL : 

#### 1) learn a classifier that generalizes well even when trained with a limited number of training instances per class.
   (Snell, Jake, Kevin Swersky, and Richard Zemel. "Prototypical networks for few-shot learning." Advances in neural information processing systems 30 (2017).)

#### 2) classifier must be adapted to accommodate new classes not seen in training, given only a few examples of each of these classes. 
   (Koch, Gregory, Richard Zemel, and Ruslan Salakhutdinov. "Siamese neural networks for one-shot image recognition." ICML deep learning workshop. Vol. 2. 2015.)
   
   (Lake, Brenden, et al. "One shot learning of simple visual concepts." Proceedings of the annual meeting of the cognitive science society. Vol. 33. No. 33. 2011)


#### Meta Learning strategy: 
To solve the overfitting problem that FSL has, considering relationship between instances in the test set can achieve larger improvement.
In episodic training, it mimics the real test environment containing few-shot support set and unlabeled query set. The consistency between training and test environment alleviates the distribution gap and improves generalization. 

<br>

### Match learning 

FSL 의 문제를 해결하기 위한 방법론들 중 성능을 잘 내는 방법을 소개합니다. 바로 Match learning 이라는 방법론인데, attention 메커니즘을 사용하여 레이블 되지 않은 클래스(쿼리데이터)를 잘 예측하도록 한다. 각 서포트셋(support set)끼리의 cosine similarity와 query set(==batch set) 과 support set 들 간의 cosine similarity 를 계산하여서 어떤 서포트셋에 해당하는지 학습하게 된다. 

Approaches that have made significant progress in few-shot learning

Uses an attention mechanism over a learned embedding of the labeled set of examples to predict classes for the unlabeled points. 
The use of episodes make the training problem more faithful to the test environment and thereby improves generalization. 


![image](https://user-images.githubusercontent.com/53431568/167287087-c6738d0e-f7ee-4ba3-8a5e-d04157dabaaf.png)

<br>

그럼 이 논문은 prototype 을 사용한 방법론인데 prototype 이 무엇인가?

### What is prototype?

![image](https://user-images.githubusercontent.com/53431568/167287222-77a460f4-c5bb-463b-8dcc-d713594f0a66.png)

위 그림과 같이 서포트 셋들의 이미지 텐서값(x)을 encoder 에 넣어서 z 라는 임베딩된 텐서 값을 생성하면, 같은 서포트셋의 임베딩텐서들의 평균을 구한다. 그것이 각각 c1, c2, c3 가 되는 것이다. 그럼 이렇게 생성된 c1, c2, c3 를 새롭게 들어오는 query 값과의 거리 유사도를 구해서 (여기 논문에서는 euclid 을 사용한다.) 각 쿼리가 어떤 레이블에 속하는지 판단한다. 

마치 k-means 클러스터링과 같은 원리이며, 아주 심플한 로직이다. 

*X 는 이미지 텐서, z 는 임베딩된 텐서 값 


각 prototype을 어떻게 계산하는지에 대한 수식과, 거리 유사도에 대한 probability 구하는 수식은 다음과 같다.

![image](https://user-images.githubusercontent.com/53431568/167288540-fed07aee-da7a-4f2c-bb0d-bbd4d0c4b973.png)


이 계산을 pseudo 코드로 나타낸 식은 아래와 같다. 

![image](https://user-images.githubusercontent.com/53431568/167288665-1c2978a3-f738-4d67-a97c-3d3b8be8e477.png)

마지막에 loss 를 업데이트하는 수식을 정리해 보면.. log 함수를 취하게 되면 % 는 - 가 되므로 아래와 같이 표현이 되는 것을 확인!

![화면 캡처 2022-05-08 174221](https://user-images.githubusercontent.com/53431568/167288732-0c2ba75c-dc15-4dca-8d7e-558701c13cd9.png)


<br>

### Discussion
- 이 논문의 의의는 다른 meta learning의 접근법 보다 간단한 로직으로 FSL의 overfitting 문제를 효과적으로 줄이면서 SOTA를 달성하였다는 것에 있다. 
- cosine 유사도 보다 euclid 유사도를 사용하였는데, euclid 계산에 적절한 방법론이 protonet 이 아닐까 하는 생각이 든다. 그렇다면 다른 거리 계산법에 적절한 또 다른 meta learning 기법을 만들 수 있지 않을까..?

<br>

### references

[1] [https://rhcsky.tistory.com/9](https://rhcsky.tistory.com/9)

---
title:  "[code] ResNet56 with Cifar-10"
excerpt: "code"

categories:
  - Deeplearning
  - CNN
  - code
tags: [Deeplearning, CNN, code]
 
classes: wide

last_modified_at: 2021-08-03T07:06:00-05:00

---

### Deep Residual Learning for Image Recognition 구현

<br>

이 논문의 resnet은 Cifar-10 데이터 셋으로 학습이 가능한데, 논문에서 제시하는 Cifar-10 을 위한 56레이어의 Resnet으로 구현하였다.

cifar-10 데이터셋 학습 조건은 다음과 같으며 동일하게 수행하였다.

Epoch : 200
BATCH SIZE: 128
Learning rate decay


앞서 해석한 논문을 보면 디테일한 설명을 볼 수 있다 => gogo!
[Deep Residual Learning for Image Recognition](!!)


<br>
<script src="https://gist.github.com/chaelin0722/5994c8354671942b5631af002077f713.js"></script>
<br>


### [학습결과]

original paper work와 비교했을 때 비슷한 그래프 양상을 보임

![image](https://user-images.githubusercontent.com/53431568/127953986-f9a3c5ac-e10f-4c79-923f-19ad0480d1f5.png)

![image](https://user-images.githubusercontent.com/53431568/127953960-7359225b-bee1-4a81-a717-58b9e19c8151.png)

Error를 비교했을 때 내 모델의 정확도가 0.92 정도로, 논문에서 제시하는 0.93과 비슷하게 나온 것을 볼 수 있다.

![image](https://user-images.githubusercontent.com/53431568/127953980-d7d857dc-a677-4e01-9ffd-f60b0ba1a04a.png)

![image](https://user-images.githubusercontent.com/53431568/127956843-b0302900-4497-4ede-b052-fb7cd86bc367.png)


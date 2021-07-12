---
title:  "[에러분석] tensorflow에서 accuracy가 0.0000e+00으로 발생한 경우"
excerpt: "error while training"

categories:
  - Deeplearning
  - CNN
  - study
tags: [Deeplearning, CNN,study]
classes: wide

last_modified_at: 2021-07-12T08:06:00-05:00
---

## tensorflow 학습 중 검증손실 및 정확도가 고정되어 변하지 않는다.

아래와 같이 loss는 8.0064, accuracy: 0.0000e+00 으로 고정된 값으로 학습되는 에러가 발생

![image](https://user-images.githubusercontent.com/53431568/124251328-c433bb00-db60-11eb-88de-b18e2f65375c.png)


- 처음에는 loss값이 nan으로 되는 에러가 생겼는데 그건 output data를 학습할 이미지 클래스 수에 맞게 해주었더니 고쳐졌다.
- 하지만 고치고 난 후 이번에는 accuracy가 0.0000e+00이 되는 문제가 발생..! 심지어 loss는 8.0064로 고정되어버렸다.


### 해결방법!

구글링을 통해서 이 문제에 대한 다양한 해결방법을 볼 수 있었다.

대부분의 해결책으로는
> 1. optimizer을 변경하기
> 2. learning rate를 충분히 작게 주기 (ex) 0.1 -> 0.001)
> 3. 학습할 이미지의 클래스 수 확인하기

여기서 나는 1번으로 고칠 수 있었다. optimizer을 SGD로 주고, learning rate는 0.1로 주었다. 내 경우에는 lr값이 많이 작았나보다.

고쳐서 나오는 화면이다.

![image](https://user-images.githubusercontent.com/53431568/125307875-675bb000-e36b-11eb-89ef-09d41897e9d0.png)

여기서의 문제는 보는것과 같이 비슷한 값이 반복되는 것이다. 과연 잘 학습이 되고있는지가 약간 의문이다. 더 지켜봐야겠다!


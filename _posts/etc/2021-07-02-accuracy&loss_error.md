---
title:  "[에러분석] tensorflow에서 accuracy가 0.0000e+00으로 발생한 경우"
excerpt: "error while training"

categories:
  - Deeplearning
  - CNN
  - study
tags: [Deeplearning, CNN,study]
classes: wide

last_modified_at: 2021-07-02T08:06:00-05:00
---

## tensorflow 학습 중 검증손실 및 정확도가 고정되어 변하지 않는다.

아래와 같이 loss는 8.0064, accuracy: 0.0000e+00 으로 고정된 값으로 학습되는 에러가 발생

![image](https://user-images.githubusercontent.com/53431568/124251328-c433bb00-db60-11eb-88de-b18e2f65375c.png)


- 처음에는 loss값이 nan으로 되는 에러가 생겼는데 그건 output data를 학습할 이미지 클래스 수에 맞게 해주었더니 고쳐졌다.
- 하지만 고치고 난 후 이번에는 accuracy가 0.0000e+00이 되는 문제가 발생..! 심지어 loss는 8.0064로 고정되어버렸다.

찾아보니 model.fit()함수 안의 매개변수인 steps_per_epoch의 설정을 잘 해줘야 한다는데.. 문제가 해결되지 않고있다.

(아직 해결 중)

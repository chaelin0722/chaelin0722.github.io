---
title:  "[EXPERIMENT] FEW SHOT 학습과 고민 노트 2"
excerpt: ""

categories:
  - FSL
  - prototype
tags: [Deeplearning, FSL, study]

last_modified_at: 2022-08-10T08:06:00-05:00
---

## 이번에는 다양한 데이터셋으로 여러 학습과 인퍼런스를 진행해 보았다.

먼저,

### RAF-DB 데이터로 진행한 실험!

RAF-DB 데이터는 기본 감정 6가지(무표정+1) 총 7가지와  복합감정 11가지를 갖고 있다. 아래 이미지에는 복합감정이 12개인데 데이터셋에는 11개의 레이블만이 존재한다.. 왜인지는 모름

<img width="1537" alt="sample" src="https://user-images.githubusercontent.com/53431568/183815126-a57ee38c-f443-4a26-8c9a-19a77bd343c2.png">

데이터셋 안에는 감사하게도 이미 align 된 이미지도 존재하기 때문에, 그 얼굴로 사용하였다.

복합 감정이미지와 기본 이미지 모두 train/test 의 여부가 파일 이름으로 존재하는데, 일단 이번 실험에서는 기본감정의 모든 데이터를 (총15339개) train에, 복합감정의 모든 데이터(총 3954개)를 validation 으로 사용하였다. 

|Train|Eval|
|------|---|
|3way-5shot 5query|4way-5shot 1query|


- epoch 500
- evaluation_episodes = 100  # Number of n-shot classification tasks to evaluate the model with
- episodes_per_epoch = 100  # epoch 당 들어갈 episode 개수


### 위에서 학습된 모델로 AFEW 데이터를 test 해보았다.
few-shot 모델로 test 만 하고싶다면 --> [여기 참고!]()

|Test accuracy|
|------|
|??%|



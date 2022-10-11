---
title:  "[EXPERIMENT] FEW SHOT 학습과 고민 노트 2"
excerpt: ""

categories:
  - fer
  - few-shot
  - Attention
  - Relation Network
tags: [fer, fewshot, paperReview]

last_modified_at: 2022-08-10T08:06:00-05:00
---


기존 학습방식과 비교해볼 때, few-shot 은 어떤 메리트가 있는지, 오버피팅의 문제를 과연 잘 해결하는지.. 라는 생각에 도달하면서 여러번의 학습을 시도해보았다.
다양한 데이터셋으로 여러 학습과 인퍼런스를 진행해 보았는데, 정리하자면 다음과 같다. 코드를 보고싶다면 [github](https://github.com/chaelin0722/Few-shot-FER/tree/master/few-shot)에 잘 정리해두었다.

먼저,

### 😀 basic emotion train, compound emotion val -> test with AFEW(basic emotion) 

이번 실험은 처음으로 RAF-DB 데이터를 사용해보았다.

RAF-DB 데이터는 기본 감정 6가지(무표정+1) 총 7가지와  복합감정 11가지를 갖고 있다. 아래 이미지에는 복합감정이 12개인데 데이터셋에는 11개의 레이블만이 존재한다.. 왜인지는 모름

<img width="1537" alt="sample" src="https://user-images.githubusercontent.com/53431568/183815126-a57ee38c-f443-4a26-8c9a-19a77bd343c2.png">

데이터셋 안에는 감사하게도 이미 align 된 이미지도 존재하기 때문에, 그 얼굴로 사용하였다.

이 실험에서는 unseen 데이터에 대해서 과연 잘 분류를 할지 여부를 파악하기 위해 진행하였다. 
복합 감정이미지와 기본 이미지 모두 train/test 의 여부가 파일 이름으로 존재하는데, 일단 이번 실험에서는 기본감정의 모든 데이터를 (총15339개) train에, 복합감정의 모든 데이터(총 3954개)를 validation 으로 사용하였다. 

#### 조건
- epoch 500
- evaluation_episodes = 100  # Number of n-shot classification tasks to evaluate the model with
- episodes_per_epoch = 100  # epoch 당 들어갈 episode 개수
- drop_lr_every = 40


#### 결과
<img width="1135" alt="1결과" src="https://user-images.githubusercontent.com/53431568/183821585-37ba911a-63ed-4754-9990-74b3633bc048.png">

|Train|Eval|
|------|---|
|3way-5shot 5query|4way-5shot 1query|

|ACC|Loss|
|------|---|
|0.87|0.3093|

eval 해야하는 데이터가 3954개 밖에 없어서 90%는 넘게 찍을 줄 알았는데 생각보다 작게 나왔다. 


### 위에서 학습된 모델로 AFEW 데이터를 test 해보았다.
AFEW 데이터의 validation 데이터셋을 사용하였다.
모델 인퍼런스 구조가 같아야 하므로 test 또한 4way-5shot 1query 로 진행이 된다. 

|Test accuracy|
|------|
|37%|

4개 중에 하나 맞추는건데.. 이리도 못맞추다니.. 실망스럽다.

<hr>

### 😀 다음 실험은,
7개의 레이블에 대해서 분류하는 모델들과 비교해야하므로 7way 로 진행하였다.

<img width="775" alt="image" src="https://user-images.githubusercontent.com/53431568/184006650-3a0a7aa6-70a2-4864-8419-db040e858d1e.png">

|Train|Eval|
|------|---| 
|3way-5shot 5query|7way-1shot 1query|

|ACC|Loss|
|------|---|
|0.89|0.27|

|Test accuracy|
|------|
|16%|



<hr>

### 😀 AFEW 로 학습 -> compound emotion 테스트 하는 실험

AFEW 데이터의 가능성을 보기위해 다양하게 실험해보고 있다. 

#### 조건
- 일단 먼저 AFEW 학습은 모든 데이터로 하였다 (절반만 사용한 버전 아님)
- epoch 500
- evaluation_episodes = 100  # Number of n-shot classification tasks to evaluate the model with
- episodes_per_epoch = 100  # epoch 당 들어갈 episode 개수
- drop_lr_every = 40


<img width="747" alt="image" src="https://user-images.githubusercontent.com/53431568/184006682-acc06b75-08ec-422a-86ee-ffed47c6f983.png">

|Train|Eval|
|------|---|
|3way-5shot 5query|4way-5shot 1query|

|ACC|Loss|
|------|---|
|0.95|0.12|

|Test accuracy|
|------|
|33%|



### 😀 두 데이터를 섞은 train dataset! 만들어서 학습하기! 1 

기존 RAF-DB 의 basic emotion 데이터에다가 AFEW의 데이터를 조금 추가해서 학습해본다(과연 AFEW의 적은 데이터로도 결과가 괜찮을지?)

- Angry (004108160, 002338014, 002613894) => 총 140개
- Disgust (002138680, 005923627, 00323664442) => 총 146개
- Happy (000231280, 001335200) => 총 148개
- Fear (002828240, 011613697, 01306270, 013949200) => 총 159개
- Neutral (005533800, 000909960, 002144977) => 총 135개
- Sad (002302057, 002643280, 005206720) => 총 139개
- Surprise (005913697, 002730440, 000252168, 013437680) => 총 153개
==> 1020장

따라서 총 16359 이미지를 학습데이터로 사용한다


images background 에서 사용된 카테고리별 이미지 파일 갯수
|Angry |Sad|Happy|Disgust|Neutral|Fear|Surprise|
|------|---|---|---|---|---|---|
|1007|2599|6105|1023|3339|514|1772|


- evaluation 데이터는 AFEW의 VALID 이미지로 사용하였다. (총 18990장)

|Train|Eval|
|------|---|
|3way-5shot 5query|4way-5shot 1query|

|ACC|Loss|
|------|---|
|0.95|0.12|

또한, imageNet으로 pretrained 된 Resnet50 으로 같은 데이터를 학습한 결과 val acc가 개선되지 않아 일단, epoch 71 에서 멈추었다. 

|ACC|Loss|
|------|---|
|0.18|2.24|

일반 cnn 모델인 resnet과 비교하기 위해서 evaluation의 support set에 들어가는 이미지를 train에서 가져오고자 한다. 
각 train이미지에서 임의로 50개씩을 evaluation의 support 데이터로 들어가게 하여 query 데이터를 전부 evaluate 할 수 있게 해보았다. 

images background 에서 사용된 카테고리별 이미지 파일 갯수

|Angry |Sad|Happy|Disgust|Neutral|Fear|Surprise|
|------|---|---|---|---|---|---|
|957|2549|6055|973|3289|464|1722|



### 😀 두 데이터를 섞은 train dataset! 만들어서 학습하기! 2

위의 실험의 경우 각 학습된 이미지들이 고르지 않아, 모든 카테고리마다 약 3000개씩 되도록 맞추어준 후 실험을 해 보았다.  



### 그 외 잡다한 생각 끄적끄적
-  학습에 AFEW 섞어서 학습하기
-  이미지 하나씩 인퍼런스하는 방법 찾기..!
-   AFEW는 비디오니까..
frame 처리된 애들 inference 해서 가장 많이 나온 감정으로 정답으로 계산하는건..?
이제껏 프레임 처리된 애들 개별로 계산했는데..

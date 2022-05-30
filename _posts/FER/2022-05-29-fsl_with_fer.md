---
title:  "[EXPERIMENT] FEW SHOT 학습과 고민 노트 1"
excerpt: ""

categories:
  - FSL
  - prototype
tags: [Deeplearning, FSL, study]

last_modified_at: 2022-05-29T08:06:00-05:00
---

## Prototypical Networks for Few-shot Learning
I followed the code from [https://github.com/oscarknagg/few-shot](https://github.com/oscarknagg/few-shot)


### 0. My server
MY OS
Ubuntu 18.04LTS

### 1. git clone

~~~
git clone https://github.com/oscarknagg/few-shot.git
~~~



### 3. Process start from git clone

1. need to edit requirement.txt

MarkupSafe==1.0  -> MarkupSafe==1.1.0
pkg-resources==0.0.0  -> #pkg-resources==0.0.0
torch==1.0.0 -> torch

if something is not working.. follow below blog's instruction. 

https://exerror.com/note-this-error-originates-from-a-subprocess-and-is-likely-not-a-problem-with-pip/

<hr>

-- Before beginning the experiment, let's do the preprocessing!


#### [TRAIN]

먼저, 전처리는 afew데이터에 있는 face 그대로 

데이터 전처리시 고민하는 것.

1. 눈 / 코 / 입 이 전부 나오지 않은 경우 학습에 추가여부

ex) 000102534, 000331374, 000628840

2. gray vs color

4. 사물에 얼굴이 가려지는 이슈 

ex) 000231280(bottle), 000359934(glasses), 000450454(glasses), 000429620(glasses), 001637640(sunglasses), 002644680(hand), 003050760

5. 모든 화면이 검정에 가까운 어두움(얼굴인지 잘 모르겠는)

ex) 000317144(delete), 000341040(saved.. check it again), 000343800(saved.. check it again), 000456960(delete), 001846440

6. alignment strange..

ex) 000340094

7. hard to recognize whether it's face or not (DELETE)

ex) 000422800, 000451774, 000633160, 001143440, 001419640, 002643280, 002852720, 003316894

8. more than one face ==> need to know which face represents the emotion

ex) 000455174, 000945960, 001748527, 001846440, 002811160, 002901240, 003118400, 003321880, 003349414

9. side face

ex) 000509960, 001114600, 001340000 ...


## analyze dataset

fewest frame in one folder : 4
gray scale

<br>

#### [VALID]

maybe you don't have to leave it.... it will work as a test so...





#### 학습 시 직면한 에러

1. 
~~~
ValueError: Cannot take a larger sample than population when 'replace=False'
~~~
위의 에러는 k shot 할건데, k 보다 내가 가진 class의 수가 적을때 발생한다.. 허허 나는 7개의 감정이 있는데 4개를 background, 3개를 evaluation으로 하였으니 k 개수는 적당해 3개로 해주었다.



~~~
return torch.stack(batch, 0, out=out)
RuntimeError: invalid argument 0: Sizes of tensors must match except in dimension 0. Got 52 and 32 in dimension 2 at /pytorch/aten/src/TH/generic/THTensor.cpp:711
~~~
이미지 사이즈 문제 이미지 사이즈가 82 여야 한다. 그 이유는 찾는중.

#### 전체적 query, support, n-way k-shot, query, train/test 설명 

images trained per one episode 

- on train
n*k + n*q = n(k+q)

ex) if 3 way 1 shot, 5 query ==> 18 images

batch = [support class1, support class2, support class3, query1, query2, query3, query1,query1, query2, query3, query1, query1, query2, query3, query1,query1, query2, query3]

- on test
n*k + n*q = n(k+q)

ex) if 1 way 1 shot, 1 query ==> 2 images

batch = [support class1, query1]


==> if episode per epoch = 10

images trained per one epoch = ( train_images + test_images ) x episode_per_epoch

ex) (18+2) * 10 = 200

여기서 주의사항!

support is not in query..! but, each episodes can be overlapped because it's randomly chosen!

<br>

#### 알아야 & 해야 할것

1. wandb 이거 깔아서 graph로 볼 수 있게 하기 
2. model의 Module 을 efficient / resnet18 로 바꾸기이이이~!

<br>
<hr>

## Train trial

### 0529
TRAIN SET 001604560 까지..



### 0530 trial

일단, frame 이 폴더 별로 잘 few-shot의 sampling대로 들어가는지 확인하기 위해 작업중.. 일단 color데이터로 학습하는 중

preprocessing TRAIN SET 003349414 까지..



### 0531 trial

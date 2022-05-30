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

ex) 000231280(bottle), 000359934(glasses), 000450454(glasses), 000429620(glasses)

5. 모든 화면이 검정에 가까운 어두움(얼굴인지 잘 모르겠는)

ex) 000317144(delete), 000341040(saved.. check it again), 000343800(saved.. check it again), 000456960(delete)

6. alignment strange..

ex) 000340094

7. hard to recognize whether it's face or not (DELETE)

ex) 000422800, 000451774, 000633160, 001143440, 001419640

8. more than one faces

ex) 000455174, 000945960

9. side face

ex) 000509960, 001114600, 001340000

<br>

#### [VALID]

maybe you don't have to leave it.... it will work as a test so...




------------------------------------------
0529
TRAIN SET
001604560
까지..

0530에 모두 마무리하고 FEW-SHOT 돌리는것 목표!


----- 0530 trial

일단, frame 이 폴더 별로 잘 few-shot의 sampling대로 들어가는지 확인하기 위해 작업중.. 일단 color데이터로 학습하는 중

#### 학습 시 직면한 에러
~~~
ValueError: Cannot take a larger sample than population when 'replace=False'
~~~
위의 에러는 k shot 할건데, k 보다 내가 가진 class의 수가 적을때 발생한다.. 허허 나는 7개의 감정이 있는데 4개를 background, 3개를 evaluation으로 하였으니 k 개수는 적당해 3개로 해주었다.




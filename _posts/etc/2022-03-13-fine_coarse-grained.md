---
title:  "[DL] Coarse-grained VS fine-grained 용어정리"
excerpt: ""

categories:
  - CNN
tags: [Deeplearning, CNN, study]

last_modified_at: 2022-03-13T08:06:00-05:00
---

## Coarse-grained VS Fine-grained 용어정리

논문을 읽다보면 classification 을 설명할때, `coarse-grained`, `fine-grained` 라는 용어를 심심치 않게 발견하고 있다.

이 용어는 프로그래밍 용어로 썼었는데 이제는 딥러닝 분야에도 적용하여 의미를 확장시켜 사용하는 것 같다.


### Coarse-grained

- 작업을 큰 단위의 프로세스로 나누어서 실행하는 방식
- classification 에 있어서는 이미지 데이터셋이 주어졌을 때, 하나의/ 각 class 를 분류하는 작업

<br>

심플하게 생각하면 우리가 지금 이미지 분류를 하는 작업을 할때 ImageNet, Mnist 같은 데이터셋을 사용하는데 이 데이터는 수 많은 class 로 레이블링이 되어있고 이를 분류하는 것이 목적인데, 이런 classification을 coarse-grained 라고 부르는 것 같다.

(Imagenet의 class 예시)

![imagenet](https://user-images.githubusercontent.com/53431568/158095004-92e82ad0-5571-4387-b048-6049ac773a46.png)

<br>

### Fine-grained 

- 작업을 큰 단위 안에서도 세분화된 작업 프로세스를 만들어서 실행하는 방식
- classification 에 있어서는 동물의 species(품종)나 브랜드 종류와 같이 분류하기 어려운 데이터셋으로 분류하는 작업

예를 들어 강아지 안에서도 셰퍼드, 그레이하운드, 보더콜리, 푸들 등등 수 많은 종의 강아지가 있다. 큰 분류인 "강아지" 라는 것은 맞추기 어렵겠지만 그 안의 소분류인 "품종"을 맞추는 것은 매우 어려운 일이다. 이런 것을 fine-grained classification 이라고 부르는 것을 확인할 수 있다. 

![92cea4dd-25d0-4478-a4de-e17f4bebb32d](https://user-images.githubusercontent.com/53431568/158095956-643249bd-8cc2-467a-b3df-b46b21a88352.jpg)


<br>

#### Reference

[1] [https://paperswithcode.com/task/fine-grained-image-classification](https://paperswithcode.com/task/fine-grained-image-classification)

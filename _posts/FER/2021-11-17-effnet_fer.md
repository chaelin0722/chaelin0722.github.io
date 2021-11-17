---
title:  "[논문정리📃] Facial expression and attributes recognition based on multi-task learning of lightweight neural networks"
excerpt: "-Multi-task EfficientNet-B2-"

categories:
  - fer
tags: [fer,CNN, paperReview]
use_math: true

last_modified_at: 2021-11-17T08:06:00-05:00
classes: wide
---

## Facial expression and attributes recognition based on multi-task learning of lightweight neural networks
#### - Multi-task EfficientNet-B2 - 

[논문원본](https://arxiv.org/abs/2103.17107)😙


#### 이 논문은 비디오(영상)을 프레임을 여러개 받아 각 프레임마다의 mean, std, min, max 값을 계산하여 얼굴표정을 인식하는 방법을 제시하고 있습니다.

### 특징

- 얼굴 속성(나이, 성별, 국적)의 분류와 얼굴 인지를 학습하기 위해서 마진이 없는 crop 된 얼굴이미지를 input값으로 학습

- 비교적 빠르고 가벼운 MobileNet, EfficientNet, RexNet architecture 를 baseline network로 사용한다.

- 얼굴 표정(감정)을 예측하는데 있어서 네트워크를 fine-tuning 하는 것의 중요성을 강조하고 있음

이 논문에서는 **Multi-task networks** 라는 것을 제시하는데 구조를 보면 아래와 같은 구조로 이루어져 있다.

![image](https://user-images.githubusercontent.com/53431568/142212302-f1d4738b-c63c-43e3-b990-e8f5445eaafd.png)

하나씩 살펴보자면, 위에서 언급한 MobileNet, EfficientNet, RexNet을 facial recognition CNN(얼굴인식 모델)의 backbone network로 사용을 하고있다. 그리고 
이 CNN을 거쳐서 각각 성별, 나이, 국적, 감정 총 4가지에 대해 예측을 진행하고 있는 것을 볼 수 있다.

먼저, backbone architecture(Face recognition CNN) 에서 수행하기 위해 들어오는 input image 는 들어온 프레임에서 얼굴을 찾아 얼굴부분만을 잘라서 244x244 크기에 맞게 이미지를 늘리는 작업을 한다.(마진이 없게끔 crop 함) 이때 MTCNN face detection을 사용하여 얼굴을 찾도록 한다. MTCNN은 느리다는 평가가 있어서 실시간으로 볼때 facebox 를 사용해볼까 생각중이다.

![image](https://user-images.githubusercontent.com/53431568/142213131-011bb93c-911b-473f-a639-229aba7cf671.png)



SOTA 성능은 다음과 같다.

![image](https://user-images.githubusercontent.com/53431568/142211562-5d7b1829-9f16-43b1-9a70-70938aea486a.png)

emotion 7개에 대해서 AFFECT 데이터셋으로 66.34% 까지 성능을 끌어올렸다.

여기서 또 짚고 넘어갈 부분은 나이를 맞추는 layer 가 성별과 민족의 layer 보다 더 깊다는 것이다. 그 이유는 성별과 민족같은 경우 그 카테고리가 몇 개 없지만,
나이의 경우 1살부터 N 살까지 매우 범위가 자잘하고 많기 때문에 레이어를 더 추가해주었다고 한다.

또, 감정인식을 위해서는 앞에서도 언급했듯이 finetuned layer을 사용해 학습하는 것이 성능이 더 좋기 때문에 이 부분을 한번 거치고 예측을 진행하게 된다.

![image](https://user-images.githubusercontent.com/53431568/142213370-ecad3b7a-bcb1-4c6e-9f0b-4fffbfce881a.png)

위의 그림의 초록색 박스부분에서 frozen weight 라는 것을 한다. weight를 frozen 한다는 것은 기존 네트워크 구조와 pretrained 된 weight를 가져와서 학습을 시킬 때, 내가 만드는 새로운 weight 로 값을 update 시키기 보다는 기존에 있는 weight를 그대로 학습 시키는 것이 학습할 때 성능이 더 좋기 때문에 사용하는 기법이라고 한다.

일단 fronzen 에 대해서는 좀더 공부해야 겠지만. 내 생각에도, 이미 SOTA 성능을 이뤄낸 네트워크 구조이고, 이 구조로 학습된 weight 값들은 그 구조에 적합한 값을 가지고 있을 것이라고 생각한다. 그렇다면 이 값을 그대로 쓰는게 아무래도 좋겠죠?! 

따라서 이 네트워크에서는 face recogntion CNN 부분을 update 하는 동안 나머지 구조는 frozen 시키고 몇번정도 학습한 이후에 함께 학습시켜 weight를 update 한다.
frozen 해서 update 하는 횟수는 각각 성별, 나이, 민족, 감정의 레이어에 따라 다르니 논문 참고!

<br>

### Emotion Recognition process

감정을 인식하는 프로세스를 간단하게 그림으로 정리해 보았다. 다음과 같이 모든 sequence 프레임 (시퀀스 데이터가 아니라면 그냥 프레임에서도 진행) 에 있는 얼굴을 찾아 244x244 로 마진 없이 crop을 한다. 그렇게 찾아낸 얼굴에 이미지에 대해서 feature을 추출해 feature값에 대해 mean, std, min, max 값을 계산해 각 계산을 concatenate 시켜준다. 

![image](https://user-images.githubusercontent.com/53431568/142214744-6b0adf3f-0266-4783-93f8-363c9984b2bc.png)


<br>

코드로 보면 이해가 빠르다 아래 코드 참고!

![image](https://user-images.githubusercontent.com/53431568/142215150-933f4e92-5279-4c9c-a9dd-a0de7616e322.png)


예쁜 아이유 얼굴로 시험해 보았다.

![image](https://user-images.githubusercontent.com/53431568/142215229-2aa6f73e-85e8-4bda-a2b2-2b29228a0bcd.png)


일단 얼굴을 잘 잡고, 기울어지는 얼굴에 대해 alignment를 잘 하고 있다. 왼쪽에 crop된 이미지를 보면 bounding box가 세로가 더 긴 직사각형 형태로 잡혔고, 얼굴도 기울어져있지만, 왼쪽의 crop된 이미지를 보면 얼굴의 기울기도 수직으로 잘 정렬되어있고 224 사이즈의 이미지로 채워져서 들어간 것을 볼 수 있다.

바로 이 이미지가 CNN의 input 값으로 들어가 feature를 뽑게 되는 것이다.

console 창을 보면 [0.00959151 0.3924815 0.6546932 ...  이런 숫자를 볼 수 있는데, 이게 한 프레임의 한 얼굴 당 feature의  mean, std, min, max 값이 나열되어있는 것이다.

<br>

### Experiments

AffectNet 데이터셋에 대한 얼굴 감정인식 성능! 무려 62.42% 로 SOTA 달성!

![image](https://user-images.githubusercontent.com/53431568/142215663-b635d262-eb3a-4eef-a96e-7e2cc13a601d.png)


특히 이 모델은 모바일에서 돌아갈 수 있도록 하는 것이 핵심이기 때문에 (그래서 backbone architecture도 가벼운거 씀) CPU 에서도 잘돌아가는지, 얼마나 빠르게 돌아가는지를 체크하였다. MobileNet-v1 이 빠르다. 코드를 보니까 이미지 한장 넣을때의 처리인데.. 나는 비디오로 실시간 처리가 중요한데.. AFEW로 학습시켰다길래 비디오로 demo 가능한가 하고 설레었지만.. 한장의 이미지처리 demo 밖에 없었다. 그래서 비디오에 처리될 수 있도록 코드를 수정하여 사용하는 중!


내 연구주제가 FER 이 핵심이고 부가적으로 GAN 모델을 쓰는 것이라 이제 FER 논문도 자주 읽고 포스팅할 예정이다..!

어찌저찌 석사생활 잘 버티자!




---
title:  "[Tensorflow] ImageNet(1000개 클래스)으로 커스텀 모델 학습시키기"
excerpt: "training with imagenet"

categories:
  - etc

tags: [Deeplearning, CNN,study, tensorflow]
classes: wide

last_modified_at: 2021-07-16T08:06:00-05:00
---

### imagenet 데이터셋으로 학습시키는 방법

imagenet dataset은 총 1000개의 클래스를 갖고있으며 각각의 데이터 갯수는 다음과 같다.

- train 이미지 수 : 1231167
- validation 이미지 수 : 50000
- test 이미지 수 : 50000

✏️ 확인방법 : 자식 디렉토리의 파일까지 모두 세는 리눅스 명령어 

`find /폴더/경로 -type f | wc -l`

먼저 모델을 학습시키기 전 imagenet 데이터셋 용량이 너무 크므로 tfrecord로 변환해준다. 

변환방법은 아주 잘 나와있다. [tfrecord 만드는 방법!](https://www.tensorflow.org/tutorials/load_data/tfrecord)

내 코드는 아래와 같은데 아주 살짝 변경을 해주었다. 이미지의 이름을 숫자로 변경하여 저장해 나중에 test 할때 용이하도록 함.

<script src="https://gist.github.com/chaelin0722/569151e16b089225ce8a7e2f84250d53.js"></script>
<br>

tfrecord로 변환이 되었다면 이제 학습을 시작한다. 먼저 tfrecord 파일을 다시 로드해 읽은 후 이것으로 학습을 진행하는 코드이다.

<br>
<script src="https://gist.github.com/chaelin0722/6c547bad64068a030c1aaac806443c88.js"></script>
<br>

만약 학습하는데 있어서 accuracy와 loss값이 이상하다면 [여기참고](https://chaelin0722.github.io/deeplearning/cnn/study/accuracy&loss_error/)

### 학습 중간에 끊긴다면 저장된 weight를 불러서 다시 학습시키면 된다.

이렇게 model.compile 코드 이전에 넣어주면 됨

<script src="https://gist.github.com/chaelin0722/161e4998c1d330f5ef85a8d5d80515c0.js"></script>

> 💡 주의사항!
> 학습이 끊긴 당시의 EPOCH 만큼 빼서 EPOCH을 다시 설정해 주어야 한다. 안그러면 만약 100번 중 12번에서 끊기고 다시 시작하면 다시 100번 돌아가기 때문에 initial_epoch = 12, epoch=100 이런식으로 다시 지정해주자

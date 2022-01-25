---
title:  "[keras] 각 모델 마다 다른 gpu 사용하기 multi-GPU"
excerpt: "tensorflow"

categories:
  - etc
tags: [study, tensorflow, etc]
classes: wide

last_modified_at: 2022-01-25T10:40:00-05:00
---

요 근래 pytorch 만 사용하다 보니까 tensorflow는 어색하다. 텐서 코드에서 두 모델을 끌어다 쓰는데 시간이 너무 걸리는것 같아 gpu 2개로 각 모델을 불러와 처리해보기로 하였다.

pytorch 로는 multi-GPU 병렬처리에 대해서 다룬적이 있는데 케라스 코드에서는 처음이라 구글링을 하면서 뜯어보았다.

pytorch로 multi-gpu 사용하는 방법은 포스팅해 두었으니 참고! => [[GPU] GPU 사용량 최대화 하기(GPU Utility) - Pytorch 분산학습을 중심으로](https://chaelin0722.github.io/etc/gpu_utility/)


<br>

사용하는 방법은 이외로 간단하다. 원하는 모델을 불러오는 코드 위에 다음과 같이 한줄 추가해주면 된다.

~~~
with tf.device('/gpu:0'):
~~~


전체적으로 보면 아래와 같이 적용해주면 된다!


<script src="https://gist.github.com/chaelin0722/976b7d95f0ce8ce46d0e7657385349a2.js"></script>


nvidia-smi 로 확인해보니 지정한 2번 3번 GPU를 잘 사용하고 있는것 체크!



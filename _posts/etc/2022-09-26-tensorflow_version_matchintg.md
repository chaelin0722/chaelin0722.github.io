---
title:  "[mask-rcnn] tensorflow-gpu 2.0.0 -> 2.5.0"
excerpt: ""

categories:
  - python
  - etc
  
tags: [study, python]

last_modified_at: 2022-09-26T08:06:00-05:00
---

MASK-R-CNN 모델을 tensorflow-gpu 2.0.0 으로 구현하였는데, 2.5.0 버전에 맞춘 과정들을 정리함 

원래 환경 버전들 TITAN RTX, CUDA 10.0, Cudnn 7.1.0, tensorflow-gpu 2.0.0
맞춰야 하는 환경 버전들 GTX 3090Ti, CUDA 11.3


TF 2.X 버전 부터는 keras 를 불러오는 방식이 다른 것 같다. 그냥 keras 를 설치하여 쓰기보다는 tensorflow에 내장된 keras 를 쓴다. 

따라서 `import keras` 인 것들은 모두다 `import tensorflow.keras` 로 변경해주었다. 

또, `keras.engine` 의 경우 `Layers` 가 없다는 에러가 뜨곤하는데 tensorflow 파일들을 체크해보니 engine 아래에 base_layer 아래에 구현되어있었다. 

직접 보고 변경하였다 ==> [https://github.com/tensorflow/tensorflow/tree/master/tensorflow/python/keras/engine](https://github.com/tensorflow/tensorflow/tree/master/tensorflow/python/keras/engine)

아래와 같이 수정해줌. 
~~~
import tensorflow 
import tensorflow.keras as keras
import tensorflow.keras.backend as K
import tensorflow.keras.layers as KL
import tensorflow.python.keras.engine.base_layer as KE
import tensorflow.keras.models as KM
~~~

또 다른 에러는 efficientNet 으로 학습시킨 모델의 weight 를 h5 로 저장하여서 load_weights 로 돌렸는데.. 그게 최신 layer로 바뀌면서 weight 수도 바뀌었다며... 

그래서 이 에러는 하는 수 없이 학습을 다시 시켜서 다시시킨 weight 를 불러와 inference 하는 것으로 하였다. 그랬더니 에러없이 돌아감.호호




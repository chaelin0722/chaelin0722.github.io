---
title:  "[딥러닝공부] 학습시 learning rate 특정 step에 맞춰 조율하고 텐서보드로 확인하기(learning rate decay)"
excerpt: "LearningRateScheduler()"

categories:
  - Deeplearning
  - CNN
  - study
tags: [Deeplearning, CNN,study]
use_math: true
classes: wide

last_modified_at: 2021-07-27T09:06:00-05:00
---

<br>

inception-v4을 구현하다가 

**"decayed every two epochs using an exponential rate of 0.94"**라는 문장을 발견!

해석하면 매 2번째 epoch 마다 0.94비율만큼 learning rate를 줄이라는 뜻이다! 동적인 학습률을 설정해주기 위해 keras에서 제공하는 **LearningRateScheduler()** 를 사용해보았다.

## LearningRateScheduler()

keras에서 제공되는 [learningratescheduler()](https://keras.io/api/callbacks/learning_rate_scheduler/)로 사용방법은 아래와 같다.

~~~python
  def step_decay(epoch):
      init_lr = 0.045  # 처음 learning rate 지정
      drop = 0.94      # 줄일 비율
      epochs_drop = 2.0  # decay 적용할 step 주기
      lrate = init_lr * math.pow(drop, math.floor((1 + epoch) / epochs_drop)) 

      return lrate
~~~
<br>

이렇게 매 2번째 epoch 마다 학습률을 줄여주는 함수를 만들었다. 이제 이것을 learningratescheduler에 적용해보자.

적용 부분!

~~~python
  # 콜백안에 스케쥴러를 넣어주고 인자로 앞서 만든 함수를 넣는다.
  callbacks = [
          tf.keras.callbacks.LearningRateScheduler(step_decay)
  ]
  
  # 학습
  model.fit(train_dataset, validation_data=val_dataset, validation_steps=validation_steps,
             epochs=EPOCH, batch_size=BATCH_SIZE,  steps_per_epoch=steps_per_epoch, callbacks=callbacks)
~~~
<br>

### Tensorboard로 확인하기

tensorboard로 확인해보고싶다면 TensorFlow Summary API를 사용해야한다.

이렇게 동적학습률과 같은 사용자 지정 스칼라 값을 기록하기 위해서 만들어진 API 인데 방법은 다음과 같다.

1. `tf.summary.create_file_writer()`을 사용해 파일 작성기를 만든다.
2. 학습률 함수 내에 `tf.summary.scalar()`을 사용해 사용자 지정 학습률을 기록한다.
3. LearningRateScheduler 콜백을 Model.fit()에 전달한다.

<br>

전체적인 구조 코드는 다음과 같다.

~~~python

  log_dir = "./logs/scalars/"+datetime.datetime.now().strftime("%Y%m%d-%H%M%S")
  file_writer = tf.summary.create_file_writer(log_dir + "/metrics")   #추가부분!
  file_writer.set_as_default()                                        #추가부분!

  def step_decay(epoch):
      init_lr = 0.045
      drop = 0.94
      epochs_drop = 2.0
      lrate = init_lr * math.pow(drop, math.floor((1 + epoch) / epochs_drop))
      
      # to check on the tensorboard!! 추가부분!
      tf.summary.scalar('learning rate', data=lrate, step=epoch)

      return lrate


  callbacks = [ tf.keras.callbacks.LearningRateScheduler(step_decay) ]

  model.fit(train_dataset, validation_data=val_dataset, validation_steps=validation_steps,
             epochs=EPOCH, batch_size=BATCH_SIZE,  steps_per_epoch=steps_per_epoch, callbacks=callbacks)
          
~~~

<br>


tensorboard를 확인해보면 다음과 같이 매 2epoch 마다 줄어드는 것 확인!
(이미지 추가하기)

### 참고

[1] [https://towardsdatascience.com/learning-rate-schedules-and-adaptive-learning-rate-methods-for-deep-learning-2c8f433990d1](https://towardsdatascience.com/learning-rate-schedules-and-adaptive-learning-rate-methods-for-deep-learning-2c8f433990d1)

[2] [https://www.tensorflow.org/tensorboard/scalars_and_keras](https://www.tensorflow.org/tensorboard/scalars_and_keras)

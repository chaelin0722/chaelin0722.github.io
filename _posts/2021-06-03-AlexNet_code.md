---
title:  "AlexNet"
excerpt: "code"

categories:
  - Deeplearning
  - CNN
  - code
tags:
  - Deeplearning
  - CNN
  - code
  
classes: wide

last_modified_at: 2021-06-03T08:06:00-05:00

---

###  ImageNet Classification with Deep Convolutional Neural Networks 구현

![alexnet](https://user-images.githubusercontent.com/53431568/119877176-a7a1d300-bf63-11eb-8839-061d7750c517.png)

위의 layer 구조를 보고 코드로 구현하였다.

앞서 해석한 논문을 보면 디테일한 설명을 볼 수 있다. 
[ImageNet Classification with Deep Convolutional Neural Networks](https://chaelin0722.github.io/cnn/paperreview/AlexNet/)


```python
import tensorflow as tf
import datetime
from tensorflow.keras.preprocessing.image import ImageDataGenerator
from keras.optimizers import SGD

#데이터셋 설정
train_datagen = ImageDataGenerator(rescale=1./255,width_shift_range=0.2,height_shift_range =0.2,
    zoom_range=0.2, horizontal_flip =True, vertical_flip = True)

train_generator = train_datagen.flow_from_directory(
    './catdog/training_set/training_set/',
    target_size=(227, 227),
    batch_size=32,
    class_mode='categorical')

test_datagen = ImageDataGenerator(rescale=1./255,width_shift_range=0.2,height_shift_range =0.2,
zoom_range=0.2, horizontal_flip =True, vertical_flip = True)

test_generator = train_datagen.flow_from_directory(
    './catdog/test_set/test_set/',
    target_size=(227, 227),
    batch_size=32,
    class_mode='categorical')



model = tf.keras.models.Sequential([
    # C1 (first layer)
    tf.keras.layers.Conv2D(filters=96, kernel_size=(11,11), strides=4, activation='relu', input_shape=(227,227,3)),
    tf.keras.layers.BatchNormalization(), # currently use batch normalization instead local response normalization
    # overlapping
    tf.keras.layers.MaxPool2D(pool_size=(3,3), strides=(2,2), padding='valid', data_format=None),

    # C2
    tf.keras.layers.Conv2D(filters=256, kernel_size=(5,5), strides=1, activation='relu', padding="same"),
    tf.keras.layers.BatchNormalization(), # currently use batch normalization instead local response normalization
    tf.keras.layers.MaxPool2D(pool_size=(3,3), strides=(2,2), padding='valid', data_format=None),

    # C3
    tf.keras.layers.Conv2D(filters=384, kernel_size=(3,3), strides=1, activation='relu',padding="same"),
    tf.keras.layers.BatchNormalization(),
    # C4
    tf.keras.layers.Conv2D(filters=384, kernel_size=(3,3), strides=1, activation='relu',padding="same"),
    tf.keras.layers.BatchNormalization(),
    # C5
    tf.keras.layers.Conv2D(filters=256, kernel_size=(3,3), strides=1, activation='relu',padding="same"),
    tf.keras.layers.BatchNormalization(),
    tf.keras.layers.MaxPool2D(pool_size=(3,3), strides=(2,2), padding='valid', data_format=None),

    # Flatten!
    tf.keras.layers.Flatten(),
    # F6
    tf.keras.layers.Dense(4096, activation='relu'),
    tf.keras.layers.Dropout(0.5),

    # F7
    tf.keras.layers.Dense(4096, activation='relu'),
    tf.keras.layers.Dropout(0.5),

    # outputlayer, Softmax
    tf.keras.layers.Dense(2, activation='softmax')
])

sgd = SGD(lr=0.01,decay=5e-4, momentum=0.9)
model.compile(loss='binary_crossentropy', optimizer='sgd',metrics=['accuracy'])

log_dir="logs/fit/" + datetime.datetime.now().strftime("%Y%m%d-%H%M%S")
tensorboard_callback = tf.keras.callbacks.TensorBoard(log_dir=log_dir)
callback_list = [tensorboard_callback]

model.summary()

# start training
model.fit_generator(train_generator, steps_per_epoch=127, epochs=10)


#모델 저장하기
model.save('my_Alexnet.h5')

#모델 평가하기
print("-------------Evaluate-----------------")
scores = model.evaluate_generator(test_generator,steps=1)
print("%s : %.2f%%" %(model.metrics_names[1],scores[1]*100))

```

- local response normalization은 요즘 사용하지 않고 대부분 batch normalization을 사용한다고 함. 
- compile 부분을 보면 optimizaer을 stochastic gradient descent를 사용하였다. 

[stochastic gradient descent](https://chaelin0722.github.io/cnn/sgd/)에 대한 설명! 

- 원래는 cifar-10 데이터 셋으로 10개의 클래스를 분류하고자 하였지만 GPU 메모리 부족으로 2개의 클래스를 분류해보았다.

### [학습결과]

cat, dog 이미지라서 accurate가 박살난것일까..

#### 참고

[1] [http://datahacker.rs/tf-alexnet/](http://datahacker.rs/tf-alexnet/)

[2] [https://towardsdatascience.com/implementing-alexnet-cnn-architecture-using-tensorflow-2-0-and-keras-2113e090ad98](https://towardsdatascience.com/implementing-alexnet-cnn-architecture-using-tensorflow-2-0-and-keras-2113e090ad98)

[3] [https://medium.com/swlh/alexnet-with-tensorflow-46f366559ce8](https://medium.com/swlh/alexnet-with-tensorflow-46f366559ce8)



## 아직 수정중인 페이지 

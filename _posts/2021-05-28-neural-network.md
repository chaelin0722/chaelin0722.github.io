---
title:  "CNN"
excerpt: "Sigmoid의 한계와 대안으로써의 ReLu"

categories:
  - Deeplearning
tags:
  - Blog
last_modified_at: 2019-04-13T08:06:00-05:00

classes: wide
---

### Sigmoid의 한계와  대안으로써의 ReLU

### 1. Sigmoid

cv_keras_ORGate.py

```python
from keras.models import Sequential
from keras.layers import Dense
import matplotlib.pyplot as plt
import numpy
##-- fix random seed for reproducibility
seed = 7
numpy.random.seed(seed)
##-- load pima indians dataset
dataset = numpy.loadtxt("OR-gate.data.csv", delimiter=",")
##-- split into input (X) and output (Y) variables
X = dataset[:,0:2]
Y = dataset[:,2]
##-- create model
# sequential network by adding
model = Sequential()
# input layer   #input dimension에는 input variable값을 넣어준다. 
model.add(Dense(2, input_dim=2, kernel_initializer='uniform', activation='sigmoid'))
# hidden layer
model.add(Dense(2, kernel_initializer='uniform', activation='sigmoid'))
## add more hidden layers
model.add(Dense(2, kernel_initializer='uniform', activation='sigmoid'))
## 추가하면 어찌 될지 추가해보기 
# output layer
model.add(Dense(1, kernel_initializer='uniform', activation='sigmoid'))
##-- Compile model
model.compile(loss='mean_squared_error', optimizer='adam', metrics=['accuracy'])
#현재로서는 adam이 최신버전이고 가장좋은 성능을 낸다.
##-- Fit the model
history = model.fit(X, Y, validation_split=0.2, epochs=200, batch_size=1, verbose=0)

##-- summarize history for accuracy
plt.plot(history.history['accuracy'])
plt.plot(history.history['val_accuracy'])
plt.title('model accuracy')
plt.ylabel('accuracy')
plt.xlabel('epoch')
plt.legend(['train', 'test'], loc='upper left')
plt.show()
##-- summarize history for loss
plt.plot(history.history['loss'])
plt.plot(history.history['val_loss'])
plt.title('model loss')
plt.ylabel('loss')
plt.xlabel('epoch')
plt.legend(['train', 'test'], loc='upper left')
plt.show()

##-- Actual test for the trained model --##
dataset_test = numpy.loadtxt("OR-gate-test.data.csv", delimiter=",")
x_test = dataset_test[:,0:2]
yhat = model.predict(x_test)

print('#-- X_tested --#')
print(x_test)
print('#-- Y_predicted --#')
print(yhat)
```

**출력결과**
<총 layer 3개, 추가없음>

[[1.7901279e-04]
[9.9992824e-01]
[9.9992824e-01]
[1.7901279e-04]
[1.7901279e-04]
[9.9992621e-01]
[9.9992824e-01]]

<hidden 2개 추가>

[[3.0719256e-04]
[9.9990296e-01]
[9.9990296e-01]
[3.0719256e-04]
[3.0719256e-04]
[9.9990296e-01]
[9.9990296e-01]]

---

<hidden 4개 추가>
[[0.00099993]
[0.99980634]
[0.99980634]
[0.00099993]
[0.00099993]
[0.99980634]
[0.99980634]]

=>  y_predict   값이 점점 1에 가까워지는 것을 확인할 수 있다. (성능이 올라가는중)

---

hidden 10개 추가

[[0.68906355]
[0.68906355]
[0.68906355]
[0.68906355]
[0.68906355]
[0.68906355]
[0.68906355]]

=>  y_predict   값이 다시 줄어들고 있다.

```python
model.add(Dense(2, input_dim=2, kernel_initializer='uniform', activation='sigmoid'))
# hidden layer
model.add(Dense(2, kernel_initializer='uniform', activation='sigmoid'))
model.add(Dense(2, kernel_initializer='uniform', activation='sigmoid'))
model.add(Dense(2, kernel_initializer='uniform', activation='sigmoid'))
model.add(Dense(2, kernel_initializer='uniform', activation='sigmoid'))
model.add(Dense(2, kernel_initializer='uniform', activation='sigmoid'))
model.add(Dense(2, kernel_initializer='uniform', activation='sigmoid'))
model.add(Dense(2, kernel_initializer='uniform', activation='sigmoid'))
model.add(Dense(2, kernel_initializer='uniform', activation='sigmoid'))
model.add(Dense(2, kernel_initializer='uniform', activation='sigmoid'))
model.add(Dense(2, kernel_initializer='uniform', activation='sigmoid'))

# output layer
model.add(Dense(1, kernel_initializer='uniform', activation='sigmoid'))
```

- **아래는 hidden layer이 sigmoid 함수로 10번 한 경우의 accuracy와 loss**

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b067529b-2387-4019-ae5e-1b68af3a9dc7/hiddenlayer_siggmoid_accuracy.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b067529b-2387-4019-ae5e-1b68af3a9dc7/hiddenlayer_siggmoid_accuracy.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e8dff7a2-1eae-4dbe-99e4-94daedf6edf1/hiddenlayer_sigmoid_10.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e8dff7a2-1eae-4dbe-99e4-94daedf6edf1/hiddenlayer_sigmoid_10.png)

sigmoid 함수는 binary classification에 적절하다. 일정한 기준 값으로 0인지 1인지 구분함으로써 분류하는 방식이다. 

하지만 위와 같이 hidden layer가 어느 정도에서 더 추가되면 정확도가 0.5~0.6으로 떨어진다. 이 문제가 발생하는 이유는 다음과 같다. 

- backpropagation : 2~4개 정도의 레이어는 학습이 잘 되나 9~10단으로 넘어가면서부터 학습이 잘 되지 않는 이유는 역전파이다. 레이어가 많을 경우 각각의 단계의 값을 미분해 최초 레이어까지 결과 값을 전달해가게 되는데, 만약 내부의 hidden layer들이 모두 sigmoid값으로 이루어져 있다면 각 단계 계산한 값은 모두 0과 1사이의 값일 수 밖에 없다.
- vanishing gradient: 여러 레이어를 갖고 있을  때 최초 입력값은 각각의 레이어에서 나온 값들을 곱해준 만큼이 결과에 영향을 주는 것이므로 최종 미분값은 결국 0에 가까운 수가 된다. 이를 경사도(기울기)가 사라지는 현상이라고 한다. 최초 입력값이 최종 결과 값에 별로 영향을 끼치지 않는다는 결론으로 수렴하게 되는 것이다.

⇒ sigmoid함수는 0<n<1 값만 다루므로 결국 chain rule을 이용해 계속 값을 곱해나간다고 했을 때 결과 값이 0에 수렴할 수 밖에 없다는 한계를 가지고 있다. 따라서 1보다 작아지지 않게 하기 위한 대안으로 ReLU함수를 적용한다. 

### ReLU

내부 hidden layer를 활성화 시키는 함수로 ReLU를 사용해봄. 

이 함수는 0보다 작은 값은 0, 0보다 큰 값은 그 값을 그대로 반환한다. 따라서 내부 hidden layer에는 ReLU를 적용하고 마지막 output layer에만 sigmoid함수를 적용하면 이전에 비해 정확도가 올라간다. 

```python
# input layer
model.add(Dense(2, input_dim=2, kernel_initializer='uniform', activation='sigmoid'))
# hidden layer
## add more hidden layers
model.add(Dense(2, kernel_initializer='uniform', activation='relu'))
model.add(Dense(2, kernel_initializer='uniform', activation='relu'))
model.add(Dense(2, kernel_initializer='uniform', activation='relu'))
model.add(Dense(2, kernel_initializer='uniform', activation='relu'))
model.add(Dense(2, kernel_initializer='uniform', activation='relu'))
model.add(Dense(2, kernel_initializer='uniform', activation='relu'))
model.add(Dense(2, kernel_initializer='uniform', activation='relu'))
model.add(Dense(2, kernel_initializer='uniform', activation='relu'))
model.add(Dense(2, kernel_initializer='uniform', activation='relu'))
model.add(Dense(2, kernel_initializer='uniform', activation='relu'))

# output layer
model.add(Dense(1, kernel_initializer='uniform', activation='sigmoid'))
```

#-- Y_predicted --#

[[0.69174945]
[0.69174945]
[0.69174945]
[0.69174945]
[0.69174945]
[0.69174945]
[0.69174945]]

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/028d5d87-6a59-42a5-9179-ae392a224c24/relu_accuracy.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/028d5d87-6a59-42a5-9179-ae392a224c24/relu_accuracy.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0a9fc498-238a-4ecd-a48c-07e62dea1e1a/relu_loss.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0a9fc498-238a-4ecd-a48c-07e62dea1e1a/relu_loss.png)

성능이 좋아진다고는 하는데 그닥 좋아진것 같진 않다. 아주 미세한 차이이다.

아마 ORGate 코드라서 값이 binary한 값이라 차이가 별로 없는듯..?

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/afbb1776-40b2-4bf3-a286-9ee5f449033b/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/afbb1776-40b2-4bf3-a286-9ee5f449033b/Untitled.png)

---
title:  "[딥러닝공부] 모델의 성능을 높이는 방법 - 개념정리"
excerpt: "손실함수, 활성화함수, optimizer, 정규화 정리"

categories:
  - CNN
tags: [Deeplearning, CNN, study]

last_modified_at: 2021-12-06T08:06:00-05:00
---

## 모델의 성능을 높이는 방법 - 개념정리 

Sigmoid의 한계와  대안으로써의 ReLU를 공부해보다가 한번 개념을 집고 넘어가는게 좋을 것 같아서 책을 보고 공부해보있다. 책은 [GAN 첫걸음] 이라는 책으로 파이토치 코드도 볼 겸 GAN에 대해서 개념을 완벽히 하고 넘어가고 싶어서 읽어보았다. 아래 내용은 모두 이 책을 참고해서 썼으므로 git 코드가 궁금하거나 책에 대해 궁금하다면 [여기!](https://github.com/makeyourownneuralnetwork/gan)

다음은 모델의 성능을 향상시킬 수 있는 방법들인 손실함수, 활성화함수, optimizer, 정규화 방법의 개념을 알아보고 정리한 내용이다.😉😉


## 손실함수 (Loss Function)

> - 회귀 : 연속적인 숫자에 대해 결과를 출력해야 하는 경우  ex)섭씨온도 : 0~100까지 숫자가 연속적으로 나와야 함
>    => 주로 평균제곱오차법 (MSE) 사용 
> 
> - 분류 : 참/거짓 혹은 1/0 과 같은 이산형 결과를 출력해야 하는 경우  ex) cat dog classification
>    => 주로 binary cross entropy (BCE) 

BCE는 확실히 틀리는 경우에 특히 큰 페널티는 방식으로 파이토치에서는 `nn.BCELoss()` 함수로 이를 지원한다.


먼저, MSE로 돌린 결과를 출력해보니 손실 값이 비교적 빨리 떨어지는 것을 확인

![image](https://user-images.githubusercontent.com/53431568/144838652-296798e4-0912-411f-be07-a1b2d04b7748.png)


BCE의 경우 손실이 점점 감소하나 MSE와 비교하면 느리게 떨어지는 것을 확인할 수 있다. 또한, 노이즈가 많은 편이며 훈련 후반부에도 계속 가끔씩 상당히 큰 값들이 기록된다.


아래 차트로 보면 오히려 성능이 나빠진 것 처럼 보이지만, 후반부쯤에는 손실이 더 작아 최종 정확도도 MSE 보다 향상되었는데, 향상된 결과도 확인해보자..!

![image](https://user-images.githubusercontent.com/53431568/144838423-eb2fd1e8-29f2-4e57-9aae-40c6f23d7c66.png)


테스트 데이터셋의 레코드 19번에 대한 결과를 MSE 와 BCE 를 비교해보았다. 확실히 BCE를 사용한 모델이 4에 대해 좀 더 정확한 결과를 내는 모습을 확인할 수 있다.

![image](https://user-images.githubusercontent.com/53431568/144837877-289e3350-9c89-400f-9401-91aad28d3e96.png)
![image](https://user-images.githubusercontent.com/53431568/144837924-76113bd3-b07c-453c-9c3f-5028f1aef581.png)

<br>

## 활성화 함수 (Activation Function)

신경망 초기에 activation function 으로는 SGD(Stochastic Gradient Descent)를 많이 사용하였다. 뉴런에서 일어나는 신호 전달 현상과 비슷하며 기울기 계산이 간편하기 때문이다. 하지만 큰 값들에 대해 기울기가 굉장히 작아지다가 기울기가 소실되는 gradient vanishing 문제가 생겼다. 결국 기울기가 없어지면 학습은 빨리 수렴하게 되는 saturation(포화) 상태가 되어버리는 것이다.

이를 대체 하기 위해 나온 아주 간단한 방법은 ReLU 형식이다. 고정된 기울기의 직선을 통해 절대 기울기가 사라지지 않는 것이다. 하지만 이 또한 문제가 있는데 0보다 작은 값들에 대해 경사가 0이기 때문에 이에 대해서도 기울기 소실의 문제가 생긴다. 

따라서, 0보다 작은 경우 미세한 기울기를 허용함으로써 문제점을 해결한 것이 LeckReLU이다. 

![image](https://user-images.githubusercontent.com/53431568/144838587-db52d8b2-2db4-4a94-8274-6f366f48442e.png)
![image](https://user-images.githubusercontent.com/53431568/144839878-a3ed00c7-d57c-4615-bf3a-a45ed7ebd844.png)
![image](https://user-images.githubusercontent.com/53431568/144839919-d15c39d4-5919-44a7-94b3-7834d721e716.png)
[이미지 출처](https://kjhov195.github.io/2020-01-07-activation_function_2/)

똑같이 에폭3을 주고 결과를 출력해보았다. 손실이 0을 향해 확실히 빠르게 감소하며 모델 훈련의 아주 초반에도 손실이 낮고 노이즈도 줄어든 것을 확인!✌️

![image](https://user-images.githubusercontent.com/53431568/144839388-a5e54519-2c2f-433a-8066-aba3cfbc0c11.png)

테스트 셋의 레코드 19에 대한 출력값을 보면 4에 대해 굉장히 높은 확률을 할당하고 있으며 다른 노드들은 거의 0에 가깝다.

`"이처럼 활성화 함수를 바꾸는 것은 최종 성능에 큰 영향을 미치며 이는 곧 더 나은 기울기를 구하고 있음을 알 수 있다."`



<br>

## optimizer

SGD (Stochastic Gradient Descent, 확률적 경사하강법)은 기본적으로 많이 쓰이고 있는 기법이다. 단, 국소 최적해에 빠져버릴 확률이 크다는 단점이 있다. 이에 대한 몇가지 대안 중 하나가 Adam optimizer 이다. 

Adam 이 제시하는 SGD의 두 문제점에 대한 해결책은 다음과 같다.

1. 관성을 이용해 국소 최적해로 바져버릴 가능성을 줄이는 것 (무거운 공은 관성이 커서 작은 구멍에 빠지지 않고 통과하는 것에 비유)
2. 학습 파라미터에 대해 각기 다른 학습률을 적용하고 학습 시 이 파라미터들을 계속 상황에 따라 수정한다.








<br>

## 정규화













































아래는 orgate 로 구현해본 sigmoid의 한계와 대안으로써의 ReLU에 대해 파악해보고자 돌려본 코드이다. 

### Sigmoid

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

![test_result](https://user-images.githubusercontent.com/53431568/119865541-bafa7180-bf56-11eb-85bd-91fbad4589a1.JPG)

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


![test_result2](https://user-images.githubusercontent.com/53431568/119865536-ba61db00-bf56-11eb-80e2-7638d6960631.JPG)

성능이 좋아진다고는 하는데 그닥 좋아진것 같진 않다. 아주 미세한 차이이다.

아마 ORGate 코드라서 값이 binary한 값이라 차이가 별로 없는듯..?

![functions](https://user-images.githubusercontent.com/53431568/119865525-b766ea80-bf56-11eb-8d54-a5051c5da036.JPG)

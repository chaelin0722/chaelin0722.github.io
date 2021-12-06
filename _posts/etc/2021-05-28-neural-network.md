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

따라서, 0보다 작은 경우 미세한 기울기를 허용함으로써 문제점을 해결한 것이 LeckReLU이다. (이미지 줄어드는거 뭐지..ㅎㅎ)

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


똑같은 환경에서 optimizer만 adam으로 두고 학습한 결과는 다음과 같다.

손실은 0으로 빠르게 떨어지며 평균 또한 낮다. Adam 옵티마이저가 꽤 효율적인 것을 확인..!✌️

![image](https://user-images.githubusercontent.com/53431568/144842263-b4e6dbbd-1d19-48d0-8558-3712d6a3a49e.png)



테스트 셋의 레코드 19를 보면 4에 대해 굉장히 높은 확률을 할당하고 있다.

![image](https://user-images.githubusercontent.com/53431568/144840598-ed022c9d-d260-4e5e-b697-69a5af543f94.png)


<br>

## 정규화 (Normalization)

이제 마지막이다..! 아자!

신경망의 가중치와 여러 신호의 값ㄷ은 가끔씩 굉장히 큰 값을 가질 때가 있다. 앞에서 보았듯, 이 경우 중요한 값이 소실되는 결과가 나올 수 있고 결국 훈련을 어렵게 만든다. 파라미터들의 범위를 감소하거나 평균을 0으로 맞춰주는 작업이 상당히 도움이 된다는 연구결과가 나왔고 이러한 방법을 `정규화`(normalization)이라고 한다.

훈련 결과 그래프를 보면, 원래 모델보다 손실이 굉장히 빠르게 감소하였다. (거의 일자 아님? ㅋㅋㅋ 줄어드는게 안보일정도로 빨리 0으로 감소함)

![image](https://user-images.githubusercontent.com/53431568/144841240-c86bc387-c717-472b-a6ce-493caccef9bf.png)



위에서 다룬 방식이 best 는 아니지만 이런식으로 4가지 기법들을 적절히 사용하여 모델의 성능을 최대치로 뽑는 것이 학습에 좋으니 잘 알아두자!

실제로 깃헙의 sota 모델들을 가져와서 돌려보면 optimizer 이나 loss function을 조절해보고 돌리면 학습이 다른 값이 나오기도 한다. 그래서 이참에 개념들을 다시 정리하니 내 머릿속도 정리되는 기분이다. ㅎㅎ

열공 퐈이링~




#### 참고

[1] [https://github.com/makeyourownneuralnetwork/gan](https://github.com/makeyourownneuralnetwork/gan)















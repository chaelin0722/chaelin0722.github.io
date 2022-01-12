---
title:  "[GAN study] KL-divergence & JS-divergence & Maximum Likelihood Estimation와 개념정리"
excerpt: "entropy와 cross entropy, MLE"

categories:
  - gan

tags: [Deeplearning, study, gan, LME, KL, JS]

classes: wide

use_math : true

last_modified_at: 2022-01-13T08:06:00-05:00

---

사실상 GAN 을 이해하려면 GAN의 손실함수를 구성하고 있는 KL-Divergence, JS-Divergence, MLE 에 대한 이해는 필수이다. 막상 GAN의 논문을 읽고 리뷰해보면서 증명을 유도해보았지만 정확한 KL-Divergence, JS-Divergence, MLE에 
대한 정리는 하지 않아서 다시 공부할 겸 정리를 해봅니다 😆😆

GAN 논문과 MINMAX 함수에 대한 유도는 => [[논문정리📃] Generative Adversarial Nets](https://chaelin0722.github.io/paperreview/generative_adversarial_nets/) 여기에서 직접 손으로 풀어본 풀이가 있으니 참고하면 도움이 될 것 같습니다.


KL-Divergence 는 모델 분포간 얼마나 가까운지에 대한 정보 손실량의 기댓값이다. 따라서, 
KL-Divergence, JS-Divergence 를 다루기 전에 이를 이루고 있는 기본적 개념인 entropy 와 CE(cross entropy)를 짚고 넘어가도록 하겠다. 

## Entropy

엔트로피는 불확실성을 설명하는 수학적 아이디어이다. 

예를 들어 동전던지기를 생각해보자. 양면이 다 앞인 동전이라면 앞면이 나올 확률은 100%이고 뒷면이 나올 확률은 0%이다. 두 결과 모두 100%의 확률로 예측이 가능하다. 이런 식으로 불확실성이 0이면 엔트로피 역시 0이라고 할 수 있다.

> 어떤 분포의 정보량은 엔트로피로 나타내며, 분포p에 대한 엔트로피의 식은 다음과 같다. 

$H(p) = -E_{x\sim p}[logp]=-\sum p(x)log p(x)$

이렇게 엔트로피는 모든 가능한 결과의 합이며, $p$는 결과에 대한 확률이다. 


## Cross Entropy

분류(classification)문제를 풀 때 크로스 엔트로피를 이용해 loss function, cost function 과 같이 손실함수를 정의한다. 

크로스 엔트로피는 실제 결과 우도($p$)와 우리가 생각하는 우도($q$) 사이의 차이에 따른 결과의 불확실성에 대한 지표이다. 

다시 동전예제를 생각해보자!

단순히 생각했을때, 동전 던지기는 각 확률이 1/2 이며 나름 공평한 결과가 나올것이라고 예상한다. 하지만 실제로는 결과가 공평하지 않을 수 있으며 불확실성이 존재한다. 여기서 착안한 것이 교차 엔트로피입니다. 만약 실제로도 동전 던지기가 공평하다면 교차 엔트로피도 낮은 상태일 것이다.


교차 엔트로피를 두 확률분포의 차이라고 했습니다. 그리고 두 분포의 차이가 없는 만큼 교차 엔트로피도 낮아지며, 정확히 일치할 경우 0이 됩니다. (일치한다는 것은 차이가 없는 것이니까요)

실제 신경망으로 부터 나온 결과도 확률분포라고 생각을 한다면, 두 분포가 다르다면 교차 엔트로피는 높고, 둘이 비슷하다면 낮을 것이다. 이는 우리가 손실함수에서 기대하는 바로 그대로 이다.!


> CE는 우리가 알고 있는 분포 p와 추정한 분포 q 사이의 차이를 정보량을 통해 나타낸다.

$H(p,q) = -E_{x \sim p}[log q(x)]$ 

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $= -\sum_i p_i(x)log p_i(x)$ 

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $= - \sum_i P(x \mid y)log P(x \mid y,\theta)$

현재 파라미터 $\theta$에서 음의 우도의 기댓값이라고 할 수 있다. 우리가 근사하고 싶은 분포 $p$와 현재 파라미터 $\theta$하에서 추론한 $q$가 얼마나 비슷한지 그 차이를 계산한 것이다.


## KL Divergence

![image](https://user-images.githubusercontent.com/53431568/145174101-fda2b40f-609c-407f-8517-e05194477aeb.png)

크로스 엔트로피처럼 두 분포간의 차이를 나타내는데, `모델의 분포간 얼마나 가까운지에 대한 정보 손실량의 기댓값`을 나타낸다. 

한 지점에서의 두 분포의 정보량 차이를 다음과 같이 수식으로 표현할때,

$logp(x) - logq(x)$

전체에 걸쳐서 표현한 수식은 다음과 같다.

$D_{KL}(p\|q) = E_{x\sim p}[logp(x)-logq(x)]$ 

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $= \sum_i p_i(logp_i-logq_i)$

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $= \sum_i p_i  log{p_i \over {q_i}}$


추정한 $p$에 대한 두 분포의 차이의 기댓값을 표현하고 있다. 여기서 KL Divergence 식을 풀어보면 엔트로피와 $-H(p)$ 크로스 엔트로피 $H(p,q)$를 더한 식으로 풀어쓸 수 있다.

$D_{KL}(p\|q)=\sum_i p_i(logp_i - log q_i)$

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $= \sum -H(p)+H(p,q)$

사실상 KL Divergence와 CE가 같은 식이 되는 것을 볼 수 있다.

이 척도로 두 분포간의 차이를 나타내지만 '거리(metric)'가 되진 못한다. 비대칭 하기 때문에 거리의 조건을 만족하지 않기 때문이다. (실제 분포인 p를 기준으로 계산하기 때문)

$D_{KL}(p\|q) \neq D_{KL}(q\|p)$

따라서 KL-divergence를 보완하여 distance metric 으로 사용할 수 있는 방법으로 나온 개념이 JSD 이다.

<br>


## JS Divergence

단순히 식을 수정하여 대칭적으로 바꾼 것이다.

수식은 다음과 같다.

$JSD(p,q) = {1 \over 2}D(P \parallel M) + {1 \over 2} D(Q \parallel M)$

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $where \; M = {1 \over 2}(P+Q)$



정리하면, 

$JSD(p,q) = {1 \over 2}$ $D_{KL}$ $(p \parallel {p+q \over 2})$ $+ {1 \over 2} D_{KL}(q  \parallel {p+q \over 2})$



$P$ 와 $Q$의 중간값, 평균을 뜻하는 $M$과 KL-Divergence를 하면서 대칭해지는 성질을 확인할 수 있다.

$JSD(P,Q) = JSD(Q,P)$


이를 통해 두 확률 분포 사이의 거리를 JS-Divergence를 통해 척도로 활용이 가능해진다. (P와 Q가 같으면 JSD는 0이 된다. P와 Q가 같다는 건 그 거리가 같고 차이가 없기 때문에 0!)


<br>

## Maximum Likelihodd Estimation (MLE)

최대우도기법은 $\theta = (\theta_1,\theta_2, \ldots \theta_n)$ 으로 구성된 확률 밀도함수 $P(x \mid \theta)$에서 관측된 표본 데이터집합 $x = (x_1, x_2, \ldots x_n)$ 이라 할 때 이 표본들에서 파라미터($\theta = (\theta_1,\theta_2, \ldots \theta_n)$)를 추정하는 방법이다.

아래 그림을 예로 보면 다음과 같이 빨간색 점의 데이터를 얻었다고 가정했을 때, 초록색과 노란색 후보 분포 중 초록색 분포에서 데이터를 얻을 가능성이 더 커 보인다. 획득한 데이터들의 문포가 초록색 곡선의 중신에 더 일치해 보이기 때문이다. 

![image](https://user-images.githubusercontent.com/53431568/145350174-989653e6-9e82-424d-8e6d-855e3c3521ae.png)


### Likelihood function

> likelihood : 지금 얻은 데이터가 이 분포에서 나왔을 가능도

아래 그림과 같이 후보 분포에 대해 각 데이터들의 likelihood를 점선의 높이로 나타낼 수 있다.

![image](https://user-images.githubusercontent.com/53431568/145350907-77076e97-10dd-4e75-b0a0-3a5fe7d118d6.png)

수치적으로 가능도 계산은 각 데이터 샘플들에서 후보 분포에 대한 높이(likelihood)를 곱한 것으로 볼 수 있으며, 수식으로는 다음과 같이 표현한다.

$p(x \mid \theta) = \prod^n_{k=1} P(x_k \mid \theta)$

위의 수식과 같이 $\theta$라는 표본에서 생각할 수 있는 모든 후보군에 대해 곱하는데 이를 전체 표본 집합의 결합확률밀도 함수, likelihood function이라고 하는 것이다!

**곱하는 이유는 데이터의 추출이 독립적으로 연달아 나오기 때문이다. 


자연로그를 이용해 나타내면 다음과 같다.


$L(\theta \mid x)=logP(x \mid \theta)=\sum^m_{i=1} log P(x_i \mid \theta)$


이제 다시 최대우도기법을 설명해보자..!

`Maximum` likelihood estimation은 함수의 최댓값을 찾는 것이다. log 함수는 단조증가 함수이기 때문에 likelihood function의 최대값을 찾는 것이나 로그 likelihood function의 최댓값을 찾는 것 두 경우 모두의 최댓값을 갖게 해주는 정의역의 함수 입력값은 동일하다.

따라서 계산의 편의를 위해 로그 likelihood 의 최댓값을 찾는다. 



(likelihood의 최댓값을 찾으려면 log 함수의 값이 최대가 되어야하는데, 즉 input 값이 최대가 되어야하는 것이다.)

![image](https://user-images.githubusercontent.com/53431568/149172625-ef800029-c4d5-4dc1-9336-5c4f3517cf06.png)



우리가 찾고자 하는 것은 파라미터 $\theta$의 값이므로 $\theta$에 대해 편미분을 하여 그 값이 0이 되는 $\theta$를 찾는 과정을 통해 로그 함수를 최대화 시켜줄 수 있는 $\theta$를 찾을 수 있다. 

${\partial \over \partial\theta}$ $L(\theta \mid x)$ $= {\partial \over \partial\theta}$ $logP(x \mid \theta)$  $= \sum^n_{i=1}$ ${\partial \over \partial\theta}$ $logP(x_i  \mid \theta) =0$  

<br>
<br>


#### 참고 

[1] [GAN 첫걸음](http://www.yes24.com/Product/Goods/97559774)

[2] [https://velog.io/@stapers/KL-Divergence-JS-Divergence](https://velog.io/@stapers/KL-Divergence-JS-Divergence)

[3] [https://aigong.tistory.com/66](https://aigong.tistory.com/66)

[4] [https://theeluwin.postype.com/post/6080524](https://theeluwin.postype.com/post/6080524)

[5] [로그함수 이미지 출처](https://namu.wiki/jump/UjMN53Qg3vjz5wmUiqCVf38PjdZFAi4x2lBfEKpf3%2Br9zT6aX8uw6gktLQrwpM%2BqG8P8mNJnjobPFncVJIZkhA%3D%3D)

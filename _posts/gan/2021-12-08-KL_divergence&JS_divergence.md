---
title:  "[GAN study] KL-divergence & JS-divergence & Maximum Likelihood Estimation와 개념정리"
excerpt: "entropy와 cross entropy, MLE"

categories:
  - gan

tags: [Deeplearning, study, gan, LME, KL, JS]

classes: wide

use_math : true

last_modified_at: 2021-12-08T08:06:00-05:00

---

사실상 GAN 을 이해하려면 GAN의 손실함수를 구성하고 있는 KL-Divergence, JS-Divergence, MLE 에 대한 이해는 필수이다. 막상 GAN의 논문을 읽고 리뷰해보면서 증명을 유도해보았지만 정확한 KL-Divergence, JS-Divergence, MLE에 
대한 정리는 하지 않아서 다시 공부할 겸 정리를 해봅니다 😆😆

GAN 논문과 MINMAX 함수에 대한 유도는 => [[논문정리📃] Generative Adversarial Nets](https://chaelin0722.github.io/paperreview/generative_adversarial_nets/) 여기에서 직접 손으로 풀어본 풀이가 있으니 참고하면 도움이 될 것 같습니다.


KL-Divergence 는 모델 분포간 얼마나 가까운지에 대한 정보 손실량의 기댓값이다. 따라서, 
KL-Divergence, JS-Divergence 를 다루기 전에 이를 이루고 있는 기본적 개념인 MSE(Mean square error) 와 CE(cross entropy)를 짚고 넘어가도록 하겠다. 


??????????????????????????? --작성중 --
## MSE 와 BCE

MSE(평균제곱오차) 손실의 정의는 출력노드에서 나온 값과 원하는 목표값 사이의 차이를 계산하는 것이다. 이는 양의값, 음의값 모두 될 수 있으며 이 오차를 제곱한다면 값은 항상 양이 된다. 평균제곱오차는 이 제곱한 오차들의 평균을 말한다. 

BCE(이진교차엔트로피) 손실은 확률과 불확실성에 기반을 둔다. 
?????????????????????

## Entropy

엔트로피는 불확실성을 설명하는 수학적 아이디어이다. 

예를 들어 동전던지기를 생각해보자. 양면이 다 앞인 동전이라면 앞면이 나올 확률은 100%이고 뒷면이 나올 확률은 0%이다. 두 결과 모두 100%의 확률로 예측이 가능하다. 이런 식으로 불확실성이 0이면 엔트로피 역시 0이라고 할 수 있다.

> 어떤 분포의 정보량은 엔트로피로 나타내며, 분포p에 대한 엔트로피의 식은 다음과 같다. 

$H(p) = -E_{x\sim p}[logp]=-\sum p(x)log p(x)$

이렇게 엔트로피는 모든 가능한 결과의 합이며, $p$는 결과에 대한 확률이다. 


## Cross Entropy

교차 엔트로피는 실제 결과가 도출될 우도와 우리가 생각하는 우도 사이의 차이에 따른 결과의 불확실성에 대한 지표이다. 

다시 동전예제를 생각해보자!

단순히 생각했을때, 동전 던지기는 각 확률이 1/2 이며 나름 공평한 결과가 나올것이라고 예상한다. 하지만 실제로는 결과가 공평하지 않을 수 있으며 불확실성이 존재한다. 여기서 착안한 것이 교차 엔트로피입니다. 만약 실제로도 동전 던지기가 공평하다면 교차 엔트로피도 낮은 상태일 것이다.


교차 엔트로피를 두 확률분포의 차이라고 했습니다. 그리고 두 분포의 차이가 없는 만큼 교차 엔트로피도 낮아지며, 정확히 일치할 경우 0이 될 것이다.

실제 신경망으로 부터 나온 결과도 확률분포라고 생각을 한다면, 두 분포가 다르다면 교차 엔트로피는 높고, 둘이 비슷하다면 낮을 것이다. 이는 우리가 손실함수에서 기대하는 바로 그대로 이다.!


> CE는 우리가 알고 있는 분포 p와 추정한 분포 q 사이의 차이를 정보량을 통해 나타낸다.

$H(p) = -E_{x\sim p}[log q(x)] = -\sum_i p_i(x)log p_i(x) =-\sum_i P(x|y)log P(x|y,\theta)$

현재 파라미터 $\theta$에서 음의 우도의 기댓값이라고 할 수 있다. 우리가 근사하고 싶은 분포 $p$와 현재 파라미터 $\theta$하에서 추론한 $q$가 얼마나 비슷한지 그 차이를 계산한 것이다.


## KL Divergence

![image](https://user-images.githubusercontent.com/53431568/145174101-fda2b40f-609c-407f-8517-e05194477aeb.png)


크로스 엔트로피처럼 두 분포간의 차이를 나타내는데, `모델의 분포간 얼마나 가까운지에 대한 정보 손실량의 기댓값`을 나타낸다. 

한 지점에서의 두 분포의 정보량 차이를 다음과 같이 수식으로 표현할때,

$logp(x) - logq(x)$

전체에 걸쳐서 표현한 수식은 다음과 같다.

$D_{KL}(p||q) = E_{x\sim p}[logp(x)-logq(x)] = \sum_ip_i(logp_i-logq_i)=\sum_i p_i  log p_i\over{q_i}$


이 척도로 닮음의 정도를 측정하지만.. 문제는 비대칭 하다는 것이다. 실제 분포인 p를 기준으로 계산하기 때문에 나오는 결과이다. 따라서 유사도를 이야기할 때 '거리'라고 표현하지 않는다. 

$D_{KL}(p||q) \neq D_{KL}(q||p)$

KL-divergence를 보완하여 distance metric 으로 사용할 수 있는 방법으로 나온 개념이 JSD 이다.

<br>


## JS Divergence

단순히 식을 수정하여 대칭적으로 바꾼 것이다.

수식은 다음과 같다.

$JSD(p,q)= 1/2 D_{KL}({p||p+q} \over{2}) + 1/2 + 1/2 D_{KL}({p||p+q} \over{2}) + 1/2$



$P$ 와 $Q$의 중간값, 평균을 뜻하는 $M$과 KL-Divergence를 하면서 대칭해지는 성질을 확인할 수 있다.

$JSD(P,Q) = JSD(Q,P)$


이를 통해 두 확률 분포 사이의 거리를 JS-Divergence를 통해 척도로 활용이 가능해진다. (P와 Q가 같으면 JSD는 0이 된다. P와 Q가 같다는 건 그 거리가 같고 차이가 없기 때문에 0!)


<br>

## Maximum Likelihodd Estimation (MLE)





<br>
<br>


#### 참고 

[1] [신경망 첫걸음]

[2] [https://velog.io/@stapers/KL-Divergence-JS-Divergence](https://velog.io/@stapers/KL-Divergence-JS-Divergence)

[3] [https://aigong.tistory.com/66](https://aigong.tistory.com/66)

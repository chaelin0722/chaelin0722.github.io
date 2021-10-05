---
title:  "[논문정리📃] Attention Is All You Need"
excerpt: "Week9 -Self Attention-"

categories:
  - paperReview
tags: [CNN, paperReview]
use_math: true

last_modified_at: 2021-10-05T08:06:00-05:00
classes: wide
---

## Attention Is All You Need
#### - Self Attention - 

[논문원본](https://arxiv.org/pdf/1706.03762.pdf)😙


이번 논문은 요즘 안쓰는 사람이 없다는 self-attention 에 대해 공부하기 위해 리뷰하였다. 정확히는 vision transformer(vit) 에 대해서 이해하려니 먼저 이 `Attention is all you need` 논문은 읽고 attention이 무엇인지 이해한 후에 vit로 넘어오면 vit의 이해에 도움이 된다고 한다. 

실제로 먼저 ViT 읽는데 무슨소린지 몰라 당황했는데 ㅎㅎ 이 논문을 읽으니 확실히 이해가 되었다. 

이번 리뷰는 그림도 많고 이해해야할 것도 많기 때문에 아주 길 것으로 예상이 된다😬😬 ㅎㅎ 그럼 이제 열심히 달려봅시다 👍 



👑🦄🐳🐬🌱☘️🍀🌼🌙🌎🌍🪐💫⭐️🌟
<br>

## 🌟 Background

그 동안 RNN은 언어 모델링과 기계번역과 같은 시계열 번역에 있어서 SOTA 성능을 보였다. 하지만 입력 시퀀스 데이터를 순차적으로 처리하는 구조이기 때문에 병렬처리가 불가능하다는 문제가 있었다. 이 때문에 메모리의 제약으로 batch에 제한이 생겨 시퀀스의 길이가 길어질수록 long term dependecy 문제가 심해진다.


> Long term dependecy 란?   
> 
>  입력시퀀스가 길어질 수록 문장 맨 앞에 나오는 입력값에 대한 정보가 손실되는 것


#### RNN 구조에 기반하여 이해해보자

<img width="1576" alt="무제" src="https://user-images.githubusercontent.com/53431568/135964393-ba8309ca-6828-4d46-bcc6-58aff02c03ec.png">

RNN 구조는 스스로 반복하면서 이전단계에서 얻은 정보가 지속되도록 하는 구조이다. A를 RNN의 한 덩어리로 보자! A는 input $x_t$를 받아서 $h_t$를 내보냄과 동시에 관련 정보(information)를 다음 step의 network(cell)에 넘겨준다. 그럼 다음 step에서는 받은 정보와 해당 step에서 받은 $x_t$(input)값을 연산해 다음 $h_t$ 값을 찾는 방식으로 작동한다. 

이렇게 `맥락을 다음 step에 넘겨줌`으로써 자연어처리, 주가 예측 등 다양한 시계열 분석이 가능해졌다. 하지만 step 이 길어지면서(문장이 길어지면서) 예전에 했던 말 즉, 문장의 앞부분에 대한 정보가 손실되어 앞의 문맥이 뒤까지 이어지지 못하는 `Long-Term Dependency` 문제가 발생하게 되는 것이다.

<hr>

Long-Term Dependency 문제를 해결하기 위해 제안된 LSTM(long-short term memory) 이다.

<img width="1495" alt="무제 2" src="https://user-images.githubusercontent.com/53431568/135964180-54ba574f-6259-40df-a2f8-a38730d7ab51.png">


'해당 정보를 얼마나 까먹고(forget gate), 얼마나 다음 Step으로 바로 이어줄지 정하는 **Gate**를` 넣는 것이 핵심이다. 그 과정이 가운데 시그마, tanh 등으로 계산되어서 정리되어 출력된다.


<br>

LSTM을 사용한 간단한 encoder-decoder 구조이다. 자연어 처리는 아래와 같은 프로세스로 진행이 된다.

<img width="988" alt="무제" src="https://user-images.githubusercontent.com/53431568/135964075-b467af78-7b7b-4d50-8746-e65bd7dbcadd.png">

이런 식으로 해당 step의 입력값과 받은 정보값을 가지고 값을 예측하는데, encoder 에서 시퀀스 데이터를 하나의 벡터값으로 압축한 후에 decoder 에서 차례대로 출력을 하게 된다. 


**이 논문에서는 recurrence를 피하는 대신 input과 output 사이의 global dependecy를 찾는 attention mechanism만 사용하는 Transformer 구조를 제안**한다. Transformer는 더 많은 병렬처리가 가능하며 state-of-the-art 수준을 보인다.



## 🌎 Transformer

이 논문에서 사용하였다고 하는 transformer에 대해서 알아봅시다~ transformer의 전체적인 구조는 아래와 같고 구체적으로는 마지막에 한 번 더 설명하게 될 테니 일단 구조만 익혀두고 넘어가기~

<img width="382" alt="무제" src="https://user-images.githubusercontent.com/53431568/135963495-b45cb7d2-9c5b-4352-a3eb-a4c4c2f5dfde.png">


이제 transformer에서 사용되는 `Attention`에 대해 찬찬히 뜯어봅시다! 위의 구조를 보면 attention 레이어를 볼 수 있는데, attention이란 간단히 말하자면 관련 있는 정보, 중요한 정보에 더 관심을 둔다는 것입니다. 그래서 말그대로 attention(주목~) 이라는 단어를 쓴 것 같아요.


<img width="1383" alt="무제" src="https://user-images.githubusercontent.com/53431568/135963346-54f4d734-2a54-4fe8-9c1a-2d6b2a21c325.png">

Attention은 query를 key-value 짝과 매핑시켜 output으로 내는 것이다. 여기서 key, value, query는 벡터값이며 output은 value의 weighted sum으로 계산된다. 

Query와 key 의 dot-product를 계산해서 이 둘 사이의 유사도를 구한 후 기울기 vanishing 문제를 예방하기 위해서 key의 차원값으로 한차례 나눠 준 다음에 softmax 를 계산해준다. 이 후 value 값으로 곱한다.

softmax를 거친 값을 value에 곱해준다면, query와 유사한 value일 수 록, 즉 `중요한 value일 수록 더 높은 값`을 가지게 됩니다. `중요한 정보에 더 관심을 둔다는 attention의 원리`를 여기서 확인할 수 있다.


key, value, query 의 세 벡터를 어떻게 처리하는지 그림과 함께 프로세스를 살펴 보겠습니다






<br>
<br>


#### 참고

[1] [https://dgkim5360.tistory.com/entry/understanding-long-short-term-memory-lstm-kr](https://dgkim5360.tistory.com/entry/understanding-long-short-term-memory-lstm-kr)

[2] [https://omicro03.medium.com/attention-is-all-you-need-transformer-paper-%EC%A0%95%EB%A6%AC-83066192d9ab](
https://omicro03.medium.com/attention-is-all-you-need-transformer-paper-%EC%A0%95%EB%A6%AC-83066192d9ab)

[3] [https://www.youtube.com/watch?v=KT58deB6oPQ](https://www.youtube.com/watch?v=KT58deB6oPQ)

[4] [https://www.youtube.com/watch?v=mxGCEWOxfe8](https://www.youtube.com/watch?v=mxGCEWOxfe8)

[5] [https://www.youtube.com/watch?v=bgsYOGhpxDc](https://www.youtube.com/watch?v=bgsYOGhpxDc)


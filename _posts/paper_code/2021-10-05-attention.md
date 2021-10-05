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

이번 리뷰는 그림도 많고 이해해야할 것도 많기 때문에 아주 머나면 여정이 될 것으로 예상이 된다😬😬 ㅎㅎ 그럼 이제 열심히 달려봅시다 👍 


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

<br>

Long-Term Dependency 문제를 해결하기 위해 제안된 LSTM(long-short term memory) 이다.

<img width="1495" alt="무제 2" src="https://user-images.githubusercontent.com/53431568/135964180-54ba574f-6259-40df-a2f8-a38730d7ab51.png">


'해당 정보를 얼마나 까먹고(forget gate), 얼마나 다음 Step으로 바로 이어줄지 정하는 **Gate**를` 넣는 것이 핵심이다. 그 과정이 가운데 시그마, tanh 등으로 계산되어서 정리되어 출력된다.


<br>

LSTM을 사용한 간단한 encoder-decoder 구조이다. 자연어 처리는 아래와 같은 프로세스로 진행이 된다.

<img width="988" alt="무제" src="https://user-images.githubusercontent.com/53431568/135964075-b467af78-7b7b-4d50-8746-e65bd7dbcadd.png">

이런 식으로 해당 step의 입력값과 받은 정보값을 가지고 값을 예측하는데, encoder 에서 시퀀스 데이터를 하나의 벡터값으로 압축한 후에 decoder 에서 차례대로 출력을 하게 된다. 


**이 논문에서는 recurrence를 피하는 대신 input과 output 사이의 global dependecy를 찾는 attention mechanism만 사용하는 Transformer 구조를 제안**한다. Transformer는 더 많은 병렬처리가 가능하며 state-of-the-art 수준을 보인다.


<br>

## 🌎 Transformer

이 논문에서 사용하였다고 하는 transformer에 대해서 알아봅시다~ transformer의 전체적인 구조는 아래와 같고 구체적으로는 마지막에 한 번 더 설명하게 될 테니 일단 구조만 익혀두고 넘어가기~

<img width="382" alt="무제" src="https://user-images.githubusercontent.com/53431568/135963495-b45cb7d2-9c5b-4352-a3eb-a4c4c2f5dfde.png">


이제 transformer에서 사용되는 `Attention`에 대해 찬찬히 뜯어봅시다! 위의 구조를 보면 attention 레이어를 볼 수 있는데, attention이란 간단히 말하자면 관련 있는 정보, 중요한 정보에 더 관심을 둔다는 것입니다. 그래서 말그대로 attention(주목~) 이라는 단어를 쓴 것 같아요.

### 🌍 Attention

![Uploading image.png…]()



Attention은 query를 key-value 짝과 매핑시켜 output으로 내는 것이다. 여기서 key, value, query는 벡터값이며 output은 value의 weighted sum으로 계산된다. 

Query와 key 의 dot-product를 계산해서 이 둘 사이의 유사도를 구한 후 기울기 vanishing 문제를 예방하기 위해서 key의 차원값으로 한차례 나눠 준 다음에 softmax 를 계산해준다. 이 후 value 값으로 곱한다.

softmax를 거친 값을 value에 곱해준다면, query와 유사한 value일 수 록, 즉 `중요한 value일 수록 더 높은 값`을 가지게 됩니다. `중요한 정보에 더 관심을 둔다는 attention의 원리`를 여기서 확인할 수 있다.


key, value, query 의 세 벡터를 어떻게 처리하는지 그림과 함께 프로세스를 살펴 보겠습니다


<img width="1004" alt="무제" src="https://user-images.githubusercontent.com/53431568/135964598-8f99affb-84ea-4f57-afb9-f6a49699ad95.png">

`나는 지금 소파에 누워있다`라는 입력 시퀀스가 입력으로 들어온다면 이것을 x 행렬벡터로 임베딩을 시켜준 후, 각 query, key, value 의 weight 행렬벡터 (각, $W^Q$, $W^K$, $W^V$) 와 곱해주어 query, key, value 벡터를 얻어낸다. 이제 이 세가지 벡터 값들로 attention 연산을 수행하는 것이다! 😲👍

> Key: 영향을 주는 단어
> 
> value : 영향 가중치
> 
> Query : 영향을 받는 단어

<br>


attention 연산을 차례로 전개해 본 결과이다. 만드느라 팔 빠지는 줄...  

#### ⭐️ 연산 순서

1) Query와 key 의 dot product를 계산해서 이 둘 사이의 유사도를 구한다
2) 기울기 vanishing 문제를 예방하기 위해서 key의 차원값($\sqrt{d_k}$)으로 한차례 나눈다.
3) softmax 계산
4) value 값으로 곱한다
5) summation


<img width="1025" alt="무제" src="https://user-images.githubusercontent.com/53431568/135965005-82ab7c9c-9b12-4c0e-9d7d-f3e804890d7b.png">
<img width="1029" alt="무제 2" src="https://user-images.githubusercontent.com/53431568/135965039-410e41a6-cd5e-47dc-8362-54d2267930ee.png">
<img width="1028" alt="무제 3" src="https://user-images.githubusercontent.com/53431568/135965067-42b11961-4035-46c6-87e6-6e765b89e4f9.png"><img width="1030" alt="무제 4" src="https://user-images.githubusercontent.com/53431568/135965120-554c0eaf-ac94-4ea5-be6f-dae09661ea4d.png">


### 🌍 Multi-Head Attention


이렇게 차근차근 계산하면 output값이 출력이 되는데, transformer 구조를 다시 자세히 보면 `Multi-Head Attention` 레이어를 쓴다는 것을 볼 수 있습니다. 이것은 위에서 공부한 self-attention을 병렬로 h번 (논문에서는 8번이라고 제시함) 수행하는 방식으로 진행된다고 한다.아래 그림처럼 각 v, k, q 에 대해서 self-attention을 8번 수행하여 나온 결과(8개)를 concat 해준 후 linear 하게 projection 한 값을 output 으로 해준다.

<img width="1005" alt="무제" src="https://user-images.githubusercontent.com/53431568/135965569-56c7f210-bc9f-43ab-81bf-db1d87dc29a7.png">

다시 벡터의 그림 으로 한번 살펴 보면, K, Q, V 세 벡터를 만드는 연산을 8번 반복, 이것으로 attention 연산을 8번 반복해 나온 출력값 $z_0$ ~ $z_7$ 을 모두 concat 을 진행시켜주면 4x32 의 매트릭스가 생성이 된다. 최종 출력값의 차원을 맞춰주기 위해 32 by 4 의 weighted matrix $W^O$를 연산해주어 최종 $z$ 값을 출력해준다.

<img width="1052" alt="무제 2" src="https://user-images.githubusercontent.com/53431568/135965913-cf0b772d-1878-4e3c-b666-ac0b6082eae9.png">


<br>

### 🌍 Positional Embedding


Self-attention은 입력 시퀀스 데이터를 순차적으로 처리하지 않기 때문에 Positional encoding 방법을 사용하고 있다. Positional encoding은 입력 시퀀스에서 단어의 순서를 표현하기 위한 임베딩 방법으로 (-1 ~ 1) 범위의 값을 반환한다.

<img width="1497" alt="무제" src="https://user-images.githubusercontent.com/53431568/135966557-af1f3d81-0332-454b-b52b-ac9e32f01c63.png">


Sin, cos 함수를 사용하였고 이런 수식을 사용한 이유는 관련 포지션에 대해서 모델이 쉽게 배울 수 있을거라 가설을 세웠다고 한다. 

#### PE 연산으로 나온 매트릭스 벡터는 단어의 임베딩 벡터와 더해져서 결국 상대적 위치의 값으로 구성된 최종 행렬이 encoder과 decoder의 입력값으로 들어간다. 

<br>

## 🪐 Transformer Details

이제 공부한 것을 바탕으로 전체적인 transformer의 구조를 파악해보자. 먼저, encoder 부분이다 residual 연결을 잘 파악하면서 보자!

<img width="790" alt="무제" src="https://user-images.githubusercontent.com/53431568/135968686-28b3a9bf-46ef-4955-b0f6-ce331469742d.png">

입력 토큰이 2개라고 가정하고 input 값을 1차원의 벡터값으로 임베딩한 값과 PE 값을 더해 self-attention 을 취해준 후 나온 ouput 값 $z$ 를 layernorm 의 input 으로 두고, attention 에 들어가기 전의 값($x$)도 함께 layernorm 처리를 해주는데, 이 두 input 값을 더해준 후, 정규화를 시켜준다. 정규화로 나온 벡터 값을 각각 feed forward 시켜준 후 아까와 같은 프로세스로 Add & Normalization 을 처리해준다. 이게 하나의 인코더이며 이것이 N 번 수행된다. (논문에서는 6번으로 함)

다음은 encoder와 decoder를 두 개만 있다고 가정 한 전체적인 encoder-decoder 프로세스인데, decoder는 encoder와 다르게 `Encoder-Decoder Attention`이라는 레이어를 사용한다. 나머지 프로세스는 같지만, 이 부분을 보면 Q, V, K 값 중, Q는 decoder 의 output 값으로 나온 벡터를 사용하고, K, V 벡터는 마지막 encoder 에서 나온 output 값으로 사용해 처리하게 된다

<img width="1368" alt="무제 2" src="https://user-images.githubusercontent.com/53431568/135968952-9cb5fee0-d7e6-4fea-9e10-f546b2e3fd6f.png">


이런식으로 각 값을 달리 사용하는 이유는, 첫 부분에서 배운 인코더-디코더의 기본 프로세스를 보면 알 수 있다. 현재 state의 input 값과 그 전 lstm 에서 나온 이전 정보 값을 갖고 함께 output 값을 내는 원리로 작동을 한다! 따라서 transformer를 사용하는 여기 decoder, encoder 에도 비슷한 프로세스로 진행을 하는 것이다.

영향을 주는 값인 key와 영향 가중치인 value 값을 encoder의 최종 ouput 값에서 가져오고, decoder의 ouput 에서 영향을 받는 값인 query 를 가져와 세 가지의 벡터로 self-attention 연산을 수행하는 것이다.  
이렇게 self-attention 연산을 수행하면, `decoder가 입력 시퀀스 데이터의 적절한 위치에 집중`할 수 있도록 도와주는 역할을 한다고 한다.

<img width="988" alt="무제" src="https://user-images.githubusercontent.com/53431568/135964075-b467af78-7b7b-4d50-8746-e65bd7dbcadd.png">

단, self-attention을 사용함으로써, 각 단어(토큰)들의 중요도를 수치화한 정보를 병렬로 연산처리하여 손실이 일어나지 않아 long term dependecy 문제점은 해결하면서, 더 깊은 네트워크 sequence에서도 SOTA 성능을 내었다는 것에 다시 한 번 의의를 가진다! 

<br>
<br>




마지막으로 내의 🔥열쩡!🔥을 보여주는 레퍼런스 리스트들.. ㅎㅎ

#### 참고

[1] [https://dgkim5360.tistory.com/entry/understanding-long-short-term-memory-lstm-kr](https://dgkim5360.tistory.com/entry/understanding-long-short-term-memory-lstm-kr)

[2] [https://omicro03.medium.com/attention-is-all-you-need-transformer-paper-%EC%A0%95%EB%A6%AC-83066192d9ab](
https://omicro03.medium.com/attention-is-all-you-need-transformer-paper-%EC%A0%95%EB%A6%AC-83066192d9ab)

[3] [https://www.youtube.com/watch?v=KT58deB6oPQ](https://www.youtube.com/watch?v=KT58deB6oPQ)

[4] [https://www.youtube.com/watch?v=mxGCEWOxfe8](https://www.youtube.com/watch?v=mxGCEWOxfe8)

[5] [https://www.youtube.com/watch?v=bgsYOGhpxDc](https://www.youtube.com/watch?v=bgsYOGhpxDc)


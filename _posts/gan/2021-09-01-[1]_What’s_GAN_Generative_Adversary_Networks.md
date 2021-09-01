---
title:  "[GAN study] What’s GAN Generative Adversary Networks? - GAN 이란?"
excerpt: ""

categories:
  - gan

tags: [Deeplearning, study, gan]

classes: wide

use_math : true

last_modified_at: 2021-09-01T08:06:00-05:00

---

<br>

GAN의 개념과 알고리즘을 공부할 수 있는 포스팅이다. [블로그 원문](https://jonathan-hui.medium.com/gan-whats-generative-adversarial-networks-and-its-application-f39ed278ef09)내용도 쉬운 영어로 쓰여있어 내용을 이해하기도 쉬웠다. 영어로 쓰여있는게 더 편하다면 [이곳에](https://jonathan-hui.medium.com/gan-whats-generative-adversarial-networks-and-its-application-f39ed278ef09) 가서 읽어보는 것을 추천한다!🖐

그리고 이 포스팅 내용은 [Generative Adversarial Nets](https://arxiv.org/pdf/1406.2661.pdf)와 [Unsupervised Representation Learning with Deep Convolutional Generative Adversarial Networks](https://arxiv.org/pdf/1511.06434.pdf)의 논문 내용을 참고한것 같다. 논문에 대한 자세한 내용은 추후 포스팅을 하겠다.

<img width="662" alt="무제" src="https://user-images.githubusercontent.com/53431568/131616172-acd97a85-820f-4d87-9db8-13b258eddf3c.png">

<br>


## GAN - Generative Adversarial Networks Gan 이란?

갠은 초상화를 그리거나 심포니를 작곡하는 것과 같은 "creation"이다.

다른 딥러닝 분야와 비교했을때 창조하는 것은 어렵다. 

컴퓨터 또는 사람이 모네의 그림을 다른 작가의 그림들과 비교하는 것은 쉬운일이다. 하지만 이런 원리가 지능(intelligence)을 이해하는데 도움이 된다.

게임 개발에 있어서 애니메이션을 만들기 위해 많은 개발 예술가를 고용하며 그들이 하는 일은 특별할 것 없는 일상적일 것이다. 

하지만 우리는 **GAN 과 자동화를 적용함으로써 우리는 일상의 루틴을 반복하기 보다는 좀 더 "창조적인 일"에 집중**할 수 있다고 한다.

이 글에서는 GAN의 개념과 알고리즘을 설명하고 있으니 잘 파악해 보도록 하자.

<br>

## What does GAN do?

GAN의 중점은 scratch로부터 데이터를 생성하는 것이다( 대부분은 이미지들이며 음악을 포함한 다른 영역들은 더 이상 생성하지 않는다. )

이미지에 국한되어있지만 GAN의 적용범위는 꽤 크다. 아래 예시와 같이 GAN은 얼룩말로부터 말을 생성해 낸다. 강화학습에서는 로봇이 더 빨리 배우는 것을 돕는다.

<img width="440" alt="horse" src="https://user-images.githubusercontent.com/53431568/131535315-f5245d45-1eda-4ebe-ad39-bb3bc192b693.png">

<br>

## Generator와 Discriminator

**`GAN의 핵심 개념이므로 집중!`**

gan은 두가지 깊은 network로 구성되어있다. 하나는 generator(생성자), 다른 하나는 discriminator(구분자)이다.

우린 먼저 생성자가 어떻게 학습을 어떻게 하는지 알기도 전에 어떻게 **이미지를 만들어 내는지**를 살펴볼 것이다.

먼저, 어떤 noise **z** 가 있다고 가정하자. x를 input으로 한 generator G인 image x 를 만들것이며 수식은 아래와 같다.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **$x=G(z)$**

개념적으로 z는 색과 모양등을 생성하는 **"latent features of images generated"**를 뜻한다. 직역하자면 생성된 이미지들의 잠재 특징(공간/영역)인데, 그렇다면 이것의 뜻은 뭘까?

<details markdown="1">
<summary>latent space🔎</summary>

<hr>

이미지를 생성하는 공간, 나타내려고 하는 대상의 정보를 충분히 담을 수 있는 공간, 'z'벡터가 존재하는 공간, 이미지의 특성을 찾아낼 수 있는 공간.

요약하자면 **"latent space에서는 어떤 특성(예를 들면 웃는 표정)을 찾아낼 수 있고 'z'벡터에서 이에 해당하는 값을 바꿈으로써 원하는 이미지/패턴(슬픈 표정으로 바꾸는 등)을 생성해 정답을 도출(딥러닝 개념으로는 패턴을 찾는)할 수 있다"**

#### 따라서 latent space는 딥러닝의 핵심이라고 볼 수 있다.

<hr>

</details>


딥러닝 classification에 있어서 우리는 모델이 학습하고 있는 속성을 제어하지 않는다. 

비슷하게 GAN에서도 우리는 z의 semantic(의미론적) 속성을 제어하지 않는다. 우리는 학습 프로세스가 알아서 학습하도록 냅둔다.
(예를 들면, z의 어떤 byte 값이 헤어컬러를 결정짓는지 제어하지 않는다) 

이 의미를 살펴보자면 생성된 이미지를 구성하고 하고 스스로 시험해보는 것이 가장 효과적인 방법이라는 것이다. 

GAN은 다른 CNN 구조들 처럼 사람이 직접 준 데이터로 학습을 하기보다는 잠재적인 공간에서 벡터값으로 스스로 생성을 하는 모델이기 때문에 위와 같은 말을 한 것 같다.

아래 이미지들은 random noise z를 이용해 GAN에 의해 생성된 이미지들이다.

<img width="379" alt="무제 2" src="https://user-images.githubusercontent.com/53431568/131542098-55a654ef-bb18-4cb7-979f-3a8cb15873b7.png">

<br>

z 의 한 특정 차원을 단계적으로 바꿀 수 있으며 그것의 의미론적 의미를 시각화할 수 있다. 


<img width="462" alt="무제" src="https://user-images.githubusercontent.com/53431568/131542435-f814ca1f-b2c2-44be-9d96-cd414d4c8d21.png">


<br>

그렇다면 이 마법같은 **generator G**는 과연 무엇일까?

> 생성자의 generator 의 앞글자 **G**를 따서 G라고 하며 구분자 또한 **D** 라고 한다

아래 레이어 구조는 DCGAN의 구조이며 DCGAN은 가장 유명한 GAN 구조 중 하나이다.

이 구조는 z를 upsampling 해 image x 를 생성하기 위해 다중 transposed 컨볼루션을 수행한다.

우리는 이것을 deep learning classifier의 반대라고 해석할 수 있다.

<img width="685" alt="무제" src="https://user-images.githubusercontent.com/53431568/131542854-9715e7e4-fce4-479a-b412-139b184eaa3d.png">

<br>

하지만 생성자 혼자로써는 random noise로 부터 창조하기만 할 뿐이다. 개념적으로 GAN의 구분자(discriminator D)는 생성자에게 어떤 이미지를 창조할 것인지에 대한 가이드를 제공해준다.

GAN의 또 다른 application을 고려해보자 바로 CycleGAN 이다. CycleGAN은 생성자를 사용해 실제 풍경을 모네 화풍의 그림으로 바꿔준다.

<img width="551" alt="무제" src="https://user-images.githubusercontent.com/53431568/131543065-782e32c2-bf4a-4460-b7cb-5b6bca72c683.png">

<br>


실제 이미지와 생성된 이미지를 학습함으로써 GAN은 구분자가 어떤 특징이 이미지를 더 실제같이 만드는지를 배우게 된다.

그렇게 되면 같은 구분자는 생성자에게 더 모네 그림같은 그림을 창조하라고 피드백을 주게 된다.

<img width="490" alt="무제 2" src="https://user-images.githubusercontent.com/53431568/131543416-50f4be1c-31a8-4c70-9b68-a11c40f82b74.png">

<br>

그렇다면 이제 기술적인 면을 살펴봅시다. ✈️

구분자는 실제 이미지(학습 샘플들)와 생성된 이미지를 구분해서 바라본다. 이렇게 입력으로 들어온 이미지가 실제 이미지인지 생성된 것인지 구분한다.

출력값인 $D(X)$ 는 Input $x$ 가 실제인지에 대한 확률값이다. (ex $P(class of input = real image)$)

<img width="485" alt="무제 3" src="https://user-images.githubusercontent.com/53431568/131543688-6d477a9e-1a34-49ef-a6aa-ed290cb185c3.png">

<br>

우린 구분자를 deep network classifier과 같이 학습시킨다. 만약 입력값이 실제 이미지 라면 $D(x) = 1$ 이 , 생성된 이미지라면 반대로 $D(x) = 0$ 이 나오도록 한다.

이 과정을 통해 구분자는 실제 이미지에 기여하는 `"특징을 식별"`한다.

반대로, 우리의 목표는 생성자가 $D(x) = 1$을 도출하는 이미지를 생성하는 것이다.

이렇게 역전파를 통해 생성자를 학습시키며 이 타겟 값들은 모두 생성자로 돌아갈 것이다. (예를 들어 구분자가 이건 진짜야! 라고 속을만한

이미지를 생성하도록 학습하는 것이다! **핵심 => 구분자를 속여라! 가짜를 진짜로 믿도록!**)🥸😲


<img width="475" alt="무제" src="https://user-images.githubusercontent.com/53431568/131544235-4d4b95f8-51cc-4501-a936-6a0420a60228.png">

<br>

우리는 생성자 구분자 각각의 **`network를 번갈아가며 학습시키고 그들 스스로 개선될 수 있도록 경쟁`**을 시킨다. 

결국 구분자는 실제와 생성된 것 사이의 아주 미세한 차이도 알아차릴 수 있게 되고, 생성자는 구분자가 이 둘 사이의 차이를 못 찾아내는 이미지를 창조해낸다. (마치 라이크 창과 방패의 싸움🛠)

GAN 모델은 결국 수렴하고 자연스러운 이미지를 생성하게 된다. 이 구분자 개념은 많은 현존하는 딥러닝 application에 적용되고 있따. GAN의 구분자는 비평가 역할을 한다.

우리는 구분자로 하여금 현존하는 딥러닝 해결책을 제시하도록 피드백을 제공해 더 나은 결과를 만들 수 있다.

<img width="672" alt="무제" src="https://user-images.githubusercontent.com/53431568/131544600-013002bf-4c03-48e8-8f72-284dcc102949.png">


<br>

### 역전파

이제 우리에게 한가지 관문(?)이 남았다 바로 간단한 수식인데, 구분자의 결과 값인 $D(x)$에서 $x$는 실제 이미지를 뜻한다.

### 구분자!
구분자의 목표는 실제이미지를 실제로, 생성된 이미지를 가짜로 인식할 가능성을 높이는 것이다!

예를 들어 연구한 데이터의 최대 가능도( i.e. the maximum likelihood of the observed data) loss(손실값)을 측정하기 위해 cross-entropy를 Deep Learning: $plog(q)$로 사용한다. 

실제이미지 p는 1이며 (true label for real image is 1) 반대로 생성된 이미지(가짜)는 반대의 라벨을 붙인다.(-1)

이를 수식으로 정리하면 다음과 같다.  

<img width="575" alt="무제 2" src="https://user-images.githubusercontent.com/53431568/131545125-029bb5fa-a39b-4b66-bbaf-87a21368d638.png">


<br>

### 생성자!
생성자쪽 수식을 보면, objective function은 모델이 구분자를 높은 가능성으로 속이는 이미지를 생성하길 원한다.

생성자의 입장에서는 작을수록 좋기 때문이다. 왜! D가 속아야 그만큼 실제이미지처럼 잘만들었다는 거니깐!

<img width="405" alt="무제 3" src="https://user-images.githubusercontent.com/53431568/131545288-4dc46e4e-fb3d-4aaa-b53d-0ac902de6e96.png">


<br>



"We often define GAN as a minimax game which G wants to minimize V while D wants to maximize it."

"GAN은 G는 V를 최소화하려고 하고 D는 V를 최대화하려고 경쟁하는 최대최소 게임"

최종 수식을 보면 아래와 같다.

<img width="566" alt="무제 4" src="https://user-images.githubusercontent.com/53431568/131545581-da6839fd-354e-4184-b564-1323c1595faa.png">


<br>


양쪽의 함수가 정의되었다면 이 둘은 번갈아가며 gradient descent를 학습한다.

학습방법은 다음과 같다.

먼저, 생성자 모델의 파라미터를 고정시키고 실제와 생성된 이미지를 갖고 구분자를 gradient discent의 반복을 한번 수행한다.

그 다음으로 바꾸어서, 구분자를 고정하고 생성자를 한번 반복해 학습시킨다. 두 네트워크를 번갈아가며 학습하는데 학습은 생성자가 양질의 이미지를 생성할때까지 학습된다. 아래는 데이터 흐름과 역전파를 위해 사용된 기울기를 요약한 것이다.

<img width="678" alt="무제 5" src="https://user-images.githubusercontent.com/53431568/131545750-f5bf1846-8c92-4dd9-8a21-62231a24247b.png">

<br>

다음은 GAN이 어떻게 수행되는지 한번에 보여주는 수도코드(의사코드)이다.

<img width="675" alt="무제" src="https://user-images.githubusercontent.com/53431568/131545861-42997a11-39ab-4976-a29e-c5d3d64eb02e.png">



<br>

### 생성자가 기울기를 없앤다?! (gradient vanishing problem)

구분자는 생성자에 대항해 일찍 이기곤하는데(?) 이것은 이른 학습에 있어서 생성된 이미지로부터 실제 이미지를 구분하는 것이 쉬우며 이것 때문에 V가 0에 도달할 수 있도록 만든다. ($log(1-D(G(z)))$ -> 0)  

(로그함수 그래프를 그려보면 왜 점점 0에 수렴한다는지 알 수 있다.)

생성자를 위한 기울기는 점점 0에 수렴해 없어질 것이고 최적화를 매우 느리게 만든다. 이를 개선하기 위해 GAN에서는 생성자에 기울기를 역전파하게 하는 반대 함수를 제공한다. 이것에 대한 설명은 GAN 논문 포스팅에서 추후 다룰 예정이다!✌️

<img width="561" alt="무제 2" src="https://user-images.githubusercontent.com/53431568/131546303-1a1525c3-74df-41c7-8f21-3794d32c08d9.png">

<br>


## More thoughs

일반적으로 데이터를 생성한다는 개념은 엄청난 잠재력을 이끌면서도 매우 위험하다. 

GAN 말고도 많은 생성 모델들이 있다. OpenAI의 GPT-2 는 실제 저널리스트가 쓴 것같은 문장을 생성한다. 그래서 인지 OpenAI는 오용을 위해 데이터셋과 학습된 모델을 오픈하지 않기로 한다.

창조를 한다는 것에는 오용 남용, 등의 문제와 악용의 문제가 있다. 항상 연구윤리를 준수하면서 연구해야 할 것을 다짐하곤 한다.

<br>
<br>


#### 참고

[1] [https://towardsdatascience.com/understanding-latent-space-in-machine-learning-de5a7c687d8d](https://towardsdatascience.com/understanding-latent-space-in-machine-learning-de5a7c687d8d)


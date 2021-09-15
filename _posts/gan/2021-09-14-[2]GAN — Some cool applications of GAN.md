---
title:  "[GAN study] Some cool applications of GAN - GAN을 활용한 어플리케이션 소개"
excerpt: ""

categories:
  - gan

tags: [Deeplearning, study, gan]

classes: wide

use_math : true

last_modified_at: 2021-09-14T08:06:00-05:00

---



Gan 학습 로드맵 두번째 시간!

오늘은 GAN을 활용한 어플리케이션에 대해 공부해 볼 건데요! GAN이 어떻게 쓰일 수 있는지! 어디에 적용될 수 있는지 알아보는 시간을 가지도록 합시다~!! ✌️

이 글은 [Jonathan Hui의 포스팅](https://jonathan-hui.medium.com/gan-some-cool-applications-of-gans-4c9ecca35900)을 참고해 정리한 내용입니다!


## Some cool applications of gan


![0_3bbZGnvcwgN8BNkD](https://user-images.githubusercontent.com/53431568/133250975-478d156e-45fb-497a-be3b-f0cd942593e0.jpeg)

<center>[Photo by SwapnIl Dwivedi] </center>  

<br>


처음 몇 년 동안엔 GAN 개발에서서는 주목할만한 진전이 있었다. 옛날 GAN으로는 호러영화에 나오는 무서운 얼굴같은 이미지가 우편 사이즈로 나왔었는데, 2017년 기준으로 gan은 진짜 사람이라고 착각하게 할 만한 1024 x 1024 사이즈의 이미지를 생성해냈다.

<img width="811" alt="무제" src="https://user-images.githubusercontent.com/53431568/133251253-4fe67cd1-480e-42e3-b68a-9a107ac87a33.png">


앞으로 몇년동안 우리는 아마 GAN이 생성한 매우 높은 퀄리티의 비디오를 보게 될 것입니다. 광고 어플리케이션도 마찬가지이며, 사람들에게 GAN에 대한 영감을 불어넣기 위해 엄청 멋있는 어플리케이션들을 살펴보겠습니다~

<br>

### Create Anime characters

게임 개발과 애니메이션 프로덕션은 매우 비싸며 각각의 반복적인 일을 위해 많은 프로덕션 아티스트를 고용해야합니다. 하지만 GAN은 캐릭터를 자동으로 생성할 수 있으며 색상도 입힐 수 있습니다! 즉! 그런 반복적인일을 한다는 것은 이제는 비생산적입니다! GAN이 알아서 만들어주기 때문이죠.

<img width="690" alt="무제 2" src="https://user-images.githubusercontent.com/53431568/133252314-fcf198be-7659-409f-9418-0c8aea6aa122.png">

대량생산된 캐릭터 이미지들입니다. 꽤 그럴싸한 캐릭터들이 만들어지네요.. 애니메이션도 이젠 양산형 산업이 되는 걸까요..!


생성자와 식별자는 컨볼루션 레이어와 batch-normalization, skip connection 된 ReLU들의 여러 레이어들로 구성되어 있습니다. 아래 이미지를 참고하면 좋을 것 같습니다.

<img width="718" alt="무제 3" src="https://user-images.githubusercontent.com/53431568/133252482-0f26913e-1a9e-41f6-b727-7ea59ec3956d.png">


<center>애니메이션 gan 논문 : [Towards the Automatic Anime Characters Creation with Generative Adversarial Networks](https://arxiv.org/pdf/1708.05509.pdf)</center>

<br>

### Pose Guided Person Image Generation

포즈에 대한 추가적인 입력으로 한 이미지를 다른 포즈들로 변환할 수 있는 어플리케이션입니다. 

이 어플리케이션의 작동 방식을 예를 들어 설명하면, 아래 그림의 오른쪽 위 이미지는 ground truth(원래 실제 이미지) 이고 오른쪽 아래의 이미지는 generated image(생성된 이미지)로 포즈가 마뀐 것을 확인해 볼 수 있죠?!

<img width="688" alt="무제 4" src="https://user-images.githubusercontent.com/53431568/133252673-679945ff-57a0-426b-b2c9-8ff0bfb35611.png">


생성된 이미지들을 정리한 결과는 다음과 같습니다. 진짜 사람이 직접 포즈를 취한것 처럼 자연스럽네요~! 😳👍👍

![무제](https://user-images.githubusercontent.com/53431568/133252821-72c720a3-2a4c-4c02-af94-61a58ea70709.png)


구조는 2-stage 이미지 생성자와 식별자로 구성되어있습니다. 생성자는 메타데이터(pose) 이미지와 원본이미지를 이용해 재구축하며 식별자는 원본이미지를 CGAN 구조에 레이블 인풋값의 한 부분으로 사용합니다.

<img width="703" alt="무제" src="https://user-images.githubusercontent.com/53431568/133253646-aba04125-3752-47e4-837e-72a022ec2a0b.png">



포즈 이미지 제네레이션 관련 논문 :  [Pose Guided Person Image Generation](https://papers.nips.cc/paper/2017/file/34ed066df378efacc9b924ec161e7639-Paper.pdf)

<br>

### CycleGAN

Cross-domain transfer GAN은 상업적 어플리케이션의 첫 회분 입니다. 이러한 GAN은 이미지를 한 영역(실제 풍경)에서 다른 영역(모네 그림 혹은 고흐 그림)으로 변환합니다.

<img width="688" alt="무제 2" src="https://user-images.githubusercontent.com/53431568/133253855-a41f8939-a509-48b8-badb-16218db37a32.png">


예를들어, CycleGAN은 얼룩말을 말로 바꿀 수 있습니다.

<img width="698" alt="무제" src="https://user-images.githubusercontent.com/53431568/133353544-bfc6c17d-f093-4c0e-87be-52315f09db28.png">


CycleGAN 은 한 영역에서 다른영역 그리고 한 영역에서 반대 방향으로 이미지들을 만들기 위해 두 가지 네트워크인 G와 F를 만듭니다. 그리고 식별자 D는 이미지가 얼마나 잘 생성되었는지 평가하게 됩니다. 예를 들어 G가 실제 이미지에서 반고흐풍의 그림으로 변환하면 Dy는 그 이미지가 실제인지, 생성된 이미지(fake)인지 구분한다.

> Domain A -> Domain B:

<img width="703" alt="무제" src="https://user-images.githubusercontent.com/53431568/133353711-62a5fca7-e9c3-4173-b255-9b02f0a21ea6.png">


이 과정을 역방향으로 반복한다. 

> Domain B -> Domain A:

<img width="492" alt="무제" src="https://user-images.githubusercontent.com/53431568/133353798-009796bf-b824-408e-b6d9-93b695c53388.png">


==> [CycleGAN 참고](https://github.com/junyanz/CycleGAN)


<br>

### StarGAN

StarGAN은 한 영역에서 다른 영억으로의 번역을 위한 image-to-image 번역입니다. 예를 들어 행복한 얼굴이 주어지면 우리는 이 얼굴을 공포에 찬 얼굴로 변환하고 싶다고 가정해봅시다.

아래 이미지를 참고하면, 

<img width="701" alt="무제" src="https://user-images.githubusercontent.com/53431568/133354300-7a063631-3d89-4615-bc57-dc773cde94da.png">


(b)에서 생성자가 input 이미지와 target domain label(화가난 얼굴 이미지라고 가정하자) 을 바탕으로 가짜 이미지를 생성하면, (c)에서 주어진 가짜 이미지와 원본 영역의 이미지(행복한 얼굴이라 가정)를 가지고 생성자를 이용해 이미지를 재구축 한다.

(d)애서는 실제이미지와 가짜 이미지를 식별자에게 제공해 도메인 분류 뿐 아니라 실제인지 아닌지 레이블을 지정한다. 비용함수에는 이미지와 레이블을 식별하는 데 필요한 판별자 비용과 함께 재구성 오류가 포함된다.


<img width="709" alt="무제" src="https://user-images.githubusercontent.com/53431568/133354353-c446910f-b008-45e8-a7ce-bd846643eb97.png">



[StarGAN: Unified Generative Adversarial Networks for Multi-Domain Image-to-Image Translation](https://arxiv.org/pdf/1711.09020.pdf)


<br>

### PixelDTGAN

유명인 사진을 기반으로 상품을 홍보하는 것은 패션 블로거와 온라인 쇼핑몰에서 매우 흔합니다. PixelDTGAN은 이미지로부터 옷 이미지와 스타일을 만들어냅니다.

<img width="684" alt="무제" src="https://user-images.githubusercontent.com/53431568/133354618-fed28ab0-1d93-4f73-9e16-9e8ee6b100b7.png">

<br>

<img width="696" alt="무제" src="https://user-images.githubusercontent.com/53431568/133354648-6cbb038c-388c-44dd-8fd6-89f0beb4df98.png">

이렇게 되면 티비에서 본, 유튜브에서 본 그 옷! 을 바로 구할 수 있게 되는 걸까요~?! ㅎㅎ

[Pixel-Level Domain Transfer](https://arxiv.org/pdf/1603.07442.pdf)




와~ 이렇게 많은 GAN application 들을 훑어보았는데요,,, 참 많네.. 연구주제가 더 산으로 갈거같은 이 느낌... 하나하나 논문을 읽어보고 또 논문에 대해 추후 포스팅을 하도록 하겠습니다..! 😘
  

  
















 A

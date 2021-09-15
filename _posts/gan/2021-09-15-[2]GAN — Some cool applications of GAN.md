---
title:  "[GAN study] Some cool applications of GAN - GAN을 활용한 어플리케이션 소개"
excerpt: ""

categories:
  - gan

tags: [Deeplearning, study, gan]

classes: wide

use_math : true

last_modified_at: 2021-09-15T08:06:00-05:00

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


👉 [Towards the Automatic Anime Characters Creation with Generative Adversarial Networks](https://arxiv.org/pdf/1708.05509.pdf)

<br>

### Pose Guided Person Image Generation

포즈에 대한 추가적인 입력으로 한 이미지를 다른 포즈들로 변환할 수 있는 어플리케이션입니다. 

이 어플리케이션의 작동 방식을 예를 들어 설명하면, 아래 그림의 오른쪽 위 이미지는 ground truth(원래 실제 이미지) 이고 오른쪽 아래의 이미지는 generated image(생성된 이미지)로 포즈가 마뀐 것을 확인해 볼 수 있죠?!

<img width="688" alt="무제 4" src="https://user-images.githubusercontent.com/53431568/133252673-679945ff-57a0-426b-b2c9-8ff0bfb35611.png">


생성된 이미지들을 정리한 결과는 다음과 같습니다. 진짜 사람이 직접 포즈를 취한것 처럼 자연스럽네요~! 😳👍👍

![무제](https://user-images.githubusercontent.com/53431568/133252821-72c720a3-2a4c-4c02-af94-61a58ea70709.png)


구조는 2-stage 이미지 생성자와 식별자로 구성되어있습니다. 생성자는 메타데이터(pose) 이미지와 원본이미지를 이용해 재구축하며 식별자는 원본이미지를 CGAN 구조에 레이블 인풋값의 한 부분으로 사용합니다.

<img width="703" alt="무제" src="https://user-images.githubusercontent.com/53431568/133253646-aba04125-3752-47e4-837e-72a022ec2a0b.png">



👉 [Pose Guided Person Image Generation](https://papers.nips.cc/paper/2017/file/34ed066df378efacc9b924ec161e7639-Paper.pdf)

<br>

### CycleGAN

Cross-domain transfer GAN은 상업적 어플리케이션의 첫 회분 입니다. 이러한 GAN은 이미지를 한 영역(실제 풍경)에서 다른 영역(모네 그림 혹은 고흐 그림)으로 변환합니다.

<img width="688" alt="무제 2" src="https://user-images.githubusercontent.com/53431568/133253855-a41f8939-a509-48b8-badb-16218db37a32.png">


예를들어, CycleGAN은 얼룩말을 말로 바꿀 수 있습니다.

<img width="698" alt="무제" src="https://user-images.githubusercontent.com/53431568/133353544-bfc6c17d-f093-4c0e-87be-52315f09db28.png">


CycleGAN 은 한 영역에서 다른영역 그리고 한 영역에서 반대 방향으로 이미지들을 만들기 위해 두 가지 네트워크인 G와 F를 만듭니다. 그리고 식별자 D는 이미지가 얼마나 잘 생성되었는지 평가하게 됩니다. 예를 들어 G가 실제 이미지에서 반고흐풍의 그림으로 변환하면 Dy는 그 이미지가 실제인지, 생성된 이미지(fake)인지 구분합니다.

> Domain A -> Domain B:

<img width="703" alt="무제" src="https://user-images.githubusercontent.com/53431568/133353711-62a5fca7-e9c3-4173-b255-9b02f0a21ea6.png">


이 과정을 역방향으로 반복. 

> Domain B -> Domain A:

<img width="492" alt="무제" src="https://user-images.githubusercontent.com/53431568/133353798-009796bf-b824-408e-b6d9-93b695c53388.png">


👉 [CycleGAN](https://github.com/junyanz/CycleGAN)


<br>

### StarGAN

StarGAN은 한 영역에서 다른 영억으로의 번역을 위한 image-to-image 번역입니다. 예를 들어 행복한 얼굴이 주어지면 우리는 이 얼굴을 공포에 찬 얼굴로 변환하고 싶다고 가정해봅시다.

아래 이미지를 참고하면, 

<img width="701" alt="무제" src="https://user-images.githubusercontent.com/53431568/133354300-7a063631-3d89-4615-bc57-dc773cde94da.png">


(b)에서 생성자가 input 이미지와 target domain label(화가난 얼굴 이미지라고 가정하자) 을 바탕으로 가짜 이미지를 생성하면, (c)에서 주어진 가짜 이미지와 원본 영역의 이미지(행복한 얼굴이라 가정)를 가지고 생성자를 이용해 이미지를 재구축 한다.

(d)애서는 실제이미지와 가짜 이미지를 식별자에게 제공해 도메인 분류 뿐 아니라 실제인지 아닌지 레이블을 지정한다. 비용함수에는 이미지와 레이블을 식별하는 데 필요한 판별자 비용과 함께 재구성 오류가 포함된다.


<img width="709" alt="무제" src="https://user-images.githubusercontent.com/53431568/133354353-c446910f-b008-45e8-a7ce-bd846643eb97.png">

👉 [StarGAN: Unified Generative Adversarial Networks for Multi-Domain Image-to-Image Translation](https://arxiv.org/pdf/1711.09020.pdf)


<br>

### PixelDTGAN

유명인 사진을 기반으로 상품을 홍보하는 것은 패션 블로거와 온라인 쇼핑몰에서 매우 흔합니다. PixelDTGAN은 이미지로부터 옷 이미지와 스타일을 만들어냅니다.

<img width="684" alt="무제" src="https://user-images.githubusercontent.com/53431568/133354618-fed28ab0-1d93-4f73-9e16-9e8ee6b100b7.png">

<br>

<img width="696" alt="무제" src="https://user-images.githubusercontent.com/53431568/133354648-6cbb038c-388c-44dd-8fd6-89f0beb4df98.png">

이렇게 되면 티비에서 본, 유튜브에서 본 그 옷! 을 바로 구할 수 있게 되는 걸까요~?! ㅎㅎ

👉 [Pixel-Level Domain Transfer](https://arxiv.org/pdf/1603.07442.pdf)


<br>

### Super resolution

이번엔 저화질 이미지에서 고화질 이미지로 만드는 GAN 기술에 대해서 소개하고자 합니다. 이 기술은 상업계에 빠르게 또, 인상적인 결과를 보여주고 있는 분야입니다.
  
<img width="690" alt="무제" src="https://user-images.githubusercontent.com/53431568/133363948-b9ededca-a16f-425a-8bea-dbfe3824a71b.png">

많은 GAN 디자인들과 비슷하게 이 모델은(SRGAN) 많은 컨볼루션 레이어와 배치 정규화, relu, 그리고 skip connection 들로 구성되어있습니다.

<img width="697" alt="무제 2" src="https://user-images.githubusercontent.com/53431568/133364061-7fe51d67-19a5-4ef5-b97c-5164364ef729.png">


👉 [Photo-Realistic Single Image Super-Resolution Using a Generative Adversarial
Network](https://arxiv.org/pdf/1609.04802.pdf)

<br>

### Progressive growing of GANs

Progressive GAN 은 광고 같은 이미지 수준을 보여주는 최조의 GAN 중 하나입니다. 아래는 GAN으로 생성된 1024 × 1024의 연예인 이미지 입니다.

<img width="682" alt="무제 3" src="https://user-images.githubusercontent.com/53431568/133364824-9ffb04ce-95eb-4ff7-8290-83011f84c7df.png">

이 기법은 분할정복(divide-and-conquer) 전략을 이용해 학습을 좀 더 실제적이게 만듭니다. 컨볼루션 레이어들은 학습될 때 2x resolution 을 만들도록 학습됩니다.


<img width="731" alt="무제 4" src="https://user-images.githubusercontent.com/53431568/133364883-48dad47f-9002-42cd-b33c-6ce87407a7ac.png">

9 phases에서는 1024 × 1024 이미지가 생성됩니다.

<img width="692" alt="무제 5" src="https://user-images.githubusercontent.com/53431568/133365052-6f1a9780-2249-493a-b215-5f08caa8d643.png">


👉 [PROGRESSIVE GROWING OF GANS FOR IMPROVED
QUALITY, STABILITY, AND VARIATION](https://arxiv.org/pdf/1710.10196.pdf)

<br>

### StyleGAN2

StyleGAN2 는 고화질의 이미지를 생성해냅니다. 아래 이미지를 보면 피사체는 정확히, 배경은 살짝 흐림으로써 더 고화질스러운 이미지가 완성이 된 것을 볼 수 있습니다.

<img width="678" alt="무제 6" src="https://user-images.githubusercontent.com/53431568/133365206-dff9d255-fd05-4e38-9532-36db16faf196.png">

#### High-resolution image synthesis

고화질 이미지 합성은 단순한 이미지 세그멘테이션(이미지 분할)이 아닙니다! segmantic map(분할된 맵)으로 부터 이미지를 생성해내는 것입니다! 오히려 반대인 격이죠. 

한편, 표본을 수집하는 것은 비용이 많이 들기 때문에, 개발 비용을 낮추기 위해 생성된 데이터로 훈련 데이터셋을 보완하고 있습니다. 이 기술로 동네를 순항하는 것 보다는 자율주행차를 학습시키는 비디오를 생성하는 것이 더 편할 것입니다.(무슨말이지?😳😳)

<img width="721" alt="무제 7" src="https://user-images.githubusercontent.com/53431568/133366050-b4604c5f-ec54-4a99-be02-bac6dc2a8c60.png">


네트워크 설계 : 

<img width="705" alt="무제" src="https://user-images.githubusercontent.com/53431568/133366084-a3bda916-834f-4657-9b78-e4264c9e8f72.png">

👉 [pix2pixHD](https://tcwang0509.github.io/pix2pixHD/)

<br>

### GauGAN

GauGAN 은 입력된 세그먼트 레이아웃으로 사실적인 이미지를 합성합니다.

<img width="603" alt="무제 2" src="https://user-images.githubusercontent.com/53431568/133368409-8bfdf8c4-d212-4071-bfe5-7bafb926a27a.png">

<br>

### Text to image (StackGAN)

텍스트를 이미지로 만드는 것은 영역-전이 GAN 의 초반 어플리케이션 중 하나입니다. 한 문장을 입력으로 넣으면 에 맞는 다수의 이미지들이 생성됩니다.

<img width="685" alt="무제 3" src="https://user-images.githubusercontent.com/53431568/133368574-86be7990-5e47-4b8d-8261-f4569161199f.png">

👉 [StackGAN github](https://github.com/hanzhanggit/StackGAN)

<img width="707" alt="무제 4" src="https://user-images.githubusercontent.com/53431568/133368591-05d980ac-746a-4076-b9e8-609958f9868b.png">

👉 [StackGAN: Text to Photo-realistic Image Synthesis
with Stacked Generative Adversarial Networks](https://arxiv.org/pdf/1612.03242v1.pdf)

### Text to Image Synthesis

텍스트를 이미지로 합성하는 다른 유명한 네트워크 구조: 

<img width="729" alt="무제 5" src="https://user-images.githubusercontent.com/53431568/133368764-2243fb41-bb91-4515-a561-4fe52462f234.png">

<br>

### Face synthesis

다른 포즈의 얼굴들을 합성 : 하나의 입력 이미지로 다른 각도에서 보이는 얼굴들을 만들어 내는 기술입니다. 예를 들어 우리는 이 기술로 이미지를 변환시켜 얼굴인식을 더 손쉽게 할 수 있습니다.

<img width="616" alt="무제 6" src="https://user-images.githubusercontent.com/53431568/133369027-e4f35637-fa9c-43c1-b6de-e704b5e8aa4e.png">

<img width="681" alt="무제" src="https://user-images.githubusercontent.com/53431568/133369117-be8dda02-0af7-448c-a6e8-0d3319108967.png">


👉 [Beyond Face Rotation: Global and Local Perception GAN for Photorealistic and
Identity Preserving Frontal View Synthesis](https://arxiv.org/pdf/1704.04086.pdf)

<br>

### Image inpainting

수 년 동안 이미지를 재건하는 것은 중요한 주제였습니다. GAN은 이미지를 재건하는데 쓰이며 사라진 부분을 생성해 채워넣을 수 있습니다.

<img width="720" alt="무제 2" src="https://user-images.githubusercontent.com/53431568/133369295-c7732ba3-aef8-4037-96e2-830799b82257.png">

👉 [Context Encoders: Feature Learning by Inpainting](https://github.com/pathak22/context-encoder)

<br>

### Learn Joint Distribution

얼굴 특징 확률( P(금발, 여성, 웃음, 안경), P(갈색, 남성, 웃음, 안경없음), 등등...)들의 조합으로 GAN을 생성하는 것은 비용이 매우 큽니다. 딱봐도 확률에 들어갈 조건이 많네요. 차원의 저주는 GAN이 지수 함수의 크기로 커져버리게 합니다. 

> 여기서 잠깐! 차원의 저주란?? 앞서 말한 것 처럼 하나의 값을 구해야하는데에 따른 조건이 너어무 많을 경우 오히려 답을 잘 못 찾게 되는 것을 말합니다. 

대신, 우리는 각각의 데이터 분배를 학습하고 데이터를 조합해 새로운 분포를 만들어낼 수 있습니다. (i.e. 다른 속성 조합들)

CoGAN learns the joing probability $P(x_1, x_2)$ by sampling $x_1 ~ P(x_1)$ and $x_2 ~ P(x_2)$

<img width="655" alt="무제 3" src="https://user-images.githubusercontent.com/53431568/133369811-94b3218d-7e5f-4327-b26d-14a37a2ede21.png">


<img width="665" alt="무제 4" src="https://user-images.githubusercontent.com/53431568/133369951-c5c1bbde-4598-445c-aa73-88b6018ce5a0.png">


👉 [CoGAN : Coupled Generative Adversarial Networks](https://arxiv.org/pdf/1606.07536.pdf)


<br>

### DiscoGAN

DiscoGAN은 매칭 스타일을 제공한다 : 많은 잠재된 어플리켕션. DiscoGAN은 레이블이나 페어링 없이 교차된 영역의 관계를 배웁니다. 예를 들어, 한 영역(핸드백) 으로부터 다른영역(신발)로 스타일을 성공적으로 전이시킬 수 있습니다.

<img width="668" alt="무제 5" src="https://user-images.githubusercontent.com/53431568/133369989-6aad93f4-16eb-48eb-b1c3-1ccecac0715f.png">

DiscoGAN 와 CycleGAN은 네트워크 구조가 매우 비슷합니다. 아래 이미지 참고!

<img width="641" alt="무제 6" src="https://user-images.githubusercontent.com/53431568/133370035-93e735bc-abca-40ee-96c8-8790258c49f6.png">


👉 [DiscoGAN](https://github.com/carpedm20/DiscoGAN-pytorch)

<br>

### Pix2Pix

Pix2Pix 교차 도메인 GAN의 논문에서 자주 인용되는 이미지 에서 이미지로의 변환입니다. 아래 이미지와 같이 위성 이미지를 지도(왼쪽 하단)로 변환하는 것을 볼 수 있습니다.

<img width="689" alt="무제" src="https://user-images.githubusercontent.com/53431568/133370802-aeb049f4-90d6-4df3-ac3f-2260558c1fc2.png">

👉 [Pix2Pix](https://github.com/phillipi/pix2pix)

<br>

### DTN

DTN은 그림으로부터 이모티콘을 생성합니다.

<img width="684" alt="무제 2" src="https://user-images.githubusercontent.com/53431568/133371092-a9f50e3e-3081-418c-960d-ef722025ae8c.png">


<img width="697" alt="무제 3" src="https://user-images.githubusercontent.com/53431568/133371115-589d181a-70f6-42fe-965c-550855f8e75d.png">


👉 [Unsupervised Cross-Domain Image Generation](https://arxiv.org/pdf/1611.02200.pdf)

<br>

### Texture Synthesis

<img width="683" alt="무제 4" src="https://user-images.githubusercontent.com/53431568/133371210-557dd7e8-d2a8-4b80-ac0b-16b451b9677c.png">


👉 [Precomputed Real-Time Texture Synthesis with
Markovian Generative Adversarial Networks](https://arxiv.org/pdf/1604.04382.pdf)


### Image editing (IcGAN)

특정 속성으로 이미지를 재건축하거나 편집한다.

<img width="683" alt="무제 5" src="https://user-images.githubusercontent.com/53431568/133371387-0defd95c-ab3d-4724-bed0-051fb8fba7ab.png">

<img width="699" alt="무제 6" src="https://user-images.githubusercontent.com/53431568/133371415-f279623c-b97b-472c-a938-5cf783b68fcd.png">

👉 [IcGAN](https://github.com/Guim3/IcGAN)

<br>

### Face aging (Age-cGAN)

약간 사진 어플로도 많이 쓰이는 기법들이 GAN에서 보이는 것 같다.

<img width="687" alt="무제 7" src="https://user-images.githubusercontent.com/53431568/133371536-431f6bd2-3104-468c-afa7-e8524c0adcd6.png">

<img width="676" alt="무제 8" src="https://user-images.githubusercontent.com/53431568/133371564-cecaf8af-a3d8-49d1-9367-57f3caa55408.png">

👉 [FACE AGING WITH CONDITIONAL GENERATIVE ADVERSARIAL NETWORKS](https://arxiv.org/pdf/1702.01983.pdf)

<br>

### DeblurGAN

DeblurGAN은 모션 디블러링을 수행합니다. 여기서, 디블러링(deblurring)은 이미지에서 흐릿한 아티팩트를 제거하는 프로세스를 말합니다. 그런데 논문을 살짝 살펴보니 제거하기 보단 선명하게 만들어 낸다라고 보면 될 것 같습니다. 아마 흐릿한 것을 제거하는게 아닐까요..!

<img width="704" alt="무제 9" src="https://user-images.githubusercontent.com/53431568/133371682-1658c838-95d5-4404-bc8f-b843df84cdbb.png">

👉 [DeblurGAN: Blind Motion Deblurring Using Conditional Adversarial Networks](https://arxiv.org/pdf/1711.07064.pdf)

<br>

### Neural Photo Editor

내용기반 이미지 편집기 : ex) 헤어밴드를 늘리는 것

<img width="323" alt="무제" src="https://user-images.githubusercontent.com/53431568/133372197-131cd6b5-e4ae-4060-92bc-df7213bc1b64.png"><img width="313" alt="무제 3" src="https://user-images.githubusercontent.com/53431568/133372231-57ccdb2a-2ffa-43de-a51c-e3f95a7f659a.png">

 👉 [Neural Photo Editor](https://github.com/ajbrock/Neural-Photo-Editor)

<br>

### Refine image

<img width="336" alt="무제 4" src="https://user-images.githubusercontent.com/53431568/133372312-ab1782d9-1c5f-42f7-837f-42c8f77bedea.png">

<br>

### Object detection

이것은 GAN으로 기존 솔루션을 향상시키는 하나의 응용 프로그램입니다. GAN으로 OD 라니.. 응용분야가 무궁무진하다.

<img width="680" alt="무제 6" src="https://user-images.githubusercontent.com/53431568/133372745-b3cfc2af-f9db-4e2a-94f7-f377838e3354.png">


👉 [Perceptual Generative Adversarial Networks for Small Object Detection](https://arxiv.org/pdf/1706.05274v2.pdf)

<br>

### Image blending

이미지들을 블렌딩한다.

<img width="699" alt="무제 7" src="https://user-images.githubusercontent.com/53431568/133372820-4ca06d84-c80d-4998-8db6-50da36b23f92.png">

👉 [GP_GAN](https://github.com/wuhuikai/GP-GAN)

### Video generation

새로운 비디오 시퀀스를 만들어내는 기법이다. 배경이 어떤건지 인식하고 forebround action에 새로운 시간 시퀀스를 만들어낸다. 한마디로 배경에서 피사체를 분리해 피사체 부분이 1-2초 후에 하게 될 모션을 예측해 보여준다.

<iframe width="665" height="510" src="https://www.youtube.com/embed/Pt1W_v-yQhw" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>


<br>

### Generate 3D objects

3D 물체 생성은 GAN으로 3D 객체를 생성할 때 자주 인용되는 논문 중 하나입니다.

<iframe width="665" height="382" src="https://www.youtube.com/embed/HO1LYJb818Q" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>


<img width="631" alt="무제 8" src="https://user-images.githubusercontent.com/53431568/133373276-06a107e2-5aba-4961-ba5b-351002aa78a2.png">

👉 [Learning a Probabilistic Latent Space of Object
Shapes via 3D Generative-Adversarial Modeling](https://proceedings.neurips.cc/paper/2016/file/44f683a84163b3523afe57c2e008bc8c-Paper.pdf)

<br>

### Music generation

GAN은 이미지가 아닌 분야에서도 활용될 수 있습니다. 이번의 음악 작곡 처럼 말이죠😲👍

<img width="683" alt="무제 9" src="https://user-images.githubusercontent.com/53431568/133373432-c5419946-368c-4139-8eca-d3fba7b4868c.png">

<img width="677" alt="무제 10" src="https://user-images.githubusercontent.com/53431568/133373464-ff6ba769-3dfc-49c8-8db7-622fe08b0760.png">

👉 [MIDINET: A CONVOLUTIONAL GENERATIVE ADVERSARIAL
NETWORK FOR SYMBOLIC-DOMAIN MUSIC GENERATION](https://arxiv.org/pdf/1703.10847.pdf)

<br>

### Medical (Anomaly Detection)

GAN은 또 다른 산업에서도 가능한데요! 의약쪽에서 종양을 감지하는 것처럼요!

<img width="712" alt="무제 11" src="https://user-images.githubusercontent.com/53431568/133373711-18f82779-242e-4184-a73f-666489b0c9e6.png">


👉 [Unsupervised Anomaly Detection with
Generative Adversarial Networks to Guide
Marker Discovery](https://arxiv.org/pdf/1703.05921.pdf)


<br>


와~ 기나긴 여정이 끝났습니다!!

이렇게 많은 GAN application 들을 훑어보았는데요,,, 생각보다 GAN의 응용분야가 많아서 놀랐습니다. ㅎㅎ 연구주제가 더 산으로 갈거같은 이 느낌... 하나하나 논문을 읽어보고 또 논문에 대해 추후 포스팅을 하도록 하겠습니다..! 😘
  









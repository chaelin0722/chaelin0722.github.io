---
title:  "[kaggle] pytorch tensor shape => .permute() .unsqueeze(), .squeeze() 알아보기"
excerpt: "U-Shape_transformer"

categories:
  - etc
tags: [study, pytorch, etc]
classes: wide

last_modified_at: 2022-01-14T10:40:00-05:00
---

![image](https://user-images.githubusercontent.com/53431568/149490718-b349e9d8-812d-497c-bd27-43212e4f3e1d.png)


캐글대회에 참여하였다. 주제는..

> "TensorFlow - Help Protect the Great Barrier Reef" , "Detect crown-of-thorns starfish in underwater image data"

아무래도 깊은 바닷속의 불가사리를 Detect 해야하다보니 푸르른 물 색깔의 개선이 필요해 보였다. 따라서 U-Shape transformer 을 사용하여 이미지 전처리를 해준 후 학습을 시키기로 하였다.

![image](https://user-images.githubusercontent.com/53431568/149491126-169a9279-453a-47e8-8e7c-53a21abae4f4.png)

깃헙 코드 여기! => [ushape github](https://github.com/LintaoPeng/U-shape_Transformer_for_Underwater_Image_Enhancement)


여기서의 문제는 이미지를 256x256으로 일괄 resize를 하여서 이미지 색을 개선시키는 것인데 아무래도 resize를 시키면 원본 사진(1280x720)보다 작아서 잡아야 하는 object의 pixel도 작아져 학습 성능이 낮아질 것을 우려했다.

따라서 이미지를 256으로 잘라서 잘린 이미지를 처리한 후, 다시 합치는 방법을 사용하였다!

최종 코드는 아래와 같다

<script src="https://gist.github.com/chaelin0722/9353e72455ddc78e0a5c0b1a01edfb08.js"></script>


옜날 데이콘에서 competition중, [제공된 baseline 코드](https://dacon.io/competitions/official/235746/codeshare/2874?page=2&dtype=recent)를 참고하여서 수정하였는데, 여기서 tensor의 shape에 대해서 짚고 넘어가야 할 일이 생겨서 한번 쓱! 정리하게로 하였다. 


<br>

#### 1. permute() 함수

> 차원의 순서를 재배치 해준다.

예를 들어 crop이라는 텐서가 있다고 가정해보자. 이 텐서는 (batch, height, width, channel)의 순서대로 나열이 되어있다. 이때 이 순서를 바꾸고 싶을 때, permute을 사용할 수 있다.

~~~
crop 의 순서가 (b, h, w, c) => (0,1,2,3)이라고 생각한다면,

crop = crop.permute(0, 3, 1, 2) # tensor 재배치 -> (b, c, h, w) 의 순서로 변경된다.
~~~

<br>

#### 2. unsqueeze() 함수

> 1인 차원을 생성한다.

이번엔 crop 텐서가 (c, h, w) 로 3차원이라고 가정해보자. 4차원으로 만들기 위해 배치 차원을 하나 추가해 보겠다.

~~~
crop이 (3, 256, 256)일 때,

crop = crop.unsqueeze(0) # tensor (1, 3, 256, 256)  => 0 번째 순서에 1인 차원이 하나 추가된다.
~~~


<br>

#### 3. squeeze() 함수

> 차원이 1인 차원을 제거한다.

~~~
crop이 (1, 3, 256, 256)일 때,

crop = crop.squeeze() # tensor (3, 256, 256)  => 1인 차원이 하나 제거됨
~~~


<br>

이미지 resize 없이 돌려서 나온 결과 확인!

전처리 전, raw 

![image](https://user-images.githubusercontent.com/53431568/149503027-d9f013ec-070b-420e-b737-87e994f3d470.png)

u-shape 으로 전처리 후!

푸른 물의 색이 걷어져서 좀 더 대비가 선명한 이미지가 되었다

![image](https://user-images.githubusercontent.com/53431568/149503115-00cfcb75-3942-4de2-bba2-f9e613c04020.png)

<br>

ㅎㅎ 하나 만드는데 1.6 초가 걸린다.. 세월아 네월아~😅😅


모든 캐글러들 화이팅~

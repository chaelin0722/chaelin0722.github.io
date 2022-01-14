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


여기서의 문제는 이미지를 256x256으로 일괄 resize를 하여서 이미지 색을 개선시키는 것인데 아무래도 resize를 시키면 원본 사진보다 작아서 잡아야 하는 object의 pixel도 작아져 학습 성능이 낮아질 것을 우려했다.

따라서 이미지를 256으로 잘라서 잘린 이미지를 처리한 후, 다시 합치는 방법을 사용하였다!

최종 코드는 아래와 같다

<script src="https://gist.github.com/chaelin0722/9353e72455ddc78e0a5c0b1a01edfb08.js"></script>







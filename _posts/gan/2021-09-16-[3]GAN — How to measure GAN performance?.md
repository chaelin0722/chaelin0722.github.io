---
title:  "[GAN study] How to measure GAN performance?- GAN을 측정하는 방법"
excerpt: ""

categories:
  - gan

tags: [Deeplearning, study, gan]

classes: wide

use_math : true

last_modified_at: 2021-09-16T08:06:00-05:00

---


Gan 학습 로드맵 세 번째 시간입니다!

오늘은 GAN의 퍼포먼스를 어떻게 측정하는지 알아보는 시간을 가지려고 합니다.

이 글은 [Jonathan Hui의 포스팅](https://jonathan-hui.medium.com/gan-how-to-measure-gan-performance-64b988c47732)을 참고해 정리한 내용입니다!



In GANs, the objective function for the generator and the discriminator usually measures how well they are doing relative to the opponent. For example, we measure how well the generator is fooling the discriminator. It is not a good metric in measuring the image quality or its diversity. As part of the GAN series, we look into the Inception Score and Fréchet Inception Distance on how to compare results from different GAN models.


### 

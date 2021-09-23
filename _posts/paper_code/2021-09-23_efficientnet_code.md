---
title:  "[code] EfficientNet with ImageNet"
excerpt: "code"

categories:
  - code
tags: [Deeplearning, CNN, code]
 
classes: wide

last_modified_at: 2021-09-23T07:06:00-05:00

---

### Rethinking Model Scaling for Convolutional Neural Networks 구현

<br>

이 논문의 efficientNet은 ImageNet으로 학습시켜 좋은 성능을 내는데, 이 모델을 가지고 Cifar-100으로 전이시켜 학습시켜도 SOTA 성능을 낼 정도로 좋다고 한다.

따라서, ImageNet으로 학습시키도록 하였고, 

> Epoch : 100
> 
> BATCH SIZE: 128
> 
> alpha(depth) = 1.2
> 
> beta(width) = 1.1
> 
> gamma(resolution) = 1.15


앞서 해석한 논문을 참고하면 코드를 이해하기 편할 것이다! => 참고! [Rethinking Model Scaling for Convolutional Neural Networks](https://chaelin0722.github.io/paperreview/efficientnet/)

아래 EfficientNet-B0를 구현하였고, 논문에서 제시한 대로 각 $\alpha$ $\beta$ $\gamma$ 값들은 각 1.2, 1.1, 1.15, $\phi$는 1로 고정하였다.

![image](https://user-images.githubusercontent.com/53431568/134504188-d5d558d8-cab8-45d0-bba6-a536389d861c.png)

![image](https://user-images.githubusercontent.com/53431568/134497453-cf0b356a-59bb-4a69-bdff-f3e0f800670b.png)


<br>
<script src="https://gist.github.com/chaelin0722/f144e09d5bfef239d818282967383373.js"></script>
<br>


### [학습결과]

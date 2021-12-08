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

GAN 논문과 MINMAX 함수에 대한 유도는 => [[논문정리📃] Generative Adversarial Nets](https://chaelin0722.github.io/paperreview/generative_adversarial_nets/) 여기에서 직접 손으로 풀어본 풀이가 있으니 
참고하면 도움이 될 것 같습니다.

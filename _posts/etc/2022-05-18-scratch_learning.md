---
title:  "[DL] What is scratch learning? "
excerpt: "Scratch learning, learn from scratch의 뜻"

categories:
  - concept

tags: [DL, ML, concept, deeplearning, scratchlearning, scratch]
classes: wide

last_modified_at: 2022-05-18T08:06:00-05:00
---
 
## Scratch learning 이란?

그동안 논문에서 scratch learning, learn from scratch 라는 말을 자주 썼는데,

과연 딥러닝에서는 무슨 뜻으로 사용하는 것인지 잘 와닿지 않았는데, 드디어 그 의미를 알게되었다.

바로

### "모델을 완전 처음부터 돌리는것이다. pre-trained 없이 모든 하이퍼파라미터 값들은 랜덤으로 설정하여서 학습하는 것!"

<br>
<br>

## Training from Scratch를 하는 이유

1. 이 학습 결과를 lower boundary (하향선)으로 지정하여서 training from scratch 하였을 때의 성능보다는 좋게 나온다는 것을 보여준다고 함

2. 다른 이유로는 pretrain의 도움 없이 backbone 이후 뒷 단의 layer 만으로 성능이 어느정도 나오는지를 볼 수 있다고도 해석한다고 한다.

<br>

--2022-09-20 에 수정됨

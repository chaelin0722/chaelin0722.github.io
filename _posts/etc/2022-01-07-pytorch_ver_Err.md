---
title:  "[pytorch 에러해결] RuntimeError: one of the variables needed for gradient computation has been modified by an inplace operation: [torch.cuda.FloatTensor [3, 4]], which is output 0 of ReluBackward0, is at version 1; expected version 0 instead. Hint: the backtrace further above shows the operation that failed to compute its gradient. The variable in question was changed in there or anywhere later. Good luck!"
excerpt: "version error"

categories:
  - etc
  
tags: [pytorch, study, pytorch, etc]
classes: wide

last_modified_at: 2022-01-07T10:40:00-05:00
---


github에서 가져온 EfficientDet의 pytorch 버전 코드를 돌리다가 발생한 문제!


`RuntimeError: one of the variables needed for gradient computation has been modified by an inplace operation: [torch.cuda.FloatTensor [3, 4]], which is output 0 of ReluBackward0, is at version 1; expected version 0 instead. Hint: the backtrace further above shows the operation that failed to compute its gradient. The variable in question was changed in there or anywhere later. Good luck!`

에러 참 길다 ㅎㅎ 특히 마지막의 Good Luck! 너무 화가난다 🤪🤪


<br>

구글링한 결과, 두 가지 해결방법을 찾을 수 있었다.

**1. torch.autograd.set_detect_anomaly(True) 추가해주기

**2. torchvision version 다르게 설치하기


먼저 1번 방법대로 코드를 추가해주었다. =>  [참고한 블로그](https://daeheepark.tistory.com/24)

안된다! 포기!



다음은 2번의 방법을 위해서 아래와 같은 버전으로 깔아 주어야 한다고 하는데. 나는 해당 버전이 존재하지 않아 깔지 못한다고 한다..

~~~
pip install torch==1.4.0 torchvision==0.5.0
~~~

이런 워닝 문구~

![image](https://user-images.githubusercontent.com/53431568/148506167-afc3f981-1c54-4e46-92d3-651b551c76c3.png)

또 열심히 구글링해서 깔 수 있었던 방법.. "=" 이 3개나 필요하다

~~~
pip install torch===1.4.0 torchvision===0.5.0
~~~

이래도 안되면

~~~
conda install pytorch==1.4.0
pip install tochvision===0.5.0
~~~

해보기!

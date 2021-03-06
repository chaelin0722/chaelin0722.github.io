---
title:  "[code] GoogLeNet"
excerpt: "code"

categories:
  - code
tags: [Deeplearning, CNN, code]
 
classes: wide

last_modified_at: 2021-06-23T08:06:00-05:00

---

### Going Deeper with convolutions(GoogLeNet) 구현

![googlenet](https://user-images.githubusercontent.com/53431568/123113561-9cb26380-d479-11eb-9d0e-a656fce80f9f.png)

위의 layer 구조를 보고 코드로 구현하였다.

앞서 해석한 논문을 보면 디테일한 설명을 볼 수 있다. 
[Going Deeper with convolutions](https://chaelin0722.github.io/cnn/paperreview/googlenet/)

<br>
<script src="https://gist.github.com/chaelin0722/abdf685e01848645ab7d823614a0e56f.js"></script>
<br>


### [학습결과]

-  epoch을 100으로 학습한 결과, accuracy, loss, top_5_accuracy 결과이다. 0.7 정도로 올랐다. epoch을 더 크게 하여 학습시키니 조금씩 더 올라가지만 여기까지 학습해보기로 한다.

<p float="left">
  <img src="https://user-images.githubusercontent.com/53431568/127103681-a25d6b8c-55f8-4d2a-aa2e-951144e50317.PNG" width="330" />
  <img src="https://user-images.githubusercontent.com/53431568/127103677-9230927f-3793-43ab-bece-ec733053d4f7.PNG" width="330" /> 
  <img src="https://user-images.githubusercontent.com/53431568/127103679-e89382fe-7df0-4dd5-8499-0e643d4c496e.PNG" width="330" />
</p>

(epoch이 10부터 시작하는 이유는 10 이하의 log기록을 날려버렸기 때문이다.. ㅎㅎ 조심하자)

<br><br>

test 이미지로 테스트 해본 결과 알맞는 정답을 찾는 것을 확인 ✌️✌️

![image](https://user-images.githubusercontent.com/53431568/127957349-a43473b7-75fe-40a6-9309-261ea96c7869.png)


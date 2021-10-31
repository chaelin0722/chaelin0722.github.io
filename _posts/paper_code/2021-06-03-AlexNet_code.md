---
title:  "[code] AlexNet"
excerpt: "code"

categories:
  - code
tags: [Deeplearning, CNN, code]
 
classes: wide

last_modified_at: 2021-06-12T08:06:00-05:00

---

###  ImageNet Classification with Deep Convolutional Neural Networks 구현

![alexnet](https://user-images.githubusercontent.com/53431568/119877176-a7a1d300-bf63-11eb-8839-061d7750c517.png)

위의 layer 구조를 보고 코드로 구현하였다.

앞서 해석한 논문을 보면 디테일한 설명을 볼 수 있다. 
[ImageNet Classification with Deep Convolutional Neural Networks]https://chaelin0722.github.io/paperreview/AlexNet/)

<br>
<script src="https://gist.github.com/chaelin0722/366489f2ac4c3fd828392f5fd975aa3d.js"></script>
<br>

- local response normalization은 요즘 사용하지 않고 대부분 batch normalization을 사용한다고 함. 

- compile 부분을 보면 optimizer을 SGD(stochastic gradient descent)를 사용하였다. 

- 원래는 cifar-10 데이터 셋으로 10개의 클래스를 분류하고자 하였지만 GPU 메모리 부족으로 2개의 클래스를 분류해보았다.

<br>

### [학습결과]

accuracy는 56.67%.

cat, dog 이미지라서 accurate가 박살난것일까..

#### 참고

[1] [http://datahacker.rs/tf-alexnet/](http://datahacker.rs/tf-alexnet/)

[2] [https://towardsdatascience.com/implementing-alexnet-cnn-architecture-using-tensorflow-2-0-and-keras-2113e090ad98](https://towardsdatascience.com/implementing-alexnet-cnn-architecture-using-tensorflow-2-0-and-keras-2113e090ad98)

[3] [https://medium.com/swlh/alexnet-with-tensorflow-46f366559ce8](https://medium.com/swlh/alexnet-with-tensorflow-46f366559ce8)


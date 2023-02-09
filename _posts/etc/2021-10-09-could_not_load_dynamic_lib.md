---
title:  "[GPU] Could not load dynamic library.. solved 해결"
excerpt: "cuda not found error"

categories:
  - etc

tags: [ etc, GPU, CUDA]
use_math: true
classes: wide

last_modified_at: 2023-02-09T08:06:00-05:00
---

오늘도 에러 기록!🤨

GPU를 사용하고, 머신러닝을 쓰는 사람들이라면 CUDA 를 사용할테니 이런 경험쯤은 한번씩은 해 보았을 것이다.

바로 **could not load dynamic library 'cuda..** ==> 특정 라이브러리를 열 수 없다는 에러! 이게 뜨면 GPU를 사용하지 않고 CPU 만으로 학습이 동작하는 것을 볼 수 있다.

(아래 이미지와 같이 메모리는 잡았지만 gpu utility 가 0 이거나 매우 낮다)

![image](https://user-images.githubusercontent.com/53431568/217710415-1e089afd-47ec-4d6c-b2d3-c678e9a963c1.png)


여기서는 두가지 에러에 대해 정리해 볼텐데 cp 방식과 link 방식으로 해결하는 방법을 두 다뤄 볼 예정이다. 


### (1) 이름은 같지만 버전이 다른 라이브러리가 있는 경우 "CP" 로 해결

`libcudart.so.11.0 ` 가 없는 에러가 떴다 다음과 같다. 

![error_](https://user-images.githubusercontent.com/53431568/136651228-65f3fda9-c6a3-439e-8896-43fdd8859f9c.PNG)

찾을 수 없다고 하는 경로인 LD_LIBRARY_PATH 에 직접 가서 확인을 해보았다.

![libcudart_list](https://user-images.githubusercontent.com/53431568/136651277-a65462fb-dd36-48db-a3ba-3b64a0a2933c.PNG)

libcudart.so.10.0 은 있는 것을 확인하였다! 그래서 단순히 이 파일을 복사해서 이름을 libcudart.so.11.0 로 바꿔주기로 했다 ㅎㅎ 간단하게 해결해보자!

아래 명령어로 복사해서 새로운 파일을 만들었다👍

~~~
sudo cp libcudart.so.10.0 libcudart.so.11.0
~~~

따란! 해결! 

![success](https://user-images.githubusercontent.com/53431568/136651307-763d427b-1f58-4a22-a372-6f420d403eae.PNG)

<br>


### (2) 아예 해당 라이브러리가 없는 경우 다운받은 후 LINK 로 해결!

이런 경우에는 CUDA 다른 버전을 깔고, 다른 버전의 라이브러리를 CP 하던가 LINK 로 연결하여서 해결을 하곤 한다. 

문제 상황

![image](https://user-images.githubusercontent.com/53431568/217709994-a982961b-1f3b-4ecf-9961-5f8d801891bc.png)

나는 tensorflow rt 에서 에러가 났으므로 일단 저 패키지를 깔아 주었다.
구글링 해보니 python 3.9 버전 이상은 8.0 이상으로 깔아야 한대서 tensorrt-8.5.3.1 버전으로 깔아 주었다. 그런데 버전을 구체적으로 적어 pip install을 하니 되지 않아서 아래 명령어로 install 완료 해줌 

~~~
 python3 -m pip install --upgrade tensorrt
~~~


![image](https://user-images.githubusercontent.com/53431568/217711022-dbf4720a-3b5e-42c4-bbec-44c5b6577415.png)

설치 완료하였으면 아래 명령어로 tensorrt 가 어디에 깔려있는지 경로를 찾는다!

~~~
python3 -c "import tensorrt; print(tensorrt.__path__)"
~~~

해당 경로에 가니 libnvinfer_plugin.so.8 와 libnvinfer.so.8 가 있음을 확인!!🤨

![image](https://user-images.githubusercontent.com/53431568/217711110-dd1fb743-26a2-40e3-b8b7-a21f0fc788d6.png)

근데,, size 가 어마어마해서 cp 하긴 좀 그렇고.. 같은 파일을 만들어 줘서 link 로 연결해주기로 결정!

먼저, 빈 파일을 만들고..

![image](https://user-images.githubusercontent.com/53431568/217711898-2dbf8672-af82-454d-83ef-7ad3eaeaf4f6.png)

진짜 파일과 link 



/home/clkim/anaconda3/envs/fsl/lib/python3.9/site-packages/tensorrt/


(2023-02-09 수정됨)


<br>


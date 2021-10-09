---
title:  "[GPU] Could not load dynamic library.. solved 해결"
excerpt: "cuda not found error"

categories:
  - etc

tags: [ etc, GPU, CUDA]
use_math: true
classes: wide

last_modified_at: 2021-10-09T08:06:00-05:00
---

오늘도 에러 기록!

오늘 생긴 에러는 **could not load dynamic library 'cuda..**  블라블라.. 특정 라이브러리를 열 수 없다는 에러! 이게 뜨면 GPU를 사용할 수 없게된다..


나는 libcudart.so.11.0 이 없다고 한다. 

![error_](https://user-images.githubusercontent.com/53431568/136651228-65f3fda9-c6a3-439e-8896-43fdd8859f9c.PNG)

그래서 찾을 수 없다고 하는 경로인 LD_LIBRARY_PATH 에 직접 가서 확인을 해보았다.

![libcudart_list](https://user-images.githubusercontent.com/53431568/136651277-a65462fb-dd36-48db-a3ba-3b64a0a2933c.PNG)

libcudart.so.10.0 은 있는 것을 확인하였다! 그래서 단순히 이 파일을 복사해서 이름을 libcudart.so.11.0 로 바꿔주기로 했다 ㅎㅎ 간단하게 해결해보자!


아래 명령어로 복사해서 새로운 파일을 만들었다👍

~~~
sudo cp libcudart.so.10.0 libcudart.so.11.0
~~~

따란! 해결! 

![success](https://user-images.githubusercontent.com/53431568/136651307-763d427b-1f58-4a22-a372-6f420d403eae.PNG)

<br>

만약 복사해줄 파일이 아예 없다면..! 다음 블로그를 참고해서 해결가능하다. 아예 cuda-10.1 과 같은 cuda lib가 담긴 폴더를 통째로 복사해서 link를 연결하는 방법이 있다.

[http://ejklike.github.io/2019/08/19/insatall-tensorflow-2.0.0-beta1-in-ubuntu-with-cuda-10-1.html](http://ejklike.github.io/2019/08/19/insatall-tensorflow-2.0.0-beta1-in-ubuntu-with-cuda-10-1.html)




<br>


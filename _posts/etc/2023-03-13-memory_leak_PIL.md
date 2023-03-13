---
title:  "[PIL] CPU bottleneck, memory leak 현상"
excerpt: "pycharm"

categories:
  - etc
tags: [PIL, pillow, etc]
classes: wide

last_modified_at: 2023-03-13T10:40:00-05:00
---

Image 를 한장 한장 읽어와서 처리해주어야 하는 코드를 돌리는데, 자꾸 메모리가 누수되는 문제에 직면하였다. htop 해보면 해당 Mem 부분이 꽉 찬 채로 프로세스가 멈춰버리는 문제이다. 


구글링을 해보니 pillow(PIL) 라이브러리를 사용할 때, image 를 .open() 이라는 함수로 읽었는데, 이렇게 할 경우 close() 로 닫아주어야 메모리 누수가 없다고 한다. 

또 다른 방법으로는 with oepn 형태로 읽어들이면 된다는 것이었다.


기존 코드는, 단순히 open 하는 형태

~~~
image = Image.open(image)
~~~

<br>

먼저 with open 형식으로 바꾸어 주었다. 

![image](https://user-images.githubusercontent.com/53431568/224608800-43b31d7e-d37c-4947-9030-36ebca6aa29a.png)


뭔가 **이미지 처리시간은 빨라진 것 같다** !! 신기한 발견 ㅎㅎ

그런데 메모리는 차곡차곡 올라가는게.. 위험하다.. (쌓여가는 memory..)

![image](https://user-images.githubusercontent.com/53431568/224608404-45f08bca-1190-47ac-a6bd-ec8395018444.png)

결국..! 멈췄다

![image](https://user-images.githubusercontent.com/53431568/224608619-3a8a3ab4-93fb-4817-a131-1aaa9d25981d.png)


<br>

with open 형식으로는 메모리가 여전히 쌓여가는 것 같아서 다시 close() 함수를 써서 테스트 해보았다.


간단하다, 읽어들인 이미지 다시 close 해줌

~~~
img.close()
~~~

메모리가 안 쌓이는 것 같긴한데.. 일단 지켜보겠다.


참고한 [깃 이슈](https://github.com/python-pillow/Pillow/issues/2019) 









---
title:  'VirtualBox 게스트 확장 설치하기'

categories:
  - study
  - etc
tags: [study, etc, virtualbox, kalilinux, hacking]

last_modified_at: 2021-07-08T08:06:00-05:00
classes: wide
---

웹 해킹을 공부하면서 virtualbox를 사용하고 있는데 화면이 고정되어서 불편함을 느꼈다. 두 화면을 띄워놓고 실습을 해야하는데 안보여!! 🥵🥵

하지만 **VirtualBox 게스트 확장**을 설치하면 화면 크기를 조정할 수 있다고 한다!

이제 설치하러 gogo~!

### (1) 리눅스를 실행하고 게스트 확장 CD이미지 삽입 클릭!

![스크린샷(16)](https://user-images.githubusercontent.com/53431568/124884898-b02f0400-e00d-11eb-9c60-a0ecce438b4b.png)

하면 다음과 같은 오류가 뜬다

![image](https://user-images.githubusercontent.com/53431568/124885147-ef5d5500-e00d-11eb-9196-fff32893658e.png)

당황하지 말고~ 자세한 정보(D)를 확인해 보자.. VBoxGuestAddition.iso를 마은트할 수 없다고 뜬다. 

![image](https://user-images.githubusercontent.com/53431568/124885413-321f2d00-e00e-11eb-90b6-62bafd6ab770.png)

재설정을 위해 가상  머신을 일단 종료하고 설정-> 저장소로 이동, 아래 그립과 같이 `가상드라이브에서 디스크 꺼내기` 클릭!

![스크린샷(18)_LI](https://user-images.githubusercontent.com/53431568/124886295-0f414880-e00f-11eb-8278-066ef13adc04.jpg)

다음과 같이 `비어있음`이라 뜰것이다 확인을 누르자

![스크린샷(20)_LI](https://user-images.githubusercontent.com/53431568/124886660-6b0bd180-e00f-11eb-9c24-58feebec88cd.jpg)


다시 가상머신을 켜고 다시 한번 **게스트 확장 CD 이미지 삽입** 클릭! 이번엔 클릭이 되는것을 확인할 수 있다 ㅎㅎ 엇 근데.. 에러..

되는 사람은 이 이후에 비번을 쳐서 로그인하고 화면을 늘리면 된단다.. 나는 안됨.. 구글링 많이 해봤지만 왜 그런지는 모르겠다.

그래서 다른 방법으로 하니 되었다.

#### 디스플레이의 그래픽 컨트롤러를 `VBoxVGA` 로 바꿔준다.

![스크린샷(22)](https://user-images.githubusercontent.com/53431568/124893518-c345d200-e015-11eb-89b5-3d1d2248cb32.png)

이제 옆으로 늘려도 그냥 큰 화면으로 늘려도 된다~! 편하게 가상머신 이용합시당 ㅎㅎ

![캡처](https://user-images.githubusercontent.com/53431568/124893132-6fd38400-e015-11eb-8313-929665e7f2de.PNG)

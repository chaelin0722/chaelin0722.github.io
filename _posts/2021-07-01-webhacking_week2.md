---
title:  '2주차 웹 해킹- 웹 해킹 기술 탐색 1'
excerpt: "week2"

categories:
  - study
  - etc
tags: [study, etc, virtualbox, kalilinux, hacking]

last_modified_at: 2021-07-01T08:06:00-05:00
classes: wide
---

### 8주간의 해킹 스터디 두번째 시간


실습 전에 DVWA를 살펴보자

### DVWA

실습에 앞서 localhost/dvwa 에 접속이 안되는 문제를 겪었다.

두 가지 방법으로 해결했는데.. 먼저, 

#### (1) `/opt/lampp/lampp restart` 를 실행 한 후 하면 접속이 된다. 

#### (2) 이래도 안되면 firefox의 맨 오른쪽 위 메뉴 -> preferences 클릭!

![pre](https://user-images.githubusercontent.com/53431568/124879620-6132a000-e008-11eb-8475-14dbf2ac182c.PNG)

맨 아래로 쭉 내려가다보면 Network Proxy가 보일것이다. 여기서 settings 클릭!

![image](https://user-images.githubusercontent.com/53431568/124879730-7dced800-e008-11eb-8841-550699f099a1.png)

이제 아래와 같이 바꿔주고 ok 클릭하면 접속이 될것이다

![바꿔야할것](https://user-images.githubusercontent.com/53431568/124879635-6394fa00-e008-11eb-96a1-26e05f689f58.PNG)

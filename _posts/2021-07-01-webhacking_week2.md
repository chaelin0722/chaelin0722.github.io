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

이번 시간에는 본격적인 웹 해킹 기술에 대해서 공부할 것이다! 크게 브루트 포스와 커맨드 인젝션 공격을 알아보고 실습을 해볼것입니다 😉

먼저 저번주차에 설치한 DVWA를 살펴보자

### DVWA

실습에 앞서 localhost/dvwa 에 접속이 안되는 문제를 겪었다.🥵

해결방법은 간단하다!

#### `/opt/lampp/lampp restart` 를 실행 한 후 다시 접속해보면 된다!


<br><br>

이제 웹해킹 기술에 대해서 알아봅시다!

### 브루트 포스 공격이란?

사용자 패스워드를 알아내기 위한 공격이고 아이디와 패스워드가 일치하는지 알아보기 위해 패스워드를 계속 대입하는 방법이다.

이 대입 방법의 종류에는 

> 1. 알파벳 순서
> 2. 딕셔너리 공격 이 있다.

![image](https://user-images.githubusercontent.com/53431568/124881746-abb51c00-e00a-11eb-81bd-e00557a3653b.png)

✏️이제 **브루트포스 공격을 실습을 통해 알아보자**

DVWA에 로그인해 들어간 후 왼쪽 메누에서 Brute Force로 들어간다. 아래와 같이 맞지 않는 아이디/비번을 입력하면 맞지 않는다고 경고 문자가 나온다. 

![image](https://user-images.githubusercontent.com/53431568/124882053-f8005c00-e00a-11eb-9d39-be1ab0ed9f69.png)

이런식으로 일일히 대입해 알아보는 방법을 dvwa를 통해 실습해보았다.

✏️이번에는 **자동**으로 브루트 포스 공격을 하는 법을 알아보자

우선 burp suite 의 proxy에서 `intercept is on` -> `intercept is off` 로 변경해주자

![image](https://user-images.githubusercontent.com/53431568/124899006-a8c22780-e01a-11eb-894f-b0ec5bc8a82d.png)

<br>

password에 aaaa를 입력하고 proxy-> HTTP history에서 가장 최근의 요청을 더블클릭해서 내용을 확인해보면

아래와 같이 맨 마지막줄을 보면 username==admin , password=aaaa 임을 확인할 수 있다.

![image](https://user-images.githubusercontent.com/53431568/124901090-a8c32700-e01c-11eb-9458-f5a4530d03c6.png)

<br>

이제, send to intruder로 intruder로 요청을 보내본다

![스크린샷(25)](https://user-images.githubusercontent.com/53431568/124902510-f4c29b80-e01d-11eb-9681-1b4cb2f9c086.png)

<br>

intruder -> position 메뉴로 들어가면 어떤 영역이 하이라이트 된 것을 확인할 수 있으며,

그림에 표시한것과 같이 쿼리스트링 부분과 쿠키 부분이 사용자가 입력한 값이 변수의 값으로 처리되어있는 부분이다.

![ㅁㅁㅁ](https://user-images.githubusercontent.com/53431568/124904827-5edc4000-e020-11eb-952f-a8068fc3b9e1.jpg)

clear$ 버튼으로 하이라이트를 제거하고 지금 당장 테스트 해볼 password 영역만 선택하여 Add$ 버튼을 누른다.

이런식으로 $$로 선택된 영역을 다양한 문자열로 치환해 테스트 할 수 있게 된다.

![image](https://user-images.githubusercontent.com/53431568/124904612-29cfed80-e020-11eb-964f-6eeb9e1025c1.png)

<br>

payloads 메뉴로 들어가서 payload type을 Brute forcer로 바꿔준다. options를 살펴보면 대입해 볼 수 있는 character가 알파벳 a~z, 숫자는 0~9까지 사용할 수 있고, 길이는 4자리로 지정되어있어 4자리 글자로 만들 수 있는 password의 조합은 총 1,679,616 개이다.

대입할 character이 늘어날 수록, max length 길이를 크게 총 조합의 수가 많아진다.

![Inkedpayload_LI](https://user-images.githubusercontent.com/53431568/124903417-ea54d180-e01e-11eb-9e8f-8864d978159b.jpg)

#### 이제 공격을 실행해보자! 우측 상단의 `Start attack` 버튼을 누르자

다음과 같이 패스워드가 일치할때까지 4자리의 조합을 시도해보는 것을 볼 수 있다.

![image](https://user-images.githubusercontent.com/53431568/124904332-d9f12680-e01f-11eb-87e1-9ef0e6bd9b06.png)

하지만 password의 조합이 복잡해질수록 시간이 오래걸린다는 단점이 있다.

이를 위한 것이 **딕셔너리 공격**이다


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

2가지 방법으로 해결했는데..! 본인에게 맞는 해결법을 실행하면 될 것 같다.

#### (1) `/opt/lampp/lampp restart` 를 실행 한 후 다시 접속해보면 된다!
#### (2) firefox의 preference에서 proxy 설정 고치기!

순서는 아래 따라하면 된다 ㅎㅎ

-1- preference 클릭

![pre](https://user-images.githubusercontent.com/53431568/124909815-dcef1580-e025-11eb-99dd-47ad5fab3d4b.PNG)

-2- preference클릭 후 나오는 페이지의 맨 아래쪽으로 이동하면 Network Proxy가 보인다! Settings... 버튼을 누르자

![network](https://user-images.githubusercontent.com/53431568/124909816-dd87ac00-e025-11eb-88ae-edc058101e7a.PNG)

-3- 바꾸어 주어야 할 내용이다 아래와 같이 적용해주자!

![바꿔야할것](https://user-images.githubusercontent.com/53431568/124909851-e7111400-e025-11eb-97f9-ff6a8ead9425.PNG)

#### ⭐️⭐️ 단, (2) 번 해결방법의 경우 해킹 실습이 되지 않으니 (2)번으로 설정해서 dvwa에 접속한 후 다시 원 상태로 바꿔주는 것이 좋다.

<br><br>

이제 웹해킹 기술에 대해서 알아봅시다!

### 브루트 포스 공격이란?

사용자 패스워드를 알아내기 위한 공격이고 아이디와 패스워드가 일치하는지 알아보기 위해 패스워드를 계속 대입하는 방법이다.

이 대입 방법의 종류에는 

> 1. 알파벳 순서
> 2. 딕셔너리 공격

![image](https://user-images.githubusercontent.com/53431568/124881746-abb51c00-e00a-11eb-81bd-e00557a3653b.png)

✏️이제 **브루트포스 공격을 실습을 통해 알아보자**

DVWA에 로그인해 들어간 후 왼쪽 메누에서 Brute Force로 들어간다. 아래와 같이 맞지 않는 아이디/비번을 입력하면 맞지 않는다고 경고 문자가 나온다. 

![image](https://user-images.githubusercontent.com/53431568/124882053-f8005c00-e00a-11eb-9d39-be1ab0ed9f69.png)

이런식으로 일일히 대입해 알아보는 방법을 dvwa를 통해 실습해보았다.

### 자동 브루트 포스 공격

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

<br>

### 딕셔너리 공격

공격에 앞서 password 파일 하나를 살펴보자.

`gedit /usr/share/john/passowrd.lst` 파일을 열면 여러 password 조합들이 딕셔너리 형태로 작성된 파일을 볼 수 있다.

![image](https://user-images.githubusercontent.com/53431568/124906568-29385680-e022-11eb-9afc-27ff6bd6b111.png)

자세히 보면 어떤 password가 많이 사용되는지 알 수 있다.

<br>

이제 burpsuite로 돌아가서 payloads를 다시보자!

load 버튼을 눌러 아까 보았던 password.lst 경로를 따라 파일을 업로드 한다.

![image](https://user-images.githubusercontent.com/53431568/124907328-f93d8300-e022-11eb-8dc2-685ef85b5c20.png)

remove 버튼을 이용해 위의 #!comment 부분을 싹 지우고 password의 조합만 나열하도록 하고 start attack!

![image](https://user-images.githubusercontent.com/53431568/124907479-2db13f00-e023-11eb-9c95-0989221c23e2.png)


###  브루트 포스 공격 대응

---
title:  '[강의] 4주차 웹 해킹- 웹 해킹 기술 탐색 3'
excerpt: "week4"

categories:
  - study
  - etc
  - hacking
tags: [study, etc, virtualbox, kalilinux, hacking]

last_modified_at: 2021-07-17T08:06:00-05:00
classes: wide
---

### 8주간의 해킹 스터디 네번째 시간

이번 시간에는 악성 파일을 업로드하고 시스템 명령을 공격하는 기술인 **파일 업로드 공격**에 대해 학습하는 시간을 가져보도록 하겠습니다~! 😉

강의를 듣고 싶으면 여기로 gogo~! => [goormedu](https://edu.goorm.io/lecture/4953/화이트해커가-되기-위한-8가지-웹-해킹-기술)


### 파일업로드 공격

파일이 업로드되는 페이지(게시판, SNS)에 악성파일(웹셀)을 업로드

주로 **`웹셀`**이라는 악성파일을 업로드하는데, 웹셀이란, 웹을 통해 시스템 명령어를 실행할 수 있는 웹페이지이다.

다음그림을 보면 이미지를 업로드하는 페이지가 있고 헤커는 이미지 대신 웹셀을 업로드한다. 만약 사용자가 이미지를 제대로확인하지 않는다면 웹쉘을 저장하게 된다.

![image](https://user-images.githubusercontent.com/53431568/125901242-6899830b-1df5-49da-8742-dc9677636a88.png)

<br>
  
다음으로 해커가 웹셀에 접근하게 되면 웹셀이 실행되고 해커가 원하는대로 시스템 명령어가 실행된다.

![image](https://user-images.githubusercontent.com/53431568/125900890-60788a52-acb8-4644-a007-84d953fb5bac.png)


이제 재미있는 실습의 시간이다 ㅎㅎ

### 파일업로드 공격 실습

먼저 웹셀 파일을 업로드해보기 위해 강사님의 깃헙에 올라와있는 webshell.php를 `/root`  디렉토리에 wget으로 다운받았다

gedit으로 웹셀코드를 살펴보면.. 사용자가 입력한걸 시스템 명령어로 실행되게 하는 코드가 짜져있다.

![image](https://user-images.githubusercontent.com/53431568/125961030-c99dbfa7-aaca-4ffb-84f4-997e293945ec.png)

<br>

### (1) LOW 단계

File upload 메뉴에 들어가서 `Browse...` 버튼을 눌러 다음과 같이 Webshell.php 파일을 업로드 한다.

![image](https://user-images.githubusercontent.com/53431568/125961167-d66cc175-4c34-4054-9d50-4984f053aabf.png)

업로드 버튼을 누르자 잘 업로드 되었다고 한다. 이제 저 경로로 가보자!

![image](https://user-images.githubusercontent.com/53431568/125961217-90beab5b-b0a1-441f-a6bd-6b8706af8107.png)

다음과 같이 커맨드 명령어 입력창이 있어 이를 통해 시스템 명령어를 실행할 수 있다.

![image](https://user-images.githubusercontent.com/53431568/125961420-d2429785-581e-482e-9517-7195ec029da1.png)

```cat /etc/passwd```로 실험한 결과 etc/passwd 내용이 출력된다!

이런식으로 시스템 명령어를 실행할 수 있는 페이지를 **`웹셀`**이라고 한다.

![image](https://user-images.githubusercontent.com/53431568/125961523-ce7cbd11-5e7a-495d-8411-a07597b260bd.png)

<br>

### (2) Medium 단계

앞에서 한 것과 같은 공격을 실행해보았다! 이번에는 이미지가 업로드가 되지 않았는데 그 이유를 알기 위해 bursuite를 통해 요청을 intercept 해서 살펴보자!

![image](https://user-images.githubusercontent.com/53431568/125961969-9236ff7e-efea-4016-8b22-dd267779f629.png)

`intercept -> on`으로 설정한 후 요청 낚아채기!

![image](https://user-images.githubusercontent.com/53431568/125962620-4627fcbc-7fc6-412a-859f-a924069567f4.png)

이 부분 때문인것 같다! 

![image](https://user-images.githubusercontent.com/53431568/125962738-f322ddc8-8d4a-4034-8e98-745051981afa.png)

<br>

공격이 되나 확인하기 위해 한번 조작해서 요청을 날려봅시다. `application/x-php` -> `image/jpg`

![image](https://user-images.githubusercontent.com/53431568/125963076-15d182e1-d62c-4468-b1b3-fc35fef5c225.png)

잘 실행이 되었다. 미디엄 단계에서는 파일 업로드 공격 대응을 위해 **`Content-Type 만 확인`**한다는 것을 알 수 있다.

![image](https://user-images.githubusercontent.com/53431568/125961217-90beab5b-b0a1-441f-a6bd-6b8706af8107.png)

<br>

### (3) High 단계

high 단계에서는 어떻게 대응하는지 소스코드를 통해 알아보자

![image](https://user-images.githubusercontent.com/53431568/125964009-26907c7d-9c8f-4a17-896f-e0ed3b4fde57.png)


실제 파일내용의 확장자를 확인하고, getimagesize() 라는 함수를 통해 실제로 파일사이즈인지 확인하고 있다. 미디엄 단계에서 한 것 처럼 intercept를 해보자. 

![캡처](https://user-images.githubusercontent.com/53431568/125964342-3602b7e2-3fde-4d36-99b1-16bb5420f412.JPG)

이번엔 수정할 부분이 2가지가 있다.

1. filename을 **`webshell.php` -> `webshell.php.jpg`**로 바꾼다. 
2. `<?php` 가 시작하기 전인 파일 내용 앞부분에 **`GIF89a`** 라는 코드를 입력한다. 

> 💡 **`GIF89`** 는 gif 파일의 표준에 정의된 값으로 파일 내용앞에 입력을 하면 이미지 파일인 것 처럼 속일 수 있다. 

![image](https://user-images.githubusercontent.com/53431568/125964595-e5ee45ff-425f-413c-a68e-199f2692b100.png)

공격 성공!

![image](https://user-images.githubusercontent.com/53431568/125961217-90beab5b-b0a1-441f-a6bd-6b8706af8107.png)



### 파일업로드 공격 대응 👨‍🏫

경로로 확인해보니 파일이름끝이 php로 끝나지 않았기 때문에 실행되지 않는 것을 볼 수 있다. 

![image](https://user-images.githubusercontent.com/53431568/125965436-21cc9f08-df44-4387-b3a3-0c39b57c11d4.png)

어떤 방법이 있을까?!🤔🤔

**파일인클루젼 공격, 파일 업로드, 패스트래버설 공격** 세 가지로 조합하면 할 수 있다! 한번 해보자!

#### 1) DVWA에서 파일 인클루젼 메뉴로 들어가서 `page = file/../../../hackable/uploads/webshell.php.jpg` 입력!

![image](https://user-images.githubusercontent.com/53431568/125965830-e6aa2290-2fc3-40e1-bbee-cf66f265e2bd.png)

security가 high 단계 상황이므로 `패스트래버설 공격`을 이용해 경로를 지정해야 한다. 원래 업로드된 파일의 위치가 dvwa/hackable/upload 아래에 있기 때문에 총 3번의 상위 디렉토리로 이동해야하기때문에 `../../../` 를 지정해주는 것이다!!

#### 2) cmd 파라미터 값 입력

강사님이 설정한 webshell.php 파일은 cmd 파라미터를 실행하는 것이므로 주소창에 바로 cmd 파라미터 값을 넘겨주어야 한다. 

다음과 같이 &cmd=cat /etc/passwd 처럼 바로 명령어를 입력한다.

![image](https://user-images.githubusercontent.com/53431568/125966976-6633e015-d0c0-4f89-8509-50d68876378d.png)

이렇게 세가지 공격을 조합히 해킹에 성공한 것을 볼 수 있다~ 뭔가 신나는데? ㅎㅎㅎ🤔🤔

<br>

이제 impossible 단계를 통한 공격 대응을 살펴보자!

소스코드를 살펴보니 진짜 이미지 파일인지 확인함과 동시에 이미지를 업로드 된 내용으로 다시 생성하고 있다! 이렇게 되면 무늬만 이미지 파일이 업로드 되는 것을 방지할 수 있게된다.

![image](https://user-images.githubusercontent.com/53431568/125967640-c1222e6c-ff29-4559-9997-f7cc16becbc8.png)

#### 또 다른 방법엔 무었이 있을까?! 살펴보자🙆‍♀️

- 업로드되는 파일이름 랜덤하게 생성해 해커가 내가 업로드한 파일에 접근하지 못하게 하는 방법
- 업로드 되는 서버를 웹어플리케이션 서버와 분리시키는 방법
- 업로드 폴더의 실행권한을 완전히 제거해 파일 인클루젼공격으로부터 완전히 차단하는 방법


<br><br>


### CAPTCHA 공격

![image](https://user-images.githubusercontent.com/53431568/125968433-0c64f7de-f6e4-4abb-9315-28b08b0e9cba.png)

위의 이미지를 많이 보았을 것이다. **CAPCHA란?!** 프로그램을 실행하는 것이 사람인지 확인하기 위해 컴퓨터는 알 수 없는 글씨 등을 확인하게 하는 것이다. 사람이 직접하지 않으면 안되는 중요한 것! ( ex) 비밀번호 변경 시 )을 할때 사용한다. 

특히, 브르투포스 공격처럼 프로그램을 이용핸 공격에 대응할 때 효과적이다.


> 💡 하지만 이를 제대로 구현하지 않으면 해커는 이를 **우회**하여 공격할 수 있다! (똑똑한것들...)

![image](https://user-images.githubusercontent.com/53431568/125968793-7e09c6fc-51e0-4060-8f7f-da27ae25ea90.png)

비밀번호 변경을 예로 들어보았다. 만약 1,2 가 제대로 구현되지 않으면 해커가 CAPTCHA를 확인한 것 처럼 꾸미고 2번만 실행하게 하는 것이다.


아쉽게도 실습부분은 개정(?)이 되서 그런지 에러가 형성되어있어 스킵한다 ㅎㅎ 이론만 알아두자! 👏👏



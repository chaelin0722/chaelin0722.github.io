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

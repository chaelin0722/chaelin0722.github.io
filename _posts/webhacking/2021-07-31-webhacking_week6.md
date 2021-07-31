---
title:  '[강의] 6주차 웹 해킹- 웹 해킹 기술 탐색 5'
excerpt: "week6"

categories:
  - study
  - etc
  - hacking
tags: [study, etc, virtualbox, kalilinux, hacking]

last_modified_at: 2021-07-31T08:06:00-05:00
classes: wide
--- 

### 8주간의 해킹 스터디 다섯번째 시간

6주차 스터디! 

강의링크는 오른쪽! => [goormedu](https://edu.goorm.io/lecture/4953/화이트해커가-되기-위한-8가지-웹-해킹-기술)

<br>

이번 시간은 스크립트 코드 삽입을 통해 개발자가 고려하지 않은 기능이 작동하게 하는 치명적인 공격인 **크로스사이트스트리밍(XSS) 공격**에 대해 알아보겠습니다. 

<hr>

### XSS 공격

XSS공격에 대해 알아보기 전 XSS에서 사용하는 언어인 **자바스트립트 언어**에 대해서 알아보도록 한다.

#### - 자바스크립트 (JS)

웹어플리케이션에서 사용되는 언어 HTML과 자바스크립트로 이루어져있음

> HTML : 텍스트, 이미지 등 정적인 내용 표시
> 
> 자바스크립트 : 동적인 기능을 구현 ( ex) 마우스를 가져가면 메뉴의 색이 변하는 것 )

JS 사용 방법!
~~~
<script>  script code    </script>
~~~ 


#### XSS 공격이란?

클라이언트 쪽의 웹브라우저를 공격하는 기법이다. 

만약 어떤 웹브라우저 기능이 입력값을 그대로 출력하는 것이라면, <script>와 같은 입력값이 그대로 웹페이지에 표시되면 위험하다. 
  
해커는 이 자바스크립트를 이용해 세션 쿠키를 알아내 탈취하게 되는 것이다.

XSS공격 2가지
> 1. reflected XSS
>
> 2. stored XSS
  
<br>
  
#### 1. Reflected XSS 공격
 
reflected는 script를 반사하기 때문에 붙여진 이름이다. 

먼저 해커가 한 사용자에게 이메일 등으로 피싱을 한다.
  
![1](https://user-images.githubusercontent.com/53431568/127724126-1218ea01-9dec-4acc-89d2-931aaaed49aa.PNG)

<br>
  
이때, 사용자가 피싱에 속아 클릭을 하게 되면 스크립트 코드가 삽입된 요청이 전송된다.
  
![2](https://user-images.githubusercontent.com/53431568/127724129-3d0a9ede-6cea-426b-8886-a526547ca6ad.PNG)

<br>
  
웹 어플리케이션은 스크립트 코드를 반사시켜 그대로 되돌려준다. 이렇게 되면 웹 브라우저는 스크립트를 실행하게 되고 얻어낸 쿠키를 해커에게 전달하게 된다.

![3](https://user-images.githubusercontent.com/53431568/127724134-af31a433-2e05-479d-8d6a-18a293cd9062.PNG)

<br>

해커가 얻어낸 세션 쿠키를 사용되면 해당 사용자의 계정으로 접속할 수 있게 된다.
  
![4](https://user-images.githubusercontent.com/53431568/127724131-8e03fa4f-29b5-49f3-88fc-a93cbb1da238.PNG)

  
  
#### 2. Stored XSS 공격

script코드가 한 번 저장되었다가 나중에 실행되는 공격!
  
먼저, 해커가 피싱을 하는 것이 아니라 서버의 게시판이나 방명록 같은 곳에 스크립트 코드를 남긴다.
  
![image](https://user-images.githubusercontent.com/53431568/127724500-9e13c762-5561-4e5f-9621-20b171b958b1.png)

<br>

이후 사용자가 방명록에 접속해 글을 읽게 되면 글을 읽은 사용자에게 스크립트 코드가 실행된다.

![image](https://user-images.githubusercontent.com/53431568/127724518-56784bb0-41e9-4ef1-8160-fccd2e574f91.png)

<br>
  
이 이후는 reflected XSS 공격과 같이 쿠키값이 해커에게 전달되고 해커는 해당 사용자의 계정으로 접속할 수 있게 된다.

![image](https://user-images.githubusercontent.com/53431568/127724536-c2e840d8-a148-470f-8a89-56f7bc683d8c.png)

  
  
  
#### 이미지 출처
  
[goormedu](https://edu.goorm.io/lecture/4953/화이트해커가-되기-위한-8가지-웹-해킹-기술)
  
  
  

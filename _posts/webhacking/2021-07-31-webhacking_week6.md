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

강의링크는 오른쪽 클릭! => [goormedu](https://edu.goorm.io/lecture/4953/화이트해커가-되기-위한-8가지-웹-해킹-기술)

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
  1. reflected XSS
  2. 
  
  
#### Reflected XSS 공격
 
reflected는 script를 반사하기 때문에 붙여진 이름이다. 
  
![1](https://user-images.githubusercontent.com/53431568/127724126-1218ea01-9dec-4acc-89d2-931aaaed49aa.PNG)

![2](https://user-images.githubusercontent.com/53431568/127724129-3d0a9ede-6cea-426b-8886-a526547ca6ad.PNG)

  
![3](https://user-images.githubusercontent.com/53431568/127724134-af31a433-2e05-479d-8d6a-18a293cd9062.PNG)

![4](https://user-images.githubusercontent.com/53431568/127724131-8e03fa4f-29b5-49f3-88fc-a93cbb1da238.PNG)

  
  

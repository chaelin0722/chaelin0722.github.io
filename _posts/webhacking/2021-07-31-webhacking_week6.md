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

#### - 자바스크립트
> 웹어플리케이션에서 사용되는 언어 HTML과 자바스크립트로 이루어져있음
> HTML : 텍스트, 이미지 등 정적인 내용 표시
> 자바스크립트 : 동적인 기능을 구현 ( ex) 마우스를 가져가면 메뉴의 색이 변하는 것 )

~~~
<script>  script code    </script>
~~~ 
와 같이 구현한다.




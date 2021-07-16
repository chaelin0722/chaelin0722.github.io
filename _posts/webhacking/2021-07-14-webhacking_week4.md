---
title:  '[강의] 4주차 웹 해킹- 웹 해킹 기술 탐색 3'
excerpt: "week4"

categories:
  - study
  - etc
  - hacking
tags: [study, etc, virtualbox, kalilinux, hacking]

last_modified_at: 2021-07-14T08:06:00-05:00
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



### 파일업로드 공격 실습

#### (1) LOW 단계












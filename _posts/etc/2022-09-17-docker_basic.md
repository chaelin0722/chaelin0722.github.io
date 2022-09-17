---
title:  "[docker] 도커 차근차근 알아보기"
excerpt: ""

categories:
  - python
  - docker
  - etc
  
tags: [study, python, docker]

last_modified_at: 2022-09-17T08:06:00-05:00
---




## docker login

먼저 아래 사이트에서 회원가입을 해준다!

[https://hub.docker.com/](https://hub.docker.com/)

다음은, ubuntu 에서 로그인 해주기! 명령어는 `docker login`

로그인부터 에러가났다! 이럴줄 알았지~!

![login_error](https://user-images.githubusercontent.com/53431568/190860428-902a264c-c112-4e65-8e99-fbe8c9db155a.png)

일단 나는 아래 명령어로 해결이 되었다.

~~~
sudo apt install gnupg2 pass
~~~

짜잔~ 로그인 성공!

![login_success](https://user-images.githubusercontent.com/53431568/190860436-f0eed2af-d161-4c9f-9e74-1757c412085e.png)

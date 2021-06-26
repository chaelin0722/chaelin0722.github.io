---
title:  '1주차 웹 해킹- 실습 환경 구성 웹 보안'
excerpt: "week1"

categories:
  - study
  - etc
tags: [study, etc, virtualbox, kalilinux, hacking]

last_modified_at: 2021-06-25T08:06:00-05:00
classes: wide
---

### 8주간의 해킹 스터디 진행 시작!

막학기를 마치고 대학원 입학전에 알차게 방학을 보내고자 스터디를 신청하였다.

강의는 goormedu에서 제공하는 "화이트해커가 되기 위한 8가지 웹 해킹 기술"을 8주에 걸쳐 스터디를 진행한다 🙆‍♀️ 예에에!

![image](https://user-images.githubusercontent.com/53431568/123431851-b2a05f80-d604-11eb-97b8-efff905b1f02.png)

짧은 강의들로 구성되어있어서 듣기 부담이 없었다 ㅎㅎ 듣고 싶으면 여기로 gogo~! => [goormedu](https://edu.goorm.io/lecture/4953/화이트해커가-되기-위한-8가지-웹-해킹-기술)

<br>
<br>

## 해킹실습을 위한 환경구성 

### ⏹ VirtualBox 설치

![virtual](https://user-images.githubusercontent.com/53431568/123504242-f93c9b00-d692-11eb-876c-5719e480321f.jpg)

먼저, [VirtualBox](https://www.virtualbox.org/wiki/Downloads)사이트에 가면 최신 버전의 파일이 준비되어있다. 

1. platform packages 중에 자신의 운영체제에 맞게 다운을 받는다.
2. 다운받은 exe 파일을 실행하면 default값 그대로 유지하면서 next버튼을 누르고 최종적으로 install 하면 끝! 간단하다.
3. 그 다음엔  extension pack을 설치해준다. 설치하겠습니까? 라는 질문에 yes! 누르면 끝

이제 virtual box의 준비가 완료되었다.
<br>

### ⏹ 칼리리눅스 설치

칼리리눅스를 설치하기 위해서는 [kali 홈페이지](kali.org)에서 최신버전을 다운받으면 된다. 하지만 강사님의 추천으로 원활한 실습을 위해 [secuacademy](http://secuacademy.com/files/)에 올라온 kali 2019버전을 다운받았다. 

kali가 다운이 되었다면 ova 파일이 생긴것을 확인할 수 있다. 이것을 클릭해 사용할 수 있지만 앞서 설치한 virtual box에서 가상시스템을 가져오기에서 가져올 수 있다. 

1. 파일-> 가상시스템가져오기 클릭!

![12](https://user-images.githubusercontent.com/53431568/123504838-98af5d00-d696-11eb-8248-75b68a4e2545.jpg)

2. 파일버튼 클릭 -> kali-linux 클릭!

![11](https://user-images.githubusercontent.com/53431568/123504837-9816c680-d696-11eb-9f42-00bca83cba7f.jpg)

3. 다음을 누르고 가져오기를 누르면 다음과 같이 kali-linux가 생긴것을 확인할 수 있다.

![생성](https://user-images.githubusercontent.com/53431568/123504839-9a792080-d696-11eb-90c2-9ccd26d24a54.PNG)


외부인터넷과 사설망 인터넷 두개를 사용하기 때문에 어댑터를 추가해줄 것이다!

어댑터2로 가서 호스트 전용어댑터를 추가해준다.

![어댑ㅌ](https://user-images.githubusercontent.com/53431568/123505018-b204d900-d697-11eb-83b9-e8a1cdd79c09.PNG)

💡 이렇게 되면 **어댑터1의 NAT를 이용해 외부 인터넷을 사용**하게 되고 **어댑터2의 호스트 전용 어댑터**로 다른 가상머신들과 별도의 사설네트워크로 통신하게 되는 것이다!!

<br><br>

### 이제 칼리리눅스를 실행해보자!

시작버튼을 누르거나 더블클릭으로 실행하면된다. (username/password는 강의에서 강사님이 설명해주시므로 거기서 확인해보면 될 것 같다 ㅎㅎ)

1. kali 화면에서 터미널을 켜주고 (터미널 메뉴는 검정색 어플을 왼쪽 메뉴에서 찾을 수 있다) `ip addr`로 eth를 확인해준다.

eth0은 아까 설정에서 본 NAP 어댑터이고 eth1은 호스트 전용 어댑터이다.

eth0은 127.0.0.1 로 ip가 설정되어있지만 eth1은 그렇지 않은 것을 확인할 수 있다.

![image](https://user-images.githubusercontent.com/53431568/123505789-090cad00-d69c-11eb-8b9f-15fd766825bc.png)

2. 네트워크 설정을 위해 편집기를 실행해 파일을 하나 추가해줄 것이다.

이미지의 마지막 초록색 어플이 `leafpad`라는 파일편집기 이다. 이 편집기를 열고 상단 메뉴의 File -> Open을 클릭해준다. 

![leopard](https://user-images.githubusercontent.com/53431568/123506024-ffd01000-d69c-11eb-94f3-76df3385b0a2.PNG)

다음으로 FileSystem -> etc/network/interfaces 파일을 수정해준다. 아래와 같이 파일의 맨 밑부분에 4줄을 추가해준다. 

![image](https://user-images.githubusercontent.com/53431568/123506068-49205f80-d69d-11eb-8463-a5029290da37.png)

![eth](https://user-images.githubusercontent.com/53431568/123505875-6143af00-d69c-11eb-9c91-bcdb39025213.PNG)

3. 위의 파일을 저장 후 ```systemctl restart networking``` 이라는 명령어로 재시작을 해준다. 다시 ```ip addr```로 확인하면 각 eth에 ip주소가 할당된 것을 확인할 수 있다. 웹브라우저를 열면 웹브라우저를 사용할 수 있는 것으로 확인할 수 있다. 
<br><br>

### 다음으로는 스냅샷 만들기!

스냅샷이라는 것은 복원지점을 만드는 것인데, 만약 kali를 쓰다가 무슨 오류가 난다면 스냅샷에 설정해둔 지점으로 복원할 수 있다. 매우 유용하므로 중간중간 만들예정이다 ㅎㅎ

virtual box 프로그램으로 들어가서 kali-linux 메뉴 오른쪽 부분을 클릭하면 스냅샷이 보인다.

![스크린샷(8)](https://user-images.githubusercontent.com/53431568/123506224-15920500-d69e-11eb-9622-98394472f055.png)

현재 kali linux 설정을 완료했으므로 스냅샷 하나를 설정해 주기로 했다. 

![스냅샷](https://user-images.githubusercontent.com/53431568/123506120-969ccc80-d69d-11eb-84a2-f38c193ede88.PNG)

후에 복원버튼을 누르면 다시 초창기 설정 부분으로 돌아올 수 있을 것이다.

![완료](https://user-images.githubusercontent.com/53431568/123506227-162a9b80-d69e-11eb-9045-6e2b34c5a00b.png)

<br><br>

### ⏹ XAMPP 설치

이제 xampp를 설치해보자! xampp는 virtual box의 kali-linux 환경의 웹브라우저에서 다운받아 주면 됩니당~

1. 나는 23bit linux를 설치했기 때문에 xampp도 32bit로 다운을 받아야 한다. 따라서 5.6.23 버전으로 설치를 해주었다. => [xampp-linux-5.6.23](https://sourceforge.net/projects/xampp/files/XAMPP%20Linux/5.6.23/xampp-linux-5.6.23-0-installer.run/download)

![image](https://user-images.githubusercontent.com/53431568/123506683-791d3200-d6a0-11eb-9655-17fec028cfe6.png)

> 💡 5.6 버전인 이유는 뒤에 사용할 DVWA와의 호환을 위한 버전이므로 주의해서 다운받도록!

<br>

2. 다시 터미널로 돌아가서 다운로드 폴더에 잘 들어가있는지 확인!

![image](https://user-images.githubusercontent.com/53431568/123506713-a5d14980-d6a0-11eb-9b0d-cb4b62be232d.png)

자세히 보면 아직 실행권한을 갖고있지 못한 것을 볼 수 있다. 다음 명령어로 실행권한을 주자!

```chmod +x ./xampp-linux-5.6.23-0-installer.run```

다시 확인하면 권한이 생긴것을 확인할 수 있다.
![image](https://user-images.githubusercontent.com/53431568/123506776-f9dc2e00-d6a0-11eb-9026-5c92f044bb45.png)

<br>

3. xampp 실행

다음 명령어로 xampp를 실행한다.

```./xampp-linux-5.6.23-0-installer.run```

<br>

4. MySQL 설정

xampp가 성공적으로 install finish 했다면 ```gedit /opt/lampp/etc/php.ini``` 명령어로 아래 파일을 수정해준다. ctrl+f 키로 `allow_url_include`를 찾고 Off -> On 으로 변경해 준다. 이 옵션은 파일인클루젼 관련 공격을 할때 필요한 옵션이다. ((메모메모👨‍🏫👨‍🏫))

![image](https://user-images.githubusercontent.com/53431568/123507236-4e80a880-d6a3-11eb-8dae-887f4a903504.png)

다음으로 MySQL을 RUNNING 시키고 Apache Web Server을 restart 시켜야 하는데.. 나는 강사님처럼 XAMPP manager가 ui로 보이지 않는다. 그래서 커맨드 라인으로 설정해주었다. 

> ```sudo /opt/lampp/lampp start``` 이 명령어를 사용하면 mysql과 apache가 시작되면서 phpmyadmin도 접속이 가능해진다.

![image](https://user-images.githubusercontent.com/53431568/123507570-1f6b3680-d6a5-11eb-8e62-30090acc37ee.png)

<br>

* phpmyadmin확인

![image](https://user-images.githubusercontent.com/53431568/123507592-4cb7e480-d6a5-11eb-87c4-7666ffd8ecc0.png)

다 끝나거나 사용을 멈추고 싶다면 ```sudo /opt/lampp/lampp stop```을 해주면 된다.

<br><br>

### ⏹ DVWA 설치 및 설정

DVWA를 다운하기 앞서 XAMPP를 통해 DVWA에서 사용할 데이터베이스를 설정할 것이다.

1. DVWA를 위한 DB 설정

phpmyadmin으로 접속하여 databases로 들어간다. 그 다음 create database 밑에 dvwa라는 이름으로 db를 하나 생성해 준다.

![image](https://user-images.githubusercontent.com/53431568/123507754-22b2f200-d6a6-11eb-83ee-59144124e0cd.png)

<br>

2. DVWA 다운

업데이트 된 DVWA를 사용하기 위해  [secuacademy](http://secuacademy.com/files/)에 올라온 DVWA를 다운로드 하였다.

- DVWA를 save한 후 확인

![image](https://user-images.githubusercontent.com/53431568/123507859-e9c74d00-d6a6-11eb-88a9-68aa2942e687.png)

이후 unzip 명령어로 풀어주고 `/opt/lampp/htdocs/dvwa`로 폴더를 옮겨준다.

![image](https://user-images.githubusercontent.com/53431568/123507930-62c6a480-d6a7-11eb-8b55-eacfb18a3c74.png)

이제 웹브라우저에서 dvwa를 접속할 수 있다!

`localhost/dvwa` 로 검색하면 끝! (username/password를 입력하라고 뜨는데 이는 kali-linux에서 설정해준것과 동일하다)

![image](https://user-images.githubusercontent.com/53431568/123507980-ac16f400-d6a7-11eb-9638-607fa7d38506.png)

처음 접속시 설정을 해주어야 하는데 녹색 부분 말고 빨간색 부분을 추가로 설정해 주어야 한다.

<br><br>

3. 우선 CAPTCHA 관련 설정을 해준다. 

[https://www.google.com/recaptcha/admin](https://www.google.com/recaptcha/admin) 주소로 이동해준다. 구글 계정 로그인 후 아래와 같은 화면이 나온다 

3-1. 그리고 다음과 같이 라벨은 dvwa 를 입력, 

![1](https://user-images.githubusercontent.com/53431568/123508169-b84f8100-d6a8-11eb-8458-085430ce9722.PNG)

3-2. 도메인에 localhost를 입력해준 후 register버튼(제출버튼)을 눌러준다. 

![2](https://user-images.githubusercontent.com/53431568/123508173-b980ae00-d6a8-11eb-8368-4c3e43858778.PNG)


이제 사이트 키가 생성이 되었다. 이 키들을 dvwa의 설정파일에 입력해 주어야 한다. 

![image](https://user-images.githubusercontent.com/53431568/123508227-02386700-d6a9-11eb-8768-2fd342b19747.png)

다시 터미널 창으로 gogo~

dvwa가 있는 폴더로 이동 후 config.inc.php를 수정해준다. 

![image](https://user-images.githubusercontent.com/53431568/123508248-32800580-d6a9-11eb-887b-f7d42fb1e498.png)

![image](https://user-images.githubusercontent.com/53431568/123508269-4c214d00-d6a9-11eb-996a-9ea594c2bbb9.png)

각각 publickey-> 사이트키 복사, privatekey-> 비밀키 를 복사해 붙여넣는다. -> save

사이트에 접속후 다시 확인해보면 아래와 같이 바뀌어져 있다.

![image](https://user-images.githubusercontent.com/53431568/123508430-64459c00-d6aa-11eb-940c-7c0167b60fa3.png)

<br><br>

4. 쓰기 권한주기

아래의 두 권한(쓰기 권한)을 주기위해 다음 명령어를 실행한다. 

![image](https://user-images.githubusercontent.com/53431568/123508479-b8508080-d6aa-11eb-92ee-9f8d6a8fe3be.png)

![image](https://user-images.githubusercontent.com/53431568/123508466-a1aa2980-d6aa-11eb-9dcb-8da8e17684fa.png)

5. create /Reset Data 버튼을 눌러 DB 생성

![image](https://user-images.githubusercontent.com/53431568/123508682-d1a5fc80-d6ab-11eb-9fd5-578f63908c7d.png)

버튼을 누르면다시 로그인 페이지로 REDIRECT 된다. 그럼 이제 모두 끝!


<br><br>

이것으로 실습을 위한 모든 환경설정이 끝이 났습니다~🎉🎉 🙆🏻‍♀♀♀ 🎉🎉♀️ 설치의 기나긴 여정이었다!

이제 본격적으로 공부를 시작해 봅시다 ㅎㅎ👩🏻‍💻

## 화이트 해커란?!

광범위하게는 모든 정보보안 관련 전문가를 칭하며 구체적으로는 **"실제 해킹 기술을 보유하고 있는 보안전문가"** 를 뜻한다.

그렇다면 화이트 해커가 불법해킹과의 차이는? `사전 허가 여부`이다. 

화이트해커의 역할이 공격자와 동일한 관점에서 해킹을 수행하기 때문에 기술적 부분에서는 차이가 없다. 

따라서 반드시 허가를 받은 후에 해킹을 해야한다!!

강사님이 환경을 구성하고 실습하기 유용한 사이트로 [exploit-db.com](exploit-db.com)을 언급하셨는데 이 사이트는 공개된 exploit이 있으며 칼리리눅스의 
메인 스폰서인 offensive security에서 관리하고 있는 데이터 베이스로 많은 exploits가 공개되어 있다고 한다.

## 보안취약점과 해킹

### 보안취약점

- 정보보안의 3요소 중 최소 한가지 영향을 미치는 소프트웨어 버그
- 정보보안의 3요소
    - 기밀성
    - 무결성
    - 가용성
- 해킹은 보안취약점을 공격하는 것

`웹 해킹 역시 웹 보안취약점을 공격해 이루어집니다`

### 해킹의 기초단계

![image](https://user-images.githubusercontent.com/53431568/123429541-05c4e300-d602-11eb-91fd-c28666d4676a.png)

예시를 들며 설명하면 프로세스는 다음과 같이 설명할 수 있다!

1. 정찰 - 구글에 대한 각종 정보 수집, 웹 서버 등 외부에서 접근가능한 ip주소가 무엇인지를 알아낸다.
2. 스캐닝 및 취약점 분석 - 서버의 운영체제가 무엇인지, 프로그램의 버전이 무엇인지를 알고 어떤 취약점이 있는지 알아낸다.
3. 공격(익스플로잇) - 어떤 사용자의 비번을 알아내거나 취약점을 알아내면 실제로 침투를 실행한다.
4. 권한상승 - 침투 후 관리자 권한을 획득하는 단계이다.
5. 백도어 관리 - 언제든지 해당 시스템에 접속할 수 있도록 사용자를 추가해 두거나 백도어를 설치해 관리하는 것이다. 
6. 흔적 지우기 - 해킹의 흔적을 지워 해킹을 당한것을 모르게 하며 추적당하지 않도록 한다. 

> 위의 프로세스는 한가지 시스템에 대한 것이며, 만약 내부의 또 다른 네트워크나 시스템이 여러개가 있다면 시스템 해킹에서 얻은 정보로 또 다시 다른 해킹을 진행할 수 있다. 

## 웹 보안의 중요성

### 버그바운티 제도

보안 취약점을 발견해 보고하면 보상해주는 제도로 구글, 페이스북 마이크로소프트가 운영 하고있다.

발견한 보안이 심각한 것일 수록 보상금액이 높아진다고 한다.💲💲

최근엔 버그바운티를 전문으로 하고있는 웹사이트도 등장한다고 한다.

[bugcrowd] 라는 회사로 버그바운티 제도를 운영하기 어려운 다른 웹사이트를 위한 사이트로 점점 시장이 커지는 추세이다.

![image](https://user-images.githubusercontent.com/53431568/123430861-9e0f9780-d603-11eb-8141-2533fc610d56.png)

최근엔 버그헌터라는 프리랜서 직업도 생긴다니 웹 해킹기술을 공부하면 이런 분야로 일하는 것도 재밌을 것 같다.


## 웹 아키텍쳐

웹은 크게 클라이언트 영역과 서버영역으로 나뉜다.

![image](https://user-images.githubusercontent.com/53431568/123435644-bc2bc680-d608-11eb-9bce-0987a16fc603.png)

(1) 클라이언트 영역에서는 웹브라우저를 이용해 웹에 접속하고 http로 인터넷을 통해 서버영역에 접속한다.

(2) 서버영역은 로직티어와 데이터 티어로 이루어져 있다. 
> 로직티어는 웹사이트의 각종 메뉴들을 서비스하고 클라이언트에서 전송된 http를 처리한다.
> 데이터티어는 데이터베이스를 말하며 로직티어의 요청에 따라 데이터를 주고, 로직티어는 받은 데이터를 가공해 최종적으로 다시 클라이언트에 전송한다.

### HTTP

- Hyper Text Transfor Protocol
- 주로 80번 포트를 이용해 서비스
- HTTP요청과 HTTP 응답으로 이루어짐
- 클라이언트가 요청, 서버가 응답
- HTTP 메소두: GET, POST, PUT, DELETE 등
- 응답코드(status code)

### HTTP 응답코드

100번대 ~500번대 까지 있다. 에러메세지와 함께 전달되는 코드이며 각 번대 별 에러의 의미는 다음과 같다.

- 1XX : 정보전달
- 2XX : 성공
- 3XX : 리다이렉션
- 4XX : 클라이언트 에러
- 5XX : 서버에러

### 세션유지와 쿠키
- HTTP는 stateless이기 때문에 세션 ID를 이용해 이전 상태를 유지한다. 예를 들면 한번 로그인하면 다른 페이지로 이동해도 다시 로그인을 하지 않아도 되는것과 같다.
- 세션ID는 보통 쿠키로 전달한다.

 **"쿠키"** 🍪란?
      - 변수 = 값 의 집합
      - 서버는 Set-Cookie헤더를 이용해 쿠키를 전달하고 브라우저에서 이를 저장해 두었다가 cookie헤더에 전송한다.
- 세션ID를 탈취함으로 세션 하이재킹 공격이 가능하다. (따라서 세션ID는 잘 보호해 주어야 한다.😮)


## HTTP 프록시와 버프스위트

### HTTP 프록시
일반적으로 우리가 웹에 접속하는 모습은 다음과 같다.

![image](https://user-images.githubusercontent.com/53431568/123437018-301a9e80-d60a-11eb-8481-def7bf8ed175.png)

하지만 중간에 HTTP 프록시를 이용하면 HTTP요청과 응답을 중간에서 가로챌 수 있게 된다. 즉, 메세지의 내용을 보거나 수정해서 보내는 것이 가능하다. 웹 해킹이나 웹 보안점을 찾을때 매우 유용하다. 

![image](https://user-images.githubusercontent.com/53431568/123437067-3dd02400-d60a-11eb-84ec-211d8f64ef63.png)

HTTP 프록시 프로그램중 대표적인것으로 `"버프스위트"`가 있다.

### 버프스위트(Burp Suite)

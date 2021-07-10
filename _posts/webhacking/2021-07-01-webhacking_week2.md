---
title:  '2주차 웹 해킹- 웹 해킹 기술 탐색 1'
excerpt: "week2"

categories:
  - study
  - etc
tags: [study, etc, virtualbox, kalilinux, hacking]

last_modified_at: 2021-07-01T08:06:00-05:00
classes: wide
---

### 8주간의 해킹 스터디 두번째 시간

이번 시간에는 본격적인 웹 해킹 기술에 대해서 공부할 것이다! 크게 브루트 포스와 커맨드 인젝션 공격을 알아보고 실습을 해볼것입니다 😉

먼저 저번주차에 설치한 DVWA를 살펴보자

### DVWA

실습에 앞서 localhost/dvwa 에 접속이 안되는 문제를 겪었다.🥵

2가지 방법으로 해결했는데..! 본인에게 맞는 해결법을 실행하면 될 것 같다.

#### (1) `/opt/lampp/lampp restart` 를 실행 한 후 다시 접속해보면 된다!
#### (2) firefox의 preference에서 proxy 설정 확인하기

순서는 아래 따라하면 된다 ㅎㅎ

-1- preference 클릭

![pre](https://user-images.githubusercontent.com/53431568/124909815-dcef1580-e025-11eb-99dd-47ad5fab3d4b.PNG)

-2- preference클릭 후 나오는 페이지의 맨 아래쪽으로 이동하면 Network Proxy가 보인다! Settings... 버튼을 누르자

![network](https://user-images.githubusercontent.com/53431568/124909816-dd87ac00-e025-11eb-88ae-edc058101e7a.PNG)

-3- 아래와 같이 'Manual proxy configuration' 에 burpsuite에 설정된 http 주소인 127.0.0.1, port = 8080 으로 설정해주고 나머지 칸은 빈칸으로 되어있는지 확인하자

![image](https://user-images.githubusercontent.com/53431568/125155677-8526ea80-e19c-11eb-9576-d8cf0e1f82f8.png)



#### ⭐️⭐️ [지난번 공부했던 내용 인 웹 해킹 1주차](https://chaelin0722.github.io/study/etc/webhacking_week1/)를 참고해 버프스위트 부분을 참고하자. 

<br><br>

이제 웹해킹 기술에 대해서 알아봅시다!

### 브루트 포스 공격이란?

사용자 패스워드를 알아내기 위한 공격이고 아이디와 패스워드가 일치하는지 알아보기 위해 패스워드를 계속 대입하는 방법이다.

이 대입 방법의 종류에는 

> 1. 알파벳 순서
> 2. 딕셔너리 공격

![image](https://user-images.githubusercontent.com/53431568/124881746-abb51c00-e00a-11eb-81bd-e00557a3653b.png)

✏️이제 **브루트포스 공격을 실습을 통해 알아보자**

DVWA에 로그인해 들어간 후 왼쪽 메누에서 Brute Force로 들어간다. 아래와 같이 맞지 않는 아이디/비번을 입력하면 맞지 않는다고 경고 문자가 나온다. 

![image](https://user-images.githubusercontent.com/53431568/124882053-f8005c00-e00a-11eb-9d39-be1ab0ed9f69.png)

이런식으로 일일히 대입해 알아보는 방법을 dvwa를 통해 실습해보았다.

### 자동 브루트 포스 공격

✏️이번에는 **자동**으로 브루트 포스 공격을 하는 법을 알아보자

우선 burp suite 의 proxy에서 `intercept is on` -> `intercept is off` 로 변경해주자

![image](https://user-images.githubusercontent.com/53431568/124899006-a8c22780-e01a-11eb-894f-b0ec5bc8a82d.png)

<br>

password에 abcd를 입력하고 proxy-> HTTP history에서 가장 최근의 요청을 더블클릭해서 내용을 확인해보면

아래와 같이 쿼리스트링 부분을 보면 username=admin , password=abcd 임을 확인할 수 있다.

![KakaoTalk_20210708_203506419](https://user-images.githubusercontent.com/53431568/124915200-3bb78d80-e02c-11eb-9e96-c3d0c677f404.jpg)

<br>

이제, send to intruder로 intruder로 요청을 보내본다

![스크린샷(25)](https://user-images.githubusercontent.com/53431568/124902510-f4c29b80-e01d-11eb-9681-1b4cb2f9c086.png)

<br>

intruder -> position 메뉴로 들어가면 어떤 영역이 하이라이트 된 것을 확인할 수 있으며,

그림에 표시한것과 같이 쿼리스트링 부분과 쿠키 부분이 사용자가 입력한 값이 변수의 값으로 처리되어있는 부분이다.

![캡처](https://user-images.githubusercontent.com/53431568/124915491-90f39f00-e02c-11eb-96e0-0eeae33466ec.PNG)

clear$ 버튼으로 하이라이트를 제거하고 지금 당장 테스트 해볼 password 영역만 선택하여 Add$ 버튼을 누른다.

이런식으로 $$로 선택된 영역을 다양한 문자열로 치환해 테스트 할 수 있게 된다.

![image](https://user-images.githubusercontent.com/53431568/124915372-6bff2c00-e02c-11eb-9f6a-fde48e8ff30c.png)


<br>

payloads 메뉴로 들어가서 payload type을 Brute forcer로 바꿔준다. options를 살펴보면 대입해 볼 수 있는 character가 알파벳 a~z, 숫자는 0~9까지 사용할 수 있고, 길이는 4자리로 지정되어있어 4자리 글자로 만들 수 있는 password의 조합은 총 1,679,616 개이다.

대입할 character이 늘어날 수록, max length 길이를 크게 총 조합의 수가 많아진다.

![Inkedpayload_LI](https://user-images.githubusercontent.com/53431568/124903417-ea54d180-e01e-11eb-9e8f-8864d978159b.jpg)

#### 이제 공격을 실행해보자! 우측 상단의 `Start attack` 버튼을 누르자

다음과 같이 패스워드가 일치할때까지 4자리의 조합을 시도해보는 것을 볼 수 있다.

![image](https://user-images.githubusercontent.com/53431568/124904332-d9f12680-e01f-11eb-87e1-9ef0e6bd9b06.png)

하지만 password의 조합이 복잡해질수록 시간이 오래걸린다는 단점이 있다.

이를 위한 것이 **딕셔너리 공격**이다

<br>

### 딕셔너리 공격

공격에 앞서 password 파일 하나를 살펴보자.

`gedit /usr/share/john/passowrd.lst` 파일을 열면 여러 password 조합들이 딕셔너리 형태로 작성된 파일을 볼 수 있다.

![image](https://user-images.githubusercontent.com/53431568/124906568-29385680-e022-11eb-9afc-27ff6bd6b111.png)

자세히 보면 어떤 password가 많이 사용되는지 알 수 있다.

<br>

이제 burpsuite로 돌아가서 payloads를 다시보자!

load 버튼을 눌러 아까 보았던 password.lst 경로를 따라 파일을 업로드 한다.

![image](https://user-images.githubusercontent.com/53431568/124907328-f93d8300-e022-11eb-8dc2-685ef85b5c20.png)

remove 버튼을 이용해 위의 #!comment 부분을 싹 지우고 password의 조합만 나열하도록 하고 start attack!

![image](https://user-images.githubusercontent.com/53431568/124907479-2db13f00-e023-11eb-9c95-0989221c23e2.png)

#### 공격 실행 결과이다
다음과 같이 status와 length를 확인할 수 있는데 맞는 password이면 length가 다르게 나온다. 하지만 password 가 password 임에도 불구하고 똑같이 나오는 에러를 발견해 해결중이다..

![image](https://user-images.githubusercontent.com/53431568/124915898-0495ac00-e02d-11eb-9c37-222f2134015c.png)


###  브루트 포스 공격 대응 👨‍🏫

브루트 포스 공격은 어떻게 막을 수 있는지 알아보자!

### (1) medium level의 경우

먼저, DVWA Security의 level을 medium으로 올린다.

다시 Brute Force에 가서 잘못된 password로 로그인을 하면 응답이 느려진 것을 확인할 수 있다.

#### 그렇다면 view source 버튼을 눌러 소스코드를 살펴보자

소스코드를 살펴보면 로그인 실패시 sleep을 해주어 응답을 느리게 한다. 이 방법으로 brute force 공격을 지연시킬 수 있다.

![image](https://user-images.githubusercontent.com/53431568/124919247-f3e73500-e030-11eb-93f1-2ecca77ce244.png)


### (2) high level의 경우

소스코드를 보면 login 실패 시 0~3 초를 랜덤하게 sleep 해주어 응답을 느리게 한다. 이런식으로 지연이 되면 해커 입장에서는 비번이 틀렸다고 간주하기 때문에 다음 요청으로 실행해버린다.

![image](https://user-images.githubusercontent.com/53431568/124919449-40327500-e031-11eb-8e37-7a3bc819e4e8.png)

### (3) impossible level 의 경우

수 차례 로그인에 실패하였기 때문에 일정 시간 이후  재 로그인을 할 수 있다고 뜬다. 이를 locking이라고 하는데 이 방법의 경우 `사실상 brutefoce 공격을 거의 완전히 차단`할 수 있다.

![image](https://user-images.githubusercontent.com/53431568/124920154-ff872b80-e031-11eb-9672-cc1a9b1592a2.png)


단, 반대로 부작용이 있는데 해커가 오히려 이 점을 느려 특정 사용자의 아이디를 이용해 일부러 password를 틀리게 해 사용자가 특정시간동안 로그인을 못하게 만든다. 🥵🥵


> 💡 정리하자면, brute force 공격을 대응하려면 잘못된 로그인 시도가 여러번 발생하면 응답시간을 느리게 하거나 locking을 걸어 반복되는 로그인 시도를 무력화 시켜야 한다.

<br><br>

## 커맨드 인젝션 공격

- 웹을 통해 시스템명령어(커맨드) 를 실행하는 공격
- 웹 내부에서 시스템 명령어를 실행하는 경우 입력값을 제대로 검사하지 않으면 해커 마음대로 시스템 명령어를 실행한다.


다음과 같이 리눅스에서는 ; 과 같은 특수문자로 두개의 명령어를 실행하게 된다. 그러면 cat /etc/passwd 명령어가 실행되게 되고 이 정보가 해커에게 넘어간다.

![image](https://user-images.githubusercontent.com/53431568/124920607-73c1cf00-e032-11eb-8f98-11dddb96cb33.png)

(ping 명령어는 ip주소를 가진 어떤 시스템이 현재 동작하고 있는지 확인할때 사용하는 명령어이다.)

<br><br>

이제 DVWA에서 실습해보자!🙆🏻‍♀️🙆🏻‍♀️

각 security level 단계별로 살펴보겠다. 

### (1) low level의 경우

127.0.0.1을 실행했을 때, 아래와 같은 결과가 나온다.

![image](https://user-images.githubusercontent.com/53431568/124921042-f34f9e00-e032-11eb-9ce9-eb226787bbcf.png)

소스코드 확인

![image](https://user-images.githubusercontent.com/53431568/124921243-26922d00-e033-11eb-8cfb-4d482b260e83.png)

소스코드를 보니 `ping -c 4` 명령어를 terminal에서 사용하면 DVWA에서 한 것과 같은 command Injection 공격을 할 수 있는것을 알 수 있다.

다음은 터미널에서 ping을 날린 결과이다. `-c 4` 는 ping을 4번 보낸다는 뜻이다. DVWA와 같은 결과가 나오는 것을 볼 수있다.

![image](https://user-images.githubusercontent.com/53431568/124921453-63f6ba80-e033-11eb-87e7-14e0784782d2.png)

이번에는 뒤에 다른 명령어를 추가해 보낸 결과이다. 내 디렉토리 결과를 함께 전송하는 것을 볼 수 있다.

![image](https://user-images.githubusercontent.com/53431568/124921723-a8825600-e033-11eb-966e-0aa808d99c5e.png)

그 외 결과

![image](https://user-images.githubusercontent.com/53431568/124922044-0dd64700-e034-11eb-9aa2-da6b529dc222.png)


#### 이런 식으로 ;  &  | 등의 특수문자 뒤에 해커가 원하는 명령어를 보낼 수 있다.

<br>

DVWA화면에서는 다음과 같이 실행하면 된다.

![image](https://user-images.githubusercontent.com/53431568/124921930-e97a6a80-e033-11eb-8527-911a5378bef1.png)


### (2) medium level의 경우

소스코드를 보면 && 과 ; 문자를 지워주는 코드가 추가되어 있어 `ping -c 4 ; id` 와 같은 명령어는 `ping -c 4 id`이런 식으로 실행되게 된다. 따라서 잘못된 명령어가 실행되는 것이다.

![image](https://user-images.githubusercontent.com/53431568/124922458-7b827300-e034-11eb-9a7a-158499222a05.png)

단, 다른 특수문자의 경우에는 해커에게 공격당할 수 있다.

### (3) high level의 경우

소스코드를 보면 medium level 보다 더 많은 문자들에 대해 대응이 가능해졌다.

![image](https://user-images.githubusercontent.com/53431568/124922750-c8664980-e034-11eb-9b1e-22409f5ea8ab.png)



###  커맨드인젝션 공격 대응 👨‍🏫

Impossible level을 보면서 커맨드 인젝션 대응은 어떻게 막을 수 있는지 알아보자!

다음은 impossible level에서의 소스코드 부분이다.

![KakaoTalk_20210708_214437622](https://user-images.githubusercontent.com/53431568/124923853-cea8f580-e035-11eb-9750-d90b50c88117.jpg)

입력된 값이 ip 주소가 맞는지 검증하고 있다. 

> 💡 정리하자면 **`이와 같이 커맨드 인젝션을 대응하기 위해서는 입력값을 확실히 검증하는 것이 관건이다.`** 


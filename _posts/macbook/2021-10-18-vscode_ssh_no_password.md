---
title:  "[mac] vscode에서 ssh password 없이 접속하기"
excerpt: "mac vscode without password"

categories:
  - mac
tags: [mac, ssh, etc]

last_modified_at: 2021-10-18T11:00:00-05:00
classes: wide
---

vscode에 적응이 될 즈음 이제 슬슬 password 없이 원격 ssh 연결을 시도해 보고자 했다! 맥에서 보안키를 발급해 서버에 키 파일을 넣어두면 비밀번호를 입력하지 않아도 되는 원리이다 ㅎㅎ

순서는 다음과 같다.🍒

### 1. 맥 보안키 생성

다음 명령어를 이용해 ssh key를 생성한다! Enter 2번 치면 생성 완료!

(Enter file in which to save the key (C:\Users\(name)\.ssh\id_rsa)가 나오는데 이건 내가 키파일을 저장할 장소를 의미, 그냥 Enter!
그 다음엔 Enter passphrase가 나오게 되는데 이것도 아무것도 누르지 않은 상태로 Enter를 눌러준다. => 총 2번 Enter!)


~~~
ssh-keygen -t rsa -b 4096 
~~~



### 2. 서버에 전송

이제 id_rsa.pub가 있는 /.ssh 경로로 가서 내가 사용할 서버에 전송을 해준다. ~ 경로는 기본 home 경로이다. 

주의!🌟 할 것은 

`id_rsa.pub`는 공개발급(public key) 키로 이 키로 서버와 공유를 하는 것이며, `id_rsa`는 private key로 절대로 타인에게 노출되면 안된다. 따라서 실수로 id_rsa를 보내는 일이 없도록 주의합시다!

이제 pub 키를 home 경로에 바로 scp 전송해주면 된다. 저는 터미널에서 아래 명령어와 같이 진행했습니다. 

~~~
scp id_rsa.pub username@ip_주소:~
~~~


### 3. authorized_keys 생성

이제 서버에 와서 id_rsa.pub 키를 vi로 열어서 내용을 복사한 후! /.ssh 경로에 `authorized_keys'파일을 만들어 key 내용을 붙여 넣어준다.

~~~
1. vi id_rsa.pub

2. 내용 드래그 복사

3. /.ssh 경로로 이동

4. vi authorized_keys

5. 내용 붙여넣고 저장
~~~

<img width="843" alt="3" src="https://user-images.githubusercontent.com/53431568/137743713-1baefb2e-52c2-40ae-8dce-121d37f8eddb.png">


### 4. vscode configuration 추가

이제 다시 vscode로 돌아와서 `f1` -> `Open SSH Configuration File` 에 들어가서

<img width="1278" alt="무제" src="https://user-images.githubusercontent.com/53431568/137745209-9c8d8a2f-0b95-41d4-8507-27604f05fcce.png">

host 정보 맨 아래에 다음과 같이 추가해주면 끝!

<img width="373" alt="무제" src="https://user-images.githubusercontent.com/53431568/137745563-eac0be8c-22bc-40c4-8be9-f1b8762fb4fe.png">




이제 비번 없이 접속가능해졌다 ㅎㅎ👍👍



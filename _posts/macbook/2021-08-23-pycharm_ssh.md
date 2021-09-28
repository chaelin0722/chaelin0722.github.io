---
title:  "[Pycharm] 파이참으로 원격 서버 연결하기"
excerpt: "ssh로 서버에서 개발하기"

categories:
- mac

tags: [ssh, linux, python, pycharm]

classes: wide

last_modified_at: 2021-09-01T08:06:00-05:00

---


맥북에서 서버에 접속하여 개발을 하려고 하는데 gui방식에 익숙해진 나로써는 iterm 으로 ssh 접속을 통한 개발이 번거롭다..

특히 1. 디버깅이 불편하고 2. 마우스가 안먹는 다는 점도 불편하다 😓😓

따라서 파이참에서 ssh 를 연결해 개발하는 방법을 사용하기로 하였다.

이 방식은 서버에 있는 파일이나 폴더들을 보거나 수정할 수 없지만 똑같은 환경 (예를 들어 아나콘다에 설정해둔 내 가상환경을 그대로 쓸 수 있음)이 사용가능하다. 또, 확실히 gui 방식으로 디버깅이 훨씬 편하다.

그럼 어떻게 설정할 수 있는지 알아보자!


## 1. pycharm 설치

 먼저 파이참을 설치해준다! 이거는 pycharm 홈페이지가서 자신의 운영체제에 맞게 설치하면 되니 스킵!  (라이센스 문제같은 것은 자신의 회사, 학교계정이 있다면 등록하면 됩니다.)

## 2. 서버 SSH 연결하기

파이참을 설치하고 시작을 하면 다음과 같은 화면이 나오는데 여기서 `New Project`를 눌러 새 프로젝트를 생성해주자.

<img width="912" alt="무제" src="https://user-images.githubusercontent.com/53431568/132128778-531f3e30-e345-4562-b592-a7ecfa444c8c.png">

그럼 또 아래의 화면이 나오는데 맨 위의 Location은 내 로컬 경로를 확인해주고 (나는 헷갈리지 않게 ssh_test 라고 해줌) 아래 Previously configured interpreter을 선택해주고 오른쪽 `...` 버튼 클릭!

<img width="1211" alt="무제" src="https://user-images.githubusercontent.com/53431568/132128905-92c521df-b9c0-427e-9b8c-611d44916323.png">

클릭하면 나오는 화면에서 SSH Interpreter 메뉴로 가주자 그러면 아래 화면이 나온다. 그리고 아까와 같이 또 `...` 버튼을 눌러주자

<img width="963" alt="무제" src="https://user-images.githubusercontent.com/53431568/132128991-dc57e6e2-5964-4141-9c29-9f0ae7c4ad8a.png">


SSH configuration 화면이 나온다. 맨 왼쪽의 `+` 버튼을 눌러서 각각 host, username, password를 입력해준다. 

<img width="944" alt="무제 2" src="https://user-images.githubusercontent.com/53431568/132129250-e4591c78-7cec-4e18-9816-e68ce36f476c.png">

그럼 아래와 같이 생성이 되어있는 것을 확인할 수 있다. 

<img width="840" alt="무제 2" src="https://user-images.githubusercontent.com/53431568/132129333-e6a1fbc9-8653-4535-a1fe-0ecb73648005.png">

만들어진 ssh 를 선택하고 Next 버튼을 누르면 아래와 같이 뜨는데..! 본인이 인터프리터로 사용할 python3 경로를 지정해 주면 됩니다. 저는 서버에서 아나콘다의 가상환경을 사용하므로 가상환경의 bin/python3 를 찾아서 설정 해주었습니다.

<img width="963" alt="무제" src="https://user-images.githubusercontent.com/53431568/132129569-0f1bc4db-06c0-40c9-9d9d-ce6adb805f68.png">

다 설정하고 난후 다시 아래 페이지로 돌아오는데! 이제 내가 pycharm에서 개발을 한다면 어디에 저장할지 `Remote project location` 을 설정해주면 됩니다. 저는 제가 작업하고 있는 폴더 옆에 새 폴더 하나를 생성해서 설정해주었습니다.

<img width="1211" alt="무제" src="https://user-images.githubusercontent.com/53431568/132129672-75880b17-dc36-4ea7-a61c-3bbedee9a6d4.png">

그리고 클릭 "Create" -끝- !!

![무제](https://user-images.githubusercontent.com/53431568/132129876-946a7445-10c5-4315-b568-36ea51f77f09.png)


다시 파이참을 실행시키면 

짜잔! 다음과 같이 서버에 연결된 작업폴더가 미리 준비가 되어있는 것을 확인할 수 있습니다.

<img width="1211" alt="무제 2" src="https://user-images.githubusercontent.com/53431568/132129837-631fd00f-e137-4441-84a6-cbaf8bc9c0d4.png">


이제 맥북으로도 gui 형식으로 서버의 프로젝트들을 작업할 수 있다~!👍🏻👍🏻😍

단지 아쉬운 점은.. 서버에서 작업했었던 이전의 프로젝트들을 불러올 수 없다는 점이 단점인데.. 찾아닌 것이 vscode이다. vscode로 더 간단하게 ssh 연결이 가능하니, 더 간편하고, 자동으로 동기화 되는 것이 필요하신 분은 [vscode ssh 원격접속](https://chaelin0722.github.io/mac/vscode_ssh/) 에서 확인해 보세요!









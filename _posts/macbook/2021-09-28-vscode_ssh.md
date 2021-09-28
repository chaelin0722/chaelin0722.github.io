---
title:  "[mac] vscode에서 ssh 원격접속하기"
excerpt: "원격접속"

categories:
  - mac
tags: [mac, ssh, etc]

last_modified_at: 2021-09-28T11:00:00-05:00
classes: wide
---

맥북쓰는 친구들은 거의 vscode를 사용한다고 한다. 따라쟁이인 나도 한번 따라해보기로 했다 ㅎㅎ🙄🙄

vscode에도 ssh 접속이 가능하다고 해서 더 좋은 것 같다. pycharm 으로 ssh 원격 접속은 가능하지만 pycharm ssh를 설정하기 전에 작업중이던 폴더와 파일들은 공유할 수 없어서 서버에 저장된 내 코드들을 볼 수 없는게 단점이었는데.. 
vscode는 그냥 다 된다 ㅠㅠㅠ 이런.. 왜 이제야 안건지!! ㅋㅋㅋㅋ 얼릉 파이참을 지워버렸다 ㅎㅎ 심지어 설치도 더 편했다..ㅎㅎ

### 1. vscode 설치

[vscode 공식홈페이지](https://code.visualstudio.com/)에 가서 자신의 운영체제에 맞는 걸로 선택해서 설치, 실행 끝! (심지어 설치도 편해..ㅎ)


### 2. Remote Development 설치

vscode를 실행하면 왼쪽에 세로로 메뉴가 나열되어있다. 그 중 제일 아래 메뉴 선택 후 `remote developemnt` 검색 후 클릭 후 설치! 끝!

<img width="1136" alt="remote_dev" src="https://user-images.githubusercontent.com/53431568/135097476-454b33a3-ca83-4828-878f-ee6a161e8575.png">

### 3. configuration에 ssh 주소 등록

`f1`키를 누르고 Remote-SSH: Open SSH Configuration File 클릭!

<img width="1136" alt="무제" src="https://user-images.githubusercontent.com/53431568/135097828-fe6faaef-9b0c-4429-8431-cd17f26ad8bf.png">

아래 와 같은 파일이 나오는데,

~~~
Host Name : username@xxx.xxx.xxx.xxx:xx
Host : xxx.xxx.xxx.xxx
User : username
Port : xx
~~~

이렇게 입력해주면 된다.

<img width="1136" alt="무제 2" src="https://user-images.githubusercontent.com/53431568/135098227-9e3d89f7-a77a-49b9-8d18-ce873cf98ef3.png">



### 4. SSH 연결

이제 ssh를 연결해서 사용해봅시다. 다시 `f1`을 눌러서 Remote-SSH:Connect to Host를 클릭하면 본인이 연결한 ssh 주소가 뜰겁니다! 이제 바로 접속해서 password 입력 후 바로 쓰면 된다!


<img width="1314" alt="무제" src="https://user-images.githubusercontent.com/53431568/135098972-5c7358f0-fe68-4030-9b4b-206a92492e8e.png">


내 프로젝트들도 바로바로 동기화 된다!! 👍👍👍 vscode 쵝오

<img width="1136" alt="무제" src="https://user-images.githubusercontent.com/53431568/135100600-8062a559-2ef0-481a-9b22-a9162745de03.png">

<br>

<div style="width:480px"><iframe allow="fullscreen" frameBorder="0" height="360" src="https://giphy.com/embed/lYv10cDlosFnA0toPb/video" width="480"></iframe></div>
<iframe src="https://giphy.com/embed/MWdOAxxPDEhNKyzXVK" width="480" height="270" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://giphy.com/gifs/ionedigital-yellow-quotes-MWdOAxxPDEhNKyzXVK">via GIPHY</a></p>



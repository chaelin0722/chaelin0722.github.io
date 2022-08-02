---
title:  "[mac] vscode에서 python 코드 relative 상대경로 import 하기"
excerpt: "import relative folder and files"

categories:
  - mac
tags: [mac, etc]

last_modified_at: 2022-08-02T11:00:00-05:00
classes: wide
---

<br>

`from tmp.utils.common import def` 이런식으로 상대경로의 폴더안에 있는 파일의 함수를 불러와 쓰고싶을 때가 있습니다.

하지만 vscode 에서는 그냥 해주지 않는다.. 이 경로를 읽어오지 못하기 때문에 `launch.json`파일에 설정을 해주어야 하는데..!

사실 이 문제때문에 vscode 열심히 쓰다가 힘들어서 pycharm로 갈아탔다가..(박쥐!) 요새 또 pycharm 의 원격접속에 에러가 떠서 vscode로 황급히 수정하면서 이 문제에 다시 직면하기로 하였다.

허헛 🤭 아무튼 사용 방법은 다음과 같습니다!


<br>

### 1. launch.json 생성하기

예를 들어 아래와 같이 폴더와 파일이 구성되어 있다고 가정해보고자 한다.

<img width="336" alt="무제" src="https://user-images.githubusercontent.com/53431568/182481535-194b2f72-931a-49b1-91f7-49a48335a3be.png">

일단 먼저 그냥 실행해보니 역시나 `no module` 에러가 뜬다.

<img width="529" alt="err" src="https://user-images.githubusercontent.com/53431568/182482238-fd59f2c3-dd7a-43c5-9b86-3abe4f558863.png">

그럼 다시, 아래 화면과 같이 디버거 메뉴를 선택해주면, launch.json 만들기 링크가 보이는데 이걸 클릭해주자!

![무제](https://user-images.githubusercontent.com/53431568/182482828-38eca61c-c69b-462f-a7fc-b8d8fe923576.png)

그럼 여기서 주의할건 `모듈`을 선택하여야 한다는 것!

<img width="1078" alt="모듈" src="https://user-images.githubusercontent.com/53431568/182483089-89d1c42a-26b2-4759-9b42-3ea71734e6ce.png">





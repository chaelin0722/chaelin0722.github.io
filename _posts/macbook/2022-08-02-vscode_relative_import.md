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

사실 이 문제때문에 vscode 열심히 쓰다가 힘들어서 pycharm 만 쓰다가.. 요새 또 pycharm 의 원격접속에 에러가 떠서 vscode로 황급히 수정하면서.. 이 문제에 다시 직면하기로 하였다.

허헛 🤭 아무튼 사용 방법은 다음과 같습니다!


<br>

### 1. launch.json 생성하기

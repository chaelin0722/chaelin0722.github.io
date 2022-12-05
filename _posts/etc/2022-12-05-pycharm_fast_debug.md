---
title:  "[pycharm] Fast debugging 파이참 로컬연결시 디버깅 속도 느릴때 빠르게 하기"
excerpt: "pycharm"

categories:
  - etc
tags: [study, pycharm, debugging]
classes: wide

last_modified_at: 2022-12-05T10:40:00-05:00
---

로컬에서 파이참을 켜 ssh로 원격 서버에 접속하여 사용할때 가끔 디버깅 속도가 느려지는 문제가 있다. 

이럴 때, `Gevent compatible` 을 체크하면 됩니다..!

아래 이미지와 같이 Settings -> Build, Execution, Deployment -> Python Debugger 에 들어가면 아래와 같이 나오는데..

여기서 `Gevent compatible` 가 해제되어있다면 체크해주세요..!

![debugging_fast_pycharm](https://user-images.githubusercontent.com/53431568/205564607-854ae61c-1f8d-41b1-900b-b564f4fc6dc6.png)


그러면 디버깅 속도가 개선되는 것을 볼 수 있습니다 ㅎㅎ

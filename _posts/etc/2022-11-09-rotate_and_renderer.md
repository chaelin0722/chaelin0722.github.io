---
title:  "[연구과제] Rotate and Renderer"
excerpt: "설치"

categories:
  - CNN
  - 3D
tags: [Deeplearning, CNN, study]

last_modified_at: 2022-11-09T08:06:00-05:00
---

공용서버 에러 -> /tmp 파일에 권한이 없다는 이유로 코드가 아예 돌아가지 않음

따라서, 개인서버에 구축해둠


아래 경로에 구축완료
~~~
/home/ivpl-d28/Pycharmprojects/Rotate-and-Render/
~~~

demo 명령어
~~~
bash experiments/v100_test.sh
~~~

수정된 부분 `test_multipose.py` 는 multi gpu 를 default 로 코드가 구현되어있다 따라서 single gpu 로 사용하기 위해

~~~
opt.gpu_ids = list(range(0, ngpus - opt.render_thread))
~~~

위의 코드를 아래와 같이 수정해줌

~~~
if ngpus > 1:
    opt.gpu_ids = list(range(0, ngpus - opt.render_thread))
else:
    opt.gpu_ids = [0]
~~~

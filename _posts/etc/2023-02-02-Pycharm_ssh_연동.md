---
title:  "[pycharm] 파이참 ssh 원격 연결 디버깅시 mapping 안될 때 (2021 ver.)"
excerpt: "pycharm"

categories:
  - etc
tags: [study, pycharm, debugging]
classes: wide

last_modified_at: 2023-02-02T10:40:00-05:00
---

최근들어 로컬에서 파이참을 켜 ssh로 원격 서버에 접속하여 사용할때 sync 가 잘 맞지 않는 문제가 계속 발생했다. 나 뿐만 아니라 다른 동료들도 sync 문제 때문에 골머리를 앓고 있던 중,,

가장 최근 버전의 pycharm (다운받을 때 default 로 받아지는 버전! 2023년 2월2일 오늘자 기준으로는 2022년에 만들어지고 23년 1월에 빌드된 버전) 은 mapping설정을 따로 하지 않아도 바로 싱크가 맞는다고 한다..ㅎㅎ

그치만 이미 깔린 버전에서 많은 프로젝트들이 연결이 되어있어서 일단은 에러해결을 1순위로 진행하되 시간이 나면 최신버전으로 갈아 탈 예정이다. ㅎㅎ

해결방법은 다음과 같다!

### (1) deployment -> ssh type으로 변경!

기존에 있던 원격 서버에서 작업하던 것을 로컬 에다 다운받아 mapping을 하여서 로컬에서 작업을 하여도 원격지의 똑같은 디렉토리에서 바로바로 update 가 되도록 사용하고 있었는데,

이때 tools -> deployment 에서 sync 작업을 통해 해주고 있었다.

하지만, 이때 deployment type 으로 된 remote python interpreter 를 `ssh type 으로 변경`해주면 update가 바로 되고 sync도 맞춰지는 것을 확인하였다! (그동안 이거 deployment 타입으로 하고 sync만 맞춰줘도 잘 작동했었는데 갑자기 왜 안되는지는 의문이다.)

아무튼 ssh type 으로만 변경해줍시다~!

아래 그림과 같이 `settings-> project -> python interpreter -> 변경!`

![pycharm_sync](https://user-images.githubusercontent.com/53431568/216233993-6b35b523-0040-4beb-b37a-a2e0dcc5f4d8.png)


ssh type으로 변경했다면 다음과 같이 아이콘이 달라진 모습을 확인할 수 있다!

![pycharm_sync_2](https://user-images.githubusercontent.com/53431568/216234239-a4c059ee-c38d-4391-a0c4-8fd589e362a4.png)


하지만.. sync 작업이 매우 번거롭고 시간도 걸리니.. 그냥 최신 버전 파이참 쓰세요.. 나도 얼른 바꿔야지 ㅎㅎ


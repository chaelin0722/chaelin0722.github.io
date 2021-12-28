---
title:  "[kaggle] kaggle submission error, 캐글 대회 노트북 제출 오류"
excerpt: "kaggle disable internet"

categories:
  - etc
  - kaggle

tags: [etc, kaggle, tensorflow, dnn, cnn]
classes: wide

last_modified_at: 2021-12-28T08:06:00-05:00
---

## 캐글 대회 노트북 제출 오류

캐글 대회는 처음이라 학습부터 제출까지 버벅거리는 중 😅😅😅😅

어찌저찌 학습과 추론까진 끝냈지만 submission 에서 자꾸 에러가 났다.

내가 참여한 대회는 object detection 분야에서 tensorflow를 사용해 불가사리를 detect 하는 대회인데..

<img width="814" alt="무제" src="https://user-images.githubusercontent.com/53431568/147571147-77693540-7bb6-4a8d-9a98-17f88bcc4e1a.png">

왠지 모르게 계속 에러가 났다.. 나름 vote 수 많이 받으신 분 코드 참고해서 했는데..!


<img width="691" alt="무제" src="https://user-images.githubusercontent.com/53431568/147572821-a2a98828-ac97-4294-8d6e-776b7022200a.png">


위의 이미지와 같은 에러가 떴다!

열심히 구글링 해보니 `internet`을 `disable` 시키고 저장하라고 한다.  알고보니 이 대회는 인터넷을 쓰지 않고 제출하는 것이 규칙이었다. 

인터넷을 끄니 submission.csv 파일이 만들어 지지 않는데.. 코드를 다시 분석해보니 어떤 부분에서 http 주소를 통해 데이터를 참조해서 쓰고있었다(인터넷 사용하는 코드였음).

그래서 그 데이터를 다시 kaggle notebook에 upload 를 한 후, internet을 끄고! 

<img width="309" alt="무제" src="https://user-images.githubusercontent.com/53431568/147572274-f188cc42-79bd-4a73-83a4-d31d0d497a76.png">&nbsp;&nbsp;&nbsp;➡️ ➡️ ➡️&nbsp;&nbsp;&nbsp;
<img width="299" alt="무제 2" src="https://user-images.githubusercontent.com/53431568/147572320-e6dcff5d-13ee-4519-aef5-0623cc7925ce.png">


`Advanced setting` -> `save output for this version` 으로 설정한 후! `save` 해준다.

<img width="698" alt="advanced_setting" src="https://user-images.githubusercontent.com/53431568/147572125-91ffee4d-2f37-4e02-9d5a-9b31c2a450b5.png">

<img width="695" alt="advanced_setting_2" src="https://user-images.githubusercontent.com/53431568/147572009-f65f8250-5bb9-4a7a-9d2d-aed4bad699ec.png">


성공!

![success](https://user-images.githubusercontent.com/53431568/147572518-7c4aafec-64cb-4429-a02c-32c3ed27c4da.png)





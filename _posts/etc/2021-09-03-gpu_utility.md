---
title:  "[GPU] GPU 사용량 최대화 하기(GPU Utility) - 분산학습을 중심으로"
excerpt: "CPU와 GPU 사용량 체크하고 문제점 파악해 보기"

categories:
  - etc

tags: [Deeplearning, etc, training, GPU, NVIDIA, CUDA]
use_math: true
classes: wide

last_modified_at: 2021-09-031T08:06:00-05:00
---

## 

![parallel](https://user-images.githubusercontent.com/53431568/131972454-0856579d-5143-4b67-b1bd-1714dc9d6dd2.PNG)


![htop](https://user-images.githubusercontent.com/53431568/131972458-3cdcfd65-a0db-4676-8b63-3a7a25534a2e.PNG)


![processor_list](https://user-images.githubusercontent.com/53431568/131972460-6d8bd291-3848-4aa1-a8a6-307e5a87de0f.PNG)


![processor_15](https://user-images.githubusercontent.com/53431568/131972463-c7a8d385-2c67-4e0a-8161-7b337051926a.PNG)


![utility](https://user-images.githubusercontent.com/53431568/131972465-6550cf86-c6a4-4108-b689-e5249fdd51a2.PNG)



최종 결과

여전히 왔다갔다하지만 80~100을 자주 찍는 것에 의의를 두기로 했다.

학습 레이어도 다르고 사용하는 데이터셋도 다르기 때문에 완전한 99% 이상을 차지하기 위해선 다른 솔루션이 필요하다고 생각된다. 

*솔루션은 계속 찾아볼 것이며 찾는다면 추후 포스팅으로 기록하겠습니다.

![image](https://user-images.githubusercontent.com/53431568/131971994-475fea05-630f-4784-b77c-68665ec3f1f2.png)

현재 학습은 GPU 1 번 하나만 사용하는데 보면 저 온도를 보면 알다시피 77도.. 게다가 하나만 돌려도 옆에있는 다른 GPU에도 무리가 가서 같이 온도가 올라가는 현상이 발생하였다.

그래서 두개를 동시에 학습시키면 내 GPU가 터질것 같아서.. 일단 하나로 하기로 하였다. ㅎㅎ 수냉식 쿨러.. 사줘...😵😵



#### 참고

[1] [https://towardsdatascience.com/efficient-pytorch-part-1-fe40ed5db76c](https://towardsdatascience.com/efficient-pytorch-part-1-fe40ed5db76c)

[2] [https://medium.com/daangn/pytorch-multi-gpu-%ED%95%99%EC%8A%B5-%EC%A0%9C%EB%8C%80%EB%A1%9C-%ED%95%98%EA%B8%B0-27270617936b](https://medium.com/daangn/pytorch-multi-gpu-%ED%95%99%EC%8A%B5-%EC%A0%9C%EB%8C%80%EB%A1%9C-%ED%95%98%EA%B8%B0-27270617936b)

[3] [https://ainote.tistory.com/14](https://ainote.tistory.com/14)













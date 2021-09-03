---
title:  "[GPU] GPU 사용량 최대화 하기(GPU Utility) - 분산학습을 중심으로"
excerpt: "CPU와 GPU 사용량 체크하고 문제점 파악해 보기"

categories:
  - etc

tags: [Deeplearning, etc, training, GPU, NVIDIA, CUDA]
use_math: true
classes: wide

last_modified_at: 2021-09-03T08:06:00-05:00
---


드디어 내 서버에도 GPU가 2개가 생겼다~!! 그동안 한개로 학습시키느라 힘들고 서러웠는데.. 드디어 2개!!

하지만, 이제 GPU가 2개나 있으니까 학습도 2배로 빨라지겠지? 라고 생각한건 내 오산이었다 ㅎㅎ

게다가 GPU util을 90% 이상으로 끌어올려야 제대로 활용하는거라고 연구실 선배가 말해주었다..!! 전혀 그런쪽으로 생각하고 있지못해 매우 당황스러웠지만, 

생각해보면 그동안 책에서, 교수님이, 개발자들이 했던 말을 생각해보니 "**`한정된 자원으로 최대를 이끌어내는 것이 프로그래머의 목표`**"라는 말이 떠올랐다. 비록 GPU를 몇 백개씩 쓰는 전문 개발자는 아니지만 2개가 생겼으니 이제 메모리 사용이라던가 파일 입출력, cpu 연산비용 등등을 따져서 개발하는 개발자로 거듭나고자 한다. 😁😁


이제 GPU를 최대한 활용하는 방법을 알아보러 가자~ gogo~ 🚗~

<br>


## 1. 내 문제점 파악하기

현재 GPU하나로 학습을 돌리면 보이는 내 GPU 상태.. 아래 명령어로 실시간 GPU상황 실시간으로 볼 수 있다.

~~~ssh
watch -n -1 nvidia-smi
~~~

오른쪽의 Volatile GPU_Util을 보면 사용량이 매우 저조한 것을 볼 수 있다.. 😅😅

![1](https://user-images.githubusercontent.com/53431568/131976128-1f9a45e0-11ab-461b-850e-8ae692bbb34f.PNG)

음,, 대환장파티가 따로 없다. 

![캡처](https://user-images.githubusercontent.com/53431568/131976132-92577760-6ec8-4c34-8b2e-d7bcd956955d.PNG)


위와 같은 문제는 GPU Bottleneck 이라고 하는데,

컴퓨터 내부에서 돌아가는 학습 프로세스를 아주 간단히 설명해보자면 **CPU**에서 데이터를 전처리해 batch로 만들면, **GPU**가 학습을 하는 프로세스이다.

그렇기 때문에 GPU 학습량이 저조한 이유는 GPU에서 한 배치(batch)의 처리를 다 끝냈는데 CPU에서 다음 batch가 아직 안만들어진 거라고 생각하면 된다. 따라서 데이터 로드나, CPU에서 하는 연산량이 매우 큰 것으로 예상한다.


#### 따라서 내가 해야할 일은

> 1. CPU가 할 일을 최적화하기
>  
> 2. GPU 2개 모두 사용하기


먼저, cpu의 경우 `htop` 명령어를 이용하여 알아보았다.

#### htop 이란?

실시간 모니터링 툴로 코어 갯수를 확인해 각 프로세스 정보를 디테일하게 모니터링이 가능하다. 

따로 설치가 필요하며 설치 명령어는

~~~ssh
sudo yum install htop
~~~

이제 설치되었으니 학습을 실행시키고 확인해보자!

![htop](https://user-images.githubusercontent.com/53431568/131972458-3cdcfd65-a0db-4676-8b63-3a7a25534a2e.PNG)


![parallel](https://user-images.githubusercontent.com/53431568/131972454-0856579d-5143-4b67-b1bd-1714dc9d6dd2.PNG)



![processor_list](https://user-images.githubusercontent.com/53431568/131972460-6d8bd291-3848-4aa1-a8a6-307e5a87de0f.PNG)


![processor_15](https://user-images.githubusercontent.com/53431568/131972463-c7a8d385-2c67-4e0a-8161-7b337051926a.PNG)


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













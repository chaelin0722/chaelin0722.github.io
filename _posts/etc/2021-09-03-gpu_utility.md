---
title:  "[GPU] GPU 사용량 최대화 하기(GPU Utility) - Pytorch 분산학습을 중심으로"
excerpt: "CPU와 GPU 사용량 체크하고 문제점 파악해 보기"

categories:
  - etc

tags: [Deeplearning, etc, training, GPU, NVIDIA, CUDA, pytorch]
use_math: true
classes: wide

last_modified_at: 2021-09-03T08:06:00-05:00
---

<br>

드디어 내 서버에도 GPU가 2개가 생겼다~!! 그동안 한개로 학습시키느라 힘든 지난날을 생각하니 눙물;;

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


위와 같은 문제는 `GPU Bottleneck` 이라고 하는데,

컴퓨터 내부에서 돌아가는 학습 프로세스를 아주 간단히 설명해보자면 

👉 **CPU**에서 데이터를 전처리해 batch로 만들면, **GPU**가 학습을 하는 프로세스이다.

그렇기 때문에 GPU 학습량이 저조한 이유는 `GPU에서 한 배치(batch)의 처리를 다 끝냈는데 CPU에서 다음 batch가 아직 준비가 되지 않았다`고 생각하면 된다. 따라서 `데이터 로드나, CPU에서 하는 연산량이 매우 큰` 것으로 예상한다.


#### 따라서 내가 해야할 일은

> 1. CPU가 할 일을 최적화하기
>  
> 2. GPU 2개 모두 사용하기


먼저, CPU의 경우 `htop` 명령어를 이용하여 알아보았다.

**htop**은 실시간 모니터링 툴로 코어 갯수를 확인해 각 프로세스 정보를 디테일하게 모니터링이 가능하다. htop은 따로 설치가 필요하며 설치 명령어는,

~~~ssh
sudo yum install htop
~~~

이제 설치되었으니 학습을 실행시키고 확인해보자!

![htop](https://user-images.githubusercontent.com/53431568/131972458-3cdcfd65-a0db-4676-8b63-3a7a25534a2e.PNG)


위 그림을 보면 맨 위에는 16개의 CPU 코어를 프로세스가 점유하고 있는 비율이 나타나있고, 각 bar는 해당 코어의 사용된 %를 표현한다. (Mem: memory, Swap: 스왑사용량)


**코어**마다 색이 들어간 | 작대기(?)의 색이 의미하는 바는 다음과 같다.

> 파랑: low-priority
> 
> 초록: normal
> 
> 빨강: kernel
> 
> 하늘: virtualiz

한편, **memory**의 | 색깔은 의미하는 바가 다르다

> 초록: 사용된
> 
> 파랑: 버퍼
> 
> 노랑: 캐쉬

#### swap

> 빨강: 사용됨


### 이제 내 CPU 상태를 해석 해보자..!

지금 프로세스 상태가 리스트 형식으로 쭉 나열되어있는데 현재 내가 돌리고 있는 파이썬 파일 프로세스는 가장 위쪽에 있고 CPU%가 82로 매우 높다는 것을 알고 있다.

(바로 왼쪽에 초록색으로 **R**로 표시되어있는데 이는 "running"상태, **S**는 "sleeping", **T**는 "traced/stopped", **D**sms disk sleep, **Z**는 zombie를 의미한다.)

82%가 심지어 99%까지 올라가는 것을 확인 후((😭😭😭)) dataloader 쪽을 살펴보기로 하였다.


<br>

## 2. CPU 솔루션

   &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 내가 사용한 방법 : `num_workers`, 와 `DistributedSampler` 

### 1. num_workers 란?

num_workers는 처리할 프로세스에 cpu코어를 할당해주는 것으로 자신의 CPU가 가진 최대 코어까지 파라미터 값으로 넣을 수 있다. 하지만 데이터 프로세싱에 무조건 많은 CPU코어를 할당해준다고 꼭 좋은 것만은 아니다.

코어 개수는 물리적으로 한정되어 있고 모든 코어를 전부 데이터 로드에 사용하게 된다면 다른 부가적인 처리에 딜레이가 생길 수 있다. 따라서, 모델에 가장 적합한 num_workers 수치를 찾아서 주는 것이 가장 좋은 방법이다!

위에 htop으로 코어 갯수를 파악할 수 있었지만 다른 명령어로도 코어를 파악할 수 있다. 

~~~ssh
grep -i processor /proc/cpuinfo

grep -i processor /proc/cpuinfo | wc -l
~~~

<img width="562" alt="무제" src="https://user-images.githubusercontent.com/53431568/131984223-05e9f106-8b6a-42c0-846e-1a4a6e0c7384.png">

나는 총 16개의 코어가 있지만 2개~10개까지 시행해본 결과 4가 적당한것 같아서 num_workers=4 로 지정해 주었다.


### 2. DistributedSampler 

DistributedSampler은 데이터셋을 쪼개어 각기 다른 GPU로 처리하게 하는 함수이다. 

각각의 Sampler는 전체 데이터를 GPU의 개수로 나눈 부분 데이터에서만 데이터를 샘플링한다. 부분 데이터를 만들기 위해 전체 데이터셋 인덱스 리스트를 무작위로 섞은 다음, 그 인덱스 리스트를 쪼개서(데이터셋을 num_replicas값으로 나뉘어 지는지 확인을 한 후, 나눌 수 없다면 샘플들이 추가해 쪼갠 후 처리) 각 GPU Sampler에 할당한다. 또, epoch 마다 각 GPU sampler에 할당되는 인덱스 리스트는 다시 무작위로 달라진다.

사용 방법은 다음과 같다. 

DataLoader가 입력을 각 프로세스에 전달하기 위해서 다음처럼 DistributedSampler를 사용한다! DistributedSampler는 DistributedData과 함께 사용해야 하며 DDP사용 방법은 
[아래](#1-dataparallel)에 참고!


사용 방법은 각자 정의한 dataset를 DistributedSampler로 감싸주고 DataLoader에서 sampler에 인자로 넣어주면 된다. 또, sampler 자체에서 shuffle기능이 있어서 그런지 `shuffle=False`로 바꿔 주어야 에러가 발생하지 않는다..! 주의!⭐️ 그 다음엔 평소에 DataLoader를 사용하듯이 똑같이 사용하면 끝! 간단하네!✌️✌️

이 둘 모두를 적용한 코드는 아래와 같다. 만약 본인이 test_dataset, validation_dataset도 있다면 마찬가지로 해주면 되겠죠?! ㅎㅎ

<script src="https://gist.github.com/chaelin0722/b237a65ef0f61b89dc5048dc9dba6b01.js"></script>


<br>


## 3. GPU 솔루션

 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 내가 사용한 방법 : pytorch의 `DataParallel`와 `Apex의 DistributedDataParalle`
 
 ### 1. DataParallel
 
pytorch에서 제공하는 함수로 GPU를 내가 지정한대로 여러개에 사용해 분산학습을 가능케 한다.

내 모델을 DataParallel로 감싸주기만 하면 된다.

<br>

### 2. Apex DistributedDataParallel

pytorch에서 제공하는 DistributedDataParallel도 있지만 이것의 큰 단점이 메모리를 불균형하게 할당한다는 것이다. 그렇게 되면 분산학습의 의미가 없어진다고 생각해 아직까지 그런 에러는 없는 Apex로 사용하기로 했다.

apex는 apex에서 DistributedDataParallel을 import해서 사용한다. 

설치방법은 아래와 같다. 보면 두가지 버전의 설치가 있는데 나는 파이썬으로 코드를 돌리기 때문에 2번으로 설치해 주었다. 

~~~ssh
git clone https://github.com/NVIDIA/apex
cd apex

## 1. for python and c
pip install -v --disable-pip-version-check --no-cache-dir --global-option="--cpp_ext" --global-option="--cuda_ext" ./

## 2. only for python
pip install -v --disable-pip-version-check --no-cache-dir ./

~~~

이렇게 설치 후, dist.init_process_group을 통해 각 GPU 마다 분산 학습을 위한 초기화를 꼭 실행해주어야 한다! 초기화 하지 않으면 실행안됨! PyTorch의 docs를 보면 multi-GPU 학습을 할 경우 backend로 nccl을 사용하라고 나와있기 때문에 nccl을 사용하였고,  init_method에서 FREEPORT에 사용 가능한 port를 지정해주면 된다. 나는 그냥 큰 숫자를 써주었다. 마지막으로 DDP로 model을 감싸줍니다. 

모두 적용된 코드는 아래와 같다.

<script src="https://gist.github.com/chaelin0722/f23cbb174827ca8b75031a8e87052197.js"></script>

<br>

## 최종 결과


<<<< gpu 두개로 분산학습되는 이미지 추가하기



여전히 왔다갔다하지만 80%~100%을 자주 찍는 것에 의의를 두기로 했다.

학습 레이어도 다르고 사용하는 데이터셋도 다르기 때문에 완전한 99% 이상을 차지하기 위해선 다른 솔루션이 필요하다고 생각된다. 

*솔루션은 계속 찾아볼 것이며 찾는다면 추후 포스팅으로 기록하겠습니다.

![image](https://user-images.githubusercontent.com/53431568/131971994-475fea05-630f-4784-b77c-68665ec3f1f2.png)

현재 학습은 GPU 1 번 하나만 사용하는데 보면 저 온도를 보면 알다시피 77도.. 게다가 하나만 돌려도 옆에있는 다른 GPU에도 무리가 가서 같이 온도가 올라가는 현상이 발생하였다.

그래서 두개를 동시에 학습시키면 내 GPU가 터질것 같아서.. 일단 하나로 하기로 하였다. ㅎㅎ 수냉식 쿨러.. 사줘...😵😵



#### 참고

[1] [https://towardsdatascience.com/efficient-pytorch-part-1-fe40ed5db76c](https://towardsdatascience.com/efficient-pytorch-part-1-fe40ed5db76c)

[2] [https://medium.com/daangn/pytorch-multi-gpu-%ED%95%99%EC%8A%B5-%EC%A0%9C%EB%8C%80%EB%A1%9C-%ED%95%98%EA%B8%B0-27270617936b](https://medium.com/daangn/pytorch-multi-gpu-%ED%95%99%EC%8A%B5-%EC%A0%9C%EB%8C%80%EB%A1%9C-%ED%95%98%EA%B8%B0-27270617936b)

[3] [https://ainote.tistory.com/14](https://ainote.tistory.com/14)













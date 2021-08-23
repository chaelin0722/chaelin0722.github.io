---
title:  "[Ubuntu] 우분투 세팅과 아나콘다 환경설정"
excerpt: "성공"

categories:
  - study
  - linux
  - ubuntu
  - etc
tags: [linux, study, etc, ubuntu]
classes: wide

last_modified_at: 2021-08-23T10:40:00-05:00
---

### 준비물

우분투 부팅 usb <우분투 버전 : ubuntu-18.04>

### 설치순서

> [1. 우분투(Ubuntu) 설치](#1.-우분투 설치-(18.04lt)) 
> 
> [2. 하드디스크 마운트](#2.-하드디스크-마운트)
> 
> 3. SSH 설정
> 
> 4. NVIDIA driver 설치
> 
> 5. CUDA toolkit 과 cuDNN 설치
> 
> 6. Anaconda 설치와 가상환경 만들기

### 준비물 

> 우분투 부팅 usb (우분투 버전 : **ubuntu-18.04LTS**)

<br>

## 1. 우분투 설치 (18.04LT)

1. 컴퓨터를 끄고 우분투 usb를 꽂은 채 컴퓨터를 켠다

2. F2 /F12/ delete 등 키를 이용해서 bios 모드로 진입

3. bios 모드에서 boot priority의 1순위를 usb로 바꿔주고 save and exit !

이제 재부팅이 될 것이다. 부팅 후에 나오는 화면에서 설정해주어야 할 것들!

#### 먼저,
- install 버튼 클릭 전에 `한글로 설정`!! -> install ubuntu 버튼 클릭

- 키보드는 영어로 설치!

- 나머지는 default 로 진행함.

- hdd 와 ssd 중 더 빠른 ssd 에 os 설치! warning 문구도 그냥 continue 해줌

- seoul 지도 클릭

- 우분투가 다 설치되면 다시 재시작한다는 말이 나오는데 이때 `usb를 뽑고 재부팅`해야한다.

- 재부팅 후 네트워크 연결을 마친 후 우분투의 환경을 설치하도록 한다

## 2. 하드디스크 마운트

만약, 새로운 하드디스크를 추가했다면 사용하기 위해서 마운트를 해주어야 한다. 이 작업을 통해서 HDD를 하나의 디렉토리로 사용할 수 있다.

1. 마운트 시킬 경로를 생성해준다. 나는 내 이름과 디스크의 용량으로 이름을 지어주었다.

~~~linux
sudo mkdir /chaelin_4TB
~~~

2. 부팅시 자동마운트

~~~linux
sudo vim /etc/fstab
~~~

## 3. SSH 설정

원격접속을 하기위해 SSH를 설정해준다.

## 6.  CUDA 설치

1. 먼저 혹시 모를 NVIDIA가 설치되어있을 가능성을 배제하기 위해 기존 CUDA를 지워준다.

~~~linux
sudo rm /etc/apt/sources.list.d/cuda*
sudo apt remove --autoremove nvidia-cuda-toolkit
sudo apt remove --autoremove nvidia-*
~~~


2. CUDA PPA를 내 시스템에 세팅하기

~~~linux
sudo apt update
sudo add-apt-repository ppa:graphics-drivers
sudo apt-key adv --fetch-keys  http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/7fa2af80.pub
sudo bash -c 'echo "deb http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64 /" > /etc/apt/sources.list.d/cuda.list'
sudo bash -c 'echo "deb http://developer.download.nvidia.com/compute/machine-learning/repos/ubuntu1804/x86_64 /" > /etc/apt/sources.list.d/cuda_learn.list'
~~~

3. CUDA10.1 버전으로 설치하기

~~~linux
sudo apt update
sudo apt install cuda-10-1
sudo apt install libcudnn7
~~~

4. CUDA 경로 설정해주기

`.profile` 파일에 특정 경로를 설정해주는데, 아래 명령어로 열고..

~~~linux
sudo vi ~/.profile
~~~

아래 내용을 복붙해서 맨 마지막에 추가해준다.

~~~linux
if [ -d "/usr/local/cuda-10.1/bin/" ]; then
    export PATH=/usr/local/cuda-10.1/bin${PATH:+:${PATH}}
    export LD_LIBRARY_PATH=/usr/local/cuda-10.1/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
fi
~~~

설치가 완료되었다.💗

<br>

### CUDA가 잘 설치되었는지 확인!

~~~linux
nvcc --version

혹은
nvcc -V
~~~

![image](https://user-images.githubusercontent.com/53431568/130415153-c20cbf8a-5b60-46d4-a584-569bc11f88cc.png)

~~~linux
nvidia-smi
~~~

#### gpu가 2개 인 것을 확인!

![image](https://user-images.githubusercontent.com/53431568/130414166-ca4701c7-f11f-4318-b8c9-8a58bafe9be6.png)

> 💡 vcc --version 또는 nvcc -V 이 안먹힐 때!
> ~~~linux 
> sudo apt install nvidia-cuda-toolkit
> ~~~

#### libcudnn 확인
~~~linux
/sbin/ldconfig -N -v $(sed ‘s/:/ /’ <<< $LD_LIBRARY_PATH) 2>/dev/null | grep libcudnn
~~~

## 

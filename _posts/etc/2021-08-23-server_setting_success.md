---
title:  "[Ubuntu] 우분투 세팅과 아나콘다 환경설정"
excerpt: "성공"

categories:
  - etc
tags: [linux, study, etc, ubuntu]
classes: wide

last_modified_at: 2021-08-23T10:40:00-05:00
---

### 준비물

우분투 부팅 usb <우분투 버전 : ubuntu-18.04>

<br>

### 설치순서

> 1. [우분투(Ubuntu) 설치](#1-우분투-설치-(18-04lt)) 
> 
> 2. [하드디스크 마운트](#2-하드디스크-마운트-하기)
> 
> 3. [SSH 설정](#3-ssh-설정)
> 
> 4. [NVIDIA driver 설치](#4-nvidia-driver-설치)
> 
> 5. [CUDA toolkit 과 cuDNN 설치](#5-cuda-toolkit-과-cudnn-설치)
>  
> 6. [Anaconda 설치와 가상환경 만들기](#6-anaconda-설치와-가상환경-만들기)

<br>

### 세팅환경

> Ubuntu18.04
> 
> NVIDIA Driver Version 470.57.02
> 
> Anaconda3
> 
> Cuda 10.1
> 
> libcudnn 7.6.5
> 
> python3.7




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


우분투가 다 설치되었다면 일단 기본적인 패키지를 설치하는 방법은 다음과 같다

~~~bash
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install vim
~~~

<br>

## 2. 하드디스크 마운트 하기

만약, 새로운 하드디스크를 추가했다면 사용하기 위해서 마운트를 해주어야 한다. 이 작업을 통해서 HDD를 하나의 디렉토리로 사용할 수 있다.

마운트 순서! (2TB 이상일때! 2TB 이하라면 아래 [mount 하는 방법!](https://seongkyun.github.io/others/2019/03/05/hdd_mnt/) 참고하기)


1. 파티션 생성

~~~bash
sudo parted /dev/sdb    
~~~

> 💡 단, 여기서 본인이 가진 파티션이름이 sda, sdb, sdc, sdx 인지 확인하고 설정해주기!

2. `mklabel` 입력 후 `gpt`
   => 내부 데이터가 모두 사라진다는 메세지가 출력 된다.

3. `yes` 입력

4. `unit GB` 입력하여 단위를 변환힌다.

5. `print` 입력하여 용량 확인

6. 파티션 나누기

~~~bash
mkpart primary 0GB /*본인용량*/GB 

# 내 경우엔, 

mkpart primary 0GB 4001GB
~~~

7. `q 입력`하여 커맨드로 돌아옴

8. 파티션 포맷 `mkfs.ext4 /dev/sdb`

9. 마운트 시킬 경로를 생성해준다. 나는 내 이름과 디스크의 용량으로 이름을 지어주었다.

~~~bash
sudo mkdir /chaelin_4TB
~~~

10. UUID 확인

~~~bash
sudo blkid
~~~

다음을 입력하여 UUID확인 및 복사하기!

11. 마운트 정보 추가 및 부팅 시 자동 마운트 설정

~~~bash
sudo vim /etc/fstab
~~~

 - 맨 아랫줄에 다음 내용을 추가하고 :wq 저장
 
 ~~~bash
 UUID=복사한UUID /마운트 경로 ext4 defaults 0 0
 
# 내 경우에는,
 
 UUID=583eb4bb-6f91-4634-b6f3-088157ae2010 /chaelin_4TB ext4 defaults 0 0
 ~~~

12. 마운트!

~~~bash
sudo mount -a
~~~

확인!

~~~
df -h
~~~

<br>

## 3. SSH 설정

원격접속을 하기위해 SSH를 설치, 설정해준다.

~~~bash
sudo apt-get install openssh-server -y 
sudo vim /etc/ssh/sshd_config # SSH 설정 파일 열기

---
# Port 22 
---
# 이렇게 되어있는 부분의 주석을 지우고 원하는 포트 번호를 써준다. 만약 그대로 22를 쓴다면 수정하지 않는다.

sudo service ssh restart # SSH 재시작
~~~

이제 다른 서버 터미널에서 ssh로 원격접속할 경우 된다면 끝!

<br>

## 4. NVIDIA driver 설치

nvidia driver는 추천하는 권장 드라이버로 자동설치한다.

~~~bash
sudo ubuntu-drivers devices # 설치가능한 드라이버 버전 확인
sudo ubuntu-drivers autoinstall 
~~~

만약 원하는 버전으로 설치하고 싶다면

~~~ bash
sudo apt install nvidia-driver-4** 
~~~
이런식으로 설치한다!

<br>

마지막으로 재부팅 후 설치확인!

~~~bash
sudo reboot

nvidia-smi 
~~~

#### gpu가 2개 인 것을 확인!

~~~bash
nvidia-smi
~~~

![image](https://user-images.githubusercontent.com/53431568/130414166-ca4701c7-f11f-4318-b8c9-8a58bafe9be6.png)


<br>

## 5. CUDA toolkit 과 cuDNN 설치

1. 먼저 혹시 모를 NVIDIA가 설치되어있을 가능성을 배제하기 위해 기존 CUDA를 지워준다.

~~~bash
sudo rm /etc/apt/sources.list.d/cuda*
sudo apt remove --autoremove nvidia-cuda-toolkit
sudo apt remove --autoremove nvidia-*
~~~


2. CUDA PPA를 내 시스템에 세팅하기

~~~bash
sudo apt update
sudo add-apt-repository ppa:graphics-drivers
sudo apt-key adv --fetch-keys  http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/7fa2af80.pub
sudo bash -c 'echo "deb http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64 /" > /etc/apt/sources.list.d/cuda.list'
sudo bash -c 'echo "deb http://developer.download.nvidia.com/compute/machine-learning/repos/ubuntu1804/x86_64 /" > /etc/apt/sources.list.d/cuda_learn.list'
~~~

3. CUDA10.1 버전으로 설치하기

~~~bash
sudo apt update
sudo apt install cuda-10-1
sudo apt install libcudnn7
~~~

4. CUDA 경로 설정해주기

`.profile` 파일에 특정 경로를 설정해주는데, 아래 명령어로 열고..

~~~bash
sudo vi ~/.profile
~~~

아래 내용을 복붙해서 맨 마지막에 추가해준다.

~~~bash
if [ -d "/usr/local/cuda-10.1/bin/" ]; then
    export PATH=/usr/local/cuda-10.1/bin${PATH:+:${PATH}}
    export LD_LIBRARY_PATH=/usr/local/cuda-10.1/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
fi
~~~

설치가 완료되었다.

<br>

### CUDA가 잘 설치되었는지 확인!

~~~bash
nvcc --version

# 혹은
nvcc -V
~~~

확인!

![image](https://user-images.githubusercontent.com/53431568/130415153-c20cbf8a-5b60-46d4-a584-569bc11f88cc.png)

> 💡 nvcc --version 또는 nvcc -V 이 안먹힐 때!
> ~~~bash
> sudo apt install nvidia-cuda-toolkit
> ~~~

#### libcudnn 확인

~~~bash
/sbin/ldconfig -N -v $(sed ‘s/:/ /’ <<< $LD_LIBRARY_PATH) 2>/dev/null | grep libcudnn
~~~

<br>

## 6. Anaconda 설치와 가상환경 만들기

먼저, 아나콘다 공식홈페이지 [Anaconda]에 들어가서 가장 최신버전 혹은 자신이 원하는 버전을 다운받아준다.

나는 wget을 이용해 리눅스 환경에서 해주었다. 


~~~bash
wget https://repo.continuum.io/archive/Anaconda3-2021.11-Linux-x86_64.sh
~~~

위의 명령어 대로 설치해주되, anaconda 버전은 [아나콘다 archive 페이지](https://repo.anaconda.com/archive/)에 가서 맘에 드는 이름으로 변경하면 된다. 


다운받은 경로로 돌아가서 anaconda를 실행해준다! (대부분 `~/Downloads` 에 있다.)

~~~bash
bash Anaconda3-2021.11-Linux-x86_64.sh
~~~

엔터 많이 치고 'yes' 쓰고 엔터!


경로 추가하고 실행하기

~~~bash
sudo vim ~/.bashrc
# 마지막 줄에 export PATH=~/anaconda3/bin:~/anaconda3/condabin:$PATH 추가 후 저장

source ~/.bashrc
~~~

아나콘다 설치 확인!

~~~bash
conda --version
~~~

> 💡 만약 conda가 실행되지 않는다면...
> 
> 1. 경로설정의 문제
> 
> 2. `source ~/.bashrc` 를 실행 안함..
> 
> 의 문제인 경우가 대부분이므로..! 일단 확인하자! 그래도 안된다면 아래를 실행하자


`~cd` 로 홈디렉토리로 돌아가서 `source ~/anaconda3/etc/profile.d/conda.sh ` 실행한 후 다시 `conda activate ~`

<br>

#### 아나콘다 가상환경 만들기

~~~bash
conda create -n 가상환경이름      # 생성
conda activate 가상환경이름       # 연결
conda deactivate               # 연결끊기
~~~

여기서 잠깐! 만약 본인이 자동으로 콘다가 실행되게 하고 싶다면, (가상환경 이름이 앞에 뜨는 것! 아래 예시) 아래 명령어처럼 true 인자를 던져주자

![image](https://user-images.githubusercontent.com/53431568/149618657-167eab58-0b6e-47be-bf26-e2239cf67754.png)

~~~
conda config --set auto_activate_base true
conda init
~~~

한 후, 세션을 나왔다가 다시 실행하면 된다.

<br>

없애고 싶다면 반대로 false!

~~~
conda config --set auto_activate_base false
conda init
~~~


#### 그 외.. 파이참 프로페셔널 (pycharm-professional) 깔기

서버에서 gui 로 개발하기 위해서 깔아준다..!


나는 학교 계정이 있으니까 `pycharm-professional`로 깔아 주었다. 그냥 무료버전은 pycharm-community

~~~
sudo snap install pycharm-professional --classic
~~~

실행 명령어는 `pycharm-professional`..!

<br><br>

기나긴 여정의 끝😆😆 여기까지 수고 많으셨습니다!

이제 개발할 일만 남았습니당~💗💗


--- 2021년 11월 20일 수정됨

<br><br>

#### 참고

[1] [mount참고](https://seongkyun.github.io/others/2019/03/05/hdd_mnt/)


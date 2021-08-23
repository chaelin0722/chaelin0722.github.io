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

### 환경 조건
준비물 : 우분투 부팅 usb 우분투 버전 : ubuntu-20.01LT




### 환경 조건
준비물 : 우분투 부팅 usb 우분투 버전 : ubuntu-18.04

1. 컴퓨터를 끄고 우분투 usb를 꽂은 채 컴퓨터를 켠다
2. F2 /F12/ delete 등 키를 이용해서 bios 모드로 진입
3. bios 모드에서 boot priority의 1순위를 usb로 바꿔주고 save and exit !

install ubuntu 버튼 클릭

korean / korean(101/104 key compatible 을 선택해준다)

나머지는 default 로 진행함.

hdd 와 ssd 중 더 빠른 ssd 에 os 설치! warning 문구도 그냥 continue 해줌

seoul 지도 클릭

우분투가 다 설치되면 다시 재시작한다는 말이 나오는데 이때 `usb를 뽑고 재부팅`해야한다.



nvidia-smi, gpu가 2개 인 것을 확인
![image](https://user-images.githubusercontent.com/53431568/130414166-ca4701c7-f11f-4318-b8c9-8a58bafe9be6.png)

> nvcc --version 또는 nvcc -V 이 안먹힐 때!
> 
~~~linux 
sudo apt install nvidia-cuda-toolkit
~~~



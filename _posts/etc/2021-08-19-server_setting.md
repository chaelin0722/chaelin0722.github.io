---
title:  "[Ubuntu] 우분투 세팅과 아나콘다 환경설정 - 실패와 원인"
excerpt: ""

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


### 에러!😡
여러 에러 중..
ubuntu를 다 설치한 후 재부팅이 안되면서 `grub` 어쩌구 에러가 발생한다. 

아마 전에 설치한 os환경에서 파티션을 잘못한 문제때문에 그런거 같은데..

### 내가 시도해 본 것

1. install 할때 erase disk, erase ubuntu 등 여러 버전의 install type 선택을 해보았지만 다 먹히지 않았다.
2. 심지어 사용자 지정 install 에서 기존에 설치된 파티션을 없애고 새로운 파티션 생성 후 / 루트로 설정해주어서 여기다가 설치하는데도 똑같은 에러가 발생!


8월 19일 오후 10:43 분 시도한것
install 선택시 partition 생성하고 거기 / 씌워서 install함  -> 실패🥵

8월 19일 오후 10:55분 시도한것
[https://devdoo.tistory.com/81](https://devdoo.tistory.com/81)   -> 실패🥵

### 결과

새롭게 우분투를 설치한 후 재시작을 하면, 

계속 부팅이 안되고 bios 모드로 진입해버린다. 게다가 bios 모드에서 boot priority가 텅 비어있다.

### 해결방법 😺😺

#### bios 모드에서, `UEFI BOOT -> LEGACY BOOT` 로 설정 변경

그 이유는 [레거시와 UEFI 차이](https://m.blog.naver.com/jjc2294/221764257268) 에서 알 수 있었다.

간단히 말하자면 내가 현재 설치한 OS 가 MBR 파티션으로 설정되어있던것 같다.

원래 설치하고자 하는 usb는 내가 rufus 로 구워서 gpt 로 했기 때문에 UEFI 로 했었지만, 다시 설치하려는 우분투는 아마도 MBR 방식이었던 것 같다.

이제 다시 부팅이 되는 것을 확인하였으니 포맷하고 다시 설치하고자 한다.

제대로된 설치방법은 -> (우분투 설치 및 환경설정)[] 여기서 확인하도록!



---
title:  "[Linux 에러해결] Failed to initialize NVML: Driver/library version mismatch 에러 해결하기"
excerpt: "nvidia-smi가 갑자기 안되는 이유"

categories:
  - etc
tags: [linux, study, etc]
classes: wide

last_modified_at: 2021-09-24T10:40:00-05:00
---

### Screen

갑자기 nvidia-smi 명령어가 안듣는 에러가 발생했다 ..?!!😱😱  아래와 같이 경고 문구가 뜬다. 

~~~
Failed to initialize NVML: Driver/library version mismatch
~~~

드라이버가 업데이트 되면서 버전이 맞지않는 이유도 있고, 

### 해결방법

#### 1.nvidia 관련 사용중인 드라이브 확인

~~~
lsmod | grep nvidia
~~~
위 명령어로 드라이브를 확인하면 다음과 같은 식으로 뜨는 것을 확인할 수 있다.

~~~
nvidia_uvm 9233456 0
nvidia_drm 43213 6
nvidia_modeset 1114112 1 nvidia_drm
nvidia 12680704 38 nvidia_uvm,nvidia_modeset
~~~

원래는 이게 뜨면 안되는 것이다.. 아래 명령어들을 하나씩 실행하여 없애주자

~~~
sudo rmmod nvidia_drm
sudo rmmod nvidia_modeset
sudo rmmod nvidia_uvm
sudo rmmod nvidia 
~~~

이제 reboot를 실행해준 후, 다시 `lsmod | grep nvidia`를 실행하였을 때 아무것도 뜨지 않으면 정상이다!

<br>

#### 기타에러

만약, `sudo rmmod nvidia_uvm`과 같은 명령어 실행 후 아래와 같은 에러가 뜬다면, nvidia를 사용중인 프로세스를 확인 후 kill! 해주면 된다.

~~~
rmmod: ERROR: Module nvidia_drm is in use
~~~

아래 명령어로 사용중인 프로세스를 확인 후, `PID`를 확인하고, 

~~~
sudo lsof /dev/nvidia*
~~~

해당 PID 프로세스를 kill! 해주자
~~~
sudo kill -9 PID
~~~


이제 `nvidia-smi`를 실행하여도 잘 되는 것을 알 수 있다~😆😆


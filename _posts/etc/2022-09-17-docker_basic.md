---
title:  "[docker] 도커 차근차근 알아보기"
excerpt: ""

categories:
  - python
  - docker
  - etc
  
tags: [study, python, docker]

last_modified_at: 2022-09-17T08:06:00-05:00
---



차근차근 해보자!

## 0. docker 설치

## 1. docker login

먼저 아래 사이트에서 회원가입을 해준다.

[https://hub.docker.com/](https://hub.docker.com/)

다음은, ubuntu 에서 로그인 해주기! 명령어는 `docker login`

로그인부터 에러가났다! 이럴줄 알았지~!

![login_error](https://user-images.githubusercontent.com/53431568/190860428-902a264c-c112-4e65-8e99-fbe8c9db155a.png)

일단 나는 아래 명령어로 해결이 되었다.

~~~
sudo apt install gnupg2 pass
~~~

짜잔~ 로그인 성공!

![login_success](https://user-images.githubusercontent.com/53431568/190860436-f0eed2af-d161-4c9f-9e74-1757c412085e.png)


## 2. nvidia-docker 설치

docker 이미지를 빌드하기 전에, 내 파일이 어떤환경에서 돌아가야 하는지 알아야 한다.

나는 python 파일을 돌리는데 GPU 를 사용해야 하기 때문에 cuda, cudnn, ubuntu 버전이 필요하다.
즉, GPU 를 사용하기 위해 Docker 을 설치한 후 nvidia-docker 이란 것도 설치해주어야 한다. 

설치는 아래 명령어를 하나씩 실행하자!

1)   
~~~
curl -s -L <https://nvidia.github.io/nvidia-docker/gpgkey> | \\
  sudo apt-key add -
~~~

2)   
~~~
distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
~~~

3)
~~~
curl -s -L <https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list> | \\
  sudo tee /etc/apt/sources.list.d/nvidia-docker.list
~~~

4)
~~~
sudo apt-get update
~~~

5) 설치!
~~~
sudo apt-get install -y nvidia-docker2
~~~

설치되었는지 확인하기 위해 docker 서비스를 재시작 해보자

~~~
sudo systemctl restart docker
~~~

docker가 gpu를 잘 찾아서 쓰는지 확인하는 명령어!
~~~
docker run --rm --gpus all ubuntu:18.04 nvidia-smi
~~~



다음으로는, 다음 명령어로 필요한 환경을 pull 해왔다.

~~~
docker pull nvidia-cuda-10.1-cudnn8-devel-ubuntu18.04
~~~

## 3. 필요한 쿠다, 우분투 환경 pull 해오기!






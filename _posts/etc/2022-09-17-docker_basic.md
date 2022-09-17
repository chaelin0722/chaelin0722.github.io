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

미루고 미루던 Docker.. 이젠 피할 수 없어서 공부를 해보았다. 뭐 다들 docker 많이 들어보았구, 여러 블로그에서 자세히 쉽게 설명해주기 때문에! 설명은 PASS~ 나는 바로 어떻게 하면 쉽고 빠르고 에러 최대한 없이 설치 할 수 있을지 정리해보았다. 후후 여러 에러사항을 거치면서 성공했으니, 잘 따라하면 당신도 docker 마스터~

차근차근 해보자!

## 0. docker 설치


https://louky0714.tistory.com/131

설치참고

<br>
## 1. docker login

도커 설치를 했다면, 아래 docker 사이트에서 회원가입을 해준다.

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


<br>
## 2. nvidia-docker 설치

docker 이미지를 빌드하기 전에, 내 파일이 어떤환경에서 돌아가야 하는지 알아야 한다.

나는 python 파일을 돌리는데 GPU 를 사용해야 하기 때문에 cuda, cudnn, ubuntu 버전이 필요하다.
즉, GPU 를 사용하기 위해 Docker 을 설치한 후 nvidia-docker 이란 것도 설치해주어야 한다. 

설치는 아래 명령어를 하나씩 실행하자!

잠깐~ 난 하나씩 하기 귀찮아요 하는 사람은 1)~3) 프로세스를 0) 한번만 해주면 됩니다. ㅎㅎ 나는 무서워서 하나씩 해봄

0) 1~3 명령어를 한번에 돌리는 명령어! 이거 한 다음에 4) 으로 가세요~
~~~
$ distribution=$(. /etc/os-release;echo $ID$VERSION_ID) \
   && curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add - \
   && curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list
~~~

차근차근 하고싶어요~ 하는 사람은 여기서부터 쭉 하면 됩니다!
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

설치되었는지 확인하기 위해 docker 서비스를 재시작 해보자!
~~~
sudo systemctl restart docker
~~~

docker가 gpu를 잘 찾아서 쓰는지 확인하는 명령어!
~~~
docker run --rm --gpus all ubuntu:18.04 nvidia-smi
~~~

gpu를 잘 찾은 것을 확인하였다 ㅎㅎ

![image](https://user-images.githubusercontent.com/53431568/190860908-154f3bad-76dd-4ffd-8b53-22549062fa25.png)

<br>

## 3. 필요한 쿠다, 우분투 환경 pull 해오기!

다음으로는, 다음 명령어로 필요한 환경을 pull 해왔다.

~~~
docker pull nvidia-cuda-10.1-cudnn8-devel-ubuntu18.04
~~~

짜잔! pull이 매우 잘 되었다 

![image](https://user-images.githubusercontent.com/53431568/190860869-63fcf6d0-461c-415f-8332-b00f3ce9a458.png)

## 4. Dockerfile 를 만들어보자

나는 python code를 돌리는 docker을 만들어야 하기 때문에, 아래와 같이 작성되었다. 혹시 본인이 js 나 c 코드다 하면 좀 다를 수 있으니 조심합시다~

먼저, 본인이 돌리고 싶은 파일과 모든 폴더, 데이터 등등을 하나의 폴더 아래에 다 정리해줍시다. 

나는 다음과 같이 `GONGIN` 이라는 폴더 아래에 싹! 정리를 해주었다! 저 폴더와 파일들만 있으면 돌아갈 수 있게 하였다. 그리고, 이 폴더 안에 Dockerfile을 위치시켜 준다.

![image](https://user-images.githubusercontent.com/53431568/190861541-7a1383c8-cc5d-4f07-b8e1-7c43a3c74a5e.png)

Dockerfile은 linux 에서 vim 으로 만들어줬다. 도커파일은 이 이름이 default 인거같으니 dockerfile 이런식으로 바꾸지 말자! 바꿔도 되는진 잘 모르겟는데 Dockerfile 이라하니 Dockerfile 이라고 하자..ㅎㅎ 에러는 피하는게..

~~~
vim Dockerfile
~~~

파일의 내용은 다음과 같이 작성하였다.

~~~
FROM nvidia/cuda:10.1-cudnn8-devel-ubuntu18.04

RUN echo 'export PATH=/usr/local/cuda-10.1/bin${PATH:+:${PATH}}' >> ~/.bashrc
RUN echo 'export LD_LIBRARY_PATH=/usr/local/cuda-10.1/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}' >> ~/.bashrc
RUN echo 'export PATH=/usr/local/cuda/bin:/$PATH' >> ~/.bashrc
RUN echo 'export LD_LIBRARY_PATH=/usr/local/cuda/lib64:${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}' >> ~/.bashrc

RUN apt-get update -y
RUN apt-get -y install libgl1-mesa-glx
RUN apt-get install -y python3-pip python-dev build-essential
RUN pip3 install --upgrade pip

ADD . /GONGIN
WORKDIR /GONGIN
RUN python3 -m pip install -r requirements.txt

CMD python3 ./toothbrush_head_final.py
~~~


## 5. image build 시키기

이제 만든 Dockerfile 을 



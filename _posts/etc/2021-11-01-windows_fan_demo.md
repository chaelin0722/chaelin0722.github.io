---
title:  "FAN 깃헙 데모 코드 윈도우 로컬에서 돌리기"
excerpt: "github demo code, process in local window10"

categories:
  - etc

tags: [etc, github, demo, FER, FAN]
classes: wide

last_modified_at: 2021-11-01T08:06:00-05:00
---

### 윈도우 아나콘다에서 video Facial Expression Recognition 하기

윈도우의 로컬 환경에서도 얼굴 감정 인식이 되는지 확인하기 위해서 [Emotion-FAN](https://github.com/Open-Debin/Emotion-FAN) 깃헙 코드를 돌려보는 중이다.


설치 명령어가 우분투 기반이라 윈도우에서 좀 돌고 돌아서 환경을 세팅중이다... 허허


이 데모의 논문도 곧 포스팅 할 예정이니 참고해보고 싶은 사람은 참고해보기~

논문 => [FRAME ATTENTION NETWORKS FOR FACIAL EXPRESSION RECOGNITION IN VIDEOS](https://arxiv.org/pdf/1907.00193.pdf) 

논문 정리 포스팅 => [진행중 11/2 예정]



## 환경설정

깃헙에서 제시하는 기본 환경 세팅은 우분투 기반이다. sudo 명령어 apt-get.. 이것을 윈도우에서 어떻게 해야할지 정리하고자 합니다.

![image](https://user-images.githubusercontent.com/53431568/139628816-0ea70cfd-2825-4454-af61-040be90138c1.png)


### 1. 먼저 아나콘다 설치하기! 아나콘다 설치는 간단하니 pass~ 하고 단, path 설정만 잘해주세요~

그런데, 'conda activate 가상환경이름'을 하니 다음과 같은 에러발생.. 

![image](https://user-images.githubusercontent.com/53431568/139629331-7e6419f5-8bb4-4602-aa6c-31dfa0b0d632.png)


그래서 error에서 제시하는 대로 conda init 을 해주었더니 해결!

![image](https://user-images.githubusercontent.com/53431568/139629510-b98f5521-cdf9-4f0b-b6c6-f9b9249591b5.png)



<br>


### 2. library 설치를 위해 [chocolatey software](https://chocolatey.org/) 설치하기 

우분투 환경이면 저대로 그냥 따라만 치면 끝인데.. 윈도우 환경이라 우회하는게 너무 귀찮다.. 허허

일단, 여기서 부터는 anaconda prompt 를 실행하든, cmd에서 실행하던, powershell 에서 실행하던 무조건 관리자 권한으로 실행한 후, 설치를 진행해 주어야 한다.


먼저 이 사이트에서 => [chocolatey software](https://chocolatey.org/install) 


![image](https://user-images.githubusercontent.com/53431568/139629656-69d669a7-272e-4072-87ec-158f6b803d14.png)

`Set-ExecutionPolicy Bypass -Scope Process -Force ......` 를 복사해서 그대로 실행해 주기~!


이제 choco 를 설치 하였으니 필요한 sudo 명령어를 설치해준다.

`choco install sudo` 하면, sudo 설치 끝!  (근데 설치할때 이미 관리자 환경이기도 하고 sudo 말고 choco install 로 진행하다보니 sudo를 쓰지 않아도 되었다.. 머쓱😅.. 그래도 알아두는 것에 의의를 두고 ㅎㅎ)

![image](https://user-images.githubusercontent.com/53431568/139630002-513a9e36-9216-425b-89c3-c750d9c6b124.png)

<br>


### 3. install 진행하기 

이제 차례대로, 

`choco install ffmpeg`

`choco install cmake -y --installargs 'ADD_CMAKE_TO_PATH=System'`

=> cmake는 이런식으로 경로를 지정하며 설치를 해야된다.

![KakaoTalk_20211101_150147692](https://user-images.githubusercontent.com/53431568/139633719-c5f2ae7e-7896-4507-be98-49e6dd95effe.png)

한번 `refreshenv`로 적용시켜 준 후, 

혹시 모르니 `cmake --version`으로 버전 체크!

![ver](https://user-images.githubusercontent.com/53431568/139630308-755ed5b7-7e44-4415-9031-af71b52d77e9.png)


*그리고 여기서 libboost-python-dev는 어떻게 설치하는지 구글링해도 모르겠어서.. 일단 스킵!

그 다음으로는 dlib 을 설치해야 하는데... 이걸 설치하려면 visual studio를 설치해주어야 한다고 한다. 그래서 [2019버전 visual studio](https://visualstudio.microsoft.com/ko/vs/)를 설치해주었다.

아래와 같이 c++를 사용한 데스크톱 개발을 선택하고 오른쪽의 설치 세부정보에서 5개의 default가 선택된대로 해도 되고 다음과 같이 선택해주어도 된다, 단 windows용 C++ CMake도구는 꼭 있어야 한다!

![visual](https://user-images.githubusercontent.com/53431568/139632629-eaecb0e7-e325-4fb6-b473-1d0a9727717e.png)

이후에, 위의 명령어대로 dlib을 설치해 주었으나 되지 않아서.. [이 사이트](https://github.com/shashankx86/dlib_compiled)를 참고하여 직접 python 3.9 버전의 dlib 을 다운받아서 그 경로로 가서 직접 install 해주었다.

먼저, 위의 사이트에서 zip 폴더로 다운받아주고, unzip 시킨후..! 아래 순서대로 해주자. 

~~~
> Download "Python Wheel"
> Go to Folder [Where you have saved "dlib-19.22.99-cp39-cp39-win_amd64.whl"]
> Open "CMD" or "POWERSHELL" [Where you have saved Python Wheel] 
> Type Command : pip install dlib-19.22.99-cp39-cp39-win_amd64.whl
~~~

명령어!

`pip install dlib-19.22.99-cp39-cp39-win_amd64.whl`

<br>

환경설정 완료!

유후 오늘은 여기까지~

libboost 를 설치 하지 않았지만 데모 코드는 돌아갈지 보고 또 다시 수정할 예정이다.

bye~😚😚

<iframe src="https://giphy.com/embed/K2guRw3xP9BjPsv7QZ" width="480" height="415" frameBorder="0" class="giphy-embed" allowFullScreen></iframe>


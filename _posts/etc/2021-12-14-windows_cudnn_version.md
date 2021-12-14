---
title:  "파일로 CUDNN 버전 확인하기"
excerpt: "cuDNN version check"

categories:
  - etc

tags: [etc, tensorflow, dnn, cnn]
classes: wide

last_modified_at: 2021-12-14T08:06:00-05:00
---

## CUDNN 버전 확인하기

구글링으로 여러 명령어로 cudnn 버전을 확인하려 했으나 다 먹히지 않았다. cudnn 설치한지 얼마 안되었지만.. 어떤 버전이었는지 기억이 안나므로.. 😹😹

쉽게 파일로 확인하는 방법을 알아내었다.

먼저, CUDA 가 설치된 폴더의 경로로 가서 `cudnn_version.h` 파일 찾기!

####  - 윈도우 경로

~~~ 
C: > Program Files > NVIDIA GPU Computing Toolkit > CUDA > v11.2 (본인CUDA 버전 폴더로) > include > cudnn_version.h
~~~

#### - ubuntu 경로

~~~
/usr/local/cuda-10.0/targets/x86_64-linux/include/ 
~~~

(cudnn_version.h 없으면 cudnn.h 등 그럴 듯한 파일 열어보면 됨)

그리고 TXT 파일이던, 프로그래밍 툴 등 열수 있는 환경에서 열어서 확인하기!

아래와 같이 내용을 확인할 수 있다.

![화면 캡처 2021-12-14 201830](https://user-images.githubusercontent.com/53431568/145991361-7d286a3e-f6dc-4c47-945b-b0f25a2af9a5.png)


~~~
CUDNN_MAJOR 8

CUDNN_MINOR 1

CUDNN_PATCHLEVEL 0
~~~

순서대로 8.1.0 이 cuDNN 버전이 되겠다

<iframe src="https://giphy.com/embed/xT5LMHxhOfscxPfIfm" width="480" height="362" frameBorder="0" class="giphy-embed" allowFullScreen></iframe>

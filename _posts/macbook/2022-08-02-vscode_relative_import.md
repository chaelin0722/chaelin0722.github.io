---
title:  "[mac] vscode에서 python 코드 relative 상대경로 import 하기"
excerpt: "import relative folder and files"

categories:
  - mac
tags: [mac, etc]

last_modified_at: 2022-08-02T11:00:00-05:00
classes: wide
---

<br>

`from tmp.utils.common import def` 이런식으로 상대경로의 폴더안에 있는 파일의 함수를 불러와 쓰고싶을 때가 있습니다.

하지만 vscode 에서는 그냥 해주지 않는다.. 이 경로를 읽어오지 못하기 때문에 `launch.json`파일에 설정을 해주어야 하는데..!

사실 이 문제때문에 vscode 열심히 쓰다가 힘들어서 pycharm로 갈아탔다가..(박쥐!) 요새 또 pycharm 의 원격접속에 에러가 떠서 vscode로 황급히 수정하면서 이 문제에 다시 직면하기로 하였다.

허헛 🤭 아무튼 사용 방법은 다음과 같습니다!


<br>

### 0. 파일 가정

예를 들어 아래와 같이 폴더와 파일이 구성되어 있다고 가정해보고자 한다.

<img width="336" alt="무제" src="https://user-images.githubusercontent.com/53431568/182481535-194b2f72-931a-49b1-91f7-49a48335a3be.png">

각각 main.py 와 test.py 는 다음과 같이 간단히 구성해 보았다. 

~~~
from subFolder2.test import add

print(add(4,5))
~~~

~~~
def add(a, b):
    
    return a+b
~~~


### 1. launch.json 생성하기

일단 먼저 그냥 실행해보니 역시나 `no module` 에러가 뜬다.

<img width="529" alt="err" src="https://user-images.githubusercontent.com/53431568/182482238-fd59f2c3-dd7a-43c5-9b86-3abe4f558863.png">

그럼 다시, 아래 화면과 같이 디버거 메뉴를 선택해주면, launch.json 만들기 링크가 보이는데 이걸 클릭해주자!

![무제](https://user-images.githubusercontent.com/53431568/182482828-38eca61c-c69b-462f-a7fc-b8d8fe923576.png)

클릭하면 다음과 같이 창이 뜨는데, 여기서 주의할건 `모듈`을 선택하여야 한다는 것!

<img width="1078" alt="모듈" src="https://user-images.githubusercontent.com/53431568/182483089-89d1c42a-26b2-4759-9b42-3ea71734e6ce.png">

이름은 실행시킬 파일이름인 main 으로 해주면,

<img width="1268" alt="tt" src="https://user-images.githubusercontent.com/53431568/182484388-1ffa8d27-2b29-467e-aa49-bd7ba2c10b39.png">

짠! 다음과 같이 json 파일이 생성된다

<img width="916" alt="무제" src="https://user-images.githubusercontent.com/53431568/182484505-1f0d58f1-fc36-4df5-b7b0-2d09ae95f25a.png">


### 2. 추가할 내용

여기서 2줄 추가해준다, 바로 "env" 와 "cwd"

"env"는 `sys.path.append` 와 비슷한 기능이라고 보면되는 것 같다. 여기 디렉토리까지를 기준으로 하게 된다!

"cwd"는 시작시 태스크 러너의 현재 작업 디렉토리를 의미하며, 실행시키는 파일을 기준으로 한다는 뜻으로 현재 열려있는 파일의 디렉토리 경로에서 디버그가 시작될 것이다.

<img width="862" alt="filepath" src="https://user-images.githubusercontent.com/53431568/182486327-9717cede-df3a-485e-8320-b0c5d1569946.png">


이 세팅을 완료하면 잘 돌아갈 겁니다! ㅎㅎ


### 참고

[1] [https://blog.naver.com/PostView.nhn?blogId=sjy263942&logNo=222326679448](https://blog.naver.com/PostView.nhn?blogId=sjy263942&logNo=222326679448)

[2] [https://www.youtube.com/watch?v=Ad-inC3mJfU](https://www.youtube.com/watch?v=Ad-inC3mJfU)

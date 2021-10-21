---
title:  "[mac] vscode에서 python 파일 args 명령어로 실행하기 (task.json)"
excerpt: "mac vscode arguments python file"

categories:
  - mac
tags: [mac, etc]

last_modified_at: 2021-10-21T11:00:00-05:00
classes: wide
---

<br>

vscode에서 주로 python code를 돌리는데, 앞에 args 인자 값을 주어 돌려야 하는 코드들이 있다. 원래 pycharm 에서 실행했을 때는 따로 parameter을 입력해주는 창이 있어서 디버깅하거나 실행할때 매번 입력 없이도 돌리기만 하면
자동으로 입력되어 돌아갔었다. 하지만 vscode 는 찾아보니 따로 json 파일을 생성해주고 그 안에 내용을 입력한 후, run 시킬때마다 `command/ctrl + shift + b` 로 실행을 해주어야 args 인자값이 실행이 된다. 그냥 run 이나 debug 버튼 누르면 하나도 적용안됨..

이럴때 보면 pycharm이 무거운 이유가 있는 것 같기도.. 흠.. 🤭 아무튼 사용 방법은 다음과 같습니다.

<br>

### 1. task.json 생성하기

만약 .vscode 라는 파일 아래에 이미 task.json 이 있다면 1번은 skip 하고 2번부터 보면 된다.

저는 없는 관계로 생성을 하겠습니다. 먼저 실행할 파일에서 `command + shift + b`를 누릅니다. (나는 맥북에서 실행하는거라 command 버튼이지만 윈도우 환경은 ctrl을 사용하면 된다.) 

그러면 다음과 같이 창이 하나 뜨는데요 여기서 빌드 작업구성 클릭!

<img width="760" alt="무제1" src="https://user-images.githubusercontent.com/53431568/138291609-5cd5d3b8-f151-49d9-89f5-22a289daaed8.png">

템플릿에서 task.json 파일 만들기 클릭!

<img width="728" alt="무제3" src="https://user-images.githubusercontent.com/53431568/138291782-a30b86fd-9a04-4328-bbcf-bb90a96ea468.png">

others 클릭!

<img width="727" alt="22" src="https://user-images.githubusercontent.com/53431568/138291962-6490933d-20c4-4e39-9ed3-770d1e72f3a3.png">


디렉토리에서 확인해보면, .vscode 아래에 task.json 이 생성된것을 확인할 수 있습니다.

![image](https://user-images.githubusercontent.com/53431568/138293145-471f4141-d9a0-467a-8a25-bd30005063d4.png)




### 2. task.json 수정하기


task.json을 눌러 파일 내용을 확인해 봅시다. 다음과 같이 아주 기본적인 파일이 내용이 생성되어있습니다.

<img width="700" alt="ㄴㄴ" src="https://user-images.githubusercontent.com/53431568/138292099-b55cdc85-2cb4-49c2-8f51-010aa8e7513d.png">


저는 이거 싹 지우고 python 을 위한 실행환경을 만들어주었는데요, 아래 코드를 그대로 복붙하면 일단 기본은 됩니다. 

~~~
{
    "version": "2.0.0",
    "tasks": [
        {
            "label": "python execute",
            "type": "shell",
            "command": "python3",
            "options": {
                "cwd": "${fileDirname}"
            },  
            "args": [
              "${file}"
             ],

            "group":{
                "kind": "build",
                "isDefault": true
            },

        },

        {
            "label": "c execute",
            "type": "shell",
            "command": "./${fileBasenameNoExtension}",
            "group":{
                "kind": "test",
                "isDefault": true
            },

            "presentation": {
                "echo": true,
                "reveal": "always",
                "focus": false,
                "panel": "shared",
                "showReuseMessage": true,
                "clear": true
            }
        }
    ]
}

~~~

하지만 제가 하고 싶은 것은 args 들을 넣어주는 건데요, 예를 들어 보면, cli 버전의 명령어는 다음과 같이 들어간다고 했을 때, 

~~~
test.py train --dataset=./ --weights=coco --logs=./logs/'
~~~

task.json 에서는 다음과 같이 입력해주시면 됩니다. 

~~~
  "args": [
          "${file}",
          "train",
          "--dataset=./",
          "--weights=coco",
          "--logs=./logs/"
  ],
~~~

<br>

전체적인 코드는 아래와 같습니다. 


~~~

{
    "version": "2.0.0",
    "tasks": [
        {
            "label": "python execute",
            "type": "shell",
            "command": "python3",
            "options": {
                "cwd": "${fileDirname}"
            },

            "args": [
                    "${file}",
                    "train",
                    "--dataset=./",
                    "--weights=coco",
                    "--logs=./logs/"
            ],

            "group":{
                "kind": "build",
                "isDefault": true
            },

        },

        {
            "label": "c execute",
            "type": "shell",
            "command": "./${fileBasenameNoExtension}",
            "group":{
                "kind": "test",
                "isDefault": true
            },

            "presentation": {
                "echo": true,
                "reveal": "always",
                "focus": false,
                "panel": "shared",
                "showReuseMessage": true,
                "clear": true
            }
        }
    ]
}


~~~


`만약 본인이 c/c++ 과 같은 환경에서도 구성하고 싶다면 이에 맞는 코드를 추가해 주어야 합니다.`


### 3. 실행

이제 실행할때 `command + shift + b` 로 하는 것 잊지마세요~😘



<iframe src="https://giphy.com/embed/lYv10cDlosFnA0toPb" width="480" height="270" frameBorder="0" class="giphy-embed" allowFullScreen></iframe>

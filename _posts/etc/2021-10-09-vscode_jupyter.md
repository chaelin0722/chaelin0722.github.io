---
title:  "[vscode] vscode 에서 Jupyter 사용해 PIL 사용하기"
excerpt: "jupyter"

categories:
  - etc
tags: [vscode, etc, jupyter, matplotlib]
classes: wide

last_modified_at: 2021-10-09T10:40:00-05:00
---

vscode 에서 서버에 원격 접속해 사용하는데 matplotLib 과 같은 이미지들이 보이지 않았다! vscode 자체에서 지원을 해주지 않는 것 같아 찾아보니 jupyter을 설치해서 따로 추가해주어야 한다고 한다.

아무래도 이미지 처리를 많이 해야하는 내 입장에선 너무 불편했다. 그렇지만 다시 파이참에서 하고싶진 않기에..😂 방법을 찾아보았다.

방법은 생각보다 심플해서 괜찮았다. 

<br>

### 1. Jupyter 설치

[https://marketplace.visualstudio.com/items?itemName=donjayamanne.jupyter](https://marketplace.visualstudio.com/items?itemName=donjayamanne.jupyter)여기 혹은 아래와 같이 vscode 내에서 
설치해주면 된다.


<img width="1456" alt="무제" src="https://user-images.githubusercontent.com/53431568/136650313-d0c8485f-588a-4a57-a687-6e84be9be945.png">


### 2. 소스 가장 윗라인에  "#%%" 를 추가해준다.

그럼 다음과 같이 코드를 감싸주는 파란색 줄이 생성된다. 이렇게 되면 파란줄 안에 포함되는 코드가 모두 실행이 됩니다ㅎㅎ

<img width="819" alt="무제" src="https://user-images.githubusercontent.com/53431568/136650595-122faf81-b50b-410b-8269-123e43ee7dca.png">



### 3. "Run Cell" 을 눌러 이미지를 확인해준다.

짠! 오른쪽에 이미지가 출력된다. 간단하쥬?!


<img width="968" alt="무제 2" src="https://user-images.githubusercontent.com/53431568/136650640-3b5f0553-9d97-4e31-9724-b8b082c6a59b.png">


<br>


<iframe src="https://giphy.com/embed/K2guRw3xP9BjPsv7QZ" width="480" height="415" frameBorder="0" class="giphy-embed" allowFullScreen></iframe>


---
title:  "[pip] Defaulting to user installation because normal site-packages is not writeable"
excerpt: ""

categories:
  - python
  - etc
  
tags: [study, python]

last_modified_at: 2022-09-15T08:06:00-05:00
---



자꾸 해당 라이브러리에 맞는 버전을 찾을 수가 없다면서 install이 안되는 경우가 있다. 그럴때는 서버 환경에 여러 버전의 Python이 설치되어있어 어느 버전에 맞게 설치해야할지 헷갈려서 나오는 문제이다.


![image](https://user-images.githubusercontent.com/53431568/190407334-79eb3c2c-459d-4b88-aa91-92c94a908579.png)

### 해결방법

~~~
python3 -m pip install [라이브러리이름]
~~~

위 과 같이 python interpreter를 명시해 주면, 제대로 설치가 된다. 

그리고, 환경들을 다 써둔 txt 파일도 한번에 다 설치가 된다. 굿!

~~~
python3 -m pip install -r requirements.txt
~~~

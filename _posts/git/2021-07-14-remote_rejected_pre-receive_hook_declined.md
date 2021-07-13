---
title:  '[GIT 에러] ![remote rejected] main -> main (pre-receive hook declined)'
excerpt: ""

categories:
  - git
  - etc
tags: [git, etc]

last_modified_at: 2021-06-13T08:06:00-05:00
classes: wide
---
###  [remote rejected] main -> main (pre-receive hook declined) 에러 잡기


![image](https://user-images.githubusercontent.com/53431568/125394140-2f448380-e3e4-11eb-9eff-67877f29e11a.png)

이런 에러가 발생

다들 강제로 푸쉬하면 해결된다고 했지만 나는 전혀 안먹혔다.

시도해본 명령어들

'''
git push -f origin
git push origin master -f
git push -u origin master
'''

이때 마다 아래와 같은 경고창이 뜬다.

![image](https://user-images.githubusercontent.com/53431568/125394294-729ef200-e3e4-11eb-80f4-02d83f1e1bb7.png)

여기서의 문제가 아마 내가 사용하는 브랜치가 main 이라서 그런듯 싶다. 그래서 사용한 명령어가 ! merge!

### 해결 순서

(1) '''git merge'''

(2) '''git checkout master''' 로 브랜치를 master로 위치하게 하기 

(3) '''git merge'''

(4) '''git log''' 로 확인하기 아래와 같이 HEAD -> master 로 가리키는 것을 알 수 있다.

![image](https://user-images.githubusercontent.com/53431568/125394497-b8f45100-e3e4-11eb-93da-c168341d4a21.png)

(5) git push 는 필요없는 것 같다. 해보니까 Everything up-to-date 이라고 뜬다.


### 참고

[1] [https://backlog.com/git-tutorial/kr/stepup/stepup2_4.html](https://backlog.com/git-tutorial/kr/stepup/stepup2_4.html)

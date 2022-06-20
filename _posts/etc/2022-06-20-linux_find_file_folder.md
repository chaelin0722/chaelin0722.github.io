---
title:  "[LINUX] 현/하위 디렉토리에서 파일 갯수, 폴더 갯수 세기"
excerpt: "linux file, folder directory"

categories:
  - etc
  - linux

tags: [linux, study]
classes: wide

last_modified_at: 2022-06-20T08:06:00-05:00
---

#### 현재 디렉토리에서 "파일" 갯수 찾기
~~~
ls -l | grep ^- | wc -l
~~~

#### 현재 디렉토리에서 "폴더" 갯수 찾기

~~~
ls -l | grep ^d | wc -l
~~~

#### 현 위치에서 하위 "파일" 갯수 찾기

~~~
find . -type f | wc -l
~~~

#### 현 위치에서 하위 "폴더" 갯수 찾기

~~~
find . -type d | wc -l
~~~

<iframe src="https://giphy.com/embed/K2guRw3xP9BjPsv7QZ" width="480" height="415" frameBorder="0" class="giphy-embed" allowFullScreen></iframe>

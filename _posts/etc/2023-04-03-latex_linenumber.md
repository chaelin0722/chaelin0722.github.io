---
title:  "[LATEX] 논문에 줄 번호 삽입하기 - linenumbers"
excerpt: ""

categories:
  - markdown
  - etc

tags: [etc, study, markdown]

last_modified_at: 2023-04-03T08:06:00-05:00
---

Overleaf 에서 마크다운 수식으로 논문으르 작성하곤 한다. overleaf 를 쓰면 훨씬 정갈한 논문쓰기에 좋기 때문이다. 😉😉

논문 리비전이 와서 수정하고, 몇 번째 라인이 잘못되었는지 파악하기 위해 논문에 라인 넘버를 달아줘야하는데 은근 문법을 찾기가 어려웠다.

딱 3줄 쓰면 되는데..

먼저 usepackage 로 선언해준 후, 넘버를 달아주고 싶은 시작과 끝 부분에 삽입해주면 된다.

~~~
\usepackage[]{lineno}  %% 선언

\linenumbers  %% 줄 넘버 시작

\nolinenumbers %% 중단하고 싶은곳에! 줄 넘버 끝! 
~~~


나는 타이틀 부터 끝까지 줄 넘버를 달아야 했으므로.. 선언은 가장 위 documentclass 다음에 선언 해주고, 줄 넘버 시작은 `\begin{document}` 다음에 바로 선언, 그리고 중단은 하지 않았다 (딱 2줄 삽입함)


~~~
\usepackage[]{lineno}  %% 선언

\begin{document}
\linenumbers  %% 줄 넘버 시작

~ ~

\end{document}
~~~

모든 공과대 학생들 저널 논문 작성할때 화이팅~ AI 가 실험결과를 토대로 논문을 작성해주는 그날 까지..!!

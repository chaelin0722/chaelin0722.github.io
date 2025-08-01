---
title:  "[FIREBASE] - Log user actions for Experimental User study"
excerpt: "participant id 를 문서로 그 아래에 모든 action log 들을 한 컬렉션에 저장하도록 하는 방법"

categories:
  - firebase
  - log
tags: [etc,firebase]
 
---

정말 오랜만의 포스팅,

HCI 분야로 필드가 바뀌면서, 논문을 준비할때마다 내가 만든 프로그램에 대하여 user study를 진행해야 한다.
특히, 사용자가 어떤식으로 프로그램과 상호작용하는지를 트래킹하기 위해서는 firebase 같은 프로그램으로 모든 사용자 패턴을 저장하는 것이 필요하다.

따라서 오늘의 사용자 패턴 저장방식은, 정식 로그인 없이 (auth 없이) 사용자가 프로그램에 치고 들어오는 id 를 document, 그리고 그 id 아래에 모든 action 들이 한 문서의 컬렉션으로 저장되게 하는 방법이다.

코드는 다음과 같다.

<script src="https://gist.github.com/chaelin0722/ceac48e8181c547a0a7c97d8cdc5cf84.js"></script>

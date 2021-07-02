---
title:  "[Jekyll] minimal-mistakes-theme에서 back to top버튼 만들기"
excerpt: "gitblog"

categories:
  - Blog
tags: [Blog, jekyll]

last_modified_at: 2021-07-02T11:00:00-05:00
classes: wide
---

글이 길어질 수록 맨 위로 한번에 올라가는 top 버튼이 필요해졌다.

아래 순서대로만 똑같이 한다면 에러는 없을 것입니다..! 👍

### 1. `_sass/minimal-mistakes/_sidebar.scss`에서 아래 내용 삽입

```
.sidebar__top {
  position: fixed;
  bottom: 1.5em;
  right: 2em;
  z-index: 10;
}
```

`_sidebar.scss`를 보면 다음과 같이 ========= 부분 아래에 추가해주면 된다.

```/* ==========================================================================
   SIDEBAR
   ========================================================================== */

/*
   Default
   ========================================================================== */

.sidebar__top {
  position: fixed;
  bottom: 1.5em;
  right: 2em;
  z-index: 10;
}

.sidebar {
  @include clearfix();
  // @include breakpoint(max-width $large) {
  //   /* fix z-index order of follow links */
  //   position: relative;
  //   z-index: 10;
  //   -webkit-transform: translate3d(0, 0, 0);
  //   transform: translate3d(0, 0, 0);
  // }```

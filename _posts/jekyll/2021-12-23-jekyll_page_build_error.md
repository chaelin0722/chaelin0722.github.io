---
title:  "[jekyll 에러] pages build and deployment"
excerpt: ""

categories:
  - Blog
tags: [Blog, jekyll]

last_modified_at: 2021-12-23T11:00:00-05:00
classes: wide
---

열심히 포스팅을 마치고 commit을 하는데 local 에서는 잘 반영이되는 내 글이 서버에서는 작동하지 않는 것을 발견했다. 😅😅

아래 이미지 처럼 원래는 commit 이 잘 적용되면 초록색 체크 표시가 떠야 하는데 이건 무슨 에러인 엑스 표시도 아니고 아예 아무런 동작을 하지 않는다.!

![git_err](https://user-images.githubusercontent.com/53431568/147227638-9d550944-e801-498f-a54e-f5fd5371d3c2.png)

구글링을 열심히 해봐도 새로나온 에러인지 파악이 힘들었다.

그래서 열심히 git 페이지 메뉴들을 클릭했는데, `Actions` 메뉴를 클릭하니 에러를 확인할 수 있었다.


![image](https://user-images.githubusercontent.com/53431568/147227879-ebad6317-49c6-45f2-b11c-4da00e389de0.png)

수많이 쌓인 `pages build and deployment` 에러들..

범인은 `_config.yml` 파일에 있었다.

원래는 theme을 주석처리 하지 않고 사용하고 있었는데 이제는 주석처리하고 theme 이던 remote_theme 이던 둘다 사용하지 않아야 한다고 한다.

이유는.. 무슨 서버를 이제 사용하지 않는다고? 하는거 같은데.. 일단 내 문제가 급하니 에러부터 처리했다.

![theme](https://user-images.githubusercontent.com/53431568/147228227-db45aa48-ea6d-464c-bb33-f10fdec391b7.png)

<br>

다시 Action 메뉴가서 확인!

문제해결!

![check2](https://user-images.githubusercontent.com/53431568/147228443-21b75927-1655-4fa9-9fbc-a4a1881c1c9c.png)


자세히 확인!

![check](https://user-images.githubusercontent.com/53431568/147228478-df905c60-f0d0-4bb8-9588-cdf99262e9b3.png)







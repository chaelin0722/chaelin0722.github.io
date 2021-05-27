---
title:  "Normalization, Regularization and Standardization"
excerpt: ""

categories:
  - Deeplearning
tags:
  - Deeplearning
classes: wide

last_modified_at: 2021-05-28T08:06:00-05:00

---


정규화 하는 이유

데이터가 가진 feature의 스케일이 심하게 차이가 나는 경우 문제가 된다.

## Normalization

- 값의 범위를 0~1사이의 값으로 조정한다. 
   -> 즉, 모든 데이터 포인트가 동일한 정도의 스케일(중요도)로 반영되도록 해준다.
- 학습 전에 scaling

ex) min-max 

### Regularization

- weight를 조정하는데 제약을 거는 기법으로 overfitting을 막기 위해 사용한다.

   ** 제약을 건다 ⇒ 모델을 좀 더 복잡하거나 유연하게 만든다는 것

ex) L1 regularization (lasso, 마름모) , L2 regularization(lidge, 원)

### Standardization

- 값의 범위를 평균0, 분산1이 되도록 변환한다.
- (정규분포→표준정규분포) 표준화 확률변수를 구하는 방법으로 z-score라고도 할 수 있다.


참고

[1] [https://blog.naver.com/PostView.nhn?blogId=qbxlvnf11&logNo=221476122182&categoryNo=52&parentCategoryNo=0&viewDate=&currentPage=1&postListTopCurrentPage=1&from=postView](https://blog.naver.com/PostView.nhn?blogId=qbxlvnf11&logNo=221476122182&categoryNo=52&parentCategoryNo=0&viewDate=&currentPage=1&postListTopCurrentPage=1&from=postView)

[2] [https://realblack0.github.io/2020/03/29/normalization-standardization-regularization.html](https://realblack0.github.io/2020/03/29/normalization-standardization-regularization.html)

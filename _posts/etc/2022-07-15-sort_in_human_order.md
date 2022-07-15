---
title:  "[python] 파일을 숫자 순서대로 나열하기"
excerpt: "sort file names in human order"

categories:
  - python
  
tags: [Deeplearning, CNN, study, python]

last_modified_at: 2022-07-15T08:06:00-05:00
---

## python 에서 숫자 순서대로 정리하기

숫자 순서대로 나열하는 함수를 생각하면 `sort()` 함수를 자주 쓰곤한다. 하지만, 이 함수는 앞 숫자 순서대로 나열하기 때문에,

예를 들면,

list = [파일1.jpg , 파일2.jpg, 파일10.jpg, 파일3.jpg, 파일11.jpg]라고 가정하면,

파일1.jpg ,  파일10.jpg, 파일11.jpg, 파일2.jpg, 파일3.jpg 순서대로 정렬될 것이다..!

<br>

파일1.jpg , 파일2.jpg, 파일3.jpg, 파일10.jpg, 파일11.jpg 이 순서와 같이 정렬하고 싶다면 아래와 같이 함수를 하나 정의해주어야 한다!

~~~
import re

def atoi(text):
    return int(text) if text.isdigit() else text

def human_order(text):
    return [ atoi(c) for c in re.split(r'(\d+)', text) ]

test_list = [파일1.jpg , 파일2.jpg, 파일10.jpg, 파일3.jpg, 파일11.jpg]

test_list.sort(key=human_order)
print(test_list)

# [파일1.jpg , 파일2.jpg, 파일3.jpg, 파일10.jpg, 파일11.jpg]
~~~


## reference

[1] [https://velog.io/@seanko29/Python-%EC%9D%B4%EB%AF%B8%EC%A7%80-%EB%B2%88%ED%98%B8-%EC%88%9C%EC%84%9C%EB%8C%80%EB%A1%9C-%EC%A0%95%EB%A0%AC%ED%95%98%EA%B8%B0](https://velog.io/@seanko29/Python-%EC%9D%B4%EB%AF%B8%EC%A7%80-%EB%B2%88%ED%98%B8-%EC%88%9C%EC%84%9C%EB%8C%80%EB%A1%9C-%EC%A0%95%EB%A0%AC%ED%95%98%EA%B8%B0)



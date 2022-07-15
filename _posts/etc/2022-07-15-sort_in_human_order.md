---
title:  "[python]"
excerpt: "sort file names in human order"

categories:
  - python
  
tags: [Deeplearning, CNN, study, python]

last_modified_at: 2022-07-15T08:06:00-05:00
---

## python 에서 숫자 순서대로 정리하기

숫자 순서대로 나열하는 함수를 생각하면 `sort()` 함수를 자주 쓰곤한다. 하지만, 이 함수는 앞 숫자 순서대로 나열하기 때문에,

예를 들면,

list = [파일1.jpg , 파일2.jpg, 파일3.jpg, 파일10.jpg, 파일11.jpg]  라고 가정하면,

파일1.jpg ,  파일10.jpg, 파일11.jpg, 파일2.jpg, 파일3.jpg 순서대로 정렬될 것이다..!

<br>

파일1.jpg , 파일2.jpg, 파일3.jpg, 파일10.jpg, 파일11.jpg 이 순서와 같이 정렬하고 싶다면 아래와 같이 함수를 하나 정의해주어야 한다!

~~~
import re

def atoi(text):
    return int(text) if text.isdigit() else text

def human_order(text):
    return [ atoi(c) for c in re.split(r'(\d+)', text) ]

test_list = ['contrast images1.jpg', 'contrast images10.jpg', 'contrast images100.jpg', 'contrast images101.jpg', 'contrast images102.jpg', 'contrast images103.jpg', 'contrast images11.jpg', 'contrast images12.jpg', 'contrast images13.jpg', 'contrast images14.jpg', 'contrast images15.jpg', 'contrast images16.jpg', 'contrast images17.jpg', 'contrast images18.jpg', 'contrast images19.jpg', 'contrast images2.jpg', 'contrast images20.jpg', 'contrast images21.jpg', 'contrast images22.jpg', 'contrast images23.jpg', 'contrast images24.jpg', 'contrast images25.jpg', 'contrast images26.jpg', 'contrast images27.jpg', 'contrast images28.jpg', 'contrast images29.jpg', 'contrast images3.jpg', 'contrast images30.jpg', 'contrast images31.jpg', 'contrast images32.jpg', 'contrast images33.jpg', 'contrast images34.jpg', 'contrast images35.jpg', 'contrast images36.jpg', 'contrast images37.jpg', 'contrast images38.jpg', 'contrast images39.jpg', 'contrast images4.jpg', 'contrast images40.jpg', 'contrast images41.jpg', 'contrast images42.jpg', 'contrast images43.jpg', 'contrast images44.jpg', 'contrast images45.jpg', 'contrast images46.jpg', 'contrast images47.jpg', 'contrast images48.jpg', 'contrast images49.jpg', 'contrast images5.jpg', 'contrast images50.jpg', 'contrast images51.jpg', 'contrast images52.jpg', 'contrast images53.jpg', 'contrast images54.jpg', 'contrast images55.jpg', 'contrast images56.jpg', 'contrast images57.jpg', 'contrast images58.jpg', 'contrast images59.jpg', 'contrast images6.jpg', 'contrast images60.jpg', 'contrast images61.jpg', 'contrast images62.jpg', 'contrast images63.jpg', 'contrast images64.jpg', 'contrast images65.jpg', 'contrast images66.jpg', 'contrast images67.jpg', 'contrast images68.jpg', 'contrast images69.jpg', 'contrast images7.jpg', 'contrast images70.jpg', 'contrast images71.jpg', 'contrast images72.jpg', 'contrast images73.jpg', 'contrast images74.jpg', 'contrast images75.jpg', 'contrast images76.jpg', 'contrast images77.jpg', 'contrast images78.jpg', 'contrast images79.jpg', 'contrast images8.jpg', 'contrast images80.jpg', 'contrast images81.jpg', 'contrast images82.jpg', 'contrast images83.jpg', 'contrast images84.jpg', 'contrast images85.jpg', 'contrast images86.jpg', 'contrast images87.jpg', 'contrast images88.jpg', 'contrast images89.jpg', 'contrast images9.jpg', 'contrast images90.jpg', 'contrast images91.jpg', 'contrast images92.jpg', 'contrast images93.jpg', 'contrast images94.jpg', 'contrast images95.jpg', 'contrast images96.jpg', 'contrast images97.jpg',
          'contrast images98.jpg', 'contrast images99.jpg']

test_list.sort(key=human_order)
print(test_list)
~~~


## reference

[1] [https://velog.io/@seanko29/Python-%EC%9D%B4%EB%AF%B8%EC%A7%80-%EB%B2%88%ED%98%B8-%EC%88%9C%EC%84%9C%EB%8C%80%EB%A1%9C-%EC%A0%95%EB%A0%AC%ED%95%98%EA%B8%B0](https://velog.io/@seanko29/Python-%EC%9D%B4%EB%AF%B8%EC%A7%80-%EB%B2%88%ED%98%B8-%EC%88%9C%EC%84%9C%EB%8C%80%EB%A1%9C-%EC%A0%95%EB%A0%AC%ED%95%98%EA%B8%B0)



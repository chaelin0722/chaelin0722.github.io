---
title:  "[Python] 배열 리스트를 순서대로 dictionary로 변환"
excerpt: "list to dict, python"

categories:
  - python
  - study
  - etc
tags: [etc,python,study]
classes: wide

last_modified_at: 2021-07-21T08:06:00-05:00
---

### 배열 순서 그대로 key값을 자동부여해서 dictionary로 변환하는방법!

바로 **enumerate()**함수를 사용하면된다.


코드는 다음처럼 간단하다!

~~~python
import numpy as np

arr = np.array(['one', 'two', 'three', 'four'])

arrayToDict = dict(enumerate(arr))

print(todict)
~~~


#### 출력결과
~~~python
{0: 'one', 1: 'two', 2: 'three', 3: 'four'}
~~~

<br>
만약 처음 key값을 1부터 시작하고 싶다면 1 이라는 파라미터를 추가해주면 된다. 아래 코드를 보자

~~~python
import numpy as np

arr = np.array([ '''your custom array values'''])

arrayToDict = dict(enumerate(arr, 1))

print(todict)
~~~

#### 출력결과
~~~python
{1: 'one', 2: 'two', 3: 'three', 4: 'four'}
~~~


<br>

이런식으로 1000개의 클래스를 갖는 imagenet 의 클래스를 리스트로 정리해 매핑하여 사용할 수 있었다.😙😙



### 참고
[1] [https://rfriend.tistory.com/623](https://rfriend.tistory.com/623)


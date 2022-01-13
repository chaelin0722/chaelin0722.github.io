---
title:  "[tqdm 에러해결]  'module' object is not callable"
excerpt: "tqdm"

categories:
  - etc
  
tags: [study, pytorch, etc]
classes: wide

last_modified_at: 2022-01-13T10:40:00-05:00
---



![image](https://user-images.githubusercontent.com/53431568/149311271-736ae3b0-3d35-4c2e-910a-082ef57ab844.png)



해결방법

~~~
from tqdm import tqdm
~~~

끝!

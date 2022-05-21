---
title:  "[Linux] pid의 사용자 확인하기 "
excerpt: "process pid로 user 체크"

categories:
  - etc
  - linux
tags: [study, linux, etc]
classes: wide

last_modified_at: 2022-05-21T10:40:00-05:00
---

공용 GPU를 돌리다보면 `nvidia-smi` 를 통해서 빈 gpu를 확인하고 돌리곤 한다.

하지만! 급히 gpu를 사용해야 한다거나 할때! 해당 gpu로 프로세스를 돌리고 있는 사람이 누구인지 체크하기 위해서 pid로 사용자를 체크하는 방법을 알아보았다.

아주 간단!

~~~
ps -f [pid]
~~~

예를 들어, 2번 과 3번 GPU를 사용하는데 누가돌리는지 알고싶다! (보니까 PID가 4995로 동일한 것을 보니 같은 사용자가 multi gpu로 사용하고 있는 것 같다.)
(참고로 PID 8545는 내꺼다 ㅎㅎ 기생해서 사용중.. )

![image](https://user-images.githubusercontent.com/53431568/169650657-98f6377c-cc71-4dec-958f-a75ca33a28c0.png)

여기서 2번 3번 gpu를 사용하고 있는 사용자를 알고싶은 것이니, 아래와 같이 치면 된다! 쉽쥬?

~~~
ps -f 4995 
~~~

짜란 다음과 같이 user 이름(UID)이 나온다

![image](https://user-images.githubusercontent.com/53431568/169650715-5138d0a8-ca16-40d2-b642-d4b1c80211c8.png)


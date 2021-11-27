---
title:  "[Pytorch 에러] RuntimeError: Input type (torch.cuda.FloatTensor) and weight type (torch.FloatTensor) should be the same"
excerpt: ".to('cuda:0') or .to(device)"

categories:
  - etc

tags: [etc, pytorch, error, dnn]
classes: wide

last_modified_at: 2021-11-25T08:06:00-05:00
---

### RuntimeError: Input type (torch.cuda.FloatTensor) and weight type (torch.FloatTensor) should be the same

pytorch 코드를 실행하는데 다음과 같은 에러가 떴다.

`input type 과 weight type 이 같아야 한다`라는데.. 원인이 뭔지 한참 헤매다가 train 할때에는 model.to(device) 로 gpu 에 할당해서 처리해주었는데, test 해볼때 model을 불러와 놓고 
gpu로 처리하지 않아서 생긴 문제였다.

pytorch 모델은 세세한 값들을 gpu로 처리할 것인지 cpu 로 처리할 것인지를 정해줄 수 있는데, 꼭 이 값들을 일치시킬 수 있도록 해주어야 한다..! 항상 주의하자 ㅎㅎ


#### train 에서..

~~~ 
# move model to the right device
model.to(device)
~~~

#### test 시 저장된 model을 state_dict  로 불러올때..

~~~
model.load_state_dict(torch.load('model_path'))
model.to(device)
~~~

만약 본인이 `.to("cuda:0")` 이런식으로 설정했다면 .to(device) 대신 .to("cuda:0")를 설정해 주면 된다!



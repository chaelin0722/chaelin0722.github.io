---
title:  "[에러분석] TypeError: can’t convert CUDA tensor to numpy. Use Tensor.cpu() to copy the tensor to host memory first"
excerpt: "numpy gpu pytorch"

categories:
  - Deeplearning
  - CNN
  - pytorch
  - error
  - study
tags: [Deeplearning, CNN,study, pytorch, error]
classes: wide

last_modified_at: 2021-08-25T08:06:00-05:00
---

## TypeError: can’t convert CUDA tensor to numpy. Use Tensor.cpu() to copy the tensor to host memory first

pytorch로 개발환경을 변경하면서 여러 에러에 직면하였다. 😂😂
 
tensorflow와 다른점은 학습시 GPU 를 사용하려면 input, label, net(신경망)에 따로 gpu로 설정해주어야 한다는 점이다.

이때 발생한 문제가, numpy가 아직 GPU를 지원하지 않는다는 것이다. 따라서 numpy는 다시 cpu로 설정해주어야 한다. 


내 코드에서

~~~python
    
    npimg = img.numpy()
    
    #to
    
    npimg = img.cpu().numpy()

~~~
 
이렇게 변경해주었더니 잘 작동한다.




### 참고

[1] [https://stackoverflow.com/questions/53900910/typeerror-can-t-convert-cuda-tensor-to-numpy-use-tensor-cpu-to-copy-the-tens](https://stackoverflow.com/questions/53900910/typeerror-can-t-convert-cuda-tensor-to-numpy-use-tensor-cpu-to-copy-the-tens)

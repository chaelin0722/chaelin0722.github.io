---
title:  "[keras] 각 모델 마다 다른 gpu 사용하기 multi-GPU"
excerpt: "tensorflow"

categories:
  - etc
tags: [study, tensorflow, etc]
classes: wide

last_modified_at: 2022-01-25T10:40:00-05:00
---

요 근래 pytorch 만 사용하다 보니까 tensorflow는 어색하다. pytorch 로는 multi-GPU 병렬처리에 대해서 다룬적이 있는데 케라스 코드에서는 처음이라 구글링을 하면서 뜯어보았다.


---
title:  "[DL] Detectron Implementation"
excerpt: "설치"

categories:
  - CNN
tags: [Deeplearning, CNN, study]

last_modified_at: 2022-10-31T08:06:00-05:00
---

### Detectron2 설치방법

![화면 캡처 2022-10-31 113743](https://user-images.githubusercontent.com/53431568/198919842-512bf4b1-6449-43e0-ab0d-ff7e386d8cb7.png)



Mask R-CNN 모델로 detection 연구를 진행해왔는데, attention을 붙이려하니 너무 레이어 수정이 어려워서 pytorch로 바꾸기 위해 detectron2를 설치하였다. 




~~~
# 가상 환경 생성
conda create -n detectron python=3.8

# 깃허브 환경
git clone https://github.com/facebookresearch/detectron2.git

# opencv, pyyaml
pip3 install opencv-python
pip install pyyaml==5.1

# torch, cuda 10.1 torch 1.8.1
pip3 install torch==1.8.1+cu101 torchvision==0.9.1+cu101 torchaudio==0.8.1 -f https://download.pytorch.org/whl/torch_stable.html

# detectron2 폴더 바깥에서
python -m pip install -e detectron2  
~~~



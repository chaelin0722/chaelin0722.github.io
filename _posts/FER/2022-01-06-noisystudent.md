---
title:  "[Paper Review๐] Noisy Student Training using Body Language Dataset Improves Facial Expression Recognition"
excerpt: "-Noisy Student FER-"

categories:
  - fer
  
tags: [fer,CNN, paperReview]
use_math: true

last_modified_at: 2022-01-06T08:06:00-05:00
classes: wide
---

## Noisy Student Training using Body Language Dataset Improves Facial Expression Recognition
#### -Noisy Student FER-

[Paper](https://arxiv.org/pdf/2008.02655.pdf)๐ 

์ด๋ฒ์, Papers with code ๊ธฐ์ค, AFEW ๋ฐ์ดํฐ๋ก SOTA์ฑ๋ฅ์ ๋ฌ์ฑํ FER ๋ผ๋ฌธ์ ๋ฆฌ๋ทฐํ๋ ค๊ณ  ํ๋ค.


์ฐธ ๋ง์ด๋ ์ฌ์ฉํ์๋ค. 3๊ฐ์ง attention ๊ธฐ๋ฒ, ์ผ๊ตด ์ด๋ฏธ์ง๋ฅผ 3๊ฐ์ง๋ก ๋๋์ด์ ๋ถ์, noisy student์ผ๋ก ํ์ต, extra dataset ์ฌ์ฉ.. ์ด๋ ๊ฒ ๋ค ์ฌ์ฉํด๋ audio๋ฅผ ์ฌ์ฉํ multi-modal model ๋ณด๋ค๋ ์๋์ ๋ญํฌ๋์ด์๋ค.


`**์๋ ์ด๋ฏธ์ง๋ AFEW ๋ฐ์ดํฐ์ ๊ธฐ์ค์ผ๋ก ๋ชจ๋ธ ์์์ด๋ค. 6์๋ฅผ ์ฐจ์งํ๋ ์ค`

![ํ๋ฉด ์บก์ฒ 2022-01-05 234023](https://user-images.githubusercontent.com/53431568/148239068-d13bd014-69be-456a-ad1f-c405249d6c4a.png)


## Introduction

- Propose an efficient model addresses the challenges posed by videos in the wild while tackling the issue of labelled data inadequacy

- Previous video-based emotion recognition used visual cues but a fusion of 5 different architectures with more than 300 million parameters. `However`, this model proposed method uses a single model with approximately 25million parameters and comparable performance

- Use SOTA pre-trained deep learning model (Enlighten-GAN) for preprocessing (because, previous methods tend to amplify noise, tone distortion, and other artefacts)

- Use three-level attention mechanism (spatial-attention block, channel-attention block, frame-attention block)

์์ฝํ์๋ฉด, ์ฑ๊ธ๋ชจ๋ธ์ ์ฌ์ฉํ๋ฉด์, ์ด๋ฏธ์ง ์ ์ฒ๋ฆฌ์ GAN, Backbone์์๋ 3๋ฒ์ attention, unlabelled dataset(extra data)์ ์ฌ์ฉํ์ฌ SOTA ์ฑ๋ฅ์ ๋ฌ์ฑํ์๋ค.


<br>

## Pre-processing

์ด๋ฏธ์ง ์ ์ฒ๋ฆฌ ๊ณผ์ ์ ๋์ํ ํด๋ณด์๋ค.

![ํ๋ฉด ์บก์ฒ 2022-01-06 202517](https://user-images.githubusercontent.com/53431568/148375825-ae5e6eaf-a3b7-4fe4-8e2a-d6eb5c3659cf.png)

๋ชจ๋  FER์ด ๊ทธ๋ฌํ๋ฏ, ๋น๋์ค ๋ฐ์ดํฐ๋ฅผ ํ๋ ์์ฒ๋ฆฌํ๊ณ , ๊ฐ ํ๋ ์์์ MTCNN์ ์ฌ์ฉํด ์ผ๊ตด์ ์ฐพ๊ณ  CROPํ ํ ๊ฐ๋๋ฅผ ๋ง์ถฐ์ฃผ๋ ์์์ ํ๋ค. (MTCNN ๋ผ๋ฌธ์ ์ฝ๋์ค์ด๋ค. ์ถํ ํฌ์คํํ๊ฒ ๋ค..!)

์ฌ๊ธฐ์ ์ถ๊ฐ๋๋๊ฒ Enlighten-GAN์ธ๋ฐ, ์์ ๊ทธ๋ฆผ์์๋ ๋ณผ ์ ์๋ฏ์ด ์ด๋์ด ํ๋ฉด์์ ์ผ๊ตด์ ํน์ง์ ์ ์ฐพ๊ธฐ ํ๋ค๊ธฐ ๋๋ฌธ์ GAN์ ์ฌ์ฉํ์ฌ์ ์ด๋ฏธ์ง๋ฅผ ๋ฐ๊ฒ ํ๋ ์ฒ๋ฆฌ๋ฅผ ํด์ฃผ์๋ค.

๊ธฐ์กด์๋ ์ ์ฒ๋ฆฌ ์๊ณ ๋ฆฌ์ฆ(gamma correction, difference of Gaussians, histogram equalization ๋ฑ)์ ์ฌ์ฉํ์๋๋ฐ, ๋ธ์ด์ฆ๋ฅผ ์์ธํ์ํค๊ณ  ํค์ด๋ ๋ค๋ฅธ ์ธ๊ณต๋ฌผ๋ค์ ์๊ณก์ํค๋ ๋ฌธ์ ๊ฐ ์์ด GAN์ ์ฌ์ฉํ๋ค๊ณ  ํ๋ค.

์ด๋ ๊ฒ ๋ฐ๊ธฐ ์ฒ๋ฆฌํด์ค ์ผ๊ตด ์ด๋ฏธ์ง๋ฅผ ๋ค์ MTCNN์ ํตํด ์์ชฝ ๋๊ณผ ์์ชฝ ์์ ์ ๋๋๋งํฌ๋ฅผ ์ฐพ์์ ๋์์ half lower crop!, ์์์ half upper ๊น์ง crop! ํด์ค ํ ๋ค์ 224x224๋ก resizeํด์ค๋ค.


<br>

## Backbone Network with Spatial-Attention

๋ค์์ ResNet18 backbone ๋คํธ์ํฌ ๊ตฌ์กฐ์ด๋ค. ๋ผ๋ฌธ์์ ์ ์ํ๋ ์ด๋ฏธ์ง๊ฐ ํท๊ฐ๋ ค์ ๋ค์ ์์ ํด์ ์ ๋ฆฌํด๋ณด์๋ค.

![image](https://user-images.githubusercontent.com/53431568/148376953-423d2542-68bb-4914-870e-d3f23add2266.png)


input์ผ๋ก๋ 224x224x9 ๋ก ์ด๋ฏธ์ง ์ ์ฒ๋ฆฌ์์ ์ป์ face, eyes, mouth ์ธ ์ฅ์ ์ด๋ฏธ์ง๋ฅผ ์๋ ฅํ๋ค. ์ด ๊ตฌ์กฐ๋ group-convolution์ ์ฌ์ฉํ์ฌ ๋๋ฆฝ์ ์ธ ์ฐ์ฐ์ ์ํํ๋ค๊ณ  ํ๋ค. (์ด๋ฏธ์ง๋ก๋ 3๊ฐ์ง ๋ชจ๋ธ์ ์ฌ์ฉํ๋ ๊ฒ ์ฒ๋ผ ๋ณด์ด์ง๋ง ํ๋์ ๋ชจ๋ธ๋ก ๋๋ฆฝ์ ์ธ ์ฐ์ฐ์ ์ํํ๋ ๊ฒ..!) ๋ณด๋ฉด, ์ฐ๋์ ํ๋๋ฆฌ์ BOX๊ฐ Residual block์ด๊ณ , ๊ฐ Residual block์์ SA(Spatial Attention) ๋ฅผ ์ํํ์ฌ 1์ฐจ์์ feature๋ฅผ ๋ฝ์์ 4๊ฐ์ block์์ ๋ฝ์ feature๋ค์ concat ์์ผ์ 960๊ฐ์ feature vector์ ๋ฝ์๋ธ๋ค. ์ด๋ ๊ฒ ๋ฝ์ feature๋ ์ด๋ฏธ์ง์์ ์ด๋ ๋ถ๋ถ์ด ์ค์ํ์ง์ ๋ํ ์ ๋ณด๋ฅผ ๊ฐ์ง๊ณ  ์๋ค.

### - Spatial Attention ์ฐ์ฐ

![image](https://user-images.githubusercontent.com/53431568/148377565-6bd267cc-9f5d-41a8-9adf-a0f703180803.png)

$W_sl$๊ณผ $W_s2$๋ ๊ฐ๊ฐ ๊ฐ์ค์น ํ๋ ฌ๊ณผ ๋ฒกํฐ์ด๋ค. $L$์ 2D-tensor๋ก ์ฐจ์์ ๋ณ๊ฒฝํด์ค channel์ด๋ผ๊ณ  ๋ณด๋ฉด ๋๋ค. ์์ธํ๊ฑด ๋ ๊ณต๋ถํด์ผ๊ฒ ์ง๋ง, ์์์ ๋ณด๋ฉด attention ๊ตฌํ๋ ์์๊ณผ ๋์ผํ๋ฐ ์ข ๋ค๋ฅธ ๊ฒ์ ์ ์ ์๋ค. attention์ ๋ณธ๋, fc ๋ ์ด์ด์์ ์ฐ์ฐ์ ํด์ฃผ์๋ ๋ฐ๋ฉด, ์ฌ๊ธฐ์๋ ํ๋ ฌ๊ณฑ์ผ๋ก ์ฐ์ฐ์ ํ๋ค. ๋๊ฐ ์์ธํ ์์๋๋ถ ์์ผ๋ฉด ์ฐ๋ฝ์ข.. ์ฃผ์์

- [attention is all you need ๋ผ๋ฌธ ์ฐธ๊ณ ](https://chaelin0722.github.io/paperreview/attention/) ๐
- [FAN](https://chaelin0722.github.io/fer/FAN/)๐
- [Audio-Video emotion recognition](https://chaelin0722.github.io/fer/Audio_video_fer/)๐


<br>

## channel Attention

![image](https://user-images.githubusercontent.com/53431568/148377763-19a17b3d-9067-4c72-838f-a9bb53bea686.png)

๊ฐ face, eyes, mouth์์ ๋ฝ์ 960feature ๋ก attention์ฐ์ฐ์ ํตํด ํ๊ท ์ ๋ธ ํ๋์ 960feature์ ๋ฝ๊ฒ ๋๋ค. ์ด๋ฅผ ๋ค์ 512 feature๋ก ์ค์ด๋ฉด ์ด feature๊ฐ `ํ ํ๋ ์์ feature`์ด ๋๋ค


์ด๋ ๊ฒ ๊ฐ ํ๋ ์์ ๋ํ ํ๊ท  feature๋ค์ ๊ณ์ฐํด ๊ตฌํด๋ด์ด 512 feature๋ฅผ ๋ง๋ค๊ฒ ๋๋ ๊ฒ์ด๋ค.

<br>

## Frame Attention

![image](https://user-images.githubusercontent.com/53431568/148379183-1fb6cee9-7974-4e66-8cf0-67236f075917.png)


๊ฐ ํ๋ ์์ ๋ํ 512 feature ์ ๋ํด์ ๋ ๋ค์ attention! ๊ทธ๋ฆฌ๊ณ  ์ต์ข์ ์ผ๋ก 7๊ฐ์ label์ ๋ํด classification ํด์ค๋๋ค.


## Noisy student training

![image](https://user-images.githubusercontent.com/53431568/148379263-f5966ab2-dbb2-41b3-91e1-88aa57facda0.png)

AFEW ๋ฐ์ดํฐ๋ฅผ ํ์ธํด๋ณด๋ ํ์คํ ์์ด ์ ์๋ค. ์ด๊ฑธ๋ก๋ง ํ์ตํ๋ฉด ์ฑ๋ฅ์ด ์ ์๋์ค๊ธด ํ ๊ฒ ๊ฐ๋ค ใใ ๊ทธ๋์ ์ด ๋ผ๋ฌธ์์ ์ ์ํ๊ฒ, Unlabelled๋ ๋ฐ์ดํฐ๋ฅผ ๊ฐ์ ธ์์ pseudo label์ ํ ํ ํ์ต์ํจ ๋ชจ๋ธ๋ก ์ฌ์ฉํ๋ ๊ฒ(noisy student ๊ฐ๋) ์ธ๋ฐ, ์ฌ๊ธฐ์๋ unlabel ๋ ๋ฐ์ดํฐ๋ฅผ BoLD(BodyLanguage Dataset)์ ์ฌ์ฉํ์๋ค. 

์์๋ ๋ค์๊ณผ ๊ฐ๋ค.

1. AFEW8.0์ผ๋ก ํ์ตํ๋ค.
2. ํ์ตํ ๊ฒ ์ค ๊ฐ์ฅ best model๋ก BoLD ๋ฐ์ดํฐ์ pseudo label์ ์งํํ๋ค.
3. AFEW8.0์ ๋ํ์ฌ label์ด ์์ฑ๋ BoLD ๋ฐ์ดํฐ๋ฅผ ํฉํ ๋ฐ์ดํฐ(์ต์ข๋ฐ์ดํฐ์)๋ก ํ์ต์ ์ํค๋๋ฐ ์ฌ๊ธฐ์ + noise๋ฅผ ์ถ๊ฐํด์ค๋ค
4. ์ต์ข ๋ฐ์ดํฐ์์ผ๋ก 3๋ฒ 4๋ฒ ๋ฐ๋ณตํ๋ค.

(์ฌ์ฉํ noise์๋ dropout(0.5), contrast, brightness, translation, sharpness, flips ๋ฑ์ ๋๋ค์ผ๋ก ์ฌ์ฉํจ)

๐ => [Noisy Student ์ฐธ๊ณ ](https://chaelin0722.github.io/paperreview/noisy_student/)

<br>

## result

![image](https://user-images.githubusercontent.com/53431568/148380117-3aa18be1-bb5b-45d1-85b9-26426772674a.png)

๊ฐ face, mouth, eyes ์ ๋ํด์ afew8.0์ผ๋ก ์ฑ๋ฅ์ธก์ ํ ๊ฒฐ๊ณผ์ธ๋ฐ์, ๊ฐ์ ํํ์ ์์ด์ ๋์ด ๋ง์ด ์ฐ์ด๋ sad๋ ์ญ์ eyes ์์ ์ฑ๋ฅ์ด ์ข๊ณ , happy์ angry ๊ฐ์ด ์์ ํํ์ด ์ค์ํ ๊ฐ์ ์ mouth์์ ์ฑ๋ฅ์ด ์ข๊ฒ ๋์จ ๊ฒ์ ํ์ธํ์์ต๋๋ค. ํ์ง๋ง best ๋ ์ด ์ธ๊ฐ์ง๋ฅผ ๋ชจ๋ ํฉํด์ ์ฌ์ฉํ๋ ๊ฒ! ์ด๋ผ๊ณ  ์ฃผ์ฅํ๋ค์

<br>

๋ค์์ iteration์ ๋ฐ๋ณตํ๋ฉด์ ์ฑ๋ฅ์ด ํฅ์๋จ + ๊ท ํ๋ง์ถ unlabelled ๋ฐ์ดํฐ์ ์ค์์ฑ์ ๋ณด์ฌ์ฃผ๋ ์ ๋๋ก ํด์ํ๋ฉด ๋  ๊ฒ ๊ฐ์ 

![image](https://user-images.githubusercontent.com/53431568/148380630-9e5fa58e-d4ec-431d-b26e-dc6179619562.png)

<br>

CK+๋ฐ์ดํฐ์์  99.69%, AFEW ์์๋ 55.17%์ ์ฑ๋ฅ์ ๋ฌ์ฑํ์๋ค๋ ๊ฒ์ ์ ์ ์์ต๋๋ค.

![image](https://user-images.githubusercontent.com/53431568/148380729-bed6ae8e-fff7-44e1-99ad-b500ccdd46b6.png)



component importance๋ฅผ ๋ณด์ฌ์ค๋๋ค. ์ญ์ ์ด๊ฒ์ ๊ฒ ๋ค ๋ถ์ด๊ณ  training๋ ๋ง์ด ๋ฐ๋ณตํ๊ฒ ์ฑ๋ฅ์ด ์ข๊ฒ ๋์ค๋ค์. 

![image](https://user-images.githubusercontent.com/53431568/148380836-3a1fd208-7ae3-43c1-95db-91dacf7ab065.png)

๊ทธ๋ผ์๋ AFEW8.0 ๋ฐ์ดํฐ ๊ธฐ์ค์ผ๋ก 6๋ฑ์ด๋ผ๋๊ฒ.. ์ด๋ ๊ฒ ๋ค ๋ถ์ฌ๋ ๊ฒฐ๊ตญ multi-modal model์ ์ด๊ธธ ์ ์๋ค๋... ๊ณต๋ถํ๋ฉด์ ๋ง์ ์๊ฐ์ ํ๊ฒ ํ๋ ๋ผ๋ฌธ์ด์์ต๋๋ค. ์ต์  ๊ธฐ๋ฒ๋ค(GAN๊ณผ attention 3๊ฐ์ง)์ ๋ชจ๋ ๋ค ๊ฐ๋ค ์ฐ๊ณ  ์ฌ์ง์ด extra dataset๊น์ง ์ฌ์ฉํ์๋๋ฐ AUDIO๋ฅผ ๊ฐ์ด ์ฌ์ฉํ model๊ณผ 10% ์ฉ์ด๋ ์ฐจ์ด๊ฐ ๋๋ค. visual ๋ง์ผ๋ก๋ ํ๊ณ๊ฐ ์๋ ๊ฒ์ธ์ง ์๋๋ฉด ์๋ก์ด ์๊ณ ๋ฆฌ์ฆ์ ์ ์ํ๊ณ  ๋ฐฉํฅ์ ์ ํํ๋ ๊ฒ์ ๊ณ ๋ฏผํด๋ด์ผ๊ฒ ๋ค..!

์ค๋๋ ๋ฌด์ฌํ ์ธ๋ฏธ๋ ์๋ฃ!๐ฝ 


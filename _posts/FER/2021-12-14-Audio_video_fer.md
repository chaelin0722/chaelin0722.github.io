---
title:  "[Paper Review๐] Exploring Emotion Features and Fusion Strategies for Audio-Video Emotion Recognition"
excerpt: "-Audio-Video Emotion Recognition-"

categories:
  - fer
tags: [fer,CNN, paperReview]
use_math: true

last_modified_at: 2021-12-14T08:06:00-05:00
classes: wide
---

## Exploring Emotion Features and Fusion Strategies for Audio-Video Emotion Recognition
#### - Audio-Video Emotion Recognition - 

[Paper](https://arxiv.org/pdf/2012.13912.pdf)๐ 2021๋ 12์ 14์ผ ๊ธฐ์ค, AFEW ๋ฐ์ดํฐ๋ก SOTA ์ฑ๋ฅ ๋ฌ์ฑ

#### ์ด ๋ผ๋ฌธ์ audio ์ video ๋ ๊ฐ์ง modal ์ fusion(ํผํฉ) ํ์ฌ SOTA ์ฑ๋ฅ์ ๋ฌ์ฑํ์๋ค.

์ด ๋ผ๋ฌธ์ ๋ฉ์ธ CONTRIBUTIONS ๋ฅผ ์ดํด๋ณด๋ฉด,

1) faceCNN์ ์ ์ดํ์ต์ ์๋ง์ ๋ฐ์ดํฐ์๊ณผ ์๋ง์ ๋ชจ๋ธ์ ์ฌ์ฉํ๋ ๊ฒ์ด ์ค์ํ๋ค (๋ชจ๋  FER ๋ผ๋ฌธ์์๋ ๋งํ์ง๋ง,, ์ผ๊ตด ์ธ์ ๋ชจ๋ธ์ ์ pre-train ์ํค๋ ๊ฒ์ด ์ฑ๋ฅ์ฌ๋ฆฌ๋ ๊ฒ์ ํต์ฌ์ด๋ผ๊ณ  ๊ฐ์กฐํ๋ค์)

2) visual feature๊ณผ audio feature์ ํผํฉ ๊ธฐ๋ฒ์ ์ํด ์ธ๊ฐ์ง attention ๋งค์ปค๋์ฆ์ ๊ณ ์ํ์๋ค.
 
3) cross-modal feature ํผํฉ์ ์ํด factorized bilinear Pooling(FBP)๊ธฐ๋ฒ์ ์ ์ฉํ์๋ค. (cross-modal ์ด๋ผ๋ ๊ฒ์ audio modal ๊ณผ video modal ์ ํผํฉํ๋ ๊ฒ์ ๋ปํ๋ค๊ณ  ์๊ฐํ๋ฉด ๋  ๊ฒ ๊ฐ๋ค.)

<br>

### Pipeline of audio-video emotion recognition

๋ผ๋ฌธ์์ ์ ์ํ๋ ๊ตฌ์กฐ๋ ์๋์ ๊ฐ์ต๋๋ค.

![image](https://user-images.githubusercontent.com/53431568/146122054-6baeb1fe-4bd2-4828-8aa6-c1013ef014ac.png)

์ดํด๋ณด๋ฉด, ๊ฐ audio ์ video ์ด๋ฏธ์ง์ ๋ํด ์ ์ฒ๋ฆฌ๋ฅผ ์ํํ ํ ๊ฐ๊ฐ์ CNN ๋ชจ๋ธ์ ์ฌ์ฉํด feature์ ๋ฝ์ ๋ค์์ attention ๋งค์ปค๋์ฆ์ ์ฌ์ฉํด ๋์จ ๊ฐ์ fusion ์ ํ์ฌ ์ต์ข feature ์ ๋ฝ๊ฒ ๋ฉ๋๋ค.

๊ฐ ํ๋ก์ธ์ค ๋ณ๋ก ์์ธํ ์ดํด๋ด์๋ค..!
<br>

### Preprocessing

์ค๋์ค์ ์ด๋ฏธ์ง ์ ์ฒ๋ฆฌ์ ์์ด์๋ ๊ฐ๋ตํ๊ฒ ๋ผ๋ฌธ ๋ด์ฉ์ ๊ทธ๋๋ก ์ฐ๊ฒ ์ต๋๋ค.

- AUDIO

> Speech spectrogram -> use hamming window with 40msec window size, 10msec shift
>  
> Log mel-spectrogram -> calculate its deltas and delta-deltas

- VIDEO 

> use dlib toolbox for face detection and alignment
> 
> Extend face bbox with ratio of 30%
>  
> Crop face and scale to 224 x 224

 (ํ ํ๋์ ๋ด์์ ์ผ๊ตด์ด ๊ฒ์ถ๋์ง ์๋๋ค๋ฉด ๋ชจ๋  ํ๋ ์๋ค์ ์ฌ์ฉํ์ง ์๋๋ค.)

<br>

### Feature Extraction

<img width="288" alt="แแฎแแฆ" src="https://user-images.githubusercontent.com/53431568/146126478-e05ec970-0197-49a9-86df-d4c070e00f78.png">

์ ์ฒด ๊ตฌ์กฐ์์ ๋ดค๋ฏ์ด, audio ์ visual ์ด๋ฏธ์ง์ ๋ํด ๊ฐ๊ธฐ ๋ค๋ฅธ CNN ๋ชจ๋ธ์ ์ฌ์ฉํ์ฌ feature ๋ฅผ ๋ฝ๊ณ  ์์ต๋๋ค. ๋จผ์ , audio ์ ๊ฒฝ์ฐ AlexNet์ ๋ง์ง๋ง pooling layer์์ feature ๊ฐ์ ๋ฝ์์ HxWxC ์ ํผ์ณ ๋งต ํํ๋ก ์ถ์ถ์ด ๋ฉ๋๋ค.

ํํธ, visual์ ๊ฒฝ์ฐ, ๋ง์ง๋ง FC๋ ์ด์ด์์ ๋์จ ๊ฐ์ ํผ์ณ๋ก ๋ฝ๊ธฐ ๋๋ฌธ์ n๊ฐ์ ๋ฒกํฐ ํํ๋ก ๊ฐ์ด ๋์ค๊ฒ ๋ฉ๋๋ค. (๋ผ๋ฌธ์์๋ VGGFace, ResNet18, IR50 ์ธ ๊ฐ์ง ๋ชจ๋ธ์ ๋ํด์ ์คํ์ ํ์๊ณ  ๊ทธ์ค IR50์ด ๊ฐ์ฅ ์ข์ ์ฑ๋ฅ์ ๋ด์ด์ ์ต์ข ๊ตฌ์กฐ ์ด๋ฏธ์ง์๋ IR๋ก ์์ฑํ ๊ฒ ๊ฐ์ต๋๋ค.)


### Three Attention machanism

์ด์  ํ๋ก์ธ์ค์ feature ์ ํผํฉํ๋ ๋ชจ๋์์ intra-modal ๋ถ๋ถ์ ์ดํด๋ด์๋ค. ์ฌ๊ธฐ๋ audio ์ video ๋ ๊ฐ๊ฐ ๊ณ์ฐํ๊ณ  ์์ต๋๋ค. ์์ง๊น์ง ๋์ ํฉ์น์ง๋ ์๋ค์.

attention์ ์์, ์ ์ ๋ ๊ฒ ๊ฐ์ค์น์ ๋ค์ ์ ๋์ฌ๊ฒ์ด๋ผ ์์๋๋ FC ๋ ์ด์ด(์๋ ์์์์๋ $W$)๋ฅผ ๋ด์ ํ์ฌ ์ ์ฌ๋๋ฅผ ๊ตฌํ๋ ์ง๋ [FAN ๋ผ๋ฌธ ๋ฆฌ๋ทฐ์์๋ ๋ฆฌ๋ทฐํ์ผ๋ฏ๋ก ์ฐธ๊ณ ํ๊ธฐ!](https://chaelin0722.github.io/fer/FAN/)

![image](https://user-images.githubusercontent.com/53431568/146122320-46987964-2bac-40dc-8457-72d267ca94ea.png)

self-attention๊ณผ relation-attention ์ธ์ transformer-attention ์ด๋ผ๋ ๊ฐ๋์ด ์ถ๊ฐ๋์๋ค! ์๋ก์ด ๊ฐ๋ ์์๊ฐ๋๊ฑฐ ๋๋ฌด ์ฌ๋ฐ๋ค์๐๐

trasnformer-attention์ ๊ธฐ์กด attention๊ณผ ๋ง์ฐฌ๊ฐ์ง์ ๊ฐ๋์ ๊ฐ๊ณ ๊ฐ๋๋ฐ, feature์ ์ฐจ์์ ์ค์ด๊ธฐ ์ํด ๋ค์๊ณผ ๊ฐ์ linear function ๊ณผ์ ์ ๊ฑฐ์ณ์ ์ฐจ์์ ๊ฐ์์ํจ ํ, ์ ์ฌ๋๋ฅผ ๊ตฌํ๊ณ  ์์ต๋๋ค.

$W_{m \times d}$๋ฅผ ํผ์ณ๊ฐ๊ณผ ๊ณฑํด์ฃผ์ด ์ฐจ์์ ์ค์ธ ๊ฐ์ $f^{`}_i$ ๋ก ์ค์ ํ์ฌ ๊ทธ ๊ฐ์ผ๋ก ๋ด์ ์ํจ ๊ฐ์ ์ ๊ณฑ์ผ๋ก ์ทจํ์ฌ ๊ฐ์ค์น๋ก ์ฌ์ฉํ๊ณ  ์์ต๋๋ค.


<br>

### FBP(Factorized Bilinear Pooling) module

![image](https://user-images.githubusercontent.com/53431568/146133144-30b4bde3-611e-4ffb-939e-2e2c6d52996c.png)

์์ ๊ฐ๋์ ์ดํดํ๊ธฐ ์ , Bilinear Pooling์ด ๋ฌด์์ธ์ง๋ฅผ ๋จผ์  ์ดํด๋ณด๊ฒ ์ต๋๋ค.

**Bilinear pooling** ์ speech ์ visual๊ณผ ๊ฐ์ด ์๋ก ๋ค๋ฅธ modal ์ (์ฐจ์์ด ๋ค๋ฅธ ๋ฒกํฐ ๊ณ์ฐ) ๋ชจ๋  feature๋ค์ด ์ํธ์์ฉํ์ฌ ๊ณ์ฐํ  ์ ์๋๋ก ํ๋ ๋ฐฉ๋ฒ์๋๋ค. 

๋ค์๊ณผ ๊ฐ์ด audio feature vector $a$ ๋ m ์ฐจ์์ ์กด์ฌํ๊ณ , video feature vector ์ $n$ ์ฐจ์์ ์กด์ฌํฉ๋๋ค. ๋ ๋ค๋ฅธ ์ฐจ์์์ ๊ณ์ฐ์ ํ๋ ค๋ฉด ์ด๋ป๊ฒ ํด์ผํ ๊น์? ์ฌ๊ธฐ์๋ projection matrix๋ฅผ ์ฌ์ฉํฉ๋๋ค.

![image](https://user-images.githubusercontent.com/53431568/146133216-a4bd7428-59a1-4d71-95cc-190d8cadfe2e.png)

์๋ ์์๊ณผ ๊ฐ์ด a์ v ๋ฅผ ๋ด์ ํ๊ธฐ ์ํด ๊ฐ์ด๋ฐ mxn ์ฐจ์์ ์กด์ฌํ๋ projection matrix ์ธ $W$ ๋ฅผ ์ถ๊ฐํ์ฌ์ ๊ณ์ฐ์ ํด์ฃผ๊ณ  ์์ต๋๋ค.

![image](https://user-images.githubusercontent.com/53431568/146133384-c580227a-77f4-4eaa-bcbf-a615512825ba.png)

์ด๋ ๊ฒ ๋ ๋ฒกํฐ์ ๋ชจ๋  ์์์ ๋ํด์ ์ํธ์์ฉํ๋ฉฐ ๊ณ์ฐํ  ์ ์์ด์ ๋ชจ๋  element๊ฐ์ ์ํธ ๊ด๊ณ์ฑ์ ํ์ํ  ์ ์๋ค๋ ์ฅ์ ์ด ์์ต๋๋ค. ํ์ง๋ง, ๊ทธ๋ฆผ์์๋ ๋ณด์ด๋ z ์ ๋ฉด์ ! ๋งค์ฐ ํฐ ๊ฒ์ ์ ์ ์์ฃ ! ์ฐ์ฐ์ ๋น์ฉ์ด ๋งค์ฐ ํฌ๊ณ  overfitting์ ์ด๋ํ  ์ํ์ด ์์ต๋๋ค.

๋ฐ๋ผ์ ์ด ๋ฌธ์ ์ ์ ํด๊ฒฐํ๊ธฐ ์ํด ์ ์๋ ๋ฐฉ๋ฒ ์ค ํ๋๊ฐ FBP ์๋๋ค.

์๋๋ ๋ผ๋ฌธ์ ์์์ ์ ๋ฆฌํด ๋ณธ๊ฒ์ธ๋ฐ์.. ์ด ๋ผ๋ฌธ์ ์์ด์ FBP ์ ๋ด์ฉ์ ๊ฐ๋ตํ๊ฒ ๋์์ [๋ค์ ๋ผ๋ฌธ์ ์ฐธ๊ณ ํ์ฌ ์์ฑํ์์ต๋๋ค.](https://arxiv.org/pdf/1901.04889.pdf)

![image](https://user-images.githubusercontent.com/53431568/146133724-059c4e38-4441-4df8-b1f3-83f945d645c2.png)

์์ ์์์ ๋ณต์กํด๋ณด์ด์ง๋ง ๋จ์ํ ์๊ฐํด๋ณด๋ฉด, Bilinear pooling์ ๋จ์  ๋๋ฌธ์, matrix factorization tricks ์ ์ฌ์ฉํด the projection matrix $W_i$ ๋ฅผ ์์ ๊ฐ์ด ๋๊ฐ์ low- rank์ ํ๋ ฌ๋ก ๋ฐ๊พธ์ด ๊ณ์ฐํ๋ ๊ฒ์๋๋ค.




### Exploration of Emotion Features

Table 1 ์ VGGFace, ResNet18, IR50 ์ธ CNN ๋ชจ๋ธ๊ณผ FER+, RAF-DB, AffectNet ๋ฐ์ดํฐ๋ค์ ๋ํด ๊ฐ๊ฐ ํ์ตํด ์คํํ ๊ฒฐ๊ณผ, IR50์ AffectNet ๋ฐ์ดํฐ์์ผ๋ก ์ ์ดํ์ตํ ๋ชจ๋ธ์ ์ฑ๋ฅ์ด ๊ฐ์ฅ ์ข๋ค๋ ๊ฒ์ ๋ณด์ฌ์ค๋๋ค.

![image](https://user-images.githubusercontent.com/53431568/146134108-3d75cf92-8ce6-48e3-9dd6-38db074a657d.png)
 
Table 2 ๋ Audio์ Visual์ ๋ํด์ ๊ฐ self, relation, transformer attention์ ์กฐํฉํด ์ฌ์ฉํ ๊ฒฐ๊ณผ๋ค ์ค, ๊ฐ์ฅ ๋์ ์ฑ๋ฅ์ ๋ณด์ด๋ ๊ฒ์ ๋ modal์ ์์ด์ transformer ์ ์ฌ์ฉํ๋ ๊ฒ์์ ๋ณด์ฌ์ค๋๋ค.

![image](https://user-images.githubusercontent.com/53431568/146134304-e3694a02-6289-4a8d-8fe1-82d5e5cd034c.png)


์ด๋ฒ์ฃผ ์ธ๋ฏธ๋๋ ๋ฌด์ฌํ ์๋ฃ..! ๐ฝ







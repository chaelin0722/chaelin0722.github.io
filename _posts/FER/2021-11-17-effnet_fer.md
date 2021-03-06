---
title:  "[Paper Review๐] Facial expression and attributes recognition based on multi-task learning of lightweight neural networks"
excerpt: "-Multi-task EfficientNet-B2-"

categories:
  - fer
tags: [fer,CNN, paperReview]
use_math: true

last_modified_at: 2021-11-17T08:06:00-05:00
classes: wide
---

## Facial expression and attributes recognition based on multi-task learning of lightweight neural networks
#### - Multi-task EfficientNet-B2 - 

[Paper](https://arxiv.org/abs/2103.17107)๐


#### ์ด ๋ผ๋ฌธ์ ๋น๋์ค(์์)์ ํ๋ ์์ ์ฌ๋ฌ๊ฐ ๋ฐ์ ๊ฐ ํ๋ ์๋ง๋ค์ mean, std, min, max ๊ฐ์ ๊ณ์ฐํ์ฌ ์ผ๊ตดํ์ ์ ์ธ์ํ๋ ๋ฐฉ๋ฒ์ ์ ์ํ๊ณ  ์์ต๋๋ค.

### ํน์ง

- ์ผ๊ตด ์์ฑ(๋์ด, ์ฑ๋ณ, ๊ตญ์ )์ ๋ถ๋ฅ์ ์ผ๊ตด ์ธ์ง๋ฅผ ํ์ตํ๊ธฐ ์ํด์ ๋ง์ง์ด ์๋ crop ๋ ์ผ๊ตด์ด๋ฏธ์ง๋ฅผ input๊ฐ์ผ๋ก ํ์ต

- ๋น๊ต์  ๋น ๋ฅด๊ณ  ๊ฐ๋ฒผ์ด MobileNet, EfficientNet, RexNet architecture ๋ฅผ baseline network๋ก ์ฌ์ฉํ๋ค.

- ์ผ๊ตด ํ์ (๊ฐ์ )์ ์์ธกํ๋๋ฐ ์์ด์ ๋คํธ์ํฌ๋ฅผ fine-tuning ํ๋ ๊ฒ์ ์ค์์ฑ์ ๊ฐ์กฐํ๊ณ  ์์

์ด ๋ผ๋ฌธ์์๋ **Multi-task networks** ๋ผ๋ ๊ฒ์ ์ ์ํ๋๋ฐ ๊ตฌ์กฐ๋ฅผ ๋ณด๋ฉด ์๋์ ๊ฐ์ ๊ตฌ์กฐ๋ก ์ด๋ฃจ์ด์ ธ ์๋ค.

![image](https://user-images.githubusercontent.com/53431568/142212302-f1d4738b-c63c-43e3-b990-e8f5445eaafd.png)

ํ๋์ฉ ์ดํด๋ณด์๋ฉด, ์์์ ์ธ๊ธํ MobileNet, EfficientNet, RexNet์ facial recognition CNN(์ผ๊ตด์ธ์ ๋ชจ๋ธ)์ backbone network๋ก ์ฌ์ฉ์ ํ๊ณ ์๋ค. ๊ทธ๋ฆฌ๊ณ  
์ด CNN์ ๊ฑฐ์ณ์ ๊ฐ๊ฐ ์ฑ๋ณ, ๋์ด, ๊ตญ์ , ๊ฐ์  ์ด 4๊ฐ์ง์ ๋ํด ์์ธก์ ์งํํ๊ณ  ์๋ ๊ฒ์ ๋ณผ ์ ์๋ค.

๋จผ์ , backbone architecture(Face recognition CNN) ์์ ์ํํ๊ธฐ ์ํด ๋ค์ด์ค๋ input image ๋ ๋ค์ด์จ ํ๋ ์์์ ์ผ๊ตด์ ์ฐพ์ ์ผ๊ตด๋ถ๋ถ๋ง์ ์๋ผ์ 244x244 ํฌ๊ธฐ์ ๋ง๊ฒ ์ด๋ฏธ์ง๋ฅผ ๋๋ฆฌ๋ ์์์ ํ๋ค.(๋ง์ง์ด ์๊ฒ๋ crop ํจ) ์ด๋ MTCNN face detection์ ์ฌ์ฉํ์ฌ ์ผ๊ตด์ ์ฐพ๋๋ก ํ๋ค. MTCNN์ ๋๋ฆฌ๋ค๋ ํ๊ฐ๊ฐ ์์ด์ ์ค์๊ฐ์ผ๋ก ๋ณผ๋ facebox ๋ฅผ ์ฌ์ฉํด๋ณผ๊น ์๊ฐ์ค์ด๋ค.

![image](https://user-images.githubusercontent.com/53431568/142213131-011bb93c-911b-473f-a639-229aba7cf671.png)



SOTA ์ฑ๋ฅ์ ๋ค์๊ณผ ๊ฐ๋ค.

![image](https://user-images.githubusercontent.com/53431568/142211562-5d7b1829-9f16-43b1-9a70-70938aea486a.png)

emotion 7๊ฐ์ ๋ํด์ AFFECT ๋ฐ์ดํฐ์์ผ๋ก 66.34% ๊น์ง ์ฑ๋ฅ์ ๋์ด์ฌ๋ ธ๋ค.

์ฌ๊ธฐ์ ๋ ์ง๊ณ  ๋์ด๊ฐ ๋ถ๋ถ์ ๋์ด๋ฅผ ๋ง์ถ๋ layer ๊ฐ ์ฑ๋ณ๊ณผ ๋ฏผ์กฑ์ layer ๋ณด๋ค ๋ ๊น๋ค๋ ๊ฒ์ด๋ค. ๊ทธ ์ด์ ๋ ์ฑ๋ณ๊ณผ ๋ฏผ์กฑ๊ฐ์ ๊ฒฝ์ฐ ๊ทธ ์นดํ๊ณ ๋ฆฌ๊ฐ ๋ช ๊ฐ ์์ง๋ง,
๋์ด์ ๊ฒฝ์ฐ 1์ด๋ถํฐ N ์ด๊น์ง ๋งค์ฐ ๋ฒ์๊ฐ ์์ํ๊ณ  ๋ง๊ธฐ ๋๋ฌธ์ ๋ ์ด์ด๋ฅผ ๋ ์ถ๊ฐํด์ฃผ์๋ค๊ณ  ํ๋ค.

๋, ๊ฐ์ ์ธ์์ ์ํด์๋ ์์์๋ ์ธ๊ธํ๋ฏ์ด finetuned layer์ ์ฌ์ฉํด ํ์ตํ๋ ๊ฒ์ด ์ฑ๋ฅ์ด ๋ ์ข๊ธฐ ๋๋ฌธ์ ์ด ๋ถ๋ถ์ ํ๋ฒ ๊ฑฐ์น๊ณ  ์์ธก์ ์งํํ๊ฒ ๋๋ค.

![image](https://user-images.githubusercontent.com/53431568/142213370-ecad3b7a-bcb1-4c6e-9f0b-4fffbfce881a.png)

์์ ๊ทธ๋ฆผ์ ์ด๋ก์ ๋ฐ์ค๋ถ๋ถ์์ frozen weight ๋ผ๋ ๊ฒ์ ํ๋ค. weight๋ฅผ frozen ํ๋ค๋ ๊ฒ์ ๊ธฐ์กด ๋คํธ์ํฌ ๊ตฌ์กฐ์ pretrained ๋ weight๋ฅผ ๊ฐ์ ธ์์ ํ์ต์ ์ํฌ ๋, ๋ด๊ฐ ๋ง๋๋ ์๋ก์ด weight ๋ก ๊ฐ์ update ์ํค๊ธฐ ๋ณด๋ค๋ ๊ธฐ์กด์ ์๋ weight๋ฅผ ๊ทธ๋๋ก ํ์ต ์ํค๋ ๊ฒ์ด ํ์ตํ  ๋ ์ฑ๋ฅ์ด ๋ ์ข๊ธฐ ๋๋ฌธ์ ์ฌ์ฉํ๋ ๊ธฐ๋ฒ์ด๋ผ๊ณ  ํ๋ค.

์ผ๋จ fronzen ์ ๋ํด์๋ ์ข๋ ๊ณต๋ถํด์ผ ๊ฒ ์ง๋ง. ๋ด ์๊ฐ์๋, ์ด๋ฏธ SOTA ์ฑ๋ฅ์ ์ด๋ค๋ธ ๋คํธ์ํฌ ๊ตฌ์กฐ์ด๊ณ , ์ด ๊ตฌ์กฐ๋ก ํ์ต๋ weight ๊ฐ๋ค์ ๊ทธ ๊ตฌ์กฐ์ ์ ํฉํ ๊ฐ์ ๊ฐ์ง๊ณ  ์์ ๊ฒ์ด๋ผ๊ณ  ์๊ฐํ๋ค. ๊ทธ๋ ๋ค๋ฉด ์ด ๊ฐ์ ๊ทธ๋๋ก ์ฐ๋๊ฒ ์๋ฌด๋๋ ์ข๊ฒ ์ฃ ?! 

๋ฐ๋ผ์ ์ด ๋คํธ์ํฌ์์๋ face recogntion CNN ๋ถ๋ถ์ update ํ๋ ๋์ ๋๋จธ์ง ๊ตฌ์กฐ๋ frozen ์ํค๊ณ  ๋ช๋ฒ์ ๋ ํ์ตํ ์ดํ์ ํจ๊ป ํ์ต์์ผ weight๋ฅผ update ํ๋ค.
frozen ํด์ update ํ๋ ํ์๋ ๊ฐ๊ฐ ์ฑ๋ณ, ๋์ด, ๋ฏผ์กฑ, ๊ฐ์ ์ ๋ ์ด์ด์ ๋ฐ๋ผ ๋ค๋ฅด๋ ๋ผ๋ฌธ ์ฐธ๊ณ !

<br>

### Emotion Recognition process

๊ฐ์ ์ ์ธ์ํ๋ ํ๋ก์ธ์ค๋ฅผ ๊ฐ๋จํ๊ฒ ๊ทธ๋ฆผ์ผ๋ก ์ ๋ฆฌํด ๋ณด์๋ค. ๋ค์๊ณผ ๊ฐ์ด ๋ชจ๋  sequence ํ๋ ์ (์ํ์ค ๋ฐ์ดํฐ๊ฐ ์๋๋ผ๋ฉด ๊ทธ๋ฅ ํ๋ ์์์๋ ์งํ) ์ ์๋ ์ผ๊ตด์ ์ฐพ์ 244x244 ๋ก ๋ง์ง ์์ด crop์ ํ๋ค. ๊ทธ๋ ๊ฒ ์ฐพ์๋ธ ์ผ๊ตด์ ์ด๋ฏธ์ง์ ๋ํด์ feature์ ์ถ์ถํด feature๊ฐ์ ๋ํด mean, std, min, max ๊ฐ์ ๊ณ์ฐํด ๊ฐ ๊ณ์ฐ์ concatenate ์์ผ์ค๋ค. 

![image](https://user-images.githubusercontent.com/53431568/142214744-6b0adf3f-0266-4783-93f8-363c9984b2bc.png)


<br>

์ฝ๋๋ก ๋ณด๋ฉด ์ดํด๊ฐ ๋น ๋ฅด๋ค ์๋ ์ฝ๋ ์ฐธ๊ณ !

![image](https://user-images.githubusercontent.com/53431568/142215150-933f4e92-5279-4c9c-a9dd-a0de7616e322.png)


์์ ์์ด์  ์ผ๊ตด๋ก ์ํํด ๋ณด์๋ค.

![image](https://user-images.githubusercontent.com/53431568/142215229-2aa6f73e-85e8-4bda-a2b2-2b29228a0bcd.png)


์ผ๋จ ์ผ๊ตด์ ์ ์ก๊ณ , ๊ธฐ์ธ์ด์ง๋ ์ผ๊ตด์ ๋ํด alignment๋ฅผ ์ ํ๊ณ  ์๋ค. ์ค๋ฅธ์ชฝ ์์์ bounding box๋ฅผ ๋ณด๋ฉด ์ธ๋ก๊ฐ ๋ ๊ธด ์ง์ฌ๊ฐํ ํํ๋ก ์กํ๊ณ , ์ผ๊ตด๋ ๊ธฐ์ธ์ด์ ธ์์ง๋ง, ์ผ์ชฝ์ crop๋ ์ด๋ฏธ์ง๋ฅผ ๋ณด๋ฉด ์ผ๊ตด์ ๊ธฐ์ธ๊ธฐ๋ ์์ง์ผ๋ก ์ ์ ๋ ฌ๋์ด์๊ณ  224 ์ฌ์ด์ฆ์ ์ด๋ฏธ์ง๋ก ์ฑ์์ ธ์ ๋ค์ด๊ฐ ๊ฒ์ ๋ณผ ์ ์๋ค.

๋ฐ๋ก ์ด ์ด๋ฏธ์ง๊ฐ CNN์ input ๊ฐ์ผ๋ก ๋ค์ด๊ฐ feature๋ฅผ ๋ฝ๊ฒ ๋๋ ๊ฒ์ด๋ค.

console ์ฐฝ์ ๋ณด๋ฉด [0.00959151 0.3924815 0.6546932 ...  ์ด๋ฐ ์ซ์๋ฅผ ๋ณผ ์ ์๋๋ฐ, ์ด๊ฒ ํ ํ๋ ์์ ํ ์ผ๊ตด ๋น feature์  mean, std, min, max ๊ฐ์ด ๋์ด๋์ด์๋ ๊ฒ์ด๋ค.

<br>

### Experiments

AffectNet ๋ฐ์ดํฐ์์ ๋ํ ์ผ๊ตด ๊ฐ์ ์ธ์ ์ฑ๋ฅ! ๋ฌด๋ ค 62.42% ๋ก SOTA ๋ฌ์ฑ!

![image](https://user-images.githubusercontent.com/53431568/142215663-b635d262-eb3a-4eef-a96e-7e2cc13a601d.png)


ํนํ ์ด ๋ชจ๋ธ์ ๋ชจ๋ฐ์ผ์์ ๋์๊ฐ ์ ์๋๋ก ํ๋ ๊ฒ์ด ํต์ฌ์ด๊ธฐ ๋๋ฌธ์ (๊ทธ๋์ backbone architecture๋ ๊ฐ๋ฒผ์ด๊ฑฐ ์) CPU ์์๋ ์๋์๊ฐ๋์ง, ์ผ๋ง๋ ๋น ๋ฅด๊ฒ ๋์๊ฐ๋์ง๋ฅผ ์ฒดํฌํ์๋ค. MobileNet-v1 ์ด ๋น ๋ฅด๋ค. ์ฝ๋๋ฅผ ๋ณด๋๊น ์ด๋ฏธ์ง ํ์ฅ ๋ฃ์๋์ ์ฒ๋ฆฌ์ธ๋ฐ.. ๋๋ ๋น๋์ค๋ก ์ค์๊ฐ ์ฒ๋ฆฌ๊ฐ ์ค์ํ๋ฐ.. AFEW๋ก ํ์ต์์ผฐ๋ค๊ธธ๋ ๋น๋์ค๋ก demo ๊ฐ๋ฅํ๊ฐ ํ๊ณ  ์ค๋ ์์ง๋ง.. ํ์ฅ์ ์ด๋ฏธ์ง์ฒ๋ฆฌ demo ๋ฐ์ ์์๋ค. ๊ทธ๋์ ๋น๋์ค์ ์ฒ๋ฆฌ๋  ์ ์๋๋ก ์ฝ๋๋ฅผ ์์ ํ์ฌ ์ฌ์ฉํ๋ ์ค!


๋ด ์ฐ๊ตฌ์ฃผ์ ๊ฐ FER ์ด ํต์ฌ์ด๊ณ  ๋ถ๊ฐ์ ์ผ๋ก GAN ๋ชจ๋ธ์ ์ฐ๋ ๊ฒ์ด๋ผ ์ด์  FER ๋ผ๋ฌธ๋ ์์ฃผ ์ฝ๊ณ  ํฌ์คํํ  ์์ ์ด๋ค..!

์ด์ฐ์ ์ฐ ์์ฌ์ํ ์ ๋ฒํฐ์!




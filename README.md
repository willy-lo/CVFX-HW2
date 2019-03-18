# CVFX-HW2

MUNIT 介紹— Multimodal Unsupervised Image-to-Image Translation

簡介:

本文是在說1對多的圖片的風格轉換（style transfer），如貓轉成多張狗的圖片，提出可透過學習圖片的 content code 以及 style code，之後透過這兩個變數做圖片生成。

成果圖

![image](https://github.com/willy-lo/CVFX-HW2/blob/master/picture_1.jpeg)

![image](https://github.com/willy-lo/CVFX-HW2/blob/master/picture_2.jpeg)



相關介紹:

Multimodal:

這邊的Multimodal指的是給定一張圖片可以生成多個圖片。

專業一點的說法就是stochastic model -> 每次都會出現不同的圖片

stochastic model 的對比為 deterministic model

deterministic model 每次都會生出固定的圖片 1 to 1 Mapping




unsupervised learning (不需pair instances)

可以學到兩個dataset共同特徵，如貓和狗就會學到眼睛、鼻子、嘴巴。

Bicycle GAN — NIPS 2017

supervised learning 透過 pair instances

學到共通特徵，然後經由random一個數值來改變輸出，以達到 1 to Many，btw這篇提出的訓練方法也挺有趣的，使用兩種model做訓練，但是兩種model的部分component相同的。




content code / style code:

content code:

我們可以想成是每個class共有的特徵，如狗和貓都有眼睛 嘴巴 鼻子
convolution layer使用Instance Normalization(IN)
近年來style transfer方面的工作，使用IN能得到較好的結果



sytle code:

我們可以想成是每個class各自的特徵，如狗有柯基犬和哈士奇，雖然都是狗，但外表差很多
convolution layer使用Batch Normalization
這邊不用IN是因為，每個style有各自的特性分佈。如果使用IN會將mean和variance移除。
這樣不管S1, S2怎麼train， 可能都會產生差不多的圖片。

概念如下

![image](https://github.com/willy-lo/CVFX-HW2/blob/master/picture_3.png)

如上圖我們可以看到

- X1 — input image

- X2 — output image

- C — conten code，每張圖片映射到一個content code, 記錄共有的特徵。

- S — style code，X2的圖片產生是基於S2的style code(這部分為random產生)，不同的style code產生不同的圖片。




model 架構

encoder — decoder 架構如下

![image](https://github.com/willy-lo/CVFX-HW2/blob/master/picture_4.png)

這邊的MLP是multilayer perceptron，可看作是linear model。



loss function

![image](https://github.com/willy-lo/CVFX-HW2/blob/master/picture_5.png)

![image](https://github.com/willy-lo/CVFX-HW2/blob/master/formulation_1.png)

上圖可看作我們希望自己的reconstruction error要最小，意思是一張圖片(x)進入encoder再進去decoder出來的結果(y)

希望x和y相近。

![image](https://github.com/willy-lo/CVFX-HW2/blob/master/picture_6.png)

上圖可看作我們希望自己的latent reconstruction error要最小，

（2）意思是大眼睛的狗狗丟進model也要生出大眼睛的貓咪
希望x和y的content code相近。

（3）意思是科基狗丟進decoder解碼後再編碼還能夠知道這style code是柯基狗
希望x和y的style code相近。











# CVFX-HW2

1.MUNIT training 進度條:

1000000個iteration怕train不完，所以我們只有train到第50萬個iteration的結果。總共大概花了2.5天的時間完成。

![image](https://github.com/willy-lo/CVFX-HW2/blob/master/%20MUNIT_train.png)

![image](https://github.com/willy-lo/CVFX-HW2/blob/master/model.png)


2.test結果比較:
我們這組選取兩張夏天的跟兩張冬天的,從圖中看出來夏天轉冬天的效果還是比冬天轉夏天來的好
![image](https://github.com/willy-lo/CVFX-HW2/blob/master/sum.png)


3.別的方法:
NEURAL-style training 進度條:
![image](https://github.com/willy-lo/CVFX-HW2/blob/master/neural-style.png)

在神經網路出現之後Neural Style成為了一個非常有意思的深度學習應用:輸入一張代表內容的圖片和一張代表風格的圖片，深度學習網路會輸出一張融合了這個風格和內容的新作品。TensorFlow是Google開源的最流行的深度學習框架。在GitHub上有開源的TensorFlow實現的Neural Style代碼。我們還是先看一下Neural Style這篇論文介紹了怎樣的方法來解決這個問題的吧。
首先，有幾個概念:
卷積神經網路(CNN):
一張輸入的圖片，會在卷積神經網的各層以一系列過濾後的圖像表示。隨著層級的一層一層處理,過濾後的圖片會通過向下取樣的方式不斷減小(比如通過池化層)。這使得每層神經網的神經元數量會原來越小。 (也就是層越深,因為經過了池化層,單個feature map會越來越小,於是每層中的神經元數量也會越來越少)。
內容重塑:
在只知道該層的輸出結果,通過重塑輸入圖像,可以看到CNN不同階段的圖像資訊。在原始的VGG-Network上的5個層級:conv1_1,conv1_2,conv1_3,conv1_4, conv1_5上重塑了輸入的圖像。



輸入的圖像是上圖中的一排房子，5個層級分別是a,b,c,d,e。 在較低層的圖像重構(abc)非常完美;在較高層(de)，詳細的圖元資訊丟失了。也就是說，這樣做提取出了圖片的內容,但是拋棄了圖元。

風格重塑:
在原始的CNN表徵之上(feature map),建立了一個新的特徵空間(feature space),這個特徵空間捕獲了輸入圖像的風格。風格的表徵計算了在CNN的不同層級間不用特徵之間的相似性。通過在CNN隱層的不同的子集上建立起來的風格的表徵,我們重構輸入圖像的風格。如此，便創造了與輸入圖像一致的風格而丟棄了全域的內容。這篇論文的關鍵是對於內容和風格的表徵在CNN中是可以分開的。可以獨立地操作兩個表徵來產生新的,可感知意義的圖像。論文中生成一個圖片,混合了來自兩個不同圖片的內容和風格表徵。一張圖片,它同時符合照片的內容表徵,和藝術畫的風格表徵。 原始照片的整體佈局被保留了，而顏色和局部的結構卻由藝術畫提供。風格表徵是一個多尺度的表徵,包括了神經網路的多層。 在圖2中看到的圖像,風格的表徵包含了整個神經網路的層級。 而風格也可以只包含一小部分較低的層級。 (見下面的圖,第一行是卷基層1，第5行是卷基層5的輸出)。 若符合了較高層級中的風格表徵,局部的圖像結構會大規模地增加,從而使得圖像在視覺上更平滑與連貫。簡言之,作者直接把局部特徵看做近似的圖片內容,這樣就得到了一個把圖片內容和圖片風格(說白了就是紋理)分開的系統,剩下的就是把一個圖片的內容和另一個圖片的風格合起來。圖像的內容和風格並不能被完全地分解開。 當風格與內容來自不同的兩個圖像時,這個被合成的新圖像並不存在在同一時刻完美地符合了兩個約束。 但是,在圖像合成中最小化的損失函數分別包括了內容與風格兩者,它們被很好地分開了。 所以,我們可以平滑地將重點既放在內容上又放在風格上。


4.MUNIT 介紹— Multimodal Unsupervised Image-to-Image Translation

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











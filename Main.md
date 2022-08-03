# Introduction

## 什麼是產業關聯表(IO Table)?

我們可以從[行政院主計總處-產業關聯表](https://www.stat.gov.tw/ct.asp?xItem=28535&ctNode=671)得到像這樣的表格，其中每一個數字的定義如下:

![Example](https://user-images.githubusercontent.com/108454425/181272376-b1755506-88a6-4f55-a8e5-3073ff6d44f9.png)

$$  x_{ij}\ \triangleq\ i產業在這個年度投入了多少產品(換算成金錢)到j產業，稱為\ ^"中間投入^"$$

在這份報告中，我們把各個產業依照[台經院產經資料庫](https://tie.tier.org.tw/db/industry_definition/index.aspx)的分類，分成26個產業。

做好這些準備之後，我們要來計算70年~105年的"產業關聯程度表"

## 產業關聯程度表

產業關聯程度表 $=\ (I-A)^{-1} $，其中的I是單位矩陣，A是用到前面提到的產業關聯表，把這個矩陣中的每一個值除以該直行的總和所得到的矩陣，也就是對每一直行做單位化。

由於我們將產業分成26個，所以最後得到的應該是一個26 $\times$ 26的矩陣，而他的每個元素所代表的意思是"如果j產業要增加1元的產出時，需要多少i產業的產品"，這邊我們用105年的資料當例子:

![螢幕擷取畫面](https://user-images.githubusercontent.com/108454425/181772325-58f4ffa3-9cdf-4b82-a9f1-829282ec1856.png)

# 研究方法

有了這個產業關聯程度矩陣之後，我們可以用它來做甚麼呢? 其實在了解了這個矩陣內每個元素的意義之後，就能夠發現如果把這個矩陣乘上一個行向量 $\widetilde{x}$ 來代表各產業增加的產出，那得到的新的行向量 $\widetilde{y}$ 就是各產業為了應付那些產出，而實際產出的產品。

$$\left( \begin{array}{cccc} a_{1, 1} & a_{1, 2} & \cdots & a_{1, 26} \\
    a_{2, 1} & a_{2, 2} & \cdots & a_{2, 26}\\
    \vdots & \vdots & \ddots & \vdots \\
    a_{26, 1} & a_{26, 2} & \cdots & a_{26, 26} \end{array} \right)
  \left( \begin{array}{c} x_1 \\
    x_2 \\
    \vdots \\
    x_{26}\end{array} \right) \ = \ 
  \left( \begin{array}{c} y_1 \\
    y_2 \\
    \vdots \\
    y_{26}\end{array} \right) $$
    
有了這些之後，我們當然就會想要找找看有沒有使產出最大化的配置，於是我們最後就找到了Cauchy–Schwarz inequality。

## 用Cauchy–Schwarz inequality 來找最佳解

首先說明一下，這邊說的"最佳解"指的是在給定的 $sum(\widetilde{y})$ 之下，讓 $\widetilde{x}$ 裡面的值盡可能的小，而根據前面的式子我們可以知道

$$ sum(\widetilde{y}) \ =\ \begin{array}{c} (a_{1, 1} \ + \ a_{1, 2} \ + \ ...\ ) \cdot x_1 \ + \ \\
                            (a_{2, 1} \ + \ a_{2, 2} \ + \ ...\ ) \cdot x_2 \ + \ \\
                            \vdots \\
                            (a_{26, 1} \ + \ a_{26, 2} \ + \ ...\ ) \cdot x_{26} \end{array}$$

如果我們令一個向量 $\widetilde{c}$ 使得

$$\widetilde{c} \ =\ (c_1 \ ,\ c_2 \ ,...) \quad ,\ c_j \ = \displaystyle\sum_{i=1}^{26} a_{i, j} \quad ,\ j\ =\ 1,\ 2,\ ...,\ 26$$ 

那我們就知道 $sum(\widetilde{y})$ 其實就等於 $\widetilde{c} \cdot \ \widetilde{x}$ ，這時就可以套上我們的Cauchy–Schwarz inequality了。

$$sum(\widetilde{y})\ =\ \widetilde{c}\cdot\widetilde{x}\ \leq\ |\widetilde{c}|\cdot|\widetilde{x}|$$

那因為我們希望的是 $|\widetilde{x}|$ 能夠小一點，所以根據Cauchy–Schwarz inequality，當 $\widetilde{c}$ 跟 $\widetilde{x}$ 平行的時候， $|\widetilde{x}|$ 會有最小值 $\displaystyle \frac{sum(\widetilde{y})}{|\widetilde{c}|}$ ，也就是說我們想要求的 $\widetilde{x}$ 就會是

$$wanted\quad \widetilde{x}\ =\ \displaystyle \frac{sum(\widetilde{y})}{|\widetilde{c}|^2}\cdot \widetilde{c}$$

## 感應度以及影響度

除了剛剛的最佳解以外，我們還可以根據產業關聯程度矩陣來定義兩個新的詞，分別是"感應度"以及"影響度"

$$ i產業的感應度\  \triangleq\ \frac{\displaystyle\sum^n_{j=1} a_{ij}}{\displaystyle\frac{1}{n}\displaystyle\sum^n_{i=1}\displaystyle\sum^n_{j=1}a_{ij}} \quad & \quad  i產業的影響度\  \triangleq\ \frac{\displaystyle\sum^n_{i=1} a_{ij}}{\displaystyle\frac{1}{n}\displaystyle\sum^n_{i=1}\displaystyle\sum^n_{j=1}a_{ij}}$$

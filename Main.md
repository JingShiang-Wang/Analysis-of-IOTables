# Abstract

在這份報告中，我們打算對產業關聯程度表進行分析，在看過別人做出來的東西後，我們想要走一些別人沒走過的分析方向，其中用的是我們曾經學過的方法。

我們最後用了兩種分析方法，這些資料每5年一份總共8份，第一種方法是用Cauchy–Schwarz inequality 來找最佳解；第二種方法我們先定義了感應度以及影響度，再把它們依照年份、產業標在圖上並進行分類，最後還有將這些資料用MDS的方法進行資料的可視化，最後根據這些分類作出一些結論。

# Introduction

## 什麼是產業關聯表(Input-Output Table)?

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

## 1.用Cauchy–Schwarz inequality 來找最佳解

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

## 小結

因為政府每年預注入產業間的資金有限，若自發性支出的目的是為了使GDP最大化，則可參照此模型依比例調整出最適合分配。

## 2.感應度以及影響度

除了剛剛的最佳解以外，我們還可以根據產業關聯程度矩陣來定義兩個新的詞，分別是"感應度"以及"影響度"

$$ i產業的感應度\  \triangleq\ \frac{\displaystyle\sum^n_{j=1} a_{ij}}{\displaystyle\frac{1}{n}\displaystyle\sum^n_{i=1}\displaystyle\sum^n_{j=1}a_{ij}} \quad , \quad  i產業的影響度\  \triangleq\ \frac{\displaystyle\sum^n_{i=1} a_{ij}}{\displaystyle\frac{1}{n}\displaystyle\sum^n_{i=1}\displaystyle\sum^n_{j=1}a_{ij}}$$

感應度是指跟其他產業來比，所有產業需要他的產品的程度；而影響度則是跟其他產業來比，他需要所有產業的產品的程度

也就是說，感應度高的產業很容易因為別的產業發展變好，他自己的發展也跟著變好；影響度高的產業如果發展變好，就會容易帶動其他產業的發展。這邊我們用highcharter 畫了105年資料的圖，其中x軸是影響度、y軸是感應度

![example 3](https://user-images.githubusercontent.com/108454425/182641107-439f5af1-9f6c-4457-bc87-83a7396c7a40.png)

接著，因為我們每一個產業有8個年份的資料，所以我們使用了MDS的方法來降維並進行資料的視覺化，例如

![沒有標準化](https://user-images.githubusercontent.com/108454425/186679042-a3a656ee-9f2e-4d33-8c17-61429ec04892.png)

## 小結

於8年的26產業關聯效果圖中，所處象限、MDS相關係數所交集的產業如下：

第一象限：石油化學工業、金屬工業

第二象限：農林漁牧礦業、批發及零售業

第三象限：通訊業、文化創意產業、環保及教育及社會服務；金融及保險及證券業、工商及個人服務業

第四象限：機械設備製造業、運輸工具及零件製造業、綜合用品製造業

當同一群產業中某個產業受到其內生或外生變數所導致影響度或感應度有所變化時，其同一產業若也之後遇到相似情況，則可依此做出影響度或感應度的預測。
也就是說若遇到市場景氣循環上表現低迷，建議可以挹注資金於第四象限產業先止血，爾後接著影響第一二象限產業，最後使整體市場回溫；若遇到市場景氣循環上表現普通或高漲，可以投入資金於第一、四象限產業去帶動整體市場經濟繼續高漲。

# Conclusion

在這份報告中，我們運用了產業關聯程度表的特性，透過40年來的資料，在以提升GDP為前提下給政府一點編列預算的建議。

# Contributors

最佳解的計算由周志霖提供

圖形分析由侯威志提供

# Reference

[行政院主計總處-產業關聯統計](https://www.stat.gov.tw/np.asp?ctNode=669)

[台經院產經資料庫](https://tie.tier.org.tw/db/industry_definition/index.aspx)

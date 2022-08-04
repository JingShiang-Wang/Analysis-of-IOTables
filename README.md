### 如果要運行這份報告中的程式，請先跑一次下面的程式，這個程式包含了一個Package跟一些簡單的自訂函數

```{r include=FALSE}

library(highcharter)

#資料是已經整理過的
data70 <- read.csv("70.csv")
data75 <- read.csv("75.csv")
data80 <- read.csv("80.csv")
data85 <- read.csv("85.csv")
data90 <- read.csv("90.csv")
data95 <- read.csv("95.csv")
data100 <- read.csv("100.csv")
data105 <- read.csv("105.csv")

symbolJ = c("農、林、漁、牧、礦業", "食品業", "紡織業", "紙業及印刷業", "石油化學工業", "生技製藥與醫療保健業", "非金屬礦物製品製造業", "金屬工業", "電子零組件製造業", "光電材料及元件業", "資訊電子業", "通訊業", "電力設備製造業", "機械設備製造業", "運輸工具及零件製造業", "綜合用品製造業", "水電燃氣業", "營造及不動產業", "批發及零售業", "運輸及倉儲業", "觀光、餐飲及休閒業", "文化創意產業", "金融、保險及證券業", "工商及個人服務業", "環保、教育及社會服務", "其他")
symbolY = c(70, 75, 80, 85, 90, 95, 100, 105)

#產業關聯程度矩陣運算
IRM <- function(M){
  len = length(M[1, ])
  M2 <- matrix(0, nrow = len, ncol = len)
  for (i in c(1:len)) {
    for (j in c(1:len)){
      M2[j, i] <- M[j, i]/M[len + 1, i]
      if (i == j) {
        M2[j, i] <- 1 - M2[j, i]
      }
      else  
        M2[j, i] <- -M2[j, i]
    }
  }
  M3 <- solve(M2)
  return(M3)
}

#某一年矩陣的感應度以及影響度
IRdata <- function(dataY){
  M3 <- IRM(dataY)
  sumM = 0
  for (i in c(1:26)) {
    for (j in c(1:26)) {
      sumM = sumM + M3[i, j]
    }
  }
  meanM = sumM / 26
  TM <- matrix(0, ncol = 26, nrow = 2)
  for (i in c(1:26)) {
    TM[1, i] = sum(M3[1:26, i]) / meanM
    TM[2, i] = sum(M3[i, 1:26]) / meanM
  }
  dataM = data.frame("Influence" = TM[1, ], "Response" = TM[2, ])
  return(dataM)
}
```

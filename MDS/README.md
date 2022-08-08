### 由於每一個產業都有8個年份的感應度以及影響度，所以我們使用了MDS的方法來降低維度並畫圖。

### 除了直接畫圖以外，我們也有把資料標準化後再用MDS畫圖，以及分配過權重的圖。

### 最後還有將原本的資料做成相關係數矩陣後，再用MDS畫一個圖。

```{r, echo=FALSE}
TM <- cbind(IRdata(data70), IRdata(data75), IRdata(data80), IRdata(data85), IRdata(data90), IRdata(data95), IRdata(data100), IRdata(data105))
row.names(TM) <- symbolJ

#固定圖形的位子
Pin <- cbind(c(10, -4, -4, 10), c(5, 5, -3, -3))

mdsA <- TM %>%
  dist() %>%          
  cmdscale()
mdsA <- data.frame(mdsA[, 1], mdsA[, 2])
colnames(mdsA) <- c("Dim.1", "Dim.2")
highchart() %>%
  hc_add_series(mdsA, type = "scatter", hcaes(x = "Dim.1", y = "Dim.2", group = row.names(mdsA))) %>%
  hc_title(text = "沒有標準化") %>%
  hc_add_series(Pin, type = "scatter", color = "white", showInLegend = FALSE)
  
#標準化
TM2 <- scale(TM)

mdsS <- TM2 %>%
  dist() %>%          
  cmdscale()
mdsS <- data.frame(mdsS[, 1], mdsS[, 2])
colnames(mdsS) <- c("Dim.1", "Dim.2")
highchart() %>%
  hc_add_series(mdsS, type = "scatter", hcaes(x = "Dim.1", y = "Dim.2", group = row.names(mdsS))) %>%
  hc_title(text = "有標準化") %>%
  hc_add_series(Pin, type = "scatter", color = "white", showInLegend = FALSE)

TM21 <- cbind(TM2[, 1:4]*5/8, TM2[, 5:8]/8, TM2[, 9:12]/8, TM2[, 13:16]/8)
mdsS1 <- TM21 %>%
  dist() %>%          
  cmdscale()
mdsS1 <- data.frame(-1*mdsS1[, 1], mdsS1[, 2])
colnames(mdsS1) <- c("Dim.1", "Dim.2")
highchart() %>%
  hc_add_series(mdsS1, type = "scatter", hcaes(x = "Dim.1", y = "Dim.2", group = row.names(mdsS1))) %>%
  hc_title(text = "標準化後，70、75高權重") %>%
  hc_add_series(Pin, type = "scatter", color = "white", showInLegend = FALSE)

TM22 <- cbind(TM2[, 1:4]/8, TM2[, 5:8]*5/8, TM2[, 9:12]/8, TM2[, 13:16]/8)
mdsS2 <- TM22 %>%
  dist() %>%          
  cmdscale()
mdsS2 <- data.frame(mdsS2[, 1], mdsS2[, 2])
colnames(mdsS2) <- c("Dim.1", "Dim.2")
highchart() %>%
  hc_add_series(mdsS2, type = "scatter", hcaes(x = "Dim.1", y = "Dim.2", group = row.names(mdsS2))) %>%
  hc_title(text = "標準化後，80、85高權重") %>%
  hc_add_series(Pin, type = "scatter", color = "white", showInLegend = FALSE)

TM23 <- cbind(TM2[, 1:4]/8, TM2[, 5:8]/8, TM2[, 9:12]*5/8, TM2[, 13:16]/8)
mdsS3 <- TM23 %>%
  dist() %>%          
  cmdscale()
mdsS3 <- data.frame(-1*mdsS3[, 1], mdsS3[, 2])
colnames(mdsS3) <- c("Dim.1", "Dim.2")
highchart() %>%
  hc_add_series(mdsS3, type = "scatter", hcaes(x = "Dim.1", y = "Dim.2", group = row.names(mdsS3))) %>%
  hc_title(text = "標準化後，90、95高權重") %>%
  hc_add_series(Pin, type = "scatter", color = "white", showInLegend = FALSE)

TM24 <- cbind(TM2[, 1:4]/8, TM2[, 5:8]/8, TM2[, 9:12]/8, TM2[, 13:16]*5/8)
mdsS4 <- TM24 %>%
  dist() %>%          
  cmdscale()
mdsS4 <- data.frame(-1*mdsS4[, 1], mdsS4[, 2])
colnames(mdsS4) <- c("Dim.1", "Dim.2")
highchart() %>%
  hc_add_series(mdsS4, type = "scatter", hcaes(x = "Dim.1", y = "Dim.2", group = row.names(mdsS4))) %>%
  hc_title(text = "標準化後，100、105高權重") %>%
  hc_add_series(Pin, type = "scatter", color = "white", showInLegend = FALSE)
  
#相關係數矩陣
mdsM <- cor(t(TM)) %>%
  dist() %>%          
  cmdscale()
mdsM <- data.frame(mdsM[, 1], mdsM[, 2])
colnames(mdsM) <- c("Dim.1", "Dim.2")
highchart() %>%
  hc_add_series(mdsM, type = "scatter", hcaes(x = "Dim.1", y = "Dim.2", group = row.names(mdsM))) %>%
  hc_title(text = "相關係數") %>%
  hc_add_series(Pin, type = "scatter", color = "white", showInLegend = FALSE)
```



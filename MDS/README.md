### 由於每一個產業都有8個年份的感應度以及影響度，所以我們使用了MDS的方法來降低維度並畫圖。

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
```

### 

### 這個資料夾內的圖片，是同一年中26個產業各自的感應度以及影響度，其中x軸是影響度而y軸是感應度。

由於有26個點比較不好分辨，所以建議自己跑一次程式，highcharter會顯示各個點的名字以及實際數值。程式碼如下(以105年為例)

```{r, echo=FALSE}
#某一年各產業的感應度以及影響度

#固定圖形大小用的座標
xm = c(0, 1.5, 0, 1.5)
ym = c(0, 0, 4.5, 4.5)
datam = data.frame("X" = xm, "Y" = ym)

#年份
dataY <- data105

highchart() %>%
  hc_add_series(IRdata(dataY), type = "scatter", hcaes(x = Influence, y = Response, group = symbolJ))%>%
  hc_title(text = "105") %>%
  hc_xAxis(plotLines = list(list(value = 1, width = 3))) %>%
  hc_yAxis(plotLines = list(list(value = 1, width = 3))) %>%
  hc_add_series(datam, type = "scatter", hcaes(x = X, y = Y), color = "white", showInLegend = FALSE)
```

### 這個資料夾內的圖片，是26個產業在8個年份各自的感應度以及影響度，其中x軸是影響度而y軸是感應度，程式碼如下(以農林漁牧礦業為例)

```{r, echo=FALSE}
#某個產業的70~105感應度及影響度
Jobdata <- function(J){
  xJ = c(IRdata(data70)[J, 1], IRdata(data75)[J, 1], IRdata(data80)[J, 1], IRdata(data85)[J, 1], IRdata(data90)[J, 1], IRdata(data95)[J, 1], IRdata(data100)[J, 1], IRdata(data105)[J, 1])
  yJ = c(IRdata(data70)[J, 2], IRdata(data75)[J, 2], IRdata(data80)[J, 2], IRdata(data85)[J, 2], IRdata(data90)[J, 2], IRdata(data95)[J, 2], IRdata(data100)[J, 2], IRdata(data105)[J, 2])
  dataJ = data.frame("Influence" = xJ, "Response" = yJ)
  return(dataJ)
}

#固定圖形大小用的座標
xm = c(0, 1.5, 0, 1.5)
ym = c(0, 0, 4.5, 4.5)
datam = data.frame("X" = xm, "Y" = ym)

#J是指哪一個職業(對應到symbolJ)
J <- 1

#畫圖
highchart() %>%
  hc_add_series(Jobdata(J), type = "scatter", 
                hcaes(x = Influence, y = Response, group = symbolY)) %>%
  hc_colors(c("red", "orange", "yellow", "green", "blue", "purple", "brown", "black")) %>%
  hc_xAxis(plotLines = list(list(value = 1, width = 3))) %>%
  hc_yAxis(plotLines = list(list(value = 1, width = 3))) %>%
  hc_title(text = symbolJ[J]) %>%
  hc_add_series(datam, type = "scatter", hcaes(x = X, y = Y), color = "white", showInLegend = FALSE)
```

---
layout: post
title:  "Stata格式数据转SPSS"
date:   2021-12-2 09:32:04 +0700
categories: [Stata, R语言]
---

### STATA格式数据转SPSS

STATA格式数据转SPSS需要借助R语言的“Haven”包，这里以CFPS儿童数据为例，具体转换过程如下：

```r
library(haven) # 加载包
dataset <- read_dta("C:/Users/jhchao/Desktop/CFPS2021/CFPS儿童数据/cfps2018child1130.dta") 
write_sav( dataset, "C:/Users/jhchao/Desktop/CFPS2021/CFPS儿童数据/cfps2018child1130.sav")
```

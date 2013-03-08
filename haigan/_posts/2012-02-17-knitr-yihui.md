--- 
layout: post
title: "关于yihui的knitr在Windows下的问题"
description: "Rstudio 在Windows平台下可能出现的问题之解答"
tags: 
- 博客
status: publish
type: post
published: true

---


以前在Ubuntu12.10下使用knitr一点问题都没有，回到windows7下后就出现问题。

很长时间内拖着，不解！今日，得答案：

library("knitr", lib.loc="E:/Program Files/R/R-2.15.2/library")
所谓knitr在Rstudio下需要安装在R的库文件中，而不要安装在Rstudio/library下！



以后安装和加载包望吸取教训！

library("Rcmdr", lib.loc="E:/enlish_programs/RStudio/R/library")
此为加载Rstudio下的包文件。


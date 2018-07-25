---
title: "Stargazer"
output: html_document
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```

Try with descirptives
```{r}
stargazer(CCPEBaseline, type = "text", title="Descriptive statistics", digits=1, out="table1.txt")
```


---
title: "Stargazer test"
author: "Duy Phan"
date: "August 4, 2018"
output:
  pdf_document: default
  html_document:
    df_print: paged
---
Install and library the stargazer package as usual.

```{r setup, include=FALSE, echo = TRUE}
knitr::opts_chunk$set(echo = FALSE)
#install.packages("stargazer")
#install.packages("tinytex")
library("stargazer")
library("tinytex")
```

To learn about stargazer, visit the guiline: https://cran.r-project.org/web/packages/stargazer/vignettes/stargazer.pdf

(?) UNSOLVED ERROR: The align argument is suppose to be TRUE, so that coefficients in each column are aligned along the decimal
point. But for now, I need to set align = FALSE to make the pdf knit work. 

(!) There 3 mode of type for stargazer:
- "latex": Default mode, which only work if we knit the rmd file in pdf for now => Fancy latex result. Yay!
- "text": Can be use for all type of knit output => Useable but not very pretty.
- "html" Only work for html knit => Very ugly talbe

(!) NOTE WHEN USE LATEX MODE: 
REMEMBER to add results ="asis" into the defining part for all chunks that use Latex stargazer (I am using R markdown)
{r mylatex1, results = "asis", echo = TRUE}

Here is the latex table in a PDF document:

```{r, results = "asis", echo = TRUE}
stargazer(attitude, type = 'latex')
```

Here is a text table:
(REMOVE THE results = "asis" argument for text mode stargazer chunk)

```{r, echo = TRUE}
stargazer(attitude, type = "text")
```

Some other examples from stargazer guideline:

1. See the first row of attitude data. Remove the credit header using header argument.
```{r, results = "asis", echo = TRUE}
stargazer(attitude[1:4,],
          title="Test", digits=3, summary=FALSE, rownames=FALSE, header = FALSE, type = "latex")
```
2. TWo OLS models.
```{r, results = "asis", eval=TRUE, echo=TRUE}
##  2 OLS models
linear.1 = lm(rating ~ complaints + privileges + learning + raises + critical,
data=attitude)
linear.2 = lm(rating ~ complaints + privileges + learning, data=attitude)
#create an indicator dependent variable, and run a probit model
attitude$high.rating = (attitude$rating > 70)
probit.model = glm(high.rating ~ learning + critical + advance, data=attitude,
family = binomial(link = "probit"))
stargazer(linear.1, linear.2, probit.model, 
          type="latex",  title="Results", align = FALSE, header = FALSE)
```
\pagebreak
3. Modify the regression model. In particular, we remove all empty lines from the table (using no.space), and use omit.stat to leave out several statistics - namely, the log-likelihood ("LL''), residual standard error ("ser") and the F-statistic ("f").  Additionally, we label each of the dependent and independent variables with an easy-to-understand name.  To do so, we use the dep.var.labels and covariate.labels arguments.hjkj

```{r, results = "asis", echo = TRUE}
stargazer(linear.1, linear.2, probit.model, title="Regression Results",
align=FALSE, dep.var.labels=c("Overall Rating","High Rating"),
covariate.labels=c("Handling of Complaints","No Special Privileges",
"Opportunity to Learn","Performance-Based Raises","Too Critical","Advancement"),
omit.stat=c("LL","ser","f"), no.space=FALSE, type ="latex", header = FALSE)
```

4. In the next table, we limit ourselves to the two linear models, and report 90 percent confidence intervals (using ci and ci.level )  instead  of  standard  errors.   In  addition,  we  report  the  coefficients  and confidence intervals on the same row (using single.row)
```{r, results = "asis", echo = TRUE}
stargazer(linear.1, linear.2, title="Regression Results",
dep.var.labels=c("Overall Rating","High Rating"),
covariate.labels=c("Handling of Complaints","No Special Privileges",
"Opportunity to Learn","Performance-Based Raises","Too Critical","Advancement"),
omit.stat=c("LL","ser","f"), ci=TRUE, ci.level=0.90, single.row=TRUE, 
type = "latex", header = FALSE)
```

5. Let us now change the order of the explanatory variables using the order argument, and remove the  covariate  labels.   In  particular,  we  would  like learning and privileges to  come  before  all  the other covariates.  In addition, of the summary statistics reported, let us keep only the number of observations  (using  the  argument keep.stat).
```{r, results = "asis", echo = TRUE}
stargazer(linear.1, linear.2, title="Regression Results",
dep.var.labels=c("Overall Rating","High Rating"),
order=c("learning", "privileges"),
keep.stat="n", ci=TRUE, ci.level=0.90, single.row=TRUE, type = "latex", header = FALSE)
```

6. Stargazer can also report the content of vectors and matrices.  Let us create a table that contains the correlation matrix for the rating, complaints and privileges variables in the 'attitude' data frame:
```{r, results = "asis", echo = TRUE}
correlation.matrix <- cor(attitude[,c("rating","complaints","privileges")])
stargazer(correlation.matrix, title="Correlation Matrix", type = "latex", header = FALSE)
```

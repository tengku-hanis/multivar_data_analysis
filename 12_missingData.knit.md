---
output:
  pdf_document: default
  html_document: default
---

# Missing data

## Introduction

### Background 

Missing data data is quite a common issue in research. The causes of missing data should always be investigated and more data should be collected if possible. There are three types of missing data:

1.  Missing completely at random (MCAR)\
2.  Missing at random (MAR)\
3.  Missing not at random (MNAR)

<center>

![Illustration of missing data mechanism for the three types of missing data. X represents an observed variable, Y represents a variable with part of missing values that have been observed, Z represents a cause of missing values and R represents a variable with part of missing values that have been missing.](NAtype.png){width=65%}

</center>

Missing data (*R*) is classified as MCAR if the cause of missing data (*Z*) is unrelated to the data (*Y*). For example, medical records that is lost due to flood and laboratory equipment malfunction. In other words, the data is missing totally by chance. We can relate this example to the figure above, the cause, *Z* is not related to the data itself that have been observed, *Y*, but only related to the data that is missing, *R*. MCAR is ideal and more convenient, though MAR is more common and realistic. 

Missing data (*R*) is said to be MAR if the cause of missing data (*Z*) is related to other observed value (*X*), but not to the variable itself (*Y*). For example, an older person is more likely complete a survey compared to the younger person. So, the missingness is related to age, a variable that has been observed. Similarly, if information on income is more likely to be missing for older individuals as they are more cautious to reveal a sensitive information as opposed to younger individuals. Thus, the missingness is related to age, which should be a variable that we have information on. So, this missingness is considered as MAR. Another example of MAR is missing a certain variable due to the old medical form do not request this information. However, a new updated medical form request this information. As the missingness is not related the variable itself and we have the information whether the patients use an old or a new medical form, we can classify this missingness as MAR. We can see from all the examples that the cause of missing data, *Z* is related to other variable, *X*, but not to the variable itself, *Y* as illustrated in the figure.

Lastly, the missing data (*R*) is considered MNAR if the cause of missing data (*Z*) is related to the variable itself (*Y*) and to other variables (*X*) as well. Also, the missingness is considered MNAR if the causes completely unknown to us. In other words, we can not logically deduce that the missing data fit MCAR or MAR types. For example, missing a weight information for an obese individuals as the normal weighing scale may not able to weigh the individuals. Thus, the missing weight values (*R*) is considered MNAR as the missingness is related the variable itself (*Y*). However, we can never be sure of this without a further investigation about the mechanism of missingnness and its causes. MNAR is the most problematic type among the three. There are a few approaches to differentiate between MCAR and MAR-MNAR. However, an approach to differentiate between MAR and MNAR has yet to be proposed. Thus, we need to use a logical reasoning to differentiate between the two types.

### Objectives

At the end of the chapter, the reader will be able to:

-   To perform a simple imputation\
-   To perform a single imputation\
-   To perform a multiple imputation

## Preliminaries

### Packages

We will use these packages:

-   `mice`: for the single and multiple imputation\
-   `VIM`: for missing data exploration\
-   `naniar`: for missing data exploration\
-   `tidyverse`: for data wrangling and manipulation\
-   `gtsummary`: to provide a nice result in a table


```r
library(mice)
library(VIM)
library(naniar)
library(tidyverse)
library(gtsummary)
```

### Dataset

We going to use the coronary dataset that we used previously in linear regression chapter. However, this dataset have been altered to generate a missing values in it.














































































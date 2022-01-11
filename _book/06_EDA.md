# Exploratory data analysis (EDA) 

## Motivation 

In statistics, exploratory data analysis (EDA) is an approach in data analysis in order to summarize their main characteristics, often with visual methods. 

A statistical model can be used or not, but primarily EDA is for seeing what the data can tell us beyond the formal modeling or hypothesis testing task. 

Exploratory data analysis was promoted by John Tukey to encourage statisticians to explore the data, and possibly formulate hypotheses that could lead to new data collection

Source: https://en.wikipedia.org/wiki/Exploratory_data_analysis

## Questions to ask before making graphs

You must ask yourselves these:

- Which data do I want to use? `data = `
- Which variable or variables do I want to plot? `mapping = aes()`
- What is (or are) the type of that variable? 
    - Are they factor (categorical) variables ? 
    - Are they numerical variables?
- Am I going to plot 
    - a single variable?
    - two variables together?
    - three variables together?
- What plot? `geom_` 

## EDA using ggplot2 package

### Usage of **ggplot2** 

1.  start with `ggplot()`
2.  supply a dataset `data = `
3.  and aesthetic mapping (with `aes()`) - variables
4.  add on layers

- geom_X : `geom_point()`, `geom_histogram()`
- scales (like `scale_colour_brewer()`)
- faceting specifications (like `facet_wrap()`)
- coordinate systems (like `coord_flip()`).


## Loading the library

We will use two packages for data exploration:

1. **tidyverse** package for plotting and data wrangling 
2. **summarytools** package for summary statistics 
3. **patchwork** to combine plots
4. **here** to point to the correct working directory

We will load the **tidyverse** and **summarytools** package

The tidyverse is an opinionated collection of R packages designed for data science. All packages share an underlying design philosophy, grammar, and data structures. 

We may find more information about the package here <https://www.tidyverse.org/>


```r
library(tidyverse)
```

```
## -- Attaching packages --------------------------------------- tidyverse 1.3.1 --
```

```
## v ggplot2 3.3.5     v purrr   0.3.4
## v tibble  3.1.6     v dplyr   1.0.7
## v tidyr   1.1.4     v stringr 1.4.0
## v readr   2.1.1     v forcats 0.5.1
```

```
## Warning: package 'tibble' was built under R version 4.1.2
```

```
## -- Conflicts ------------------------------------------ tidyverse_conflicts() --
## x dplyr::filter() masks stats::filter()
## x dplyr::lag()    masks stats::lag()
```

```r
library(summarytools)
```

```
## 
## Attaching package: 'summarytools'
```

```
## The following object is masked from 'package:tibble':
## 
##     view
```

```r
library(patchwork)
library(here)
```

```
## here() starts at C:/Tengku/Sync_PC_Laptop/tulis-buku/multivariable-analysis/fork-profKIM/multivar_data_analysis
```


## The dataset

Let's read the peptic ulcer data which in MS Excel format. To do this we will

- load the **readxl** package



```r
library(readxl)
```

- use the function `read_xlsx()` to read data into R


```r
pep <- read_xlsx(here('data', 'peptic_ulcer.xlsx'))
```

And examine the data

- number of observations
- type of variables
- name of variables


```r
glimpse(pep)
```

```
## Rows: 121
## Columns: 34
## $ age                 <dbl> 42, 66, 67, 19, 77, 39, 62, 71, 69, 97, 52, 21, 57~
## $ gender              <chr> "male", "female", "male", "male", "male", "male", ~
## $ epigastric_pain     <chr> "yes", "yes", "yes", "yes", "yes", "yes", "yes", "~
## $ vomiting            <chr> "no", "no", "no", "no", "yes", "no", "no", "yes", ~
## $ nausea              <chr> "no", "no", "no", "no", "yes", "no", "no", "no", "~
## $ fever               <chr> "no", "no", "no", "no", "no", "yes", "no", "yes", ~
## $ diarrhea            <chr> "no", "no", "yes", "no", "no", "no", "no", "yes", ~
## $ malena              <chr> "no", "no", "no", "no", "no", "no", "no", "no", "n~
## $ onset_more_24_hrs   <chr> "no", "no", "no", "yes", "yes", "yes", "yes", "no"~
## $ NSAIDS              <chr> "no", "no", "yes", "no", "no", "no", "no", "no", "~
## $ septic_shock        <chr> "no", "no", "no", "no", "no", "no", "no", "no", "n~
## $ previous_OGDS       <chr> "no", "no", "no", "yes", "no", "no", "no", "no", "~
## $ ASA                 <dbl> 1, 1, 1, 1, 2, 1, 2, 2, 1, 1, 2, 1, 2, 1, 1, 2, 2,~
## $ systolic            <dbl> 141, 197, 126, 90, 147, 115, 103, 159, 145, 105, 1~
## $ diastolic           <dbl> 98, 88, 73, 40, 82, 86, 55, 68, 75, 65, 74, 50, 86~
## $ inotropes           <chr> "no", "no", "no", "no", "no", "no", "no", "no", "n~
## $ pulse               <dbl> 109, 126, 64, 112, 89, 96, 100, 57, 86, 100, 109, ~
## $ tenderness          <chr> "generalized", "generalized", "generalized", "loca~
## $ guarding            <chr> "yes", "yes", "yes", "yes", "no", "yes", "yes", "n~
## $ hemoglobin          <dbl> 18.0, 12.0, 12.0, 12.0, 11.0, 18.0, 8.1, 13.3, 11.~
## $ twc                 <dbl> 6.0, 6.0, 13.0, 20.0, 21.0, 4.0, 5.0, 12.0, 6.0, 2~
## $ platelet            <dbl> 415, 292, 201, 432, 324, 260, 461, 210, 293, 592, ~
## $ creatinine          <dbl> 135, 66, 80, 64, 137, 102, 69, 92, 94, 104, 58, 24~
## $ albumin             <chr> "27", "28", "32", "42", "38", "38", "30", "41", "N~
## $ PULP                <dbl> 2, 3, 3, 2, 7, 1, 2, 5, 3, 4, 2, 3, 4, 3, 5, 5, 1,~
## $ admission_to_op_hrs <dbl> 2, 2, 3, 3, 3, 3, 4, 4, 4, 4, 4, 5, 5, 6, 6, 6, 6,~
## $ perforation         <dbl> 0.5, 1.0, 0.5, 0.5, 1.0, 1.0, 3.0, 1.5, 0.5, 1.5, ~
## $ degree_perforation  <chr> "small", "small", "small", "small", "small", "smal~
## $ side_perforation    <chr> "distal stomach", "distal stomach", "distal stomac~
## $ ICU                 <chr> "no", "no", "no", "no", "yes", "no", "yes", "no", ~
## $ SSSI                <chr> "no", "no", "no", "no", "no", "no", "no", "no", "n~
## $ anast_leak          <chr> "no", "no", "no", "no", "no", "no", "no", "no", "n~
## $ sepsis              <chr> "no", "no", "no", "no", "no", "no", "yes", "no", "~
## $ outcome             <chr> "alive", "alive", "alive", "alive", "alive", "aliv~
```
The are quite a number of variables. For the sake of simplicity, we will select variables which we think might be important for simple data exploration. 

To do that, we will use `select()` function


```r
pep <- pep %>% 
  select(age, systolic, diastolic, hemoglobin, twc, ASA, PULP, perforation,
         gender, epigastric_pain, malena, tenderness, degree_perforation, outcome)
```



## Exploratory data analysis


### Statistics 

Overall results


```r
pep %>% descr()
```

```
## Non-numerical variable(s) ignored: gender, epigastric_pain, malena, tenderness, degree_perforation, outcome
```

```
## Descriptive Statistics  
## pep  
## N: 121  
## 
##                        age      ASA   diastolic   hemoglobin   perforation     PULP   systolic      twc
## ----------------- -------- -------- ----------- ------------ ------------- -------- ---------- --------
##              Mean    60.43     1.55       72.07        12.32          1.22     3.53     128.56    13.03
##           Std.Dev    18.05     0.62       13.99         3.33          0.91     2.28      24.51     6.66
##               Min    19.00     1.00       38.00         3.30          0.30     0.00      67.00     2.00
##                Q1    49.00     1.00       63.00        10.00          0.50     2.00     112.00     9.00
##            Median    64.00     1.00       71.00        12.00          1.00     3.00     128.00    12.00
##                Q3    75.00     2.00       81.00        15.00          1.50     5.00     143.00    16.00
##               Max    97.00     3.00      116.00        19.40          5.00     9.00     197.00    37.00
##               MAD    17.79     0.00       13.34         2.97          0.74     2.97      22.24     5.93
##               IQR    26.00     1.00       18.00         5.00          1.00     3.00      31.00     7.00
##                CV     0.30     0.40        0.19         0.27          0.74     0.65       0.19     0.51
##          Skewness    -0.55     0.66        0.12        -0.22          1.66     0.25       0.36     0.78
##       SE.Skewness     0.22     0.22        0.22         0.22          0.22     0.22       0.22     0.22
##          Kurtosis    -0.53    -0.55        0.22         0.02          3.34    -0.91       0.58     0.60
##           N.Valid   121.00   121.00      121.00       121.00        121.00   121.00     121.00   121.00
##         Pct.Valid   100.00   100.00      100.00       100.00        100.00   100.00     100.00   100.00
```

```r
pep %>% freq()
```

```
## Variable(s) ignored: age, systolic, diastolic, hemoglobin, twc
```

```
## Frequencies  
## pep$ASA  
## Type: Numeric  
## 
##               Freq   % Valid   % Valid Cum.   % Total   % Total Cum.
## ----------- ------ --------- -------------- --------- --------------
##           1     63     52.07          52.07     52.07          52.07
##           2     50     41.32          93.39     41.32          93.39
##           3      8      6.61         100.00      6.61         100.00
##        <NA>      0                               0.00         100.00
##       Total    121    100.00         100.00    100.00         100.00
## 
## pep$PULP  
## Type: Numeric  
## 
##               Freq   % Valid   % Valid Cum.   % Total   % Total Cum.
## ----------- ------ --------- -------------- --------- --------------
##           0     10      8.26           8.26      8.26           8.26
##           1     20     16.53          24.79     16.53          24.79
##           2     11      9.09          33.88      9.09          33.88
##           3     22     18.18          52.07     18.18          52.07
##           4     21     17.36          69.42     17.36          69.42
##           5     10      8.26          77.69      8.26          77.69
##           6      8      6.61          84.30      6.61          84.30
##           7     16     13.22          97.52     13.22          97.52
##           8      2      1.65          99.17      1.65          99.17
##           9      1      0.83         100.00      0.83         100.00
##        <NA>      0                               0.00         100.00
##       Total    121    100.00         100.00    100.00         100.00
## 
## pep$perforation  
## Type: Numeric  
## 
##               Freq   % Valid   % Valid Cum.   % Total   % Total Cum.
## ----------- ------ --------- -------------- --------- --------------
##         0.3      4      3.31           3.31      3.31           3.31
##         0.5     43     35.54          38.84     35.54          38.84
##         0.7      2      1.65          40.50      1.65          40.50
##         0.8      2      1.65          42.15      1.65          42.15
##           1     25     20.66          62.81     20.66          62.81
##         1.5     18     14.88          77.69     14.88          77.69
##           2     13     10.74          88.43     10.74          88.43
##         2.5      3      2.48          90.91      2.48          90.91
##           3      9      7.44          98.35      7.44          98.35
##           5      2      1.65         100.00      1.65         100.00
##        <NA>      0                               0.00         100.00
##       Total    121    100.00         100.00    100.00         100.00
## 
## pep$gender  
## Type: Character  
## 
##                Freq   % Valid   % Valid Cum.   % Total   % Total Cum.
## ------------ ------ --------- -------------- --------- --------------
##       female     25     20.66          20.66     20.66          20.66
##         male     96     79.34         100.00     79.34         100.00
##         <NA>      0                               0.00         100.00
##        Total    121    100.00         100.00    100.00         100.00
## 
## pep$epigastric_pain  
## Type: Character  
## 
##               Freq   % Valid   % Valid Cum.   % Total   % Total Cum.
## ----------- ------ --------- -------------- --------- --------------
##          no      5      4.13           4.13      4.13           4.13
##         yes    116     95.87         100.00     95.87         100.00
##        <NA>      0                               0.00         100.00
##       Total    121    100.00         100.00    100.00         100.00
## 
## pep$malena  
## Type: Character  
## 
##               Freq   % Valid   % Valid Cum.   % Total   % Total Cum.
## ----------- ------ --------- -------------- --------- --------------
##          no    117     96.69          96.69     96.69          96.69
##         yes      4      3.31         100.00      3.31         100.00
##        <NA>      0                               0.00         100.00
##       Total    121    100.00         100.00    100.00         100.00
## 
## pep$tenderness  
## Type: Character  
## 
##                     Freq   % Valid   % Valid Cum.   % Total   % Total Cum.
## ----------------- ------ --------- -------------- --------- --------------
##       generalized     84     69.42          69.42     69.42          69.42
##         localized     37     30.58         100.00     30.58         100.00
##              <NA>      0                               0.00         100.00
##             Total    121    100.00         100.00    100.00         100.00
## 
## pep$degree_perforation  
## Type: Character  
## 
##                  Freq   % Valid   % Valid Cum.   % Total   % Total Cum.
## -------------- ------ --------- -------------- --------- --------------
##          large     26     21.49          21.49     21.49          21.49
##       moderate     20     16.53          38.02     16.53          38.02
##          small     75     61.98         100.00     61.98         100.00
##           <NA>      0                               0.00         100.00
##          Total    121    100.00         100.00    100.00         100.00
## 
## pep$outcome  
## Type: Character  
## 
##               Freq   % Valid   % Valid Cum.   % Total   % Total Cum.
## ----------- ------ --------- -------------- --------- --------------
##       alive     83     68.60          68.60     68.60          68.60
##        dead     38     31.40         100.00     31.40         100.00
##        <NA>      0                               0.00         100.00
##       Total    121    100.00         100.00    100.00         100.00
```

Stratified for outcome


```r
pep %>% group_by(outcome) %>%
  descr()
```

```
## Non-numerical variable(s) ignored: gender, epigastric_pain, malena, tenderness, degree_perforation
```

```
## Descriptive Statistics  
## pep  
## Group: outcome = alive  
## N: 83  
## 
##                        age      ASA   diastolic   hemoglobin   perforation     PULP   systolic      twc
## ----------------- -------- -------- ----------- ------------ ------------- -------- ---------- --------
##              Mean    58.20     1.48       73.25        12.74          0.95     3.16     130.95    12.75
##           Std.Dev    18.53     0.63       14.02         3.14          0.60     2.35      25.06     6.00
##               Min    19.00     1.00       40.00         4.50          0.30     0.00      70.00     3.70
##                Q1    43.00     1.00       65.00        11.00          0.50     1.00     113.00     8.00
##            Median    62.00     1.00       72.00        12.00          0.70     3.00     129.00    12.00
##                Q3    73.00     2.00       82.00        15.00          1.50     5.00     147.00    16.00
##               Max    97.00     3.00      116.00        19.40          3.00     9.00     197.00    32.00
##               MAD    19.27     0.00       13.34         2.97          0.30     2.97      25.20     5.93
##               IQR    28.50     1.00       16.50         4.00          0.75     4.00      33.50     8.00
##                CV     0.32     0.43        0.19         0.25          0.63     0.74       0.19     0.47
##          Skewness    -0.45     0.93        0.15         0.07          1.41     0.43       0.34     0.69
##       SE.Skewness     0.26     0.26        0.26         0.26          0.26     0.26       0.26     0.26
##          Kurtosis    -0.71    -0.24        0.35        -0.21          1.66    -0.87       0.06     0.29
##           N.Valid    83.00    83.00       83.00        83.00         83.00    83.00      83.00    83.00
##         Pct.Valid   100.00   100.00      100.00       100.00        100.00   100.00     100.00   100.00
## 
## Group: outcome = dead  
## N: 38  
## 
##                        age      ASA   diastolic   hemoglobin   perforation     PULP   systolic      twc
## ----------------- -------- -------- ----------- ------------ ------------- -------- ---------- --------
##              Mean    65.29     1.68       69.50        11.41          1.82     4.34     123.34    13.64
##           Std.Dev    16.14     0.57       13.75         3.57          1.16     1.89      22.71     7.96
##               Min    23.00     1.00       38.00         3.30          0.30     1.00      67.00     2.00
##                Q1    57.00     1.00       60.00        10.00          1.00     3.00     111.00     9.00
##            Median    68.50     2.00       67.00        11.25          1.50     4.00     122.00    12.00
##                Q3    78.00     2.00       80.00        15.00          2.50     6.00     139.00    20.00
##               Max    92.00     3.00      100.00        17.00          5.00     8.00     197.00    37.00
##               MAD    15.57     0.00       12.60         3.71          0.74     1.48      18.53     7.41
##               IQR    21.00     1.00       19.50         4.75          1.50     2.75      26.50    11.00
##                CV     0.25     0.34        0.20         0.31          0.64     0.44       0.18     0.58
##          Skewness    -0.71     0.11        0.04        -0.50          0.91     0.24       0.30     0.71
##       SE.Skewness     0.38     0.38        0.38         0.38          0.38     0.38       0.38     0.38
##          Kurtosis    -0.17    -0.76       -0.32        -0.50          0.61    -0.99       1.94     0.11
##           N.Valid    38.00    38.00       38.00        38.00         38.00    38.00      38.00    38.00
##         Pct.Valid   100.00   100.00      100.00       100.00        100.00   100.00     100.00   100.00
```

```r
with(pep, stby(outcome, degree_perforation, freq))
```

```
## Frequencies  
## pep$outcome  
## Type: Character  
## Group: degree_perforation = large  
## 
##               Freq   % Valid   % Valid Cum.   % Total   % Total Cum.
## ----------- ------ --------- -------------- --------- --------------
##       alive      9     34.62          34.62     34.62          34.62
##        dead     17     65.38         100.00     65.38         100.00
##        <NA>      0                               0.00         100.00
##       Total     26    100.00         100.00    100.00         100.00
## 
## Group: degree_perforation = moderate  
## 
##               Freq   % Valid   % Valid Cum.   % Total   % Total Cum.
## ----------- ------ --------- -------------- --------- --------------
##       alive     13     65.00          65.00     65.00          65.00
##        dead      7     35.00         100.00     35.00         100.00
##        <NA>      0                               0.00         100.00
##       Total     20    100.00         100.00    100.00         100.00
## 
## Group: degree_perforation = small  
## 
##               Freq   % Valid   % Valid Cum.   % Total   % Total Cum.
## ----------- ------ --------- -------------- --------- --------------
##       alive     61     81.33          81.33     81.33          81.33
##        dead     14     18.67         100.00     18.67         100.00
##        <NA>      0                               0.00         100.00
##       Total     75    100.00         100.00    100.00         100.00
```


```r
with(pep, stby(outcome, gender, freq))
```

```
## Frequencies  
## pep$outcome  
## Type: Character  
## Group: gender = female  
## 
##               Freq   % Valid   % Valid Cum.   % Total   % Total Cum.
## ----------- ------ --------- -------------- --------- --------------
##       alive     13     52.00          52.00     52.00          52.00
##        dead     12     48.00         100.00     48.00         100.00
##        <NA>      0                               0.00         100.00
##       Total     25    100.00         100.00    100.00         100.00
## 
## Group: gender = male  
## 
##               Freq   % Valid   % Valid Cum.   % Total   % Total Cum.
## ----------- ------ --------- -------------- --------- --------------
##       alive     70     72.92          72.92     72.92          72.92
##        dead     26     27.08         100.00     27.08         100.00
##        <NA>      0                               0.00         100.00
##       Total     96    100.00         100.00    100.00         100.00
```

Another useful package is the **gt** package


### Plots

## One variable: Distribution of a categorical variable

### Bar chart

The frequency of the outcome  


```r
pep %>% group_by(outcome) %>% summarise(freq = n())
```

```
## # A tibble: 2 x 2
##   outcome  freq
##   <chr>   <int>
## 1 alive      83
## 2 dead       38
```

To plot the distribution of a categorical variable, we can use a bar chart.


```r
ggplot(data = pep) + 
  geom_bar(mapping = aes(x = outcome)) 
```

![](06_EDA_files/figure-latex/unnamed-chunk-12-1.pdf)<!-- --> 

But we can also pass the `aes()` to ggplot


```r
ggplot(data = pep, mapping = aes(x = outcome)) + 
  geom_bar()
```

![](06_EDA_files/figure-latex/unnamed-chunk-13-1.pdf)<!-- --> 

Combining **dplyr** and **ggplot** (both are packages inside the **tidyverse** metapackage)

**dplyr** part:



```r
pep_age <- pep %>% group_by(outcome) %>% 
  summarize(mean_age = mean(age)) 
pep_age
```

```
## # A tibble: 2 x 2
##   outcome mean_age
##   <chr>      <dbl>
## 1 alive       58.2
## 2 dead        65.3
```

**ggplot** part:


```r
ggplot(pep_age, mapping = aes(x = outcome, y = mean_age)) + 
  geom_col()
```

![](06_EDA_files/figure-latex/unnamed-chunk-15-1.pdf)<!-- --> 

**dplyr** and **ggplot** in action:


```r
pep %>% group_by(outcome) %>% 
  summarize(mean_age = mean(age)) %>% 
  ggplot(mapping = aes(x = outcome, y = mean_age, fill = outcome)) + 
  geom_col() +
  ylab("Mean age (Years)")
```

![](06_EDA_files/figure-latex/unnamed-chunk-16-1.pdf)<!-- --> 

Excellent resource from this website <http://www.cookbook-r.com/Graphs/Bar_and_line_graphs_(ggplot2)/>. It is a must go to website!!


## One variable: Distribution of a numerical variable

Plot distribution of values of a numerical variable

### Histogram

To see the distribution of a numerical variable, we can plot a histogram. To specify the number of bin, we can use bindwidth and add some customization


```r
ggplot(data = pep, mapping = aes(x = systolic)) + 
  geom_histogram(binwidth = 10, fill = "blue") +
  ylab("Frequency") +
  xlab("Systolic Blood Pressure") +
  ggtitle("Systolic BP distribution")
```

![](06_EDA_files/figure-latex/unnamed-chunk-17-1.pdf)<!-- --> 


**ggplot2** has lots of flexibility and personalization. For example, the histogram above is very plain. We can improve it by setting the line color and fill color, the theme, the size, the symbols and many other parameters. 


### Density curve

Let us create a density curve. Density curve is useful to examine the distribution of observations.


```r
ggplot(data = pep, mapping = aes(x = diastolic)) + 
  geom_density() +
  xlab("Diastolic BP (mmHg)") +
  ylab("Density") +
  labs(title = "Density distribution for diastolic BP",
       caption = "Source : Peptic ulcer disease data")
```

![](06_EDA_files/figure-latex/unnamed-chunk-18-1.pdf)<!-- --> 

### Histogram and density curve together

If we want to display both the histogram and the density curve, we can use `geom_histogram()` and `geom_density()` in the single line of codes.


```r
ggplot(pep, mapping = aes(x = diastolic)) + 
  geom_histogram(mapping = aes(y = ..density..), binwidth = 10) + 
  geom_density(colour = 'red') +
  xlab("Diastolic BP (mmHg)") +
  ylab("Density") +
  labs(title = "Density distribution for diastolic BP",
       caption = "Source : Peptic ulcer disease data") +
  theme_bw()
```

![](06_EDA_files/figure-latex/unnamed-chunk-19-1.pdf)<!-- --> 

## Two variables: Plotting a numerical and a categorical variable

### Overlaying histogramss and boxplot

Now, examine the distribution of a numerical variable (var **age**) based on variable **outcome** by overlaying histograms.


```r
hist_age <- ggplot(data = pep, aes(x = age, fill = outcome)) +
    geom_histogram(binwidth = 5, aes(y = ..density..),  
                   position = "identity", alpha = 0.75) + 
  geom_density(alpha = 0.25) +
  xlab("Age") +
  ylab("Density") +
  labs(title = "Density distribution",
       caption = "Source : Peptic ulcer disease data") +
  theme_bw()

box_age <- ggplot(data = pep, aes(x = outcome, y = age)) +
  geom_boxplot(outlier.shape = NA) + 
  geom_dotplot(binaxis = "y", binwidth = 1, fill = NA, colour = "blue", alpha = 0.75) +
  xlab('Outcome') + ylab('Age') +
  labs(title = "Box-plot",
       caption = "Source : Peptic ulcer disease data") +
  theme_bw()


hist_age |  box_age
```

![](06_EDA_files/figure-latex/unnamed-chunk-20-1.pdf)<!-- --> 
## Three variables: Plotting a numerical and two categorical variables

It is hard to visualize three variables in a single histogram plot. Perhaps we can use `facet_.()` function to split the plots. 

### Faceting the plots 

We can see better plots if we split the histogram based on certain grouping. 

In this example, we stratify the distribution of variable age (a numerical variable) based on outcome and gender (both are categorical variables)


```r
ggplot(data = pep, aes(x = age, fill = gender)) +
    geom_histogram(binwidth = 5, aes(y = ..density..),  
                   position = "identity", alpha = 0.25) + 
  geom_density(aes(colour = gender), alpha = 0.55) +
  xlab("Age") +
  ylab("Density") +
  labs(title = "Density distribution of age for outcome and gender",
       caption = "Source : Peptic ulcer disease data") +
  theme_bw() +
  facet_wrap( ~ outcome)
```

![](06_EDA_files/figure-latex/unnamed-chunk-21-1.pdf)<!-- --> 

And the summary statistics for the plots are


```r
pep %>% group_by(outcome, gender) %>%
  summarize(mean_age = mean(age, na.rm = TRUE), sd_age = sd(age, na.rm = TRUE))
```

```
## `summarise()` has grouped output by 'outcome'. You can override using the `.groups` argument.
```

```
## # A tibble: 4 x 4
## # Groups:   outcome [2]
##   outcome gender mean_age sd_age
##   <chr>   <chr>     <dbl>  <dbl>
## 1 alive   female     70.2   11.4
## 2 alive   male       56.0   18.8
## 3 dead    female     70.1   11.2
## 4 dead    male       63.1   17.7
```

## Line plot

Line graphs are typically used for visualizing how one continuous variable, on the y-axis, changes in relation to another continuous variable, on the x-axis. This is very useful for longitudinal data. 

Load gapminder package so we can use the gapminder data


```r
library(gapminder)
```


As with bar graphs, there are exceptions. Line graphs can also be used with a discrete variable on the x-axis. This is appropriate when the variable is ordered (e.g., "small," "medium," "large").

How about the life expectancy for Asia continent and also Malaysia in comparison 

1.  gapminder data
2.  continent == "Asia"
3.  life expectancy
4.  trend



```r
gapminder %>% filter(continent == "Asia") %>% 
  ggplot(mapping = aes(x = year, y = lifeExp, colour = country)) +
  geom_line(show.legend = FALSE)
```

![](06_EDA_files/figure-latex/unnamed-chunk-24-1.pdf)<!-- --> 


And the summary statistics


```r
gapminder %>%
  filter(continent == 'Asia') %>%
  filter(year == 1962 | year == 1982 | year == 2002) %>%
  group_by(year) %>%
  summarise(mean_life = mean(lifeExp, na.rm = TRUE), 
            sd_life = sd(lifeExp, na.rm = TRUE))
```

```
## # A tibble: 3 x 3
##    year mean_life sd_life
##   <int>     <dbl>   <dbl>
## 1  1962      51.6    9.82
## 2  1982      62.6    8.54
## 3  2002      69.2    8.37
```

## Plotting means and error bars 

We want to error bar for life expectancy for Asia continent only. Error bar that will contain

- mean
- standard deviation 

Our approach is:

- use filter to choose Asia continent only `filter()`
- generate the mean and SD for life expectancy using `mutate()`
- plot the scatterplot (country vs mean life expectancy) `geom_point()`
- plot errorbar `geom_errorbar()`


```r
gap_continent <- gapminder %>% filter(continent == "Asia") %>%
  group_by(country) %>% mutate(mean = mean(lifeExp), sd = sd(lifeExp)) %>%
  arrange(desc(mean))
```

Plot them with `coord_flip()`


```r
gap_continent %>%   
  ggplot(mapping = aes(x = country, y = mean)) + 
  geom_point(mapping = aes(x = country, y = mean)) +
  geom_errorbar(mapping = aes(ymin = mean - sd, ymax = mean + sd),
                position = position_dodge()) +
  coord_flip()
```

![](06_EDA_files/figure-latex/unnamed-chunk-27-1.pdf)<!-- --> 


## Scatterplot with fit line

The steps below shows that:

- data is pep
- fit line between age and size of perforation
- plot scatterplot
- plot fit line


```r
pep_fit <- pep %>% 
  ggplot(mapping = aes(x = age, y = perforation, colour = outcome)) +
  geom_point(show.legend = FALSE) +
  geom_smooth(method = lm, se = FALSE) +
  ylab('Size of perforation') +
  xlab('Age of patient') +
  labs(title = 'Distribution and fit line',
       caption = 'Source: Peptic ulcer data') +
  theme_bw() 
pep_fit 
```

```
## `geom_smooth()` using formula 'y ~ x'
```

![](06_EDA_files/figure-latex/unnamed-chunk-28-1.pdf)<!-- --> 

Now, let us see if the patterns are similar for men and women. We will use `facet_wrap()` to split the plots based on variable gender


```r
pep_fit + facet_grid(. ~ gender)
```

```
## `geom_smooth()` using formula 'y ~ x'
```

![](06_EDA_files/figure-latex/unnamed-chunk-29-1.pdf)<!-- --> 

References:

1. ggplot2: Elegant Graphics for Data Analysis. https://ggplot2-book.org/arranging-plots.html
2. ggplot2. https://ggplot2.tidyverse.org/
3. gapminder. https://cran.r-project.org/web/packages/gapminder/README.html 

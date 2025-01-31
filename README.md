## Intuitive Likert Data Transformation and Direct Plot Tool

Self-designed Package `surveylikert`: Background, Description, and Examples


### **Background**:

The `HH::likert()` function provides a robust way to generate
Likert-scale plots, but it requires the input dataframe to be in a
specific format: survey or questionnaire questions must be organized as
rows, and responses should be in columns. This format differs from the
typical structure of survey data, where questions are represented by
columns and individual responses occupy each row. As a result, users
often need to transform their data using mutations like `pivot_longer`
or `pivot_wider` before plotting, which can be time-consuming,
particularly for those new to R.

To simplify this process, I developed this `surveylikert` package. It
automates the transformation of survey data to the required format for
`HH::likert()` and allows users to create Likert plots directly from raw
survey data, without manual reshaping.

### **Description**:

Please refer to DESCRIPTION file in the repository. There are two functions in this package.

`prepare_likert_data()` transforms a questionnaire dataframe (questions in columns and responses in rows) into a format compatible with `HH::likert()` for likert-type plots. 

`plot_likert()` allows likert-type plots directly using survey/questionnaire data, incorporating dataframe transformation and `HH:likert()`.

### **Examples**:

Below are some examples for this package. Please run the Test.qmd file locally to see the dataframes output since it could not be displayed neatly in this file.


``` {.r .cell-code}
# install.packages("devtools")
# devtools::install_github("yichen917/surveylikert", force = TRUE)

library(tidyverse)
```

``` {.r .cell-code}
library(dplyr)
library(tidyr)
library(ggplot2)
library(tidyr)
library(fivethirtyeight)
library(surveylikert)
```
``` {.r .cell-code}
# Example 1

library(psych)
```

``` {.r .cell-code}
# Load the bfi dataset
data("bfi")
head(bfi)
```

``` {.r .cell-code}
# prepare_likert_data
trans_bfi <- prepare_likert_data(bfi,
                     question_cols = c('A1', 'A2', 'A3', 'A4', 'A5'),
                     na_action = 'omit',
                     response_levels_in_order = c(1,2,3,4,5,6))

trans_bfi
```

``` {.r .cell-code}
# check NAs
colSums(is.na(bfi))
```

``` {.r .cell-code}
# Create likert plot directly from survey/questionnaire data
plot_likert(bfi, 
            c('A1', 'A2', 'A3', 'A4', 'A5'),
            na_action = 'as_category',
            response_levels_in_order = c(1,2,3,4,5,6,7),
            na_category = 7,
            main = "Likert Plot",
            positive.order = TRUE)
```


![](Test_files/figure-markdown/unnamed-chunk-5-1.png)



``` {.r .cell-code}
# Plot likert data directly with survey/questionnair data
plot_likert(bfi, 
            c('A1', 'A2', 'A3', 'A4', 'A5'),
            na_action = 'omit',
            response_levels_in_order = c(1,2,3,4,5,6),
            main = "Likert Plot",
            positive.order = FALSE)
```


![](Test_files/figure-markdown/unnamed-chunk-6-1.png)


``` {.r .cell-code}
# Example 2

library(likert)
```

``` {.r .cell-code}
data("pisaitems")
pisa_data <- as_tibble(pisaitems)
pisa_data <- pisa_data[, c(grep("^ST24Q", names(pisa_data), value = TRUE))]

head(pisa_data)
```

``` {.r .cell-code}
# prepare_likert_data
transform_data <- prepare_likert_data(pisa_data, 
                                   colnames(pisa_data), 
                                   response_levels_in_order = c("Strongly disagree", "Disagree", "No response", "Agree", "Strongly agree"),
                                   na_action = "as_category", 
                                   na_category = "No response")
head(transform_data)
```

``` {.r .cell-code}
# Create likert plot using transformed data in HH::likert()
HH::likert(Question~., transform_data, 
           main = "PISA Item 24: Reading Attitudes",
           xlab = "Percent", ylab = "",
           scales = list(x = list(cex = 1),
                          y = list(cex = 1)),
           positive.order = TRUE)
```


![](Test_files/figure-markdown/unnamed-chunk-9-1.png)


``` {.r .cell-code}
# Plot likert data directly with plot_likert()
plot_likert(pisa_data, 
            colnames(pisa_data),
            na_action = 'as_category',
            response_levels_in_order = c("Strongly disagree", "Disagree", "Neutral", "Agree", "Strongly agree"),
            na_category = "Neutral",
            main = "PISA Item 24: Reading Attitudes",
            positive.order = TRUE)
```


![](Test_files/figure-markdown/unnamed-chunk-10-1.png)


# (APPENDIX) Appendix {-}

# Rubric for inference {#appendix-rubric}

<!-- Please don't mess with the next few lines! -->
<style>h5{font-size:2em;color:#0000FF}h6{font-size:1.5em;color:#0000FF}div.answer{margin-left:5%;border:1px solid #0000FF;border-left-width:10px;padding:25px} div.summary{background-color:rgba(30,144,255,0.1);border:3px double #0000FF;padding:25px}</style><p style="color:#ffffff">2.0</p>
<!-- Please don't mess with the previous few lines! -->

This is the R Markdown outline for running inference, both a hypothesis test and a confidence interval.


## Exploratory data analysis {-}

### Use data documentation (help files, code books, Google, etc.) to determine as much as possible about the data provenance and structure. {-}

::: {.answer}

Please write up your answer here


```r
# Add code here to print the data
```


```r
# Add code here to glimpse the variables
```

:::

### Prepare the data for analysis. [Not always necessary.] {-}

::: {.answer}


```r
# Add code here to prepare the data for analysis.
```

:::

### Make tables or plots to explore the data visually. {-}

::: {.answer}


```r
# Add code here to make tables or plots.
```

:::


## Hypotheses {-}

### Identify the sample (or samples) and a reasonable population (or populations) of interest. {-}

::: {.answer}

Please write up your answer here.

:::

### Express the null and alternative hypotheses as contextually meaningful full sentences. {-}

::: {.answer}

$H_{0}:$ Null hypothesis goes here.

$H_{A}:$ Alternative hypothesis goes here.

:::

### Express the null and alternative hypotheses in symbols (when possible). {-}

::: {.answer}

$H_{0}: math$

$H_{A}: math$

:::


## Model {-}

### Identify the sampling distribution model. {-}

::: {.answer}

Please write up your answer here.

:::

### Check the relevant conditions to ensure that model assumptions are met. {-}

::: {.answer}

Please write up your answer here. (Some conditions may require R code as well.)

:::


## Mechanics {-}

### Compute the test statistic. {-}

::: {.answer}


```r
# Add code here to compute the test statistic.
```

:::

### Report the test statistic in context (when possible). {-}

::: {.answer}

Please write up your answer here.

:::

### Plot the null distribution. {-}

::: {.answer}


```r
# IF CONDUCTING A SIMULATION...
set.seed(1)
# Add code here to simulate the null distribution.
```


```r
# Add code here to plot the null distribution.
```

:::

### Calculate the P-value. {-}

::: {.answer}


```r
# Add code here to calculate the P-value.
```

:::

### Interpret the P-value as a probability given the null. {-}

::: {.answer}

Please write up your answer here.

:::


## Conclusion {-}

### State the statistical conclusion. {-}

::: {.answer}

Please write up your answer here.

:::

### State (but do not overstate) a contextually meaningful conclusion. {-}

::: {.answer}

Please write up your answer here.

:::

### Express reservations or uncertainty about the generalizability of the conclusion. {-}

::: {.answer}

Please write up your answer here.

:::

### Identify the possibility of either a Type I or Type II error and state what making such an error means in the context of the hypotheses. {-}

::: {.answer}

Please write up your answer here.

:::

## Confidence interval {-}

### Check the relevant conditions to ensure that model assumptions are met.  {-}

::: {.answer}

Please write up your answer here. (Some conditions may require R code as well.)

:::

### Calculate and graph the confidence interval. {-}

::: {.answer}


```r
# Add code here to calculate the confidence interval.
```


```r
# Add code here to graph the confidence interval.
```

:::

### State (but do not overstate) a contextually meaningful interpretation. {-}

::: {.answer}

Please write up your answer here.

:::

### If running a two-sided test, explain how the confidence interval reinforces the conclusion of the hypothesis test. [Not always applicable.] {-}

::: {.answer}

Please write up your answer here.

:::

### When comparing two groups, comment on the effect size and the practical significance of the result. [Not always applicable.] {-}

::: {.answer}

Please write up your answer here.

:::


# Concordance with *Introduction to Modern Statistics* (IMS) {#appendix-concordance}

This book is meant to be somewhat aligned pedagogically with part of the book [*Introduction to Modern Statistics* (IMS)](https://openintro-ims.netlify.app/) by Mine &#199;etinkaya-Rundel and Johanna Hardin. But it's not a perfect, one-to-one match. The table below shows the concordance between the two books with some notes that explain when one book does something different from the other.

| This book | IMS       | Notes                                                 |
|-----------|-----------|-------------------------------------------------------|
| Ch. 1     |           | This book contains a specific introduction to R and RStudio with some basic statistical vocabulary. |
|           | Ch. 1     | IMS introduces a lot of vocabulary. This book introduces most of that same vocabulary, but across multiple chapters. |
| Ch. 2     |           | This book contains a specific introduction to R Markdown. |
|           | Ch. 2     | IMS discusses study design and sampling. Some of that information is scattered across multiple chapters of this book, but not all of it. (For example, this book doesn't get into stratified or cluster sampling.) |
|           | Ch. 3     | IMS has "Applications" chapters at the end of each section. In this book, the applications are woven into each chapter. |
| Ch. 3     | Ch. 4     | Categorical data.         | 
| Ch. 4     | Ch. 5     | Numerical data.           |
| Ch. 5     |           | This book has a dedicated chapter on manipulating data using `dplyr`. |
|           | Ch. 6     | Applications.                 |
| Ch. 6     | Ch. 7     | Correlation.                  |
| Ch. 7     | Ch. 7     | Simple linear regression.     |
|           | Ch. 8     | Multiple regression---not covered in this book.   |
|           | Ch. 9     | Logistic regression---not covered in this book.   |
|           | Ch. 10    | Applications.                 |
| Ch. 8     | Ch. 11    | Introduction to randomization, Part 1---This book takes four chapters to cover the material that IMS covers in one chapter.              |
| Ch. 9     | Ch. 11    | Introduction to randomization, Part 2.            |
| Ch. 10    | Ch. 11    | Hypothesis testing with randomization, Part 1.    |
| Ch. 11    | Ch. 11    | Hypothesis testing with randomization, Part 2.    |
| Ch. 12    | Ch. 12    | Confidence intervals.         |
| Ch. 13    | Ch. 13    | Normal models---This book takes two chapters to cover the material that IMS covers in one chapter.                |
| Ch. 14    | Ch. 13    | Sampling distribution models. |
|           | Ch. 14    | IMS has a chapter on decision errors that was covered in this book back in Ch. 10. It also covers the concept of power, which is not covered in this book. |
|           | Ch. 15    | Applications.                 |
| Ch. 15    | Ch. 16    | Inference for one proportion. |
| Ch. 16    | Ch. 17    | Inference for two proportions.|
| Ch. 17    |           | Chi-square goodness-of-fit test. (This is only covered in IMS in a standalone R tutorial appearing in Ch. 23.)           |
| Ch. 18    | Ch. 18    | Chi-square test for independence. |
| Ch. 19    | Ch. 19    | Inference for one mean.       |
| Ch. 20    | Ch. 21    | Inference for paired data.    |
| Ch. 21    | Ch. 20    | Inference for two independent means.    |
| Ch. 22    | Ch. 22    | ANOVA. This is the last chapter of this book. |
|           | Ch. 23    | Applications.                 |
|           | Ch. 24    | Inference for linear regression with a single predictor. |
|           | Ch. 25    | Inference for linear regression with multiple predictors.|
|           | Ch. 26    | Inference for logistic regression.    |
|           | Ch. 27    | Applications.                         |

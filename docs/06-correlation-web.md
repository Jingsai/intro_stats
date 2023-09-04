#  Correlation {#correlation}

<!-- Please don't mess with the next few lines! -->
<style>h5{font-size:2em;color:#0000FF}h6{font-size:1.5em;color:#0000FF}div.answer{margin-left:5%;border:1px solid #0000FF;border-left-width:10px;padding:25px} div.summary{background-color:rgba(30,144,255,0.1);border:3px double #0000FF;padding:25px}</style><p style="color:#ffffff">2.0</p>
<!-- Please don't mess with the previous few lines! -->

::: {.summary}

### Functions introduced in this chapter {-}

`cor`

:::


## Introduction {#correlation-intro}

In this chapter, we will learn about the concept of correlation, which is a way of measuring a linear relationship between two numerical variables.

### Install new packages {#correlation-install}

If you are using RStudio Workbench, you do not need to install any packages. (Any packages you need should already be installed by the server administrators.)

If you are using R and RStudio on your own machine instead of accessing RStudio Workbench through a browser, you'll need to type the following command at the Console:

```
install.packages("faraway")
```

### Download the R notebook file {#correlation-download}

Check the upper-right corner in RStudio to make sure you're in your `intro_stats` project. Then click on the following link to download this chapter as an R notebook file (`.Rmd`).

<a href = "https://vectorposse.github.io/intro_stats/chapter_downloads/06-correlation.Rmd" download>https://vectorposse.github.io/intro_stats/chapter_downloads/06-correlation.Rmd</a>

Once the file is downloaded, move it to your project folder in RStudio and open it there.

### Restart R and run all chunks {#correlation-restart}

In RStudio, select "Restart R and Run All Chunks" from the "Run" menu.

### Load packages {#correlation-load}

We load the now-standard `tidyverse` package. We also include the `faraway` package to access data about Chicago in the 1970s.


```r
library(tidyverse)
```

```
## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.2 ──
## ✔ ggplot2 3.3.6      ✔ purrr   0.3.4 
## ✔ tibble  3.1.8      ✔ dplyr   1.0.10
## ✔ tidyr   1.2.0      ✔ stringr 1.4.1 
## ✔ readr   2.1.2      ✔ forcats 0.5.2 
## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
## ✖ dplyr::filter() masks stats::filter()
## ✖ dplyr::lag()    masks stats::lag()
```

```r
library(faraway)
```


## Redlining in Chicago {#correlation-redlining}

The data set we will use throughout this chapter is from Chicago in the 1970s studying the practice of "redlining".

##### Exercise 1 {-}

Do an internet search for "redlining".

Consult at least two or three sources. Then, in your own words (not copied and pasted from any of the websites you consulted), explain what "redlining" means.

::: {.answer}

Please write up your answer here.

:::

*****


The `chredlin` data set appears in the `faraway` package accompanying a book by Julian Faraway (*Practical Regression and Anova using R*, 2002.) Faraway explains:

> "In a study of insurance availability in Chicago, the U.S. Commission on Civil Rights attempted to examine charges by several community organizations that insurance companies were redlining their neighborhoods, i.e. canceling policies or refusing to insure or renew. First the Illinois Department of Insurance provided the number of cancellations, non-renewals, new policies, and renewals of homeowners and residential fire insurance policies by ZIP code for the months of December 1977 through February 1978. The companies that provided this information account for more than 70% of the homeowners insurance policies written in the City of Chicago. The department also supplied the number of FAIR plan policies written an renewed in Chicago by zip code for the months of December 1977 through May 1978. Since most FAIR plan policyholders secure such coverage only after they have been rejected by the voluntary market, rather than as a result of a preference for that type of insurance, the distribution of FAIR plan policies is another measure of insurance availability in the voluntary market."

In other words, the degree to which residents obtained FAIR policies can be seen as an indirect measure of redlining. This participation in an "involuntary" market is thought to be largely driven by rejection of coverage under more traditional insurance plans.

### Exploratory data analysis {#correlation-eda}

Before we learn about correlation, let's get to know our data a little better.

Type `?chredlin` at the Console to read the help file. While it's not very informative about how the data was collected, it does have crucial information about the way the data is structured.

Here is the data set:


```r
chredlin
```

```
##       race fire theft  age involact income side
## 60626 10.0  6.2    29 60.4      0.0 11.744    n
## 60640 22.2  9.5    44 76.5      0.1  9.323    n
## 60613 19.6 10.5    36 73.5      1.2  9.948    n
## 60657 17.3  7.7    37 66.9      0.5 10.656    n
## 60614 24.5  8.6    53 81.4      0.7  9.730    n
## 60610 54.0 34.1    68 52.6      0.3  8.231    n
## 60611  4.9 11.0    75 42.6      0.0 21.480    n
## 60625  7.1  6.9    18 78.5      0.0 11.104    n
## 60618  5.3  7.3    31 90.1      0.4 10.694    n
## 60647 21.5 15.1    25 89.8      1.1  9.631    n
## 60622 43.1 29.1    34 82.7      1.9  7.995    n
## 60631  1.1  2.2    14 40.2      0.0 13.722    n
## 60646  1.0  5.7    11 27.9      0.0 16.250    n
## 60656  1.7  2.0    11  7.7      0.0 13.686    n
## 60630  1.6  2.5    22 63.8      0.0 12.405    n
## 60634  1.5  3.0    17 51.2      0.0 12.198    n
## 60641  1.8  5.4    27 85.1      0.0 11.600    n
## 60635  1.0  2.2     9 44.4      0.0 12.765    n
## 60639  2.5  7.2    29 84.2      0.2 11.084    n
## 60651 13.4 15.1    30 89.8      0.8 10.510    n
## 60644 59.8 16.5    40 72.7      0.8  9.784    n
## 60624 94.4 18.4    32 72.9      1.8  7.342    n
## 60612 86.2 36.2    41 63.1      1.8  6.565    n
## 60607 50.2 39.7   147 83.0      0.9  7.459    n
## 60623 74.2 18.5    22 78.3      1.9  8.014    s
## 60608 55.5 23.3    29 79.0      1.5  8.177    s
## 60616 62.3 12.2    46 48.0      0.6  8.212    s
## 60632  4.4  5.6    23 71.5      0.3 11.230    s
## 60609 46.2 21.8     4 73.1      1.3  8.330    s
## 60653 99.7 21.6    31 65.0      0.9  5.583    s
## 60615 73.5  9.0    39 75.4      0.4  8.564    s
## 60638 10.7  3.6    15 20.8      0.0 12.102    s
## 60629  1.5  5.0    32 61.8      0.0 11.876    s
## 60636 48.8 28.6    27 78.1      1.4  9.742    s
## 60621 98.9 17.4    32 68.6      2.2  7.520    s
## 60637 90.6 11.3    34 73.4      0.8  7.388    s
## 60652  1.4  3.4    17  2.0      0.0 13.842    s
## 60620 71.2 11.9    46 57.0      0.9 11.040    s
## 60619 94.1 10.5    42 55.9      0.9 10.332    s
## 60649 66.1 10.7    43 67.5      0.4 10.908    s
## 60617 36.4 10.8    34 58.0      0.9 11.156    s
## 60655  1.0  4.8    19 15.2      0.0 13.323    s
## 60643 42.5 10.4    25 40.8      0.5 12.960    s
## 60628 35.1 15.6    28 57.8      1.0 11.260    s
## 60627 47.4  7.0     3 11.4      0.2 10.080    s
## 60633 34.0  7.1    23 49.2      0.3 11.428    s
## 60645  3.1  4.9    27 46.6      0.0 13.731    n
```

##### Exercise 2 {-}

What do each of the rows of this data set represent? You'll need to refer to the help file. (They are *not* individual people.)

::: {.answer}

Please write up your answer here.

:::

##### Exercise 3 {-}

The `race` variable is numeric. Why? What do these numbers represent? (Again, refer to the help file.)

::: {.answer}

Please write up your answer here.

:::

*****

The `glimpse` command gives a concise overview of all the variables present.


```r
glimpse(chredlin)
```

```
## Rows: 47
## Columns: 7
## $ race     <dbl> 10.0, 22.2, 19.6, 17.3, 24.5, 54.0, 4.9, 7.1, 5.3, 21.5, 43.1…
## $ fire     <dbl> 6.2, 9.5, 10.5, 7.7, 8.6, 34.1, 11.0, 6.9, 7.3, 15.1, 29.1, 2…
## $ theft    <dbl> 29, 44, 36, 37, 53, 68, 75, 18, 31, 25, 34, 14, 11, 11, 22, 1…
## $ age      <dbl> 60.4, 76.5, 73.5, 66.9, 81.4, 52.6, 42.6, 78.5, 90.1, 89.8, 8…
## $ involact <dbl> 0.0, 0.1, 1.2, 0.5, 0.7, 0.3, 0.0, 0.0, 0.4, 1.1, 1.9, 0.0, 0…
## $ income   <dbl> 11.744, 9.323, 9.948, 10.656, 9.730, 8.231, 21.480, 11.104, 1…
## $ side     <fct> n, n, n, n, n, n, n, n, n, n, n, n, n, n, n, n, n, n, n, n, n…
```

##### Exercise 4(a) {-}

Which variable listed above represents participation in the FAIR plan? How is it measured? (Again, refer to the help file.)

::: {.answer}

Please write up your answer here.

:::

##### Exercise 4(b) {-}

Why is it important to analyze the number of plans *per 100 housing units* as opposed to the total number of plans across each ZIP code? (Hint: what happens if some ZIP codes are larger than others?)

::: {.answer}

Please write up your answer here.

:::


*****

We are interested in the association between `race` and `involact`. If redlining plays a role in driving people toward FAIR plan policies, we would expect there to be a relationship between the racial composition of a ZIP code and the number of FAIR plan policies obtained in that ZIP code.

##### Exercise 5(a) {-}

Since `race` is a numerical variable, what type of graph or chart is appropriate for visualizing it? (You may need to refer back to the "Numerical data" chapter.)

::: {.answer}

Please write up your answer here.

:::

##### Exercise 5(b) {-}

Using `ggplot` code, create the type of graph you identified above. (Again, refer back to the "Numerical data" chapter for sample code if you've forgotten.) After creating the initial plot, be sure to go back and set the `binwidth` and `boundary` to sensible values.

::: {.answer}


```r
# Add code here to create a plot of race
```

:::

##### Exercise 5(c) {-}

Describe the shape of the `race` variable using the three key shape descriptors (modes, symmetry, and outliers).

::: {.answer}

Please write up your answer here.

:::

##### Exercise 5(d) {-}

Create the same kind of graph as above, but for `involact`. (Again, go back and set the `binwidth` and `boundary` to sensible values.)

::: {.answer}


```r
# Add code here to create a plot of race
```

:::

##### Exercise 5(e) {-}

Describe the shape of the `involact` variable using the three key shape descriptors (modes, symmetry, and outliers).

::: {.answer}

Please write up your answer here.

:::

##### Exercise 5(f) {-}

Since both `race` and `involact` are numerical variables, what type of graph or chart is appropriate for visualizing the relationship between them?

::: {.answer}

Please write up your answer here.

:::

##### Exercise 5(g) {-}

For our research question, is `race` functioning as a predictor variable or as the response variable? What about `involact`? Why? Explain why it makes more sense to think of one of them as the predictor and the other as the response.

::: {.answer}

Please write up your answer here.

:::

##### Exercise 5(h) {-}

Using `ggplot` code, create the type of graph you identified above. Be sure to put `involact` on the y-axis and race` on the x-axis.

::: {.answer}


```r
# Add code here to create a plot of involact against race
```

:::


*****


## Correlation {#correlation-correlation}

The word *correlation* describes a linear relationship between two numerical variables. As long as certain conditions are met, we can calculate a statistic called the *correlation coefficient*, often denoted with a lowercase r.

There are several different ways to compute a statistic that measures correlation. The most common way, and the way we will learn in this chapter, is often attributed to an English mathematician named Karl Pearson. According to his [Wikipedia page](https://en.wikipedia.org/wiki/Karl_Pearson),

> "Pearson was also a proponent of social Darwinism, eugenics and scientific racism."

##### Exercise 6 {-}

Do an internet search for each of the following terms:

- Social Darwinism
- Eugenics
- Scientific racism

Consult at least two or three sources for each term. Then, in your own words (not copied and pasted from any of the websites you consulted), explain what these terms mean.

::: {.answer}

Please write up your answer here.

:::

*****

While Pearson is often credited with its discovery, the so-called "Pearson correlation coefficient" was first developed by a French scientist, Auguste Bravais. Due to the misattribution of discovery, along with the desire to disassociate the useful tool of correlation from its problematic applications to racism and eugenics, we will just refer to it as the *correlation coefficient* (without a name attached).

The correlation coefficient, r, has some important properties.

- The correlation coefficient is a number between -1 and 1.
- A value close to 0 indicates little or no correlation.
- A value close to 1 indicates strong positive correlation.
- A value close to -1 indicates strong negative correlation.

In between 0 and 1 (or -1), we often use words like weak, moderately weak, moderate, and moderately strong. There are no exact cutoffs for when such words apply. You must learn from experience how to judge scatterplots and r values to make such determinations.

A correlation is positive when low values of one variable are associated with low values of the other value. Similarly, high values of one variable are associated with high values of the other. For example, exercise is positively correlated with burning calories. Low exercise levels will burn a few calories; high exercise levels burn more calories, on average.

A correlation is negative when low values of one variable are associated with high values of the other value, and vice versa. For example, tooth brushing is negatively correlated with cavities. Less tooth brushing may result in more cavities; more tooth brushing is associated with fewer calories, on average.


## Conditions for correlation {#correlation-conditions}

Two variables are considered "associated" any time there is any type of relationship between them (i.e., they are not independent). However, in statistics, we reserve the word "correlation" for situations meeting more stringent conditions:

1. The two variables must be numerical.^[There are other ways of measuring association for variables that are not numerical, but these aren't covered in this course.]
2. There is a somewhat linear relationship between the variables, as shown in a scatterplot.
3. There are no serious outliers.

For condition (2) above, keep in mind that real data in scatterplots very rarely lines up in a perfect straight line. Instead, you will see a "cloud" of dots. All we want to know is whether that cloud of dots mostly moves from one corner of the scatterplot to the other. Violations of this condition will usually be for one of two reasons:

- The dots are scattered completely randomly with no discernible pattern.
- The dots have a pattern or shape to them, but that shape is curved and not linear.

##### Exercise 7 {-}

Check the three conditions for the relationship between `involact` and `race`. For conditions (2) and (3), you'll need to check the scatterplot you created above. (You did create a scatterplot for one of the exercises above, right?)

::: {.answer}

Please write up your answer here.

1. 
2. 
3. 

:::


## Calculating correlation {#correlation-calculating}

Since the conditions are met, We calculate the correlation coefficient using the `cor` command.


```r
cor(chredlin$race, chredlin$involact)
```

```
## [1] 0.713754
```

The order of the variables doesn't matter; correlation is symmetric, so the r value is the same independent of the choice of response and predictor variables.

Since the correlation between `involact` and `race` is a positive number and slightly closer to 1 than 0, we might call this a "moderate" positive correlation. You can tell from the scatterplot above that the relationship is not a strong relationship. The words you choose should match the graphs you create and the statistics you calculate.

##### Exercise 8(a) {-}

Create a scatterplot of `income` against `race`. (Put `income` on the y-axis and `race` on the x-axis.)

::: {.answer}


```r
# Add code here to create a scatterplot of income against race
```

:::

##### Exercise 8(b) {-}

Check the three conditions for the relationship between `income` and `race`. Which condition is pretty seriously violated here?

::: {.answer}

Please write up your answer here.

1. 
2. 
3. 

:::

##### Exercise 9(a) {-}

Create a scatterplot of `theft` against `fire`. (Put `theft` on the y-axis and `fire` on the x-axis.)

::: {.answer}


```r
# Add code here to create a scatterplot of theft against fire
```

:::

##### Exercise 9(b) {-}

Check the three conditions for the relationship between `theft` and `fire`. Which condition is pretty seriously violated here?

::: {.answer}

1. 
2. 
3. 

Please write up your answer here.

:::


##### Exercise 9(c) {-}

Even though the conditions are not met, what if you calculated the correlation coefficient anyway? Try it.

::: {.answer}


```r
# Add code here to calculate the correlation coefficient between theft and fire
```

:::


##### Exercise 9(d) {-}

Suppose you hadn't looked at the scatterplot and you only saw the correlation coefficient you calculated in the previous part. What would your conclusion be about the relationship between `theft` and `fire`. Why would that conclusion be misleading?

::: {.answer}

Please write up your answer here.

:::

The lesson learned here is that you should never try to interpret a correlation coefficient without looking at a plot of the data to assure that the conditions are met and that the result is a sensible thing to interpret.


## Correlation is not causation {#correlation-causation}

When two variables are correlated---indeed, associated in any way, not just in a linear relationship---that means that there is a relationship between them. However, that does not mean that one variable *causes* the other variable.

For example, we discovered above that there was a moderate correlation between the racial composition of a ZIP code and the new FAIR policies created in those ZIP codes. However, being part of a racial minority does not cause someone to seek out alternative forms of insurance, at least not directly. In this case, the racial composition of certain neighborhoods, though racist policies, affected the availability of certain forms of insurance for residents in those neighborhoods. And that, in turn, caused residents to seek other forms of insurance.

In the Chicago example, there is still likely a causal connection between one variable (`race`) and the other (`involact`), but it was indirect. In other cases, there is no causal connection at all. Here are a few of my favorite examples.

##### Exercise 10 {-}

Ice cream sales are positively correlated with drowning deaths. Does eating ice cream cause you to drown? (Perhaps the myth about swimming within one hour of eating is really true!) Does drowning deaths cause ice cream sales to rise? (Perhaps people are so sad about all the drownings that they have to go out for ice cream to cheer themselves up?)

See if you can figure out the real reason why ice cream sales are positively correlated with drowning deaths.

::: {.answer}

Please write up your answer here.

:::

*****

In the Chicago example, the causal effect was indirect. In the example from the exercise above, there is no causation whatsoever between the two variables. Instead, the causal effect was generated by a third factor that caused both ice cream sales to go up, and also happened to cause drowning deaths to go up. (Or, equivalently stated, it caused ice cream sales to be low during certain times of the year and also caused the drowning deaths to be low as well.) Such a factor is called a *lurking variable*. When a correlation between two variables exists due solely to the intervention of a lurking variable, that correlation is called a *spurious correlation*. The correlation is real; a scatterplot of ice cream sales and drowning deaths would show a positive relationship. But the reasons for that correlation to exist have nothing to do with any kind of direct causal link between the two.

Here's another one:

##### Exercise 11 {-}

Most studies involving children create a number of weird correlations. For example, the height of children is very strongly correlated to pretty much everything you can measure about scholastic aptitude. For example, vocabulary count (the number of words children can use fluently in a sentence) is strongly correlated to height. Are tall people just smarter than short people?

The answer is, of course, no. The correlation is spurious. So what's the lurking variable?

::: {.answer}

Please write up your answer here.

:::


## Observational studies versus experiments {#correlation-obs-exp}

So when is a statistical finding (like correlation, for example) evidence of a causal relationship? Before we can answer that question, we need a few more definitions.

A lot of data comes from "observational studies" where we simply observe or measure things as they are "in the wild," so to speak. We don't interfere in any way. We just write down what we see. Polls are usually observational in that we ask people questions and record their responses. We do not try to manipulate their responses in any way. We just ask the questions and observe the answers. Field studies are often observational. We go out in nature and write stuff down as we observe it.

Another way to gather data is an *experiment*. In an experiment, we introduce a manipulation or treatment to try to ascertain its effect. For example. if we're testing a new drug, we will likely give the drug to one group of patients and a *placebo* to the other.

##### Exercise 12 {-}

Here's another internet rabbit hole for you. First, look up the definition of placebo. You do not need to write up your own version of that definition here; just familiarize yourself with the term if you're not already familiar with it. Next, find some websites about the *placebo effect* and read those.

Given what you have learned about the placebo effect, why is it important to have a placebo group in a drug trial? Why not just give one set of patients the drug and compare them to another group that takes no pill at all?

::: {.answer}

Please write up your answer here.

:::

*****


The goal of the experiment is to learn whether the *treatment* (in this example, the drug) is effective when compared to the *control* (in this example, the placebo).

Note that the word "effective" implies a causal claim. We want to know if the drug *causes* patients to get better.

Unlike an observational study, in which the relationship between variables can be caused by a lurking variable, in an experiment, we purposefully manipulate one of the variables and try to control all others. For example, we manipulate the drug variable (we purposefully give some people the drug and others the placebo). But we control the amount of the drug given and the schedule on which patients are required to take the pills.

There are lots of things we cannot control. For example, it would be very difficult to control the diet of every person in the experiment. Could diet play a role in whether a patient gets better? Sure, so how do we know diet is not a lurking variable? In the context of an experiment, lurking variables are often called "confounders" or "confounding variables". (The two terms are basically synonymous.)

One way to mitigate the effect of confounders that we cannot directly control is to *randomize* the patients into the treatment and control groups. With random selection, there will likely be people who have relatively healthy diets in both the control and treatment groups. If the drugs work, in theory they should still work better for the treatment group than for those taking the placebo. And likewise, patients with less healthy diets will generally be mixed up in both groups, and the drug should also work better for them.

The mantra of experimental design is, "Control as much as you can. Randomize to take care of the rest."

There are lots of aspects of experimental design that we will not go into here (for example, blinding and blocking). But we will continue to mention the differences between observational studies and experiments in future chapters as we exercise caution in making causal claims.


## Prediction versus explanation {#correlation-pred-exp}

Even when claims are not causal, we can use associations (and correlations more specifically) for purposes of *prediction*.

##### Exercise 13 {-}

If I tell you that ice cream sales are high right now, can you make a reasonable prediction about the relative number of drowning deaths this month (high or low)? Why or why not?

::: {.answer}

Please write up your answer here.

:::

*****


So even when there is no direct causal link between two variables, if they are positively correlated, then large values of one variable are associated with large values of the other variable. So if I tell you one value is large, it is reasonable to predict that the other value will be large as well.

We use the language "predictor" variable and "response" variable to reinforce this idea.

In a properly designed and controlled experiment, we can use different language. In this case, we can *explain* the outcome using the treatment variable. If we've controlled for everything else, the only possible explanation for a difference between the treatment and control groups must be the treatment variable. If the patients get better on the drug (more so than those on the placebo) and we've controlled for every other possible confounding variable, the only possible explanation is that the drug works. The drug "explains" the difference in the response variable.

Be careful, as sometimes statisticians use the term "explanatory variable" to mean any kind of variable that predicts or explains. In this course, we will try to use the term "predictor variable" exclusively.


## Conclusion {#correlation-conclusion}

If we have two numerical variables that have a linear association between them (also assuming there are no serious outliers), we can compute the correlation coefficient that measures the strength and direction of that linear association.

Keep in mind that in an observational study, this correlation is a measure of association, but it does not signify that one variable causes the other. It's possible that one variable causes the other, but it's also possible that a third "lurking" variable is responsible for the association. Either way, the fact that a relationship exists means it is possible to use values of one variable to make reasonable predictions about the values of the other variable.

In a properly designed experiment, the manipulation of one variable while controlling for others (and randomizing to take care of other confounders) ensures that there is a causal link between the treatment variable and the response of interest. In this case, the treatment can "explain" the response, not just predict it.


### Preparing and submitting your assignment {#correlation-prep}

1. From the "Run" menu, select "Restart R and Run All Chunks".
2. Deal with any code errors that crop up. Repeat steps 1–-2 until there are no more code errors.
3. Spell check your document by clicking the icon with "ABC" and a check mark.
4. Hit the "Preview" button one last time to generate the final draft of the `.nb.html` file.
5. Proofread the HTML file carefully. If there are errors, go back and fix them, then repeat steps 1--5 again.

If you have completed this chapter as part of a statistics course, follow the directions you receive from your professor to submit your assignment.




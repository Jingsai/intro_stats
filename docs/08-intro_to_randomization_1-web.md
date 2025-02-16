# Introduction to randomization, Part 1 {#randomization1}

<!-- Please don't mess with the next few lines! -->
<style>h5{font-size:2em;color:#0000FF}h6{font-size:1.5em;color:#0000FF}div.answer{margin-left:5%;border:1px solid #0000FF;border-left-width:10px;padding:25px} div.summary{background-color:rgba(30,144,255,0.1);border:3px double #0000FF;padding:25px}</style><p style="color:#ffffff">2.0</p>
<!-- Please don't mess with the previous few lines! -->

::: {.summary}

### Functions introduced in this chapter {-}

`set.seed`, `rflip`, `do`

:::


## Introduction {#randomization1-intro}

In this module, we'll learn about randomization and simulation. When we want to understand how sampling works, it's helpful to simulate the process of drawing samples repeatedly from a population. In the days before computing, this was very difficult to do. Now, a few simple lines of computer code can generate thousands (even millions) of random samples, often in a matter of seconds or less.


### Install new packages {#randomization1-install}

If you are using RStudio Workbench, you do not need to install any packages. (Any packages you need should already be installed by the server administrators.)

If you are using R and RStudio on your own machine instead of accessing RStudio Workbench through a browser, you'll need to type the following command at the Console:

```
install.packages("mosaic")
```

### Download the R notebook file {#randomization1-download}

Check the upper-right corner in RStudio to make sure you're in your `intro_stats` project. Then click on the following link to download this chapter as an R notebook file (`.Rmd`).

<a  target="_blank" href = "https://raw.githubusercontent.com/Jingsai/intro_stats/main/docs/chapter_downloads/08-intro_to_randomization_1.Rmd"
      Download = "08-intro_to_randomization_1.Rmd">
      <button type = "button"> Right Click and Save the link as a File </button>
</a>

Once the file is downloaded, move it to your project folder in RStudio and open it there.

### Restart R and run all chunks {#randomization1-restart}

In RStudio, select "Restart R and Run All Chunks" from the "Run" menu.

### Load packages {#randomization1-load}

We load the `tidyverse` package. The `mosaic` package contains some tools for making it easier to learn about randomization and simulation.


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
library(mosaic)
```

```
## Registered S3 method overwritten by 'mosaic':
##   method                           from   
##   fortify.SpatialPolygonsDataFrame ggplot2
## 
## The 'mosaic' package masks several functions from core packages in order to add 
## additional features.  The original behavior of these functions should not be affected by this.
## 
## Attaching package: 'mosaic'
## 
## The following object is masked from 'package:Matrix':
## 
##     mean
## 
## The following objects are masked from 'package:dplyr':
## 
##     count, do, tally
## 
## The following object is masked from 'package:purrr':
## 
##     cross
## 
## The following object is masked from 'package:ggplot2':
## 
##     stat
## 
## The following objects are masked from 'package:stats':
## 
##     binom.test, cor, cor.test, cov, fivenum, IQR, median, prop.test,
##     quantile, sd, t.test, var
## 
## The following objects are masked from 'package:base':
## 
##     max, mean, min, prod, range, sample, sum
```


## Sample and population {#randomization1-sample-pop}

The goal of the next few chapters is to help you think about the process of sampling from a population. What do these terms mean?

A *population* is a group of objects we would like to study. If that sounds vague, that's because it is. A population can be a group of any size and of any type of thing in which we're interested. Often, populations refer to groups of people. For example, in an election, the population of interest is all voters. But if you're a biologist, you might study populations of other kinds of organisms. If you're an engineer, you might study populations of bolts on bridges. If you're in finance, you might study populations of loans.

Populations are usually inaccessible in their entirety. It is impossible to survey every voter in any reasonably sized election, for example. Therefore, to study them, we have to collect a *sample*. A sample is a subset of the population. We might conduct a poll of 2000 voters to try to learn about voting intentions for the entire population. Of course, for that to work, the sample has to be *representative* of its population. We'll have more to say about that in the future.


## Flipping a coin {#randomization1-coin}

Before we talk about how samples are obtained from populations in the real world, we're going to perform some simulations.

One of the simplest acts to simulate is flipping a coin. We could get an actual coin and physically flip it over and over again, but that is time-consuming and annoying. It is much easier to flip a "virtual" coin inside the computer. One way to accomplish this in R is to use the `rflip` command from the `mosaic` package. To make sure we're flipping a fair coin, we'll say that we want a 50% chance of heads by including the parameter `prob = 0.5`.

One more bit of technical detail. Since there will be some randomness involved here, we will need to include an R command to ensure that we all get the same results every time this code runs. This is called "setting the seed". Don't worry too much about what this is doing under the hood. The basic idea is that two people who start with the same seed will generate the same sequence of "random" numbers.

The seed `1234` in the chunk below is totally arbitrary. It could have been any number at all. (And, in fact, we'll use different numbers just for fun.) If you change the seed, you will get different output, so we all need to use the same seed. But the actual common value we all use for the seed is irrelevant.

Here is one coin flip with a 50% chance of coming up heads:


```r
set.seed(1234)
rflip(1, prob = 0.5)
```

```
## 
## Flipping 1 coin [ Prob(Heads) = 0.5 ] ...
## 
## T
## 
## Number of Heads: 0 [Proportion Heads: 0]
```

Here are ten coin flips, each with a 50% chance of coming up heads:


```r
set.seed(1234)
rflip(10, prob = 0.5)
```

```
## 
## Flipping 10 coins [ Prob(Heads) = 0.5 ] ...
## 
## T H H H H H T T H H
## 
## Number of Heads: 7 [Proportion Heads: 0.7]
```

Just to confirm that this is a random process, let's flip ten coins again (but without setting the seed again):


```r
rflip(10, prob = 0.5)
```

```
## 
## Flipping 10 coins [ Prob(Heads) = 0.5 ] ...
## 
## H H T H T H T T T T
## 
## Number of Heads: 4 [Proportion Heads: 0.4]
```

If we return to the previous seed of 1234, we should obtain the same ten coin flips we did at first:


```r
set.seed(1234)
rflip(10, prob = 0.5)
```

```
## 
## Flipping 10 coins [ Prob(Heads) = 0.5 ] ...
## 
## T H H H H H T T H H
## 
## Number of Heads: 7 [Proportion Heads: 0.7]
```

And just to see the effect of setting a different seed:


```r
set.seed(9999)
rflip(10, prob = 0.5)
```

```
## 
## Flipping 10 coins [ Prob(Heads) = 0.5 ] ...
## 
## H H H T H H T H H H
## 
## Number of Heads: 8 [Proportion Heads: 0.8]
```

##### Exercise 1 {-}

In ten coin flips, how many would you generally expect to come up heads? Is that the actual number of heads you saw in the simulations above? Why aren't the simulations coming up with the expected number of heads each time?

::: {.answer}

Please write up your answer here.

:::


## Multiple simulations {#randomization1-multiple}

Suppose now that you are not the only person flipping coins. Suppose a bunch of people in a room are all flipping coins. We'll start with ten coin flips per person, a task that could be reasonably done even without a computer.

You might observe three heads in ten flips. Fine, but what about everyone else in the room? What numbers of heads will they see?

The `do` command from `mosaic` is a way of doing something multiple times. Imagine there are twenty people in the room, each flipping a coin ten times, each time with a 50% probability of coming up heads. Observe:


```r
set.seed(12345)
do(20) * rflip(10, prob = 0.5)
```

```
##     n heads tails prop
## 1  10     2     8  0.2
## 2  10     5     5  0.5
## 3  10     5     5  0.5
## 4  10     4     6  0.4
## 5  10     4     6  0.4
## 6  10     7     3  0.7
## 7  10     6     4  0.6
## 8  10     5     5  0.5
## 9  10     7     3  0.7
## 10 10     7     3  0.7
## 11 10     6     4  0.6
## 12 10     7     3  0.7
## 13 10     7     3  0.7
## 14 10     6     4  0.6
## 15 10     7     3  0.7
## 16 10     6     4  0.6
## 17 10     7     3  0.7
## 18 10     3     7  0.3
## 19 10     4     6  0.4
## 20 10     7     3  0.7
```

The syntax could not be any simpler: `do(20) *` means, literally, "do twenty times." In other words, this command is telling R to repeat an action twenty times, where the action is flipping a single coin ten times.

You'll notice that in place of a list of outcomes (H or T) of all the individual flips, we have instead a summary of the number of heads and tails each person sees. Each row represents a person, and the columns give information about each person's flips. (There are `n = 10` flips for each person, but then the number of heads/tails---and the corresponding "proportion" of heads---changes from person to person.)

Looking at the above rows and columns, we see that the output of our little coin-flipping experiment is actually stored in a data frame! Let's give it a name and work with it.


```r
set.seed(12345)
coin_flips_20_10 <- do(20) * rflip(10, prob = 0.5)
coin_flips_20_10
```

```
##     n heads tails prop
## 1  10     2     8  0.2
## 2  10     5     5  0.5
## 3  10     5     5  0.5
## 4  10     4     6  0.4
## 5  10     4     6  0.4
## 6  10     7     3  0.7
## 7  10     6     4  0.6
## 8  10     5     5  0.5
## 9  10     7     3  0.7
## 10 10     7     3  0.7
## 11 10     6     4  0.6
## 12 10     7     3  0.7
## 13 10     7     3  0.7
## 14 10     6     4  0.6
## 15 10     7     3  0.7
## 16 10     6     4  0.6
## 17 10     7     3  0.7
## 18 10     3     7  0.3
## 19 10     4     6  0.4
## 20 10     7     3  0.7
```

It is significant that we can store our outcomes this way. Because we have a data frame, we can apply all our data analysis tools (graphs, charts, tables, summary statistics, etc.) to the "data" generated from our set of simulations.

For example, what is the mean number of heads these twenty people observed?


```r
mean(coin_flips_20_10$heads)
```

```
## [1] 5.6
```

##### Exercise 2 {-}

The data frame `coin_flips_20_10` contains four variables: `n`, `heads`, `tails`, and `prop`. In the code chunk above, we calculated `mean(coin_flips_20_10$heads)` which gave us the mean count of heads for all people flipping coins. Instead of calculating the mean count of heads, change the variable from `heads` to `prop` to calculate the mean *proportion* of heads. Then explain why your answer makes sense in light of the mean count of heads calculated above.

::: {.answer}


```r
# Add code here to calculate the mean proportion of heads.
```

Please write up your answer here.

:::

*****

Let's look at a histogram of the number of heads we see in the simulated flips. (The fancy stuff in `scale_x_continuous` is just making sure that the x-axis goes from 0 to 10 and that the tick marks appear on each whole number.)


```r
ggplot(coin_flips_20_10, aes(x = heads)) +
    geom_histogram(binwidth = 0.5) +
    scale_x_continuous(limits = c(-1, 11), breaks = seq(0, 10, 1))
```

```
## Warning: Removed 2 rows containing missing values (geom_bar).
```

<img src="08-intro_to_randomization_1-web_files/figure-html/unnamed-chunk-11-1.png" width="672" />

Let's do the same thing, but now let's consider the *proportion* of heads.


```r
ggplot(coin_flips_20_10, aes(x = prop)) +
    geom_histogram(binwidth = 0.05) +
    scale_x_continuous(limits = c(-0.1, 1.1), breaks = seq(0, 1, 0.1))
```

```
## Warning: Removed 2 rows containing missing values (geom_bar).
```

<img src="08-intro_to_randomization_1-web_files/figure-html/unnamed-chunk-12-1.png" width="672" />


## Bigger and better! {#randomization1-bigger}

With only twenty people, it was possible that, for example, nobody would get all heads or all tails. Indeed, in `coin_flips_20_10` there were no people who got all heads or all tails. Also, there were more people with six and seven heads than with five heads, even though we "expected" the average to be five heads. There is nothing particularly significant about that; it happened by pure chance alone. Another run through the above commands would generate a somewhat different outcome. That's what happens when things are random.

Instead, let's imagine that we recruited way more people to flip coins with us. Let's try it again with 2000 people:


```r
set.seed(1234)
coin_flips_2000_10 <- do(2000) * rflip(10, prob = 0.5)
coin_flips_2000_10
```

```
##       n heads tails prop
## 1    10     4     6  0.4
## 2    10     4     6  0.4
## 3    10     4     6  0.4
## 4    10     6     4  0.6
## 5    10     5     5  0.5
## 6    10     4     6  0.4
## 7    10     4     6  0.4
## 8    10     4     6  0.4
## 9    10     3     7  0.3
## 10   10     1     9  0.1
## 11   10     5     5  0.5
## 12   10     5     5  0.5
## 13   10     7     3  0.7
## 14   10     7     3  0.7
## 15   10     5     5  0.5
## 16   10     3     7  0.3
## 17   10     5     5  0.5
## 18   10     5     5  0.5
## 19   10     9     1  0.9
## 20   10     6     4  0.6
## 21   10     7     3  0.7
## 22   10     2     8  0.2
## 23   10     6     4  0.6
## 24   10     6     4  0.6
## 25   10     5     5  0.5
## 26   10     4     6  0.4
## 27   10     5     5  0.5
## 28   10     5     5  0.5
## 29   10     6     4  0.6
## 30   10     6     4  0.6
## 31   10     3     7  0.3
## 32   10     3     7  0.3
## 33   10     4     6  0.4
## 34   10     5     5  0.5
## 35   10     7     3  0.7
## 36   10     6     4  0.6
## 37   10     4     6  0.4
## 38   10     3     7  0.3
## 39   10     7     3  0.7
## 40   10     6     4  0.6
## 41   10     6     4  0.6
## 42   10     3     7  0.3
## 43   10     7     3  0.7
## 44   10     9     1  0.9
## 45   10     7     3  0.7
## 46   10     5     5  0.5
## 47   10     4     6  0.4
## 48   10     6     4  0.6
## 49   10     7     3  0.7
## 50   10     8     2  0.8
## 51   10     6     4  0.6
## 52   10     5     5  0.5
## 53   10     7     3  0.7
## 54   10     7     3  0.7
## 55   10     5     5  0.5
## 56   10     6     4  0.6
## 57   10     5     5  0.5
## 58   10     5     5  0.5
## 59   10     7     3  0.7
## 60   10     3     7  0.3
## 61   10     4     6  0.4
## 62   10     6     4  0.6
## 63   10     6     4  0.6
## 64   10     6     4  0.6
## 65   10     5     5  0.5
## 66   10     6     4  0.6
## 67   10     5     5  0.5
## 68   10     4     6  0.4
## 69   10     4     6  0.4
## 70   10     4     6  0.4
## 71   10     4     6  0.4
## 72   10     4     6  0.4
## 73   10     7     3  0.7
## 74   10     3     7  0.3
## 75   10     7     3  0.7
## 76   10     6     4  0.6
## 77   10     6     4  0.6
## 78   10     4     6  0.4
## 79   10     7     3  0.7
## 80   10     4     6  0.4
## 81   10     4     6  0.4
## 82   10     1     9  0.1
## 83   10     7     3  0.7
## 84   10     7     3  0.7
## 85   10     7     3  0.7
## 86   10     3     7  0.3
## 87   10     6     4  0.6
## 88   10     4     6  0.4
## 89   10     7     3  0.7
## 90   10     4     6  0.4
## 91   10     3     7  0.3
## 92   10     4     6  0.4
## 93   10     5     5  0.5
## 94   10     6     4  0.6
## 95   10     6     4  0.6
## 96   10     4     6  0.4
## 97   10     7     3  0.7
## 98   10     5     5  0.5
## 99   10     5     5  0.5
## 100  10     4     6  0.4
## 101  10     6     4  0.6
## 102  10     3     7  0.3
## 103  10     5     5  0.5
## 104  10     6     4  0.6
## 105  10     5     5  0.5
## 106  10     6     4  0.6
## 107  10     2     8  0.2
## 108  10     4     6  0.4
## 109  10     4     6  0.4
## 110  10     2     8  0.2
## 111  10     5     5  0.5
## 112  10     4     6  0.4
## 113  10     5     5  0.5
## 114  10     4     6  0.4
## 115  10     1     9  0.1
## 116  10     5     5  0.5
## 117  10     2     8  0.2
## 118  10     8     2  0.8
## 119  10     4     6  0.4
## 120  10     7     3  0.7
## 121  10     5     5  0.5
## 122  10     7     3  0.7
## 123  10     5     5  0.5
## 124  10     6     4  0.6
## 125  10     4     6  0.4
## 126  10     6     4  0.6
## 127  10     8     2  0.8
## 128  10     2     8  0.2
## 129  10     6     4  0.6
## 130  10     4     6  0.4
## 131  10     6     4  0.6
## 132  10     3     7  0.3
## 133  10     3     7  0.3
## 134  10     5     5  0.5
## 135  10     6     4  0.6
## 136  10     3     7  0.3
## 137  10     7     3  0.7
## 138  10     6     4  0.6
## 139  10     5     5  0.5
## 140  10     5     5  0.5
## 141  10     4     6  0.4
## 142  10     7     3  0.7
## 143  10     3     7  0.3
## 144  10     4     6  0.4
## 145  10     4     6  0.4
## 146  10     6     4  0.6
## 147  10     6     4  0.6
## 148  10     6     4  0.6
## 149  10     7     3  0.7
## 150  10     8     2  0.8
## 151  10     3     7  0.3
## 152  10     3     7  0.3
## 153  10     4     6  0.4
## 154  10     4     6  0.4
## 155  10     3     7  0.3
## 156  10     2     8  0.2
## 157  10     3     7  0.3
## 158  10     7     3  0.7
## 159  10     5     5  0.5
## 160  10     3     7  0.3
## 161  10     4     6  0.4
## 162  10     6     4  0.6
## 163  10     4     6  0.4
## 164  10     5     5  0.5
## 165  10     4     6  0.4
## 166  10     4     6  0.4
## 167  10     3     7  0.3
## 168  10     4     6  0.4
## 169  10     4     6  0.4
## 170  10     4     6  0.4
## 171  10     4     6  0.4
## 172  10     4     6  0.4
## 173  10     7     3  0.7
## 174  10     3     7  0.3
## 175  10     8     2  0.8
## 176  10     5     5  0.5
## 177  10     8     2  0.8
## 178  10     4     6  0.4
## 179  10     5     5  0.5
## 180  10     3     7  0.3
## 181  10     7     3  0.7
## 182  10     5     5  0.5
## 183  10     4     6  0.4
## 184  10     3     7  0.3
## 185  10     6     4  0.6
## 186  10     6     4  0.6
## 187  10     7     3  0.7
## 188  10     3     7  0.3
## 189  10     5     5  0.5
## 190  10     7     3  0.7
## 191  10     4     6  0.4
## 192  10     6     4  0.6
## 193  10     4     6  0.4
## 194  10     5     5  0.5
## 195  10     5     5  0.5
## 196  10     8     2  0.8
## 197  10     9     1  0.9
## 198  10     5     5  0.5
## 199  10     7     3  0.7
## 200  10     5     5  0.5
## 201  10     4     6  0.4
## 202  10     5     5  0.5
## 203  10     3     7  0.3
## 204  10     5     5  0.5
## 205  10     6     4  0.6
## 206  10     3     7  0.3
## 207  10     4     6  0.4
## 208  10     3     7  0.3
## 209  10     4     6  0.4
## 210  10     9     1  0.9
## 211  10     4     6  0.4
## 212  10     5     5  0.5
## 213  10     6     4  0.6
## 214  10     3     7  0.3
## 215  10     5     5  0.5
## 216  10     7     3  0.7
## 217  10     4     6  0.4
## 218  10     6     4  0.6
## 219  10     4     6  0.4
## 220  10     4     6  0.4
## 221  10     4     6  0.4
## 222  10     4     6  0.4
## 223  10    10     0  1.0
## 224  10     4     6  0.4
## 225  10     3     7  0.3
## 226  10     8     2  0.8
## 227  10     7     3  0.7
## 228  10     6     4  0.6
## 229  10     6     4  0.6
## 230  10     4     6  0.4
## 231  10     6     4  0.6
## 232  10     4     6  0.4
## 233  10     6     4  0.6
## 234  10     3     7  0.3
## 235  10     4     6  0.4
## 236  10     4     6  0.4
## 237  10     5     5  0.5
## 238  10     3     7  0.3
## 239  10     4     6  0.4
## 240  10     7     3  0.7
## 241  10     8     2  0.8
## 242  10     6     4  0.6
## 243  10     6     4  0.6
## 244  10     7     3  0.7
## 245  10     6     4  0.6
## 246  10     6     4  0.6
## 247  10     8     2  0.8
## 248  10     4     6  0.4
## 249  10     4     6  0.4
## 250  10     4     6  0.4
## 251  10     4     6  0.4
## 252  10     5     5  0.5
## 253  10     5     5  0.5
## 254  10     3     7  0.3
## 255  10     4     6  0.4
## 256  10     5     5  0.5
## 257  10     6     4  0.6
## 258  10     6     4  0.6
## 259  10     6     4  0.6
## 260  10     8     2  0.8
## 261  10     5     5  0.5
## 262  10     5     5  0.5
## 263  10     1     9  0.1
## 264  10     6     4  0.6
## 265  10     3     7  0.3
## 266  10     4     6  0.4
## 267  10     6     4  0.6
## 268  10     7     3  0.7
## 269  10     7     3  0.7
## 270  10     5     5  0.5
## 271  10     5     5  0.5
## 272  10     5     5  0.5
## 273  10     5     5  0.5
## 274  10     6     4  0.6
## 275  10     5     5  0.5
## 276  10     6     4  0.6
## 277  10     6     4  0.6
## 278  10     5     5  0.5
## 279  10     5     5  0.5
## 280  10     5     5  0.5
## 281  10    10     0  1.0
## 282  10     5     5  0.5
## 283  10     7     3  0.7
## 284  10     4     6  0.4
## 285  10     5     5  0.5
## 286  10     6     4  0.6
## 287  10     6     4  0.6
## 288  10     3     7  0.3
## 289  10     6     4  0.6
## 290  10     5     5  0.5
## 291  10     7     3  0.7
## 292  10     4     6  0.4
## 293  10     4     6  0.4
## 294  10     3     7  0.3
## 295  10     8     2  0.8
## 296  10     2     8  0.2
## 297  10     5     5  0.5
## 298  10     4     6  0.4
## 299  10     7     3  0.7
## 300  10     3     7  0.3
## 301  10     3     7  0.3
## 302  10     6     4  0.6
## 303  10     6     4  0.6
## 304  10     6     4  0.6
## 305  10     4     6  0.4
## 306  10     5     5  0.5
## 307  10     4     6  0.4
## 308  10     5     5  0.5
## 309  10     3     7  0.3
## 310  10     6     4  0.6
## 311  10     6     4  0.6
## 312  10     5     5  0.5
## 313  10     4     6  0.4
## 314  10     3     7  0.3
## 315  10     5     5  0.5
## 316  10     3     7  0.3
## 317  10     4     6  0.4
## 318  10     6     4  0.6
## 319  10     4     6  0.4
## 320  10     2     8  0.2
## 321  10     5     5  0.5
## 322  10     6     4  0.6
## 323  10     4     6  0.4
## 324  10     6     4  0.6
## 325  10     4     6  0.4
## 326  10     4     6  0.4
## 327  10     6     4  0.6
## 328  10     5     5  0.5
## 329  10     7     3  0.7
## 330  10     4     6  0.4
## 331  10     3     7  0.3
## 332  10     4     6  0.4
## 333  10     5     5  0.5
## 334  10     5     5  0.5
## 335  10     6     4  0.6
## 336  10     4     6  0.4
## 337  10     3     7  0.3
## 338  10     6     4  0.6
## 339  10     4     6  0.4
## 340  10     2     8  0.2
## 341  10     7     3  0.7
## 342  10     3     7  0.3
## 343  10     6     4  0.6
## 344  10     4     6  0.4
## 345  10     0    10  0.0
## 346  10     3     7  0.3
## 347  10     6     4  0.6
## 348  10     5     5  0.5
## 349  10     7     3  0.7
## 350  10     3     7  0.3
## 351  10     6     4  0.6
## 352  10     7     3  0.7
## 353  10     6     4  0.6
## 354  10     8     2  0.8
## 355  10     6     4  0.6
## 356  10     4     6  0.4
## 357  10     8     2  0.8
## 358  10     2     8  0.2
## 359  10     4     6  0.4
## 360  10     6     4  0.6
## 361  10     2     8  0.2
## 362  10     4     6  0.4
## 363  10     5     5  0.5
## 364  10     4     6  0.4
## 365  10     7     3  0.7
## 366  10     6     4  0.6
## 367  10     6     4  0.6
## 368  10     2     8  0.2
## 369  10     4     6  0.4
## 370  10     6     4  0.6
## 371  10     2     8  0.2
## 372  10     4     6  0.4
## 373  10     2     8  0.2
## 374  10     4     6  0.4
## 375  10     8     2  0.8
## 376  10     6     4  0.6
## 377  10     6     4  0.6
## 378  10     6     4  0.6
## 379  10     6     4  0.6
## 380  10     6     4  0.6
## 381  10     6     4  0.6
## 382  10     8     2  0.8
## 383  10     4     6  0.4
## 384  10     6     4  0.6
## 385  10     4     6  0.4
## 386  10     3     7  0.3
## 387  10     6     4  0.6
## 388  10     4     6  0.4
## 389  10     6     4  0.6
## 390  10     5     5  0.5
## 391  10     4     6  0.4
## 392  10     6     4  0.6
## 393  10     6     4  0.6
## 394  10     5     5  0.5
## 395  10     4     6  0.4
## 396  10     6     4  0.6
## 397  10     4     6  0.4
## 398  10     7     3  0.7
## 399  10     4     6  0.4
## 400  10     6     4  0.6
## 401  10     3     7  0.3
## 402  10     6     4  0.6
## 403  10     7     3  0.7
## 404  10     4     6  0.4
## 405  10     6     4  0.6
## 406  10     3     7  0.3
## 407  10     7     3  0.7
## 408  10     8     2  0.8
## 409  10     4     6  0.4
## 410  10     6     4  0.6
## 411  10     4     6  0.4
## 412  10     3     7  0.3
## 413  10     4     6  0.4
## 414  10     7     3  0.7
## 415  10     3     7  0.3
## 416  10     5     5  0.5
## 417  10     5     5  0.5
## 418  10     7     3  0.7
## 419  10     6     4  0.6
## 420  10     5     5  0.5
## 421  10     6     4  0.6
## 422  10     3     7  0.3
## 423  10     5     5  0.5
## 424  10     4     6  0.4
## 425  10     5     5  0.5
## 426  10     5     5  0.5
## 427  10     3     7  0.3
## 428  10     6     4  0.6
## 429  10     4     6  0.4
## 430  10     6     4  0.6
## 431  10     7     3  0.7
## 432  10     7     3  0.7
## 433  10     5     5  0.5
## 434  10     4     6  0.4
## 435  10     4     6  0.4
## 436  10     3     7  0.3
## 437  10     4     6  0.4
## 438  10     5     5  0.5
## 439  10     7     3  0.7
## 440  10     5     5  0.5
## 441  10     5     5  0.5
## 442  10     7     3  0.7
## 443  10     8     2  0.8
## 444  10     6     4  0.6
## 445  10     5     5  0.5
## 446  10     4     6  0.4
## 447  10     3     7  0.3
## 448  10     5     5  0.5
## 449  10     6     4  0.6
## 450  10     7     3  0.7
## 451  10     9     1  0.9
## 452  10     5     5  0.5
## 453  10     5     5  0.5
## 454  10     3     7  0.3
## 455  10     5     5  0.5
## 456  10     5     5  0.5
## 457  10     5     5  0.5
## 458  10     3     7  0.3
## 459  10     3     7  0.3
## 460  10     5     5  0.5
## 461  10     4     6  0.4
## 462  10     7     3  0.7
## 463  10     7     3  0.7
## 464  10     3     7  0.3
## 465  10     4     6  0.4
## 466  10     5     5  0.5
## 467  10     5     5  0.5
## 468  10     3     7  0.3
## 469  10     8     2  0.8
## 470  10     5     5  0.5
## 471  10     6     4  0.6
## 472  10     5     5  0.5
## 473  10     7     3  0.7
## 474  10     4     6  0.4
## 475  10     4     6  0.4
## 476  10     5     5  0.5
## 477  10     2     8  0.2
## 478  10     6     4  0.6
## 479  10     6     4  0.6
## 480  10     2     8  0.2
## 481  10     6     4  0.6
## 482  10     5     5  0.5
## 483  10     5     5  0.5
## 484  10     6     4  0.6
## 485  10     4     6  0.4
## 486  10     5     5  0.5
## 487  10     6     4  0.6
## 488  10     3     7  0.3
## 489  10     3     7  0.3
## 490  10     6     4  0.6
## 491  10     4     6  0.4
## 492  10     7     3  0.7
## 493  10     4     6  0.4
## 494  10     6     4  0.6
## 495  10     4     6  0.4
## 496  10     8     2  0.8
## 497  10     5     5  0.5
## 498  10     6     4  0.6
## 499  10     6     4  0.6
## 500  10     4     6  0.4
## 501  10     4     6  0.4
## 502  10     5     5  0.5
## 503  10     3     7  0.3
## 504  10     3     7  0.3
## 505  10     6     4  0.6
## 506  10     5     5  0.5
## 507  10     6     4  0.6
## 508  10     5     5  0.5
## 509  10     5     5  0.5
## 510  10     6     4  0.6
## 511  10     5     5  0.5
## 512  10     4     6  0.4
## 513  10     6     4  0.6
## 514  10     5     5  0.5
## 515  10     5     5  0.5
## 516  10     9     1  0.9
## 517  10     4     6  0.4
## 518  10     2     8  0.2
## 519  10     3     7  0.3
## 520  10     4     6  0.4
## 521  10     2     8  0.2
## 522  10     6     4  0.6
## 523  10     6     4  0.6
## 524  10     7     3  0.7
## 525  10     5     5  0.5
## 526  10     7     3  0.7
## 527  10     7     3  0.7
## 528  10     2     8  0.2
## 529  10     4     6  0.4
## 530  10     8     2  0.8
## 531  10     5     5  0.5
## 532  10     6     4  0.6
## 533  10     8     2  0.8
## 534  10     3     7  0.3
## 535  10     4     6  0.4
## 536  10     6     4  0.6
## 537  10     8     2  0.8
## 538  10     4     6  0.4
## 539  10     4     6  0.4
## 540  10     6     4  0.6
## 541  10     5     5  0.5
## 542  10     4     6  0.4
## 543  10     5     5  0.5
## 544  10     5     5  0.5
## 545  10     3     7  0.3
## 546  10     4     6  0.4
## 547  10     6     4  0.6
## 548  10     4     6  0.4
## 549  10     6     4  0.6
## 550  10     4     6  0.4
## 551  10     6     4  0.6
## 552  10     3     7  0.3
## 553  10     5     5  0.5
## 554  10     6     4  0.6
## 555  10     5     5  0.5
## 556  10     8     2  0.8
## 557  10     2     8  0.2
## 558  10     5     5  0.5
## 559  10     4     6  0.4
## 560  10     5     5  0.5
## 561  10     4     6  0.4
## 562  10     6     4  0.6
## 563  10     6     4  0.6
## 564  10     4     6  0.4
## 565  10     2     8  0.2
## 566  10     3     7  0.3
## 567  10     6     4  0.6
## 568  10     3     7  0.3
## 569  10     5     5  0.5
## 570  10     7     3  0.7
## 571  10     8     2  0.8
## 572  10     6     4  0.6
## 573  10     4     6  0.4
## 574  10     6     4  0.6
## 575  10     3     7  0.3
## 576  10     4     6  0.4
## 577  10     5     5  0.5
## 578  10     7     3  0.7
## 579  10     4     6  0.4
## 580  10     4     6  0.4
## 581  10     2     8  0.2
## 582  10     6     4  0.6
## 583  10     5     5  0.5
## 584  10     5     5  0.5
## 585  10     5     5  0.5
## 586  10     6     4  0.6
## 587  10     6     4  0.6
## 588  10     8     2  0.8
## 589  10     5     5  0.5
## 590  10     8     2  0.8
## 591  10     5     5  0.5
## 592  10     6     4  0.6
## 593  10     7     3  0.7
## 594  10     3     7  0.3
## 595  10     4     6  0.4
## 596  10     2     8  0.2
## 597  10     5     5  0.5
## 598  10     6     4  0.6
## 599  10     6     4  0.6
## 600  10     7     3  0.7
## 601  10     4     6  0.4
## 602  10     6     4  0.6
## 603  10     6     4  0.6
## 604  10     5     5  0.5
## 605  10     5     5  0.5
## 606  10     7     3  0.7
## 607  10     7     3  0.7
## 608  10     6     4  0.6
## 609  10     3     7  0.3
## 610  10     4     6  0.4
## 611  10     9     1  0.9
## 612  10     6     4  0.6
## 613  10     5     5  0.5
## 614  10     4     6  0.4
## 615  10     6     4  0.6
## 616  10     4     6  0.4
## 617  10     7     3  0.7
## 618  10     3     7  0.3
## 619  10     6     4  0.6
## 620  10     5     5  0.5
## 621  10     7     3  0.7
## 622  10     5     5  0.5
## 623  10     5     5  0.5
## 624  10     5     5  0.5
## 625  10     6     4  0.6
## 626  10     3     7  0.3
## 627  10     4     6  0.4
## 628  10     8     2  0.8
## 629  10     6     4  0.6
## 630  10     6     4  0.6
## 631  10     5     5  0.5
## 632  10     3     7  0.3
## 633  10     5     5  0.5
## 634  10     4     6  0.4
## 635  10     6     4  0.6
## 636  10     7     3  0.7
## 637  10     5     5  0.5
## 638  10     4     6  0.4
## 639  10     4     6  0.4
## 640  10     5     5  0.5
## 641  10     3     7  0.3
## 642  10     4     6  0.4
## 643  10     5     5  0.5
## 644  10     7     3  0.7
## 645  10     5     5  0.5
## 646  10     5     5  0.5
## 647  10     5     5  0.5
## 648  10     4     6  0.4
## 649  10     5     5  0.5
## 650  10     7     3  0.7
## 651  10     3     7  0.3
## 652  10     6     4  0.6
## 653  10     6     4  0.6
## 654  10     8     2  0.8
## 655  10     7     3  0.7
## 656  10     4     6  0.4
## 657  10     7     3  0.7
## 658  10     5     5  0.5
## 659  10     7     3  0.7
## 660  10     6     4  0.6
## 661  10     2     8  0.2
## 662  10     8     2  0.8
## 663  10     2     8  0.2
## 664  10     6     4  0.6
## 665  10     4     6  0.4
## 666  10     3     7  0.3
## 667  10     5     5  0.5
## 668  10     6     4  0.6
## 669  10     6     4  0.6
## 670  10     4     6  0.4
## 671  10     7     3  0.7
## 672  10     2     8  0.2
## 673  10     2     8  0.2
## 674  10     6     4  0.6
## 675  10     5     5  0.5
## 676  10     8     2  0.8
## 677  10     5     5  0.5
## 678  10     5     5  0.5
## 679  10     5     5  0.5
## 680  10     5     5  0.5
## 681  10     6     4  0.6
## 682  10     4     6  0.4
## 683  10     2     8  0.2
## 684  10     6     4  0.6
## 685  10     4     6  0.4
## 686  10     5     5  0.5
## 687  10     5     5  0.5
## 688  10     6     4  0.6
## 689  10     6     4  0.6
## 690  10     4     6  0.4
## 691  10     4     6  0.4
## 692  10     4     6  0.4
## 693  10     5     5  0.5
## 694  10     5     5  0.5
## 695  10     5     5  0.5
## 696  10     5     5  0.5
## 697  10     6     4  0.6
## 698  10     6     4  0.6
## 699  10     5     5  0.5
## 700  10     7     3  0.7
## 701  10     2     8  0.2
## 702  10     7     3  0.7
## 703  10     7     3  0.7
## 704  10     1     9  0.1
## 705  10     5     5  0.5
## 706  10     5     5  0.5
## 707  10     4     6  0.4
## 708  10     4     6  0.4
## 709  10     6     4  0.6
## 710  10     3     7  0.3
## 711  10     4     6  0.4
## 712  10     5     5  0.5
## 713  10     8     2  0.8
## 714  10     3     7  0.3
## 715  10     6     4  0.6
## 716  10     5     5  0.5
## 717  10     4     6  0.4
## 718  10     2     8  0.2
## 719  10     3     7  0.3
## 720  10     1     9  0.1
## 721  10     3     7  0.3
## 722  10     6     4  0.6
## 723  10     3     7  0.3
## 724  10     5     5  0.5
## 725  10     5     5  0.5
## 726  10     7     3  0.7
## 727  10     7     3  0.7
## 728  10     3     7  0.3
## 729  10     4     6  0.4
## 730  10     5     5  0.5
## 731  10     7     3  0.7
## 732  10     6     4  0.6
## 733  10     7     3  0.7
## 734  10     8     2  0.8
## 735  10     6     4  0.6
## 736  10     2     8  0.2
## 737  10     6     4  0.6
## 738  10     6     4  0.6
## 739  10     5     5  0.5
## 740  10     4     6  0.4
## 741  10     6     4  0.6
## 742  10     5     5  0.5
## 743  10     5     5  0.5
## 744  10     4     6  0.4
## 745  10     5     5  0.5
## 746  10     4     6  0.4
## 747  10     3     7  0.3
## 748  10     5     5  0.5
## 749  10     6     4  0.6
## 750  10     6     4  0.6
## 751  10     7     3  0.7
## 752  10     4     6  0.4
## 753  10     4     6  0.4
## 754  10     5     5  0.5
## 755  10     6     4  0.6
## 756  10     6     4  0.6
## 757  10     3     7  0.3
## 758  10     5     5  0.5
## 759  10     4     6  0.4
## 760  10     5     5  0.5
## 761  10     5     5  0.5
## 762  10     5     5  0.5
## 763  10     5     5  0.5
## 764  10     4     6  0.4
## 765  10     5     5  0.5
## 766  10     5     5  0.5
## 767  10     5     5  0.5
## 768  10     5     5  0.5
## 769  10     7     3  0.7
## 770  10     3     7  0.3
## 771  10     2     8  0.2
## 772  10     6     4  0.6
## 773  10     8     2  0.8
## 774  10     5     5  0.5
## 775  10     7     3  0.7
## 776  10     6     4  0.6
## 777  10     5     5  0.5
## 778  10     7     3  0.7
## 779  10     3     7  0.3
## 780  10     5     5  0.5
## 781  10     6     4  0.6
## 782  10     3     7  0.3
## 783  10     4     6  0.4
## 784  10     5     5  0.5
## 785  10     5     5  0.5
## 786  10     7     3  0.7
## 787  10     5     5  0.5
## 788  10     5     5  0.5
## 789  10     2     8  0.2
## 790  10     6     4  0.6
## 791  10     5     5  0.5
## 792  10     8     2  0.8
## 793  10     5     5  0.5
## 794  10     4     6  0.4
## 795  10     6     4  0.6
## 796  10     5     5  0.5
## 797  10     7     3  0.7
## 798  10     6     4  0.6
## 799  10     5     5  0.5
## 800  10     5     5  0.5
## 801  10     3     7  0.3
## 802  10     4     6  0.4
## 803  10     3     7  0.3
## 804  10     3     7  0.3
## 805  10     3     7  0.3
## 806  10     5     5  0.5
## 807  10     5     5  0.5
## 808  10     7     3  0.7
## 809  10     4     6  0.4
## 810  10     7     3  0.7
## 811  10     5     5  0.5
## 812  10     5     5  0.5
## 813  10     5     5  0.5
## 814  10     5     5  0.5
## 815  10     5     5  0.5
## 816  10     4     6  0.4
## 817  10     7     3  0.7
## 818  10     4     6  0.4
## 819  10     4     6  0.4
## 820  10     3     7  0.3
## 821  10     6     4  0.6
## 822  10     6     4  0.6
## 823  10     6     4  0.6
## 824  10     8     2  0.8
## 825  10     3     7  0.3
## 826  10     3     7  0.3
## 827  10     6     4  0.6
## 828  10     7     3  0.7
## 829  10     5     5  0.5
## 830  10     3     7  0.3
## 831  10     6     4  0.6
## 832  10     6     4  0.6
## 833  10     5     5  0.5
## 834  10     6     4  0.6
## 835  10     5     5  0.5
## 836  10     8     2  0.8
## 837  10     5     5  0.5
## 838  10     5     5  0.5
## 839  10     3     7  0.3
## 840  10     2     8  0.2
## 841  10     4     6  0.4
## 842  10     6     4  0.6
## 843  10     7     3  0.7
## 844  10     7     3  0.7
## 845  10     3     7  0.3
## 846  10     3     7  0.3
## 847  10     3     7  0.3
## 848  10     4     6  0.4
## 849  10     5     5  0.5
## 850  10     6     4  0.6
## 851  10     4     6  0.4
## 852  10     3     7  0.3
## 853  10     4     6  0.4
## 854  10     5     5  0.5
## 855  10     4     6  0.4
## 856  10     6     4  0.6
## 857  10     6     4  0.6
## 858  10     7     3  0.7
## 859  10     5     5  0.5
## 860  10     5     5  0.5
## 861  10     4     6  0.4
## 862  10     6     4  0.6
## 863  10     4     6  0.4
## 864  10     6     4  0.6
## 865  10     6     4  0.6
## 866  10     6     4  0.6
## 867  10     2     8  0.2
## 868  10     4     6  0.4
## 869  10     3     7  0.3
## 870  10     5     5  0.5
## 871  10     7     3  0.7
## 872  10     5     5  0.5
## 873  10     5     5  0.5
## 874  10     4     6  0.4
## 875  10     6     4  0.6
## 876  10     7     3  0.7
## 877  10     4     6  0.4
## 878  10     3     7  0.3
## 879  10     5     5  0.5
## 880  10     7     3  0.7
## 881  10     6     4  0.6
## 882  10     7     3  0.7
## 883  10     8     2  0.8
## 884  10     6     4  0.6
## 885  10     3     7  0.3
## 886  10     6     4  0.6
## 887  10     4     6  0.4
## 888  10     4     6  0.4
## 889  10     5     5  0.5
## 890  10     5     5  0.5
## 891  10     7     3  0.7
## 892  10     5     5  0.5
## 893  10     7     3  0.7
## 894  10     5     5  0.5
## 895  10     6     4  0.6
## 896  10     3     7  0.3
## 897  10     6     4  0.6
## 898  10     4     6  0.4
## 899  10     4     6  0.4
## 900  10     2     8  0.2
## 901  10     7     3  0.7
## 902  10     7     3  0.7
## 903  10     6     4  0.6
## 904  10     7     3  0.7
## 905  10     4     6  0.4
## 906  10     3     7  0.3
## 907  10     3     7  0.3
## 908  10     3     7  0.3
## 909  10     6     4  0.6
## 910  10     5     5  0.5
## 911  10     5     5  0.5
## 912  10     8     2  0.8
## 913  10     7     3  0.7
## 914  10     5     5  0.5
## 915  10     3     7  0.3
## 916  10     6     4  0.6
## 917  10     3     7  0.3
## 918  10     6     4  0.6
## 919  10     4     6  0.4
## 920  10     8     2  0.8
## 921  10     5     5  0.5
## 922  10     6     4  0.6
## 923  10     2     8  0.2
## 924  10     6     4  0.6
## 925  10     3     7  0.3
## 926  10     5     5  0.5
## 927  10     4     6  0.4
## 928  10     3     7  0.3
## 929  10     6     4  0.6
## 930  10     5     5  0.5
## 931  10     5     5  0.5
## 932  10     4     6  0.4
## 933  10     4     6  0.4
## 934  10     4     6  0.4
## 935  10     7     3  0.7
## 936  10     3     7  0.3
## 937  10     2     8  0.2
## 938  10     5     5  0.5
## 939  10     3     7  0.3
## 940  10     6     4  0.6
## 941  10     5     5  0.5
## 942  10     6     4  0.6
## 943  10     5     5  0.5
## 944  10     4     6  0.4
## 945  10     4     6  0.4
## 946  10     3     7  0.3
## 947  10     3     7  0.3
## 948  10     4     6  0.4
## 949  10     4     6  0.4
## 950  10     5     5  0.5
## 951  10     9     1  0.9
## 952  10     3     7  0.3
## 953  10     7     3  0.7
## 954  10     8     2  0.8
## 955  10     7     3  0.7
## 956  10     6     4  0.6
## 957  10     5     5  0.5
## 958  10     5     5  0.5
## 959  10     7     3  0.7
## 960  10     5     5  0.5
## 961  10     4     6  0.4
## 962  10     5     5  0.5
## 963  10     7     3  0.7
## 964  10     5     5  0.5
## 965  10     4     6  0.4
## 966  10     5     5  0.5
## 967  10     8     2  0.8
## 968  10     5     5  0.5
## 969  10     4     6  0.4
## 970  10     6     4  0.6
## 971  10     6     4  0.6
## 972  10     3     7  0.3
## 973  10     5     5  0.5
## 974  10     4     6  0.4
## 975  10     6     4  0.6
## 976  10     4     6  0.4
## 977  10     4     6  0.4
## 978  10     5     5  0.5
## 979  10     8     2  0.8
## 980  10     5     5  0.5
## 981  10     6     4  0.6
## 982  10     5     5  0.5
## 983  10     4     6  0.4
## 984  10     3     7  0.3
## 985  10     7     3  0.7
## 986  10     6     4  0.6
## 987  10     4     6  0.4
## 988  10     4     6  0.4
## 989  10     4     6  0.4
## 990  10     5     5  0.5
## 991  10     7     3  0.7
## 992  10     2     8  0.2
## 993  10     4     6  0.4
## 994  10     5     5  0.5
## 995  10     5     5  0.5
## 996  10     4     6  0.4
## 997  10     7     3  0.7
## 998  10     4     6  0.4
## 999  10     4     6  0.4
## 1000 10     2     8  0.2
## 1001 10     8     2  0.8
## 1002 10     5     5  0.5
## 1003 10     4     6  0.4
## 1004 10     6     4  0.6
## 1005 10     5     5  0.5
## 1006 10     3     7  0.3
## 1007 10     7     3  0.7
## 1008 10     5     5  0.5
## 1009 10     6     4  0.6
## 1010 10     5     5  0.5
## 1011 10     6     4  0.6
## 1012 10     7     3  0.7
## 1013 10     4     6  0.4
## 1014 10     3     7  0.3
## 1015 10     7     3  0.7
## 1016 10     5     5  0.5
## 1017 10     7     3  0.7
## 1018 10     8     2  0.8
## 1019 10     5     5  0.5
## 1020 10     6     4  0.6
## 1021 10     4     6  0.4
## 1022 10     6     4  0.6
## 1023 10     7     3  0.7
## 1024 10     5     5  0.5
## 1025 10     6     4  0.6
## 1026 10     5     5  0.5
## 1027 10     4     6  0.4
## 1028 10     5     5  0.5
## 1029 10     6     4  0.6
## 1030 10     3     7  0.3
## 1031 10     4     6  0.4
## 1032 10     5     5  0.5
## 1033 10     3     7  0.3
## 1034 10     6     4  0.6
## 1035 10     5     5  0.5
## 1036 10     5     5  0.5
## 1037 10     4     6  0.4
## 1038 10     5     5  0.5
## 1039 10     4     6  0.4
## 1040 10     7     3  0.7
## 1041 10     5     5  0.5
## 1042 10     6     4  0.6
## 1043 10     4     6  0.4
## 1044 10     9     1  0.9
## 1045 10     4     6  0.4
## 1046 10     6     4  0.6
## 1047 10     6     4  0.6
## 1048 10     5     5  0.5
## 1049 10     3     7  0.3
## 1050 10     8     2  0.8
## 1051 10     4     6  0.4
## 1052 10     6     4  0.6
## 1053 10     6     4  0.6
## 1054 10     7     3  0.7
## 1055 10     5     5  0.5
## 1056 10     5     5  0.5
## 1057 10     6     4  0.6
## 1058 10     5     5  0.5
## 1059 10     7     3  0.7
## 1060 10     7     3  0.7
## 1061 10     3     7  0.3
## 1062 10     4     6  0.4
## 1063 10     8     2  0.8
## 1064 10     5     5  0.5
## 1065 10     7     3  0.7
## 1066 10     6     4  0.6
## 1067 10     6     4  0.6
## 1068 10     4     6  0.4
## 1069 10     6     4  0.6
## 1070 10     5     5  0.5
## 1071 10     6     4  0.6
## 1072 10     6     4  0.6
## 1073 10     4     6  0.4
## 1074 10     5     5  0.5
## 1075 10     4     6  0.4
## 1076 10     4     6  0.4
## 1077 10     5     5  0.5
## 1078 10     6     4  0.6
## 1079 10     6     4  0.6
## 1080 10     4     6  0.4
## 1081 10     7     3  0.7
## 1082 10     3     7  0.3
## 1083 10     3     7  0.3
## 1084 10     3     7  0.3
## 1085 10     2     8  0.2
## 1086 10     4     6  0.4
## 1087 10     4     6  0.4
## 1088 10     4     6  0.4
## 1089 10     9     1  0.9
## 1090 10     7     3  0.7
## 1091 10     8     2  0.8
## 1092 10     6     4  0.6
## 1093 10     4     6  0.4
## 1094 10     4     6  0.4
## 1095 10     5     5  0.5
## 1096 10     4     6  0.4
## 1097 10     7     3  0.7
## 1098 10     5     5  0.5
## 1099 10     8     2  0.8
## 1100 10     3     7  0.3
## 1101 10     3     7  0.3
## 1102 10     6     4  0.6
## 1103 10     7     3  0.7
## 1104 10     6     4  0.6
## 1105 10     5     5  0.5
## 1106 10     5     5  0.5
## 1107 10     6     4  0.6
## 1108 10     8     2  0.8
## 1109 10     5     5  0.5
## 1110 10     7     3  0.7
## 1111 10     7     3  0.7
## 1112 10     5     5  0.5
## 1113 10     3     7  0.3
## 1114 10     5     5  0.5
## 1115 10     4     6  0.4
## 1116 10     3     7  0.3
## 1117 10     5     5  0.5
## 1118 10     4     6  0.4
## 1119 10     4     6  0.4
## 1120 10     2     8  0.2
## 1121 10     7     3  0.7
## 1122 10     5     5  0.5
## 1123 10     8     2  0.8
## 1124 10     6     4  0.6
## 1125 10     5     5  0.5
## 1126 10     6     4  0.6
## 1127 10     5     5  0.5
## 1128 10     4     6  0.4
## 1129 10     5     5  0.5
## 1130 10     7     3  0.7
## 1131 10     5     5  0.5
## 1132 10     4     6  0.4
## 1133 10     4     6  0.4
## 1134 10     6     4  0.6
## 1135 10     5     5  0.5
## 1136 10     6     4  0.6
## 1137 10     5     5  0.5
## 1138 10     4     6  0.4
## 1139 10     3     7  0.3
## 1140 10     6     4  0.6
## 1141 10     6     4  0.6
## 1142 10     4     6  0.4
## 1143 10     4     6  0.4
## 1144 10     2     8  0.2
## 1145 10     2     8  0.2
## 1146 10     8     2  0.8
## 1147 10     5     5  0.5
## 1148 10     4     6  0.4
## 1149 10     4     6  0.4
## 1150 10     5     5  0.5
## 1151 10     5     5  0.5
## 1152 10     5     5  0.5
## 1153 10     6     4  0.6
## 1154 10     6     4  0.6
## 1155 10     7     3  0.7
## 1156 10     4     6  0.4
## 1157 10     3     7  0.3
## 1158 10     7     3  0.7
## 1159 10     4     6  0.4
## 1160 10     5     5  0.5
## 1161 10     5     5  0.5
## 1162 10     5     5  0.5
## 1163 10     7     3  0.7
## 1164 10     6     4  0.6
## 1165 10     5     5  0.5
## 1166 10     4     6  0.4
## 1167 10     7     3  0.7
## 1168 10     6     4  0.6
## 1169 10     7     3  0.7
## 1170 10     5     5  0.5
## 1171 10     6     4  0.6
## 1172 10     6     4  0.6
## 1173 10     7     3  0.7
## 1174 10     4     6  0.4
## 1175 10     7     3  0.7
## 1176 10     7     3  0.7
## 1177 10     3     7  0.3
## 1178 10     6     4  0.6
## 1179 10     5     5  0.5
## 1180 10     5     5  0.5
## 1181 10     5     5  0.5
## 1182 10     6     4  0.6
## 1183 10     2     8  0.2
## 1184 10     5     5  0.5
## 1185 10     2     8  0.2
## 1186 10     6     4  0.6
## 1187 10     6     4  0.6
## 1188 10     3     7  0.3
## 1189 10     4     6  0.4
## 1190 10     4     6  0.4
## 1191 10     4     6  0.4
## 1192 10     6     4  0.6
## 1193 10     7     3  0.7
## 1194 10     3     7  0.3
## 1195 10     3     7  0.3
## 1196 10     3     7  0.3
## 1197 10     4     6  0.4
## 1198 10     3     7  0.3
## 1199 10     1     9  0.1
## 1200 10     6     4  0.6
## 1201 10     7     3  0.7
## 1202 10     2     8  0.2
## 1203 10     4     6  0.4
## 1204 10     5     5  0.5
## 1205 10     6     4  0.6
## 1206 10     4     6  0.4
## 1207 10     4     6  0.4
## 1208 10     5     5  0.5
## 1209 10     6     4  0.6
## 1210 10     3     7  0.3
## 1211 10     2     8  0.2
## 1212 10     3     7  0.3
## 1213 10     3     7  0.3
## 1214 10     4     6  0.4
## 1215 10     5     5  0.5
## 1216 10     5     5  0.5
## 1217 10     6     4  0.6
## 1218 10     6     4  0.6
## 1219 10     4     6  0.4
## 1220 10     3     7  0.3
## 1221 10     5     5  0.5
## 1222 10     5     5  0.5
## 1223 10     4     6  0.4
## 1224 10     7     3  0.7
## 1225 10     5     5  0.5
## 1226 10     4     6  0.4
## 1227 10     5     5  0.5
## 1228 10     5     5  0.5
## 1229 10     3     7  0.3
## 1230 10     6     4  0.6
## 1231 10     5     5  0.5
## 1232 10     5     5  0.5
## 1233 10     5     5  0.5
## 1234 10     6     4  0.6
## 1235 10     4     6  0.4
## 1236 10     5     5  0.5
## 1237 10     4     6  0.4
## 1238 10     6     4  0.6
## 1239 10     6     4  0.6
## 1240 10     7     3  0.7
## 1241 10     8     2  0.8
## 1242 10     6     4  0.6
## 1243 10     6     4  0.6
## 1244 10     5     5  0.5
## 1245 10     4     6  0.4
## 1246 10     6     4  0.6
## 1247 10     4     6  0.4
## 1248 10     8     2  0.8
## 1249 10     2     8  0.2
## 1250 10     5     5  0.5
## 1251 10     4     6  0.4
## 1252 10     6     4  0.6
## 1253 10     6     4  0.6
## 1254 10     4     6  0.4
## 1255 10     2     8  0.2
## 1256 10     7     3  0.7
## 1257 10     5     5  0.5
## 1258 10     7     3  0.7
## 1259 10     5     5  0.5
## 1260 10     6     4  0.6
## 1261 10     6     4  0.6
## 1262 10     5     5  0.5
## 1263 10     6     4  0.6
## 1264 10     4     6  0.4
## 1265 10     7     3  0.7
## 1266 10     4     6  0.4
## 1267 10     3     7  0.3
## 1268 10     4     6  0.4
## 1269 10     5     5  0.5
## 1270 10     3     7  0.3
## 1271 10     5     5  0.5
## 1272 10     4     6  0.4
## 1273 10     7     3  0.7
## 1274 10     5     5  0.5
## 1275 10     4     6  0.4
## 1276 10     8     2  0.8
## 1277 10     5     5  0.5
## 1278 10     4     6  0.4
## 1279 10     3     7  0.3
## 1280 10     4     6  0.4
## 1281 10     5     5  0.5
## 1282 10     5     5  0.5
## 1283 10     4     6  0.4
## 1284 10     7     3  0.7
## 1285 10     4     6  0.4
## 1286 10     3     7  0.3
## 1287 10     4     6  0.4
## 1288 10     4     6  0.4
## 1289 10     5     5  0.5
## 1290 10     3     7  0.3
## 1291 10     7     3  0.7
## 1292 10     6     4  0.6
## 1293 10     5     5  0.5
## 1294 10     5     5  0.5
## 1295 10     7     3  0.7
## 1296 10     2     8  0.2
## 1297 10     4     6  0.4
## 1298 10     2     8  0.2
## 1299 10     4     6  0.4
## 1300 10     6     4  0.6
## 1301 10     4     6  0.4
## 1302 10     6     4  0.6
## 1303 10     5     5  0.5
## 1304 10     9     1  0.9
## 1305 10     5     5  0.5
## 1306 10     5     5  0.5
## 1307 10     5     5  0.5
## 1308 10     5     5  0.5
## 1309 10     6     4  0.6
## 1310 10     1     9  0.1
## 1311 10     6     4  0.6
## 1312 10     2     8  0.2
## 1313 10     6     4  0.6
## 1314 10     6     4  0.6
## 1315 10     7     3  0.7
## 1316 10     9     1  0.9
## 1317 10     5     5  0.5
## 1318 10     4     6  0.4
## 1319 10     6     4  0.6
## 1320 10     3     7  0.3
## 1321 10     4     6  0.4
## 1322 10     3     7  0.3
## 1323 10     6     4  0.6
## 1324 10     6     4  0.6
## 1325 10     6     4  0.6
## 1326 10     4     6  0.4
## 1327 10     6     4  0.6
## 1328 10     6     4  0.6
## 1329 10     5     5  0.5
## 1330 10     5     5  0.5
## 1331 10     3     7  0.3
## 1332 10     6     4  0.6
## 1333 10     2     8  0.2
## 1334 10     4     6  0.4
## 1335 10     8     2  0.8
## 1336 10     3     7  0.3
## 1337 10     4     6  0.4
## 1338 10     5     5  0.5
## 1339 10     4     6  0.4
## 1340 10     7     3  0.7
## 1341 10     3     7  0.3
## 1342 10     3     7  0.3
## 1343 10     7     3  0.7
## 1344 10     7     3  0.7
## 1345 10     4     6  0.4
## 1346 10     3     7  0.3
## 1347 10     7     3  0.7
## 1348 10     3     7  0.3
## 1349 10     4     6  0.4
## 1350 10     4     6  0.4
## 1351 10     7     3  0.7
## 1352 10     5     5  0.5
## 1353 10     6     4  0.6
## 1354 10     8     2  0.8
## 1355 10     3     7  0.3
## 1356 10     7     3  0.7
## 1357 10     4     6  0.4
## 1358 10     4     6  0.4
## 1359 10     4     6  0.4
## 1360 10     3     7  0.3
## 1361 10     4     6  0.4
## 1362 10     7     3  0.7
## 1363 10     7     3  0.7
## 1364 10     9     1  0.9
## 1365 10     5     5  0.5
## 1366 10     8     2  0.8
## 1367 10     5     5  0.5
## 1368 10     7     3  0.7
## 1369 10     3     7  0.3
## 1370 10     8     2  0.8
## 1371 10     9     1  0.9
## 1372 10     5     5  0.5
## 1373 10     6     4  0.6
## 1374 10     6     4  0.6
## 1375 10     8     2  0.8
## 1376 10     6     4  0.6
## 1377 10     3     7  0.3
## 1378 10     3     7  0.3
## 1379 10     5     5  0.5
## 1380 10     6     4  0.6
## 1381 10     4     6  0.4
## 1382 10     7     3  0.7
## 1383 10     8     2  0.8
## 1384 10     7     3  0.7
## 1385 10     5     5  0.5
## 1386 10     5     5  0.5
## 1387 10     6     4  0.6
## 1388 10     4     6  0.4
## 1389 10     6     4  0.6
## 1390 10     6     4  0.6
## 1391 10     6     4  0.6
## 1392 10     3     7  0.3
## 1393 10     5     5  0.5
## 1394 10     4     6  0.4
## 1395 10     2     8  0.2
## 1396 10     5     5  0.5
## 1397 10     4     6  0.4
## 1398 10     6     4  0.6
## 1399 10     3     7  0.3
## 1400 10     6     4  0.6
## 1401 10     6     4  0.6
## 1402 10     3     7  0.3
## 1403 10     4     6  0.4
## 1404 10     6     4  0.6
## 1405 10     5     5  0.5
## 1406 10     6     4  0.6
## 1407 10     6     4  0.6
## 1408 10     4     6  0.4
## 1409 10     4     6  0.4
## 1410 10     6     4  0.6
## 1411 10     4     6  0.4
## 1412 10     7     3  0.7
## 1413 10     5     5  0.5
## 1414 10     6     4  0.6
## 1415 10     5     5  0.5
## 1416 10     4     6  0.4
## 1417 10     7     3  0.7
## 1418 10     7     3  0.7
## 1419 10     6     4  0.6
## 1420 10     3     7  0.3
## 1421 10     6     4  0.6
## 1422 10     3     7  0.3
## 1423 10     6     4  0.6
## 1424 10     8     2  0.8
## 1425 10     5     5  0.5
## 1426 10     6     4  0.6
## 1427 10     3     7  0.3
## 1428 10     8     2  0.8
## 1429 10     5     5  0.5
## 1430 10     4     6  0.4
## 1431 10     6     4  0.6
## 1432 10     6     4  0.6
## 1433 10     6     4  0.6
## 1434 10     3     7  0.3
## 1435 10     7     3  0.7
## 1436 10     5     5  0.5
## 1437 10     5     5  0.5
## 1438 10     3     7  0.3
## 1439 10     6     4  0.6
## 1440 10     4     6  0.4
## 1441 10     5     5  0.5
## 1442 10     7     3  0.7
## 1443 10     4     6  0.4
## 1444 10     6     4  0.6
## 1445 10     4     6  0.4
## 1446 10     7     3  0.7
## 1447 10     6     4  0.6
## 1448 10     3     7  0.3
## 1449 10     4     6  0.4
## 1450 10     6     4  0.6
## 1451 10     5     5  0.5
## 1452 10     5     5  0.5
## 1453 10     8     2  0.8
## 1454 10     6     4  0.6
## 1455 10     5     5  0.5
## 1456 10     4     6  0.4
## 1457 10     7     3  0.7
## 1458 10     7     3  0.7
## 1459 10     5     5  0.5
## 1460 10     4     6  0.4
## 1461 10     5     5  0.5
## 1462 10     7     3  0.7
## 1463 10     3     7  0.3
## 1464 10     6     4  0.6
## 1465 10     5     5  0.5
## 1466 10     5     5  0.5
## 1467 10     4     6  0.4
## 1468 10     2     8  0.2
## 1469 10     4     6  0.4
## 1470 10     6     4  0.6
## 1471 10     6     4  0.6
## 1472 10     7     3  0.7
## 1473 10     5     5  0.5
## 1474 10     6     4  0.6
## 1475 10     3     7  0.3
## 1476 10     6     4  0.6
## 1477 10     7     3  0.7
## 1478 10     6     4  0.6
## 1479 10     5     5  0.5
## 1480 10     9     1  0.9
## 1481 10     7     3  0.7
## 1482 10     6     4  0.6
## 1483 10     6     4  0.6
## 1484 10     5     5  0.5
## 1485 10     3     7  0.3
## 1486 10     4     6  0.4
## 1487 10     6     4  0.6
## 1488 10     6     4  0.6
## 1489 10     3     7  0.3
## 1490 10     6     4  0.6
## 1491 10     5     5  0.5
## 1492 10     6     4  0.6
## 1493 10     4     6  0.4
## 1494 10     5     5  0.5
## 1495 10     3     7  0.3
## 1496 10     7     3  0.7
## 1497 10     5     5  0.5
## 1498 10     6     4  0.6
## 1499 10     5     5  0.5
## 1500 10     0    10  0.0
## 1501 10     4     6  0.4
## 1502 10     3     7  0.3
## 1503 10     6     4  0.6
## 1504 10     4     6  0.4
## 1505 10     5     5  0.5
## 1506 10     6     4  0.6
## 1507 10     3     7  0.3
## 1508 10     4     6  0.4
## 1509 10     4     6  0.4
## 1510 10     6     4  0.6
## 1511 10     5     5  0.5
## 1512 10     4     6  0.4
## 1513 10     4     6  0.4
## 1514 10     3     7  0.3
## 1515 10     2     8  0.2
## 1516 10     1     9  0.1
## 1517 10     3     7  0.3
## 1518 10     8     2  0.8
## 1519 10     4     6  0.4
## 1520 10     6     4  0.6
## 1521 10     7     3  0.7
## 1522 10     5     5  0.5
## 1523 10     2     8  0.2
## 1524 10     4     6  0.4
## 1525 10     5     5  0.5
## 1526 10     6     4  0.6
## 1527 10     5     5  0.5
## 1528 10     6     4  0.6
## 1529 10     6     4  0.6
## 1530 10     7     3  0.7
## 1531 10     7     3  0.7
## 1532 10     3     7  0.3
## 1533 10     7     3  0.7
## 1534 10     5     5  0.5
## 1535 10     3     7  0.3
## 1536 10     5     5  0.5
## 1537 10     3     7  0.3
## 1538 10     2     8  0.2
## 1539 10     4     6  0.4
## 1540 10     3     7  0.3
## 1541 10     4     6  0.4
## 1542 10     3     7  0.3
## 1543 10     6     4  0.6
## 1544 10     3     7  0.3
## 1545 10     5     5  0.5
## 1546 10     8     2  0.8
## 1547 10     6     4  0.6
## 1548 10     5     5  0.5
## 1549 10     5     5  0.5
## 1550 10     3     7  0.3
## 1551 10     6     4  0.6
## 1552 10     6     4  0.6
## 1553 10     2     8  0.2
## 1554 10     5     5  0.5
## 1555 10     5     5  0.5
## 1556 10     2     8  0.2
## 1557 10     7     3  0.7
## 1558 10     6     4  0.6
## 1559 10     4     6  0.4
## 1560 10     7     3  0.7
## 1561 10     7     3  0.7
## 1562 10     4     6  0.4
## 1563 10     4     6  0.4
## 1564 10     6     4  0.6
## 1565 10     4     6  0.4
## 1566 10     6     4  0.6
## 1567 10     4     6  0.4
## 1568 10     6     4  0.6
## 1569 10     6     4  0.6
## 1570 10     5     5  0.5
## 1571 10     6     4  0.6
## 1572 10     6     4  0.6
## 1573 10     4     6  0.4
## 1574 10     4     6  0.4
## 1575 10     6     4  0.6
## 1576 10     9     1  0.9
## 1577 10     4     6  0.4
## 1578 10     6     4  0.6
## 1579 10     4     6  0.4
## 1580 10     4     6  0.4
## 1581 10     5     5  0.5
## 1582 10     2     8  0.2
## 1583 10     6     4  0.6
## 1584 10     4     6  0.4
## 1585 10     8     2  0.8
## 1586 10     8     2  0.8
## 1587 10     4     6  0.4
## 1588 10     3     7  0.3
## 1589 10     6     4  0.6
## 1590 10     4     6  0.4
## 1591 10     4     6  0.4
## 1592 10     6     4  0.6
## 1593 10     4     6  0.4
## 1594 10     3     7  0.3
## 1595 10     4     6  0.4
## 1596 10     7     3  0.7
## 1597 10     5     5  0.5
## 1598 10     4     6  0.4
## 1599 10     8     2  0.8
## 1600 10     6     4  0.6
## 1601 10     7     3  0.7
## 1602 10     5     5  0.5
## 1603 10     5     5  0.5
## 1604 10     3     7  0.3
## 1605 10     5     5  0.5
## 1606 10     5     5  0.5
## 1607 10     4     6  0.4
## 1608 10     7     3  0.7
## 1609 10     4     6  0.4
## 1610 10     5     5  0.5
## 1611 10     6     4  0.6
## 1612 10     4     6  0.4
## 1613 10     6     4  0.6
## 1614 10     3     7  0.3
## 1615 10     7     3  0.7
## 1616 10     6     4  0.6
## 1617 10     5     5  0.5
## 1618 10     3     7  0.3
## 1619 10     6     4  0.6
## 1620 10     9     1  0.9
## 1621 10     6     4  0.6
## 1622 10     7     3  0.7
## 1623 10     8     2  0.8
## 1624 10     5     5  0.5
## 1625 10     4     6  0.4
## 1626 10     3     7  0.3
## 1627 10     3     7  0.3
## 1628 10     4     6  0.4
## 1629 10     8     2  0.8
## 1630 10     6     4  0.6
## 1631 10     5     5  0.5
## 1632 10     5     5  0.5
## 1633 10     5     5  0.5
## 1634 10     5     5  0.5
## 1635 10     4     6  0.4
## 1636 10     8     2  0.8
## 1637 10     6     4  0.6
## 1638 10     4     6  0.4
## 1639 10     6     4  0.6
## 1640 10     7     3  0.7
## 1641 10     4     6  0.4
## 1642 10     7     3  0.7
## 1643 10     5     5  0.5
## 1644 10     6     4  0.6
## 1645 10     3     7  0.3
## 1646 10     6     4  0.6
## 1647 10     4     6  0.4
## 1648 10     3     7  0.3
## 1649 10     4     6  0.4
## 1650 10     4     6  0.4
## 1651 10     6     4  0.6
## 1652 10     3     7  0.3
## 1653 10     6     4  0.6
## 1654 10     8     2  0.8
## 1655 10     4     6  0.4
## 1656 10     4     6  0.4
## 1657 10     5     5  0.5
## 1658 10     6     4  0.6
## 1659 10     3     7  0.3
## 1660 10     5     5  0.5
## 1661 10     5     5  0.5
## 1662 10     5     5  0.5
## 1663 10     3     7  0.3
## 1664 10     8     2  0.8
## 1665 10     5     5  0.5
## 1666 10     6     4  0.6
## 1667 10     5     5  0.5
## 1668 10     4     6  0.4
## 1669 10     7     3  0.7
## 1670 10     4     6  0.4
## 1671 10     5     5  0.5
## 1672 10     3     7  0.3
## 1673 10     3     7  0.3
## 1674 10     3     7  0.3
## 1675 10     6     4  0.6
## 1676 10     3     7  0.3
## 1677 10     6     4  0.6
## 1678 10     4     6  0.4
## 1679 10     8     2  0.8
## 1680 10     4     6  0.4
## 1681 10     6     4  0.6
## 1682 10     4     6  0.4
## 1683 10     6     4  0.6
## 1684 10     6     4  0.6
## 1685 10     4     6  0.4
## 1686 10     6     4  0.6
## 1687 10     7     3  0.7
## 1688 10     6     4  0.6
## 1689 10     5     5  0.5
## 1690 10     5     5  0.5
## 1691 10     6     4  0.6
## 1692 10     6     4  0.6
## 1693 10     7     3  0.7
## 1694 10     5     5  0.5
## 1695 10     6     4  0.6
## 1696 10     5     5  0.5
## 1697 10     5     5  0.5
## 1698 10     5     5  0.5
## 1699 10     3     7  0.3
## 1700 10     7     3  0.7
## 1701 10     6     4  0.6
## 1702 10     5     5  0.5
## 1703 10     4     6  0.4
## 1704 10     5     5  0.5
## 1705 10     8     2  0.8
## 1706 10     3     7  0.3
## 1707 10     7     3  0.7
## 1708 10     5     5  0.5
## 1709 10     4     6  0.4
## 1710 10     4     6  0.4
## 1711 10     6     4  0.6
## 1712 10     6     4  0.6
## 1713 10     6     4  0.6
## 1714 10     6     4  0.6
## 1715 10     5     5  0.5
## 1716 10     7     3  0.7
## 1717 10     3     7  0.3
## 1718 10     7     3  0.7
## 1719 10     4     6  0.4
## 1720 10     6     4  0.6
## 1721 10     5     5  0.5
## 1722 10     1     9  0.1
## 1723 10     6     4  0.6
## 1724 10     1     9  0.1
## 1725 10     5     5  0.5
## 1726 10     4     6  0.4
## 1727 10     5     5  0.5
## 1728 10     4     6  0.4
## 1729 10     5     5  0.5
## 1730 10     6     4  0.6
## 1731 10     6     4  0.6
## 1732 10     5     5  0.5
## 1733 10     5     5  0.5
## 1734 10     4     6  0.4
## 1735 10     5     5  0.5
## 1736 10     5     5  0.5
## 1737 10     3     7  0.3
## 1738 10     5     5  0.5
## 1739 10     5     5  0.5
## 1740 10     7     3  0.7
## 1741 10     4     6  0.4
## 1742 10     4     6  0.4
## 1743 10     5     5  0.5
## 1744 10     4     6  0.4
## 1745 10     2     8  0.2
## 1746 10     8     2  0.8
## 1747 10     5     5  0.5
## 1748 10     4     6  0.4
## 1749 10     6     4  0.6
## 1750 10     6     4  0.6
## 1751 10     7     3  0.7
## 1752 10     5     5  0.5
## 1753 10     4     6  0.4
## 1754 10     4     6  0.4
## 1755 10     5     5  0.5
## 1756 10     2     8  0.2
## 1757 10     7     3  0.7
## 1758 10     2     8  0.2
## 1759 10     4     6  0.4
## 1760 10     5     5  0.5
## 1761 10     6     4  0.6
## 1762 10     5     5  0.5
## 1763 10     3     7  0.3
## 1764 10     5     5  0.5
## 1765 10     8     2  0.8
## 1766 10     5     5  0.5
## 1767 10     6     4  0.6
## 1768 10     4     6  0.4
## 1769 10     7     3  0.7
## 1770 10     6     4  0.6
## 1771 10     5     5  0.5
## 1772 10     4     6  0.4
## 1773 10     5     5  0.5
## 1774 10     6     4  0.6
## 1775 10     6     4  0.6
## 1776 10     3     7  0.3
## 1777 10     3     7  0.3
## 1778 10     4     6  0.4
## 1779 10     3     7  0.3
## 1780 10     5     5  0.5
## 1781 10     6     4  0.6
## 1782 10     5     5  0.5
## 1783 10     5     5  0.5
## 1784 10     4     6  0.4
## 1785 10     3     7  0.3
## 1786 10     6     4  0.6
## 1787 10     5     5  0.5
## 1788 10     7     3  0.7
## 1789 10     2     8  0.2
## 1790 10     4     6  0.4
## 1791 10     5     5  0.5
## 1792 10     5     5  0.5
## 1793 10     5     5  0.5
## 1794 10     6     4  0.6
## 1795 10     7     3  0.7
## 1796 10     5     5  0.5
## 1797 10     6     4  0.6
## 1798 10     4     6  0.4
## 1799 10     5     5  0.5
## 1800 10     6     4  0.6
## 1801 10     6     4  0.6
## 1802 10     6     4  0.6
## 1803 10     2     8  0.2
## 1804 10     4     6  0.4
## 1805 10     5     5  0.5
## 1806 10     5     5  0.5
## 1807 10     7     3  0.7
## 1808 10     2     8  0.2
## 1809 10     5     5  0.5
## 1810 10     6     4  0.6
## 1811 10     5     5  0.5
## 1812 10     4     6  0.4
## 1813 10     5     5  0.5
## 1814 10     4     6  0.4
## 1815 10     4     6  0.4
## 1816 10     7     3  0.7
## 1817 10     7     3  0.7
## 1818 10     8     2  0.8
## 1819 10     3     7  0.3
## 1820 10     5     5  0.5
## 1821 10     4     6  0.4
## 1822 10     6     4  0.6
## 1823 10     6     4  0.6
## 1824 10     6     4  0.6
## 1825 10     5     5  0.5
## 1826 10     5     5  0.5
## 1827 10     5     5  0.5
## 1828 10     5     5  0.5
## 1829 10     7     3  0.7
## 1830 10     4     6  0.4
## 1831 10     4     6  0.4
## 1832 10     6     4  0.6
## 1833 10     4     6  0.4
## 1834 10     3     7  0.3
## 1835 10     5     5  0.5
## 1836 10     7     3  0.7
## 1837 10     6     4  0.6
## 1838 10     7     3  0.7
## 1839 10     4     6  0.4
## 1840 10     6     4  0.6
## 1841 10     6     4  0.6
## 1842 10     8     2  0.8
## 1843 10     4     6  0.4
## 1844 10     6     4  0.6
## 1845 10     3     7  0.3
## 1846 10     2     8  0.2
## 1847 10     4     6  0.4
## 1848 10     5     5  0.5
## 1849 10     3     7  0.3
## 1850 10     6     4  0.6
## 1851 10     5     5  0.5
## 1852 10     9     1  0.9
## 1853 10     1     9  0.1
## 1854 10     6     4  0.6
## 1855 10     7     3  0.7
## 1856 10     5     5  0.5
## 1857 10     9     1  0.9
## 1858 10     8     2  0.8
## 1859 10     6     4  0.6
## 1860 10     5     5  0.5
## 1861 10     4     6  0.4
## 1862 10     5     5  0.5
## 1863 10     4     6  0.4
## 1864 10     8     2  0.8
## 1865 10     4     6  0.4
## 1866 10     6     4  0.6
## 1867 10     3     7  0.3
## 1868 10     7     3  0.7
## 1869 10     5     5  0.5
## 1870 10     7     3  0.7
## 1871 10     7     3  0.7
## 1872 10     9     1  0.9
## 1873 10     4     6  0.4
## 1874 10     7     3  0.7
## 1875 10     6     4  0.6
## 1876 10     7     3  0.7
## 1877 10     7     3  0.7
## 1878 10     5     5  0.5
## 1879 10     6     4  0.6
## 1880 10     6     4  0.6
## 1881 10     4     6  0.4
## 1882 10     5     5  0.5
## 1883 10     5     5  0.5
## 1884 10     4     6  0.4
## 1885 10     5     5  0.5
## 1886 10     6     4  0.6
## 1887 10     5     5  0.5
## 1888 10     3     7  0.3
## 1889 10     6     4  0.6
## 1890 10     2     8  0.2
## 1891 10     4     6  0.4
## 1892 10     6     4  0.6
## 1893 10     4     6  0.4
## 1894 10     6     4  0.6
## 1895 10     4     6  0.4
## 1896 10     4     6  0.4
## 1897 10     4     6  0.4
## 1898 10     6     4  0.6
## 1899 10     5     5  0.5
## 1900 10     7     3  0.7
## 1901 10     4     6  0.4
## 1902 10     3     7  0.3
## 1903 10     6     4  0.6
## 1904 10     6     4  0.6
## 1905 10     2     8  0.2
## 1906 10     5     5  0.5
## 1907 10     3     7  0.3
## 1908 10     4     6  0.4
## 1909 10     5     5  0.5
## 1910 10     4     6  0.4
## 1911 10     5     5  0.5
## 1912 10     6     4  0.6
## 1913 10     8     2  0.8
## 1914 10     7     3  0.7
## 1915 10     3     7  0.3
## 1916 10     4     6  0.4
## 1917 10     4     6  0.4
## 1918 10     4     6  0.4
## 1919 10     4     6  0.4
## 1920 10     4     6  0.4
## 1921 10     4     6  0.4
## 1922 10     3     7  0.3
## 1923 10     5     5  0.5
## 1924 10     4     6  0.4
## 1925 10     8     2  0.8
## 1926 10     5     5  0.5
## 1927 10     5     5  0.5
## 1928 10     3     7  0.3
## 1929 10     6     4  0.6
## 1930 10     7     3  0.7
## 1931 10     4     6  0.4
## 1932 10     5     5  0.5
## 1933 10     4     6  0.4
## 1934 10     3     7  0.3
## 1935 10     6     4  0.6
## 1936 10     7     3  0.7
## 1937 10     5     5  0.5
## 1938 10     5     5  0.5
## 1939 10     5     5  0.5
## 1940 10     5     5  0.5
## 1941 10     3     7  0.3
## 1942 10     4     6  0.4
## 1943 10     3     7  0.3
## 1944 10     7     3  0.7
## 1945 10     4     6  0.4
## 1946 10     3     7  0.3
## 1947 10     4     6  0.4
## 1948 10     5     5  0.5
## 1949 10     6     4  0.6
## 1950 10     6     4  0.6
## 1951 10     4     6  0.4
## 1952 10     9     1  0.9
## 1953 10     5     5  0.5
## 1954 10     5     5  0.5
## 1955 10     5     5  0.5
## 1956 10     4     6  0.4
## 1957 10     3     7  0.3
## 1958 10     7     3  0.7
## 1959 10     6     4  0.6
## 1960 10     3     7  0.3
## 1961 10     4     6  0.4
## 1962 10     7     3  0.7
## 1963 10     7     3  0.7
## 1964 10     6     4  0.6
## 1965 10     6     4  0.6
## 1966 10     4     6  0.4
## 1967 10     7     3  0.7
## 1968 10     6     4  0.6
## 1969 10     5     5  0.5
## 1970 10     4     6  0.4
## 1971 10     4     6  0.4
## 1972 10     1     9  0.1
## 1973 10     7     3  0.7
## 1974 10     3     7  0.3
## 1975 10     4     6  0.4
## 1976 10     5     5  0.5
## 1977 10     4     6  0.4
## 1978 10     4     6  0.4
## 1979 10     3     7  0.3
## 1980 10     3     7  0.3
## 1981 10     4     6  0.4
## 1982 10     4     6  0.4
## 1983 10     5     5  0.5
## 1984 10     4     6  0.4
## 1985 10     2     8  0.2
## 1986 10     4     6  0.4
## 1987 10     4     6  0.4
## 1988 10     4     6  0.4
## 1989 10     5     5  0.5
## 1990 10     7     3  0.7
## 1991 10     3     7  0.3
## 1992 10     4     6  0.4
## 1993 10     6     4  0.6
## 1994 10     4     6  0.4
## 1995 10     7     3  0.7
## 1996 10     4     6  0.4
## 1997 10     6     4  0.6
## 1998 10     6     4  0.6
## 1999 10     3     7  0.3
## 2000 10     8     2  0.8
```

This is the same idea as before, but now there are 2000 rows in the data frame instead of 20.


```r
mean(coin_flips_2000_10$heads)
```

```
## [1] 5.0245
```


```r
ggplot(coin_flips_2000_10, aes(x = heads)) +
    geom_histogram(binwidth = 0.5) +
    scale_x_continuous(limits = c(-1, 11), breaks = seq(0, 10, 1))
```

```
## Warning: Removed 2 rows containing missing values (geom_bar).
```

<img src="08-intro_to_randomization_1-web_files/figure-html/unnamed-chunk-15-1.png" width="672" />

This is helpful. In contrast with the set of simulations with twenty people, the last histogram gives us something closer to what we expect. The mode is at five heads, and every possible number of heads is represented, with decreasing counts as one moves away from five. With 2000 people flipping coins, all possible outcomes---including rare ones---are better represented.

Here is the the same histogram, but this time with the proportion of heads instead of the count of heads:


```r
ggplot(coin_flips_2000_10, aes(x = prop)) +
    geom_histogram(binwidth = 0.05) +
    scale_x_continuous(limits = c(-0.1, 1.1), breaks = seq(0, 1, 0.1))
```

```
## Warning: Removed 2 rows containing missing values (geom_bar).
```

<img src="08-intro_to_randomization_1-web_files/figure-html/unnamed-chunk-16-1.png" width="672" />

##### Exercise 3 {-}

Do you think the shape of the distribution would be appreciably different if we used 20,000 or even 200,000 people? Why or why not? (Normally, I would encourage you to test your theory by trying it in R. However, it takes a *long* time to simulate that many flips and I don't want you to tie up resources and memory. Think through this in your head.)

::: {.answer}

Please write up your answer here.

:::

*****

From now on, we will insist on using at least a thousand simulations---if not more---to make sure that we represent the full range of possible outcomes.^[There is some theory behind choosing the number of times we need to simulate, but we're not going to get into all that.]


## More flips {#randomization1-more}

Now let's increase the number of coin flips each person performs. We'll still use 2000 simulations (imagine 2000 people all flipping coins), but this time, each person will flip the coin 1000 times instead of only 10 times. The first code chunk below accounts for a substantial amount of the time it takes to run the code in this document.


```r
set.seed(1234)
coin_flips_2000_1000 <- do(2000) * rflip(1000, prob = 0.5)
coin_flips_2000_1000
```

```
##         n heads tails  prop
## 1    1000   485   515 0.485
## 2    1000   515   485 0.515
## 3    1000   481   519 0.481
## 4    1000   508   492 0.508
## 5    1000   499   501 0.499
## 6    1000   516   484 0.516
## 7    1000   497   503 0.497
## 8    1000   497   503 0.497
## 9    1000   494   506 0.494
## 10   1000   528   472 0.528
## 11   1000   495   505 0.495
## 12   1000   483   517 0.483
## 13   1000   520   480 0.520
## 14   1000   528   472 0.528
## 15   1000   478   522 0.478
## 16   1000   516   484 0.516
## 17   1000   493   507 0.493
## 18   1000   524   476 0.524
## 19   1000   473   527 0.473
## 20   1000   516   484 0.516
## 21   1000   529   471 0.529
## 22   1000   516   484 0.516
## 23   1000   535   465 0.535
## 24   1000   491   509 0.491
## 25   1000   500   500 0.500
## 26   1000   497   503 0.497
## 27   1000   507   493 0.507
## 28   1000   515   485 0.515
## 29   1000   493   507 0.493
## 30   1000   482   518 0.482
## 31   1000   485   515 0.485
## 32   1000   493   507 0.493
## 33   1000   498   502 0.498
## 34   1000   490   510 0.490
## 35   1000   485   515 0.485
## 36   1000   495   505 0.495
## 37   1000   488   512 0.488
## 38   1000   496   504 0.496
## 39   1000   491   509 0.491
## 40   1000   488   512 0.488
## 41   1000   488   512 0.488
## 42   1000   524   476 0.524
## 43   1000   500   500 0.500
## 44   1000   516   484 0.516
## 45   1000   514   486 0.514
## 46   1000   479   521 0.479
## 47   1000   488   512 0.488
## 48   1000   469   531 0.469
## 49   1000   515   485 0.515
## 50   1000   520   480 0.520
## 51   1000   486   514 0.486
## 52   1000   507   493 0.507
## 53   1000   509   491 0.509
## 54   1000   467   533 0.467
## 55   1000   467   533 0.467
## 56   1000   504   496 0.504
## 57   1000   483   517 0.483
## 58   1000   513   487 0.513
## 59   1000   518   482 0.518
## 60   1000   493   507 0.493
## 61   1000   516   484 0.516
## 62   1000   507   493 0.507
## 63   1000   509   491 0.509
## 64   1000   508   492 0.508
## 65   1000   511   489 0.511
## 66   1000   491   509 0.491
## 67   1000   524   476 0.524
## 68   1000   515   485 0.515
## 69   1000   524   476 0.524
## 70   1000   510   490 0.510
## 71   1000   482   518 0.482
## 72   1000   498   502 0.498
## 73   1000   507   493 0.507
## 74   1000   490   510 0.490
## 75   1000   501   499 0.501
## 76   1000   502   498 0.502
## 77   1000   520   480 0.520
## 78   1000   528   472 0.528
## 79   1000   504   496 0.504
## 80   1000   501   499 0.501
## 81   1000   507   493 0.507
## 82   1000   486   514 0.486
## 83   1000   500   500 0.500
## 84   1000   505   495 0.505
## 85   1000   494   506 0.494
## 86   1000   505   495 0.505
## 87   1000   512   488 0.512
## 88   1000   521   479 0.521
## 89   1000   497   503 0.497
## 90   1000   501   499 0.501
## 91   1000   489   511 0.489
## 92   1000   497   503 0.497
## 93   1000   500   500 0.500
## 94   1000   470   530 0.470
## 95   1000   511   489 0.511
## 96   1000   504   496 0.504
## 97   1000   460   540 0.460
## 98   1000   493   507 0.493
## 99   1000   477   523 0.477
## 100  1000   489   511 0.489
## 101  1000   511   489 0.511
## 102  1000   519   481 0.519
## 103  1000   491   509 0.491
## 104  1000   464   536 0.464
## 105  1000   493   507 0.493
## 106  1000   497   503 0.497
## 107  1000   515   485 0.515
## 108  1000   491   509 0.491
## 109  1000   472   528 0.472
## 110  1000   505   495 0.505
## 111  1000   503   497 0.503
## 112  1000   489   511 0.489
## 113  1000   530   470 0.530
## 114  1000   510   490 0.510
## 115  1000   521   479 0.521
## 116  1000   488   512 0.488
## 117  1000   453   547 0.453
## 118  1000   489   511 0.489
## 119  1000   486   514 0.486
## 120  1000   481   519 0.481
## 121  1000   495   505 0.495
## 122  1000   484   516 0.484
## 123  1000   534   466 0.534
## 124  1000   500   500 0.500
## 125  1000   497   503 0.497
## 126  1000   524   476 0.524
## 127  1000   494   506 0.494
## 128  1000   505   495 0.505
## 129  1000   479   521 0.479
## 130  1000   493   507 0.493
## 131  1000   488   512 0.488
## 132  1000   482   518 0.482
## 133  1000   519   481 0.519
## 134  1000   497   503 0.497
## 135  1000   531   469 0.531
## 136  1000   481   519 0.481
## 137  1000   510   490 0.510
## 138  1000   500   500 0.500
## 139  1000   476   524 0.476
## 140  1000   493   507 0.493
## 141  1000   490   510 0.490
## 142  1000   469   531 0.469
## 143  1000   484   516 0.484
## 144  1000   534   466 0.534
## 145  1000   491   509 0.491
## 146  1000   510   490 0.510
## 147  1000   507   493 0.507
## 148  1000   495   505 0.495
## 149  1000   526   474 0.526
## 150  1000   497   503 0.497
## 151  1000   510   490 0.510
## 152  1000   496   504 0.496
## 153  1000   470   530 0.470
## 154  1000   502   498 0.502
## 155  1000   485   515 0.485
## 156  1000   516   484 0.516
## 157  1000   513   487 0.513
## 158  1000   510   490 0.510
## 159  1000   484   516 0.484
## 160  1000   517   483 0.517
## 161  1000   512   488 0.512
## 162  1000   492   508 0.492
## 163  1000   513   487 0.513
## 164  1000   478   522 0.478
## 165  1000   503   497 0.503
## 166  1000   485   515 0.485
## 167  1000   489   511 0.489
## 168  1000   477   523 0.477
## 169  1000   508   492 0.508
## 170  1000   530   470 0.530
## 171  1000   476   524 0.476
## 172  1000   510   490 0.510
## 173  1000   475   525 0.475
## 174  1000   479   521 0.479
## 175  1000   497   503 0.497
## 176  1000   505   495 0.505
## 177  1000   506   494 0.506
## 178  1000   514   486 0.514
## 179  1000   511   489 0.511
## 180  1000   536   464 0.536
## 181  1000   487   513 0.487
## 182  1000   489   511 0.489
## 183  1000   487   513 0.487
## 184  1000   503   497 0.503
## 185  1000   493   507 0.493
## 186  1000   530   470 0.530
## 187  1000   496   504 0.496
## 188  1000   495   505 0.495
## 189  1000   481   519 0.481
## 190  1000   503   497 0.503
## 191  1000   482   518 0.482
## 192  1000   504   496 0.504
## 193  1000   513   487 0.513
## 194  1000   523   477 0.523
## 195  1000   512   488 0.512
## 196  1000   512   488 0.512
## 197  1000   508   492 0.508
## 198  1000   528   472 0.528
## 199  1000   498   502 0.498
## 200  1000   529   471 0.529
## 201  1000   516   484 0.516
## 202  1000   490   510 0.490
## 203  1000   498   502 0.498
## 204  1000   499   501 0.499
## 205  1000   502   498 0.502
## 206  1000   498   502 0.498
## 207  1000   503   497 0.503
## 208  1000   521   479 0.521
## 209  1000   509   491 0.509
## 210  1000   509   491 0.509
## 211  1000   492   508 0.492
## 212  1000   496   504 0.496
## 213  1000   516   484 0.516
## 214  1000   494   506 0.494
## 215  1000   487   513 0.487
## 216  1000   509   491 0.509
## 217  1000   487   513 0.487
## 218  1000   490   510 0.490
## 219  1000   520   480 0.520
## 220  1000   495   505 0.495
## 221  1000   500   500 0.500
## 222  1000   491   509 0.491
## 223  1000   511   489 0.511
## 224  1000   475   525 0.475
## 225  1000   515   485 0.515
## 226  1000   477   523 0.477
## 227  1000   501   499 0.501
## 228  1000   509   491 0.509
## 229  1000   490   510 0.490
## 230  1000   498   502 0.498
## 231  1000   494   506 0.494
## 232  1000   521   479 0.521
## 233  1000   477   523 0.477
## 234  1000   510   490 0.510
## 235  1000   517   483 0.517
## 236  1000   506   494 0.506
## 237  1000   477   523 0.477
## 238  1000   490   510 0.490
## 239  1000   524   476 0.524
## 240  1000   503   497 0.503
## 241  1000   514   486 0.514
## 242  1000   506   494 0.506
## 243  1000   482   518 0.482
## 244  1000   507   493 0.507
## 245  1000   504   496 0.504
## 246  1000   501   499 0.501
## 247  1000   482   518 0.482
## 248  1000   480   520 0.480
## 249  1000   511   489 0.511
## 250  1000   497   503 0.497
## 251  1000   471   529 0.471
## 252  1000   510   490 0.510
## 253  1000   523   477 0.523
## 254  1000   485   515 0.485
## 255  1000   505   495 0.505
## 256  1000   507   493 0.507
## 257  1000   473   527 0.473
## 258  1000   495   505 0.495
## 259  1000   465   535 0.465
## 260  1000   501   499 0.501
## 261  1000   460   540 0.460
## 262  1000   499   501 0.499
## 263  1000   524   476 0.524
## 264  1000   514   486 0.514
## 265  1000   503   497 0.503
## 266  1000   469   531 0.469
## 267  1000   496   504 0.496
## 268  1000   489   511 0.489
## 269  1000   507   493 0.507
## 270  1000   466   534 0.466
## 271  1000   482   518 0.482
## 272  1000   520   480 0.520
## 273  1000   513   487 0.513
## 274  1000   492   508 0.492
## 275  1000   486   514 0.486
## 276  1000   498   502 0.498
## 277  1000   507   493 0.507
## 278  1000   494   506 0.494
## 279  1000   499   501 0.499
## 280  1000   498   502 0.498
## 281  1000   459   541 0.459
## 282  1000   495   505 0.495
## 283  1000   498   502 0.498
## 284  1000   495   505 0.495
## 285  1000   488   512 0.488
## 286  1000   518   482 0.518
## 287  1000   502   498 0.502
## 288  1000   503   497 0.503
## 289  1000   476   524 0.476
## 290  1000   495   505 0.495
## 291  1000   495   505 0.495
## 292  1000   503   497 0.503
## 293  1000   482   518 0.482
## 294  1000   518   482 0.518
## 295  1000   514   486 0.514
## 296  1000   520   480 0.520
## 297  1000   498   502 0.498
## 298  1000   523   477 0.523
## 299  1000   516   484 0.516
## 300  1000   483   517 0.483
## 301  1000   504   496 0.504
## 302  1000   505   495 0.505
## 303  1000   502   498 0.502
## 304  1000   486   514 0.486
## 305  1000   540   460 0.540
## 306  1000   510   490 0.510
## 307  1000   507   493 0.507
## 308  1000   482   518 0.482
## 309  1000   509   491 0.509
## 310  1000   486   514 0.486
## 311  1000   474   526 0.474
## 312  1000   511   489 0.511
## 313  1000   484   516 0.484
## 314  1000   499   501 0.499
## 315  1000   496   504 0.496
## 316  1000   505   495 0.505
## 317  1000   487   513 0.487
## 318  1000   520   480 0.520
## 319  1000   483   517 0.483
## 320  1000   515   485 0.515
## 321  1000   513   487 0.513
## 322  1000   509   491 0.509
## 323  1000   520   480 0.520
## 324  1000   509   491 0.509
## 325  1000   480   520 0.480
## 326  1000   524   476 0.524
## 327  1000   507   493 0.507
## 328  1000   509   491 0.509
## 329  1000   493   507 0.493
## 330  1000   464   536 0.464
## 331  1000   526   474 0.526
## 332  1000   513   487 0.513
## 333  1000   505   495 0.505
## 334  1000   509   491 0.509
## 335  1000   500   500 0.500
## 336  1000   499   501 0.499
## 337  1000   520   480 0.520
## 338  1000   491   509 0.491
## 339  1000   488   512 0.488
## 340  1000   483   517 0.483
## 341  1000   508   492 0.508
## 342  1000   474   526 0.474
## 343  1000   482   518 0.482
## 344  1000   485   515 0.485
## 345  1000   516   484 0.516
## 346  1000   511   489 0.511
## 347  1000   490   510 0.490
## 348  1000   519   481 0.519
## 349  1000   493   507 0.493
## 350  1000   508   492 0.508
## 351  1000   492   508 0.492
## 352  1000   500   500 0.500
## 353  1000   503   497 0.503
## 354  1000   478   522 0.478
## 355  1000   511   489 0.511
## 356  1000   495   505 0.495
## 357  1000   472   528 0.472
## 358  1000   468   532 0.468
## 359  1000   504   496 0.504
## 360  1000   478   522 0.478
## 361  1000   485   515 0.485
## 362  1000   503   497 0.503
## 363  1000   487   513 0.487
## 364  1000   482   518 0.482
## 365  1000   485   515 0.485
## 366  1000   507   493 0.507
## 367  1000   477   523 0.477
## 368  1000   504   496 0.504
## 369  1000   502   498 0.502
## 370  1000   492   508 0.492
## 371  1000   485   515 0.485
## 372  1000   491   509 0.491
## 373  1000   502   498 0.502
## 374  1000   483   517 0.483
## 375  1000   510   490 0.510
## 376  1000   508   492 0.508
## 377  1000   500   500 0.500
## 378  1000   501   499 0.501
## 379  1000   518   482 0.518
## 380  1000   528   472 0.528
## 381  1000   500   500 0.500
## 382  1000   486   514 0.486
## 383  1000   487   513 0.487
## 384  1000   511   489 0.511
## 385  1000   483   517 0.483
## 386  1000   485   515 0.485
## 387  1000   485   515 0.485
## 388  1000   520   480 0.520
## 389  1000   486   514 0.486
## 390  1000   492   508 0.492
## 391  1000   519   481 0.519
## 392  1000   478   522 0.478
## 393  1000   509   491 0.509
## 394  1000   494   506 0.494
## 395  1000   482   518 0.482
## 396  1000   490   510 0.490
## 397  1000   488   512 0.488
## 398  1000   538   462 0.538
## 399  1000   483   517 0.483
## 400  1000   515   485 0.515
## 401  1000   489   511 0.489
## 402  1000   511   489 0.511
## 403  1000   486   514 0.486
## 404  1000   501   499 0.501
## 405  1000   497   503 0.497
## 406  1000   515   485 0.515
## 407  1000   514   486 0.514
## 408  1000   504   496 0.504
## 409  1000   526   474 0.526
## 410  1000   481   519 0.481
## 411  1000   505   495 0.505
## 412  1000   504   496 0.504
## 413  1000   511   489 0.511
## 414  1000   510   490 0.510
## 415  1000   494   506 0.494
## 416  1000   515   485 0.515
## 417  1000   510   490 0.510
## 418  1000   488   512 0.488
## 419  1000   490   510 0.490
## 420  1000   506   494 0.506
## 421  1000   489   511 0.489
## 422  1000   514   486 0.514
## 423  1000   524   476 0.524
## 424  1000   492   508 0.492
## 425  1000   502   498 0.502
## 426  1000   519   481 0.519
## 427  1000   500   500 0.500
## 428  1000   516   484 0.516
## 429  1000   515   485 0.515
## 430  1000   496   504 0.496
## 431  1000   479   521 0.479
## 432  1000   481   519 0.481
## 433  1000   521   479 0.521
## 434  1000   485   515 0.485
## 435  1000   492   508 0.492
## 436  1000   507   493 0.507
## 437  1000   507   493 0.507
## 438  1000   497   503 0.497
## 439  1000   516   484 0.516
## 440  1000   491   509 0.491
## 441  1000   518   482 0.518
## 442  1000   490   510 0.490
## 443  1000   502   498 0.502
## 444  1000   521   479 0.521
## 445  1000   504   496 0.504
## 446  1000   495   505 0.495
## 447  1000   500   500 0.500
## 448  1000   513   487 0.513
## 449  1000   497   503 0.497
## 450  1000   488   512 0.488
## 451  1000   497   503 0.497
## 452  1000   532   468 0.532
## 453  1000   519   481 0.519
## 454  1000   487   513 0.487
## 455  1000   500   500 0.500
## 456  1000   509   491 0.509
## 457  1000   506   494 0.506
## 458  1000   508   492 0.508
## 459  1000   524   476 0.524
## 460  1000   520   480 0.520
## 461  1000   509   491 0.509
## 462  1000   551   449 0.551
## 463  1000   512   488 0.512
## 464  1000   497   503 0.497
## 465  1000   500   500 0.500
## 466  1000   493   507 0.493
## 467  1000   508   492 0.508
## 468  1000   514   486 0.514
## 469  1000   524   476 0.524
## 470  1000   508   492 0.508
## 471  1000   493   507 0.493
## 472  1000   513   487 0.513
## 473  1000   515   485 0.515
## 474  1000   494   506 0.494
## 475  1000   487   513 0.487
## 476  1000   464   536 0.464
## 477  1000   511   489 0.511
## 478  1000   484   516 0.484
## 479  1000   527   473 0.527
## 480  1000   485   515 0.485
## 481  1000   495   505 0.495
## 482  1000   515   485 0.515
## 483  1000   484   516 0.484
## 484  1000   464   536 0.464
## 485  1000   541   459 0.541
## 486  1000   512   488 0.512
## 487  1000   506   494 0.506
## 488  1000   500   500 0.500
## 489  1000   522   478 0.522
## 490  1000   507   493 0.507
## 491  1000   521   479 0.521
## 492  1000   511   489 0.511
## 493  1000   486   514 0.486
## 494  1000   501   499 0.501
## 495  1000   515   485 0.515
## 496  1000   473   527 0.473
## 497  1000   499   501 0.499
## 498  1000   515   485 0.515
## 499  1000   519   481 0.519
## 500  1000   488   512 0.488
## 501  1000   508   492 0.508
## 502  1000   484   516 0.484
## 503  1000   484   516 0.484
## 504  1000   502   498 0.502
## 505  1000   489   511 0.489
## 506  1000   495   505 0.495
## 507  1000   519   481 0.519
## 508  1000   521   479 0.521
## 509  1000   506   494 0.506
## 510  1000   515   485 0.515
## 511  1000   499   501 0.499
## 512  1000   514   486 0.514
## 513  1000   527   473 0.527
## 514  1000   504   496 0.504
## 515  1000   469   531 0.469
## 516  1000   489   511 0.489
## 517  1000   503   497 0.503
## 518  1000   531   469 0.531
## 519  1000   497   503 0.497
## 520  1000   499   501 0.499
## 521  1000   483   517 0.483
## 522  1000   501   499 0.501
## 523  1000   481   519 0.481
## 524  1000   516   484 0.516
## 525  1000   491   509 0.491
## 526  1000   486   514 0.486
## 527  1000   492   508 0.492
## 528  1000   498   502 0.498
## 529  1000   522   478 0.522
## 530  1000   487   513 0.487
## 531  1000   477   523 0.477
## 532  1000   501   499 0.501
## 533  1000   490   510 0.490
## 534  1000   487   513 0.487
## 535  1000   490   510 0.490
## 536  1000   484   516 0.484
## 537  1000   489   511 0.489
## 538  1000   502   498 0.502
## 539  1000   490   510 0.490
## 540  1000   493   507 0.493
## 541  1000   509   491 0.509
## 542  1000   523   477 0.523
## 543  1000   501   499 0.501
## 544  1000   482   518 0.482
## 545  1000   498   502 0.498
## 546  1000   481   519 0.481
## 547  1000   502   498 0.502
## 548  1000   499   501 0.499
## 549  1000   504   496 0.504
## 550  1000   487   513 0.487
## 551  1000   481   519 0.481
## 552  1000   483   517 0.483
## 553  1000   488   512 0.488
## 554  1000   491   509 0.491
## 555  1000   532   468 0.532
## 556  1000   509   491 0.509
## 557  1000   495   505 0.495
## 558  1000   493   507 0.493
## 559  1000   519   481 0.519
## 560  1000   475   525 0.475
## 561  1000   523   477 0.523
## 562  1000   474   526 0.474
## 563  1000   461   539 0.461
## 564  1000   479   521 0.479
## 565  1000   528   472 0.528
## 566  1000   502   498 0.502
## 567  1000   503   497 0.503
## 568  1000   501   499 0.501
## 569  1000   487   513 0.487
## 570  1000   504   496 0.504
## 571  1000   504   496 0.504
## 572  1000   509   491 0.509
## 573  1000   493   507 0.493
## 574  1000   498   502 0.498
## 575  1000   488   512 0.488
## 576  1000   514   486 0.514
## 577  1000   482   518 0.482
## 578  1000   483   517 0.483
## 579  1000   500   500 0.500
## 580  1000   485   515 0.485
## 581  1000   503   497 0.503
## 582  1000   476   524 0.476
## 583  1000   518   482 0.518
## 584  1000   502   498 0.502
## 585  1000   496   504 0.496
## 586  1000   501   499 0.501
## 587  1000   501   499 0.501
## 588  1000   520   480 0.520
## 589  1000   489   511 0.489
## 590  1000   499   501 0.499
## 591  1000   484   516 0.484
## 592  1000   504   496 0.504
## 593  1000   510   490 0.510
## 594  1000   499   501 0.499
## 595  1000   490   510 0.490
## 596  1000   503   497 0.503
## 597  1000   486   514 0.486
## 598  1000   489   511 0.489
## 599  1000   505   495 0.505
## 600  1000   493   507 0.493
## 601  1000   490   510 0.490
## 602  1000   482   518 0.482
## 603  1000   522   478 0.522
## 604  1000   525   475 0.525
## 605  1000   503   497 0.503
## 606  1000   471   529 0.471
## 607  1000   501   499 0.501
## 608  1000   504   496 0.504
## 609  1000   495   505 0.495
## 610  1000   504   496 0.504
## 611  1000   494   506 0.494
## 612  1000   530   470 0.530
## 613  1000   484   516 0.484
## 614  1000   489   511 0.489
## 615  1000   500   500 0.500
## 616  1000   508   492 0.508
## 617  1000   492   508 0.492
## 618  1000   478   522 0.478
## 619  1000   534   466 0.534
## 620  1000   489   511 0.489
## 621  1000   503   497 0.503
## 622  1000   504   496 0.504
## 623  1000   484   516 0.484
## 624  1000   494   506 0.494
## 625  1000   483   517 0.483
## 626  1000   509   491 0.509
## 627  1000   520   480 0.520
## 628  1000   489   511 0.489
## 629  1000   501   499 0.501
## 630  1000   500   500 0.500
## 631  1000   483   517 0.483
## 632  1000   514   486 0.514
## 633  1000   513   487 0.513
## 634  1000   499   501 0.499
## 635  1000   492   508 0.492
## 636  1000   464   536 0.464
## 637  1000   508   492 0.508
## 638  1000   506   494 0.506
## 639  1000   499   501 0.499
## 640  1000   500   500 0.500
## 641  1000   512   488 0.512
## 642  1000   491   509 0.491
## 643  1000   510   490 0.510
## 644  1000   487   513 0.487
## 645  1000   484   516 0.484
## 646  1000   475   525 0.475
## 647  1000   501   499 0.501
## 648  1000   478   522 0.478
## 649  1000   490   510 0.490
## 650  1000   493   507 0.493
## 651  1000   510   490 0.510
## 652  1000   493   507 0.493
## 653  1000   519   481 0.519
## 654  1000   542   458 0.542
## 655  1000   495   505 0.495
## 656  1000   527   473 0.527
## 657  1000   537   463 0.537
## 658  1000   509   491 0.509
## 659  1000   461   539 0.461
## 660  1000   502   498 0.502
## 661  1000   508   492 0.508
## 662  1000   496   504 0.496
## 663  1000   487   513 0.487
## 664  1000   510   490 0.510
## 665  1000   488   512 0.488
## 666  1000   517   483 0.517
## 667  1000   503   497 0.503
## 668  1000   456   544 0.456
## 669  1000   470   530 0.470
## 670  1000   475   525 0.475
## 671  1000   510   490 0.510
## 672  1000   492   508 0.492
## 673  1000   492   508 0.492
## 674  1000   506   494 0.506
## 675  1000   492   508 0.492
## 676  1000   485   515 0.485
## 677  1000   500   500 0.500
## 678  1000   499   501 0.499
## 679  1000   512   488 0.512
## 680  1000   490   510 0.490
## 681  1000   502   498 0.502
## 682  1000   489   511 0.489
## 683  1000   499   501 0.499
## 684  1000   493   507 0.493
## 685  1000   494   506 0.494
## 686  1000   515   485 0.515
## 687  1000   488   512 0.488
## 688  1000   487   513 0.487
## 689  1000   504   496 0.504
## 690  1000   504   496 0.504
## 691  1000   481   519 0.481
## 692  1000   487   513 0.487
## 693  1000   512   488 0.512
## 694  1000   512   488 0.512
## 695  1000   474   526 0.474
## 696  1000   498   502 0.498
## 697  1000   504   496 0.504
## 698  1000   510   490 0.510
## 699  1000   501   499 0.501
## 700  1000   517   483 0.517
## 701  1000   507   493 0.507
## 702  1000   478   522 0.478
## 703  1000   536   464 0.536
## 704  1000   484   516 0.484
## 705  1000   482   518 0.482
## 706  1000   485   515 0.485
## 707  1000   510   490 0.510
## 708  1000   487   513 0.487
## 709  1000   484   516 0.484
## 710  1000   504   496 0.504
## 711  1000   499   501 0.499
## 712  1000   507   493 0.507
## 713  1000   490   510 0.490
## 714  1000   511   489 0.511
## 715  1000   521   479 0.521
## 716  1000   507   493 0.507
## 717  1000   504   496 0.504
## 718  1000   489   511 0.489
## 719  1000   487   513 0.487
## 720  1000   502   498 0.502
## 721  1000   502   498 0.502
## 722  1000   491   509 0.491
## 723  1000   484   516 0.484
## 724  1000   500   500 0.500
## 725  1000   512   488 0.512
## 726  1000   491   509 0.491
## 727  1000   496   504 0.496
## 728  1000   485   515 0.485
## 729  1000   523   477 0.523
## 730  1000   515   485 0.515
## 731  1000   503   497 0.503
## 732  1000   509   491 0.509
## 733  1000   487   513 0.487
## 734  1000   508   492 0.508
## 735  1000   480   520 0.480
## 736  1000   499   501 0.499
## 737  1000   495   505 0.495
## 738  1000   502   498 0.502
## 739  1000   516   484 0.516
## 740  1000   493   507 0.493
## 741  1000   484   516 0.484
## 742  1000   475   525 0.475
## 743  1000   483   517 0.483
## 744  1000   508   492 0.508
## 745  1000   523   477 0.523
## 746  1000   502   498 0.502
## 747  1000   503   497 0.503
## 748  1000   519   481 0.519
## 749  1000   483   517 0.483
## 750  1000   484   516 0.484
## 751  1000   501   499 0.501
## 752  1000   494   506 0.494
## 753  1000   511   489 0.511
## 754  1000   507   493 0.507
## 755  1000   493   507 0.493
## 756  1000   501   499 0.501
## 757  1000   507   493 0.507
## 758  1000   507   493 0.507
## 759  1000   522   478 0.522
## 760  1000   475   525 0.475
## 761  1000   501   499 0.501
## 762  1000   478   522 0.478
## 763  1000   504   496 0.504
## 764  1000   506   494 0.506
## 765  1000   499   501 0.499
## 766  1000   492   508 0.492
## 767  1000   503   497 0.503
## 768  1000   501   499 0.501
## 769  1000   512   488 0.512
## 770  1000   491   509 0.491
## 771  1000   503   497 0.503
## 772  1000   484   516 0.484
## 773  1000   525   475 0.525
## 774  1000   527   473 0.527
## 775  1000   514   486 0.514
## 776  1000   507   493 0.507
## 777  1000   485   515 0.485
## 778  1000   482   518 0.482
## 779  1000   502   498 0.502
## 780  1000   492   508 0.492
## 781  1000   494   506 0.494
## 782  1000   501   499 0.501
## 783  1000   492   508 0.492
## 784  1000   502   498 0.502
## 785  1000   516   484 0.516
## 786  1000   505   495 0.505
## 787  1000   497   503 0.497
## 788  1000   492   508 0.492
## 789  1000   497   503 0.497
## 790  1000   511   489 0.511
## 791  1000   499   501 0.499
## 792  1000   507   493 0.507
## 793  1000   493   507 0.493
## 794  1000   491   509 0.491
## 795  1000   480   520 0.480
## 796  1000   512   488 0.512
## 797  1000   520   480 0.520
## 798  1000   482   518 0.482
## 799  1000   511   489 0.511
## 800  1000   517   483 0.517
## 801  1000   497   503 0.497
## 802  1000   513   487 0.513
## 803  1000   502   498 0.502
## 804  1000   521   479 0.521
## 805  1000   505   495 0.505
## 806  1000   479   521 0.479
## 807  1000   508   492 0.508
## 808  1000   516   484 0.516
## 809  1000   500   500 0.500
## 810  1000   517   483 0.517
## 811  1000   479   521 0.479
## 812  1000   493   507 0.493
## 813  1000   507   493 0.507
## 814  1000   519   481 0.519
## 815  1000   496   504 0.496
## 816  1000   497   503 0.497
## 817  1000   498   502 0.498
## 818  1000   500   500 0.500
## 819  1000   507   493 0.507
## 820  1000   527   473 0.527
## 821  1000   463   537 0.463
## 822  1000   506   494 0.506
## 823  1000   511   489 0.511
## 824  1000   523   477 0.523
## 825  1000   515   485 0.515
## 826  1000   527   473 0.527
## 827  1000   519   481 0.519
## 828  1000   490   510 0.490
## 829  1000   505   495 0.505
## 830  1000   511   489 0.511
## 831  1000   469   531 0.469
## 832  1000   492   508 0.492
## 833  1000   497   503 0.497
## 834  1000   523   477 0.523
## 835  1000   480   520 0.480
## 836  1000   493   507 0.493
## 837  1000   529   471 0.529
## 838  1000   523   477 0.523
## 839  1000   499   501 0.499
## 840  1000   523   477 0.523
## 841  1000   501   499 0.501
## 842  1000   505   495 0.505
## 843  1000   523   477 0.523
## 844  1000   504   496 0.504
## 845  1000   492   508 0.492
## 846  1000   470   530 0.470
## 847  1000   493   507 0.493
## 848  1000   511   489 0.511
## 849  1000   485   515 0.485
## 850  1000   510   490 0.510
## 851  1000   498   502 0.498
## 852  1000   506   494 0.506
## 853  1000   501   499 0.501
## 854  1000   519   481 0.519
## 855  1000   514   486 0.514
## 856  1000   489   511 0.489
## 857  1000   513   487 0.513
## 858  1000   533   467 0.533
## 859  1000   485   515 0.485
## 860  1000   499   501 0.499
## 861  1000   490   510 0.490
## 862  1000   508   492 0.508
## 863  1000   482   518 0.482
## 864  1000   496   504 0.496
## 865  1000   496   504 0.496
## 866  1000   525   475 0.525
## 867  1000   500   500 0.500
## 868  1000   480   520 0.480
## 869  1000   493   507 0.493
## 870  1000   500   500 0.500
## 871  1000   489   511 0.489
## 872  1000   503   497 0.503
## 873  1000   479   521 0.479
## 874  1000   500   500 0.500
## 875  1000   499   501 0.499
## 876  1000   502   498 0.502
## 877  1000   485   515 0.485
## 878  1000   515   485 0.515
## 879  1000   512   488 0.512
## 880  1000   509   491 0.509
## 881  1000   499   501 0.499
## 882  1000   477   523 0.477
## 883  1000   515   485 0.515
## 884  1000   490   510 0.490
## 885  1000   505   495 0.505
## 886  1000   499   501 0.499
## 887  1000   495   505 0.495
## 888  1000   527   473 0.527
## 889  1000   514   486 0.514
## 890  1000   513   487 0.513
## 891  1000   505   495 0.505
## 892  1000   504   496 0.504
## 893  1000   482   518 0.482
## 894  1000   499   501 0.499
## 895  1000   491   509 0.491
## 896  1000   474   526 0.474
## 897  1000   513   487 0.513
## 898  1000   492   508 0.492
## 899  1000   504   496 0.504
## 900  1000   511   489 0.511
## 901  1000   488   512 0.488
## 902  1000   534   466 0.534
## 903  1000   485   515 0.485
## 904  1000   471   529 0.471
## 905  1000   511   489 0.511
## 906  1000   502   498 0.502
## 907  1000   517   483 0.517
## 908  1000   520   480 0.520
## 909  1000   525   475 0.525
## 910  1000   517   483 0.517
## 911  1000   495   505 0.495
## 912  1000   497   503 0.497
## 913  1000   493   507 0.493
## 914  1000   496   504 0.496
## 915  1000   472   528 0.472
## 916  1000   503   497 0.503
## 917  1000   512   488 0.512
## 918  1000   488   512 0.488
## 919  1000   482   518 0.482
## 920  1000   496   504 0.496
## 921  1000   474   526 0.474
## 922  1000   502   498 0.502
## 923  1000   490   510 0.490
## 924  1000   516   484 0.516
## 925  1000   488   512 0.488
## 926  1000   489   511 0.489
## 927  1000   477   523 0.477
## 928  1000   511   489 0.511
## 929  1000   486   514 0.486
## 930  1000   482   518 0.482
## 931  1000   486   514 0.486
## 932  1000   506   494 0.506
## 933  1000   492   508 0.492
## 934  1000   482   518 0.482
## 935  1000   509   491 0.509
## 936  1000   511   489 0.511
## 937  1000   477   523 0.477
## 938  1000   507   493 0.507
## 939  1000   506   494 0.506
## 940  1000   497   503 0.497
## 941  1000   506   494 0.506
## 942  1000   495   505 0.495
## 943  1000   513   487 0.513
## 944  1000   511   489 0.511
## 945  1000   486   514 0.486
## 946  1000   486   514 0.486
## 947  1000   511   489 0.511
## 948  1000   492   508 0.492
## 949  1000   475   525 0.475
## 950  1000   490   510 0.490
## 951  1000   488   512 0.488
## 952  1000   493   507 0.493
## 953  1000   485   515 0.485
## 954  1000   509   491 0.509
## 955  1000   486   514 0.486
## 956  1000   504   496 0.504
## 957  1000   477   523 0.477
## 958  1000   512   488 0.512
## 959  1000   501   499 0.501
## 960  1000   487   513 0.487
## 961  1000   493   507 0.493
## 962  1000   492   508 0.492
## 963  1000   512   488 0.512
## 964  1000   505   495 0.505
## 965  1000   494   506 0.494
## 966  1000   494   506 0.494
## 967  1000   493   507 0.493
## 968  1000   502   498 0.502
## 969  1000   498   502 0.498
## 970  1000   498   502 0.498
## 971  1000   517   483 0.517
## 972  1000   525   475 0.525
## 973  1000   530   470 0.530
## 974  1000   503   497 0.503
## 975  1000   486   514 0.486
## 976  1000   525   475 0.525
## 977  1000   503   497 0.503
## 978  1000   493   507 0.493
## 979  1000   485   515 0.485
## 980  1000   485   515 0.485
## 981  1000   529   471 0.529
## 982  1000   508   492 0.508
## 983  1000   495   505 0.495
## 984  1000   488   512 0.488
## 985  1000   519   481 0.519
## 986  1000   515   485 0.515
## 987  1000   464   536 0.464
## 988  1000   524   476 0.524
## 989  1000   522   478 0.522
## 990  1000   520   480 0.520
## 991  1000   508   492 0.508
## 992  1000   512   488 0.512
## 993  1000   504   496 0.504
## 994  1000   481   519 0.481
## 995  1000   450   550 0.450
## 996  1000   500   500 0.500
## 997  1000   499   501 0.499
## 998  1000   487   513 0.487
## 999  1000   481   519 0.481
## 1000 1000   498   502 0.498
## 1001 1000   520   480 0.520
## 1002 1000   492   508 0.492
## 1003 1000   532   468 0.532
## 1004 1000   512   488 0.512
## 1005 1000   503   497 0.503
## 1006 1000   482   518 0.482
## 1007 1000   486   514 0.486
## 1008 1000   518   482 0.518
## 1009 1000   469   531 0.469
## 1010 1000   468   532 0.468
## 1011 1000   471   529 0.471
## 1012 1000   524   476 0.524
## 1013 1000   500   500 0.500
## 1014 1000   514   486 0.514
## 1015 1000   510   490 0.510
## 1016 1000   478   522 0.478
## 1017 1000   518   482 0.518
## 1018 1000   503   497 0.503
## 1019 1000   512   488 0.512
## 1020 1000   506   494 0.506
## 1021 1000   492   508 0.492
## 1022 1000   513   487 0.513
## 1023 1000   499   501 0.499
## 1024 1000   469   531 0.469
## 1025 1000   497   503 0.497
## 1026 1000   491   509 0.491
## 1027 1000   508   492 0.508
## 1028 1000   498   502 0.498
## 1029 1000   500   500 0.500
## 1030 1000   513   487 0.513
## 1031 1000   502   498 0.502
## 1032 1000   528   472 0.528
## 1033 1000   482   518 0.482
## 1034 1000   497   503 0.497
## 1035 1000   510   490 0.510
## 1036 1000   509   491 0.509
## 1037 1000   490   510 0.490
## 1038 1000   500   500 0.500
## 1039 1000   470   530 0.470
## 1040 1000   481   519 0.481
## 1041 1000   510   490 0.510
## 1042 1000   465   535 0.465
## 1043 1000   501   499 0.501
## 1044 1000   495   505 0.495
## 1045 1000   490   510 0.490
## 1046 1000   491   509 0.491
## 1047 1000   497   503 0.497
## 1048 1000   495   505 0.495
## 1049 1000   532   468 0.532
## 1050 1000   497   503 0.497
## 1051 1000   510   490 0.510
## 1052 1000   488   512 0.488
## 1053 1000   480   520 0.480
## 1054 1000   532   468 0.532
## 1055 1000   484   516 0.484
## 1056 1000   512   488 0.512
## 1057 1000   491   509 0.491
## 1058 1000   498   502 0.498
## 1059 1000   495   505 0.495
## 1060 1000   482   518 0.482
## 1061 1000   495   505 0.495
## 1062 1000   489   511 0.489
## 1063 1000   486   514 0.486
## 1064 1000   515   485 0.515
## 1065 1000   500   500 0.500
## 1066 1000   494   506 0.494
## 1067 1000   520   480 0.520
## 1068 1000   516   484 0.516
## 1069 1000   497   503 0.497
## 1070 1000   511   489 0.511
## 1071 1000   499   501 0.499
## 1072 1000   475   525 0.475
## 1073 1000   480   520 0.480
## 1074 1000   508   492 0.508
## 1075 1000   487   513 0.487
## 1076 1000   483   517 0.483
## 1077 1000   500   500 0.500
## 1078 1000   502   498 0.502
## 1079 1000   471   529 0.471
## 1080 1000   526   474 0.526
## 1081 1000   494   506 0.494
## 1082 1000   507   493 0.507
## 1083 1000   508   492 0.508
## 1084 1000   487   513 0.487
## 1085 1000   493   507 0.493
## 1086 1000   504   496 0.504
## 1087 1000   514   486 0.514
## 1088 1000   512   488 0.512
## 1089 1000   499   501 0.499
## 1090 1000   531   469 0.531
## 1091 1000   485   515 0.485
## 1092 1000   515   485 0.515
## 1093 1000   475   525 0.475
## 1094 1000   473   527 0.473
## 1095 1000   487   513 0.487
## 1096 1000   481   519 0.481
## 1097 1000   486   514 0.486
## 1098 1000   466   534 0.466
## 1099 1000   475   525 0.475
## 1100 1000   513   487 0.513
## 1101 1000   497   503 0.497
## 1102 1000   523   477 0.523
## 1103 1000   491   509 0.491
## 1104 1000   521   479 0.521
## 1105 1000   489   511 0.489
## 1106 1000   512   488 0.512
## 1107 1000   496   504 0.496
## 1108 1000   517   483 0.517
## 1109 1000   533   467 0.533
## 1110 1000   527   473 0.527
## 1111 1000   533   467 0.533
## 1112 1000   497   503 0.497
## 1113 1000   490   510 0.490
## 1114 1000   481   519 0.481
## 1115 1000   491   509 0.491
## 1116 1000   489   511 0.489
## 1117 1000   472   528 0.472
## 1118 1000   511   489 0.511
## 1119 1000   494   506 0.494
## 1120 1000   545   455 0.545
## 1121 1000   498   502 0.498
## 1122 1000   490   510 0.490
## 1123 1000   516   484 0.516
## 1124 1000   475   525 0.475
## 1125 1000   494   506 0.494
## 1126 1000   537   463 0.537
## 1127 1000   481   519 0.481
## 1128 1000   495   505 0.495
## 1129 1000   488   512 0.488
## 1130 1000   490   510 0.490
## 1131 1000   486   514 0.486
## 1132 1000   527   473 0.527
## 1133 1000   501   499 0.501
## 1134 1000   505   495 0.505
## 1135 1000   502   498 0.502
## 1136 1000   494   506 0.494
## 1137 1000   495   505 0.495
## 1138 1000   517   483 0.517
## 1139 1000   480   520 0.480
## 1140 1000   477   523 0.477
## 1141 1000   505   495 0.505
## 1142 1000   516   484 0.516
## 1143 1000   526   474 0.526
## 1144 1000   518   482 0.518
## 1145 1000   495   505 0.495
## 1146 1000   511   489 0.511
## 1147 1000   493   507 0.493
## 1148 1000   506   494 0.506
## 1149 1000   498   502 0.498
## 1150 1000   504   496 0.504
## 1151 1000   509   491 0.509
## 1152 1000   487   513 0.487
## 1153 1000   504   496 0.504
## 1154 1000   496   504 0.496
## 1155 1000   512   488 0.512
## 1156 1000   477   523 0.477
## 1157 1000   514   486 0.514
## 1158 1000   511   489 0.511
## 1159 1000   475   525 0.475
## 1160 1000   464   536 0.464
## 1161 1000   448   552 0.448
## 1162 1000   526   474 0.526
## 1163 1000   538   462 0.538
## 1164 1000   499   501 0.499
## 1165 1000   487   513 0.487
## 1166 1000   509   491 0.509
## 1167 1000   501   499 0.501
## 1168 1000   481   519 0.481
## 1169 1000   509   491 0.509
## 1170 1000   486   514 0.486
## 1171 1000   487   513 0.487
## 1172 1000   491   509 0.491
## 1173 1000   489   511 0.489
## 1174 1000   475   525 0.475
## 1175 1000   474   526 0.474
## 1176 1000   473   527 0.473
## 1177 1000   513   487 0.513
## 1178 1000   517   483 0.517
## 1179 1000   497   503 0.497
## 1180 1000   469   531 0.469
## 1181 1000   520   480 0.520
## 1182 1000   457   543 0.457
## 1183 1000   532   468 0.532
## 1184 1000   500   500 0.500
## 1185 1000   514   486 0.514
## 1186 1000   522   478 0.522
## 1187 1000   517   483 0.517
## 1188 1000   518   482 0.518
## 1189 1000   503   497 0.503
## 1190 1000   506   494 0.506
## 1191 1000   504   496 0.504
## 1192 1000   509   491 0.509
## 1193 1000   506   494 0.506
## 1194 1000   511   489 0.511
## 1195 1000   496   504 0.496
## 1196 1000   513   487 0.513
## 1197 1000   505   495 0.505
## 1198 1000   512   488 0.512
## 1199 1000   495   505 0.495
## 1200 1000   512   488 0.512
## 1201 1000   495   505 0.495
## 1202 1000   527   473 0.527
## 1203 1000   495   505 0.495
## 1204 1000   513   487 0.513
## 1205 1000   515   485 0.515
## 1206 1000   488   512 0.488
## 1207 1000   495   505 0.495
## 1208 1000   494   506 0.494
## 1209 1000   505   495 0.505
## 1210 1000   500   500 0.500
## 1211 1000   483   517 0.483
## 1212 1000   505   495 0.505
## 1213 1000   523   477 0.523
## 1214 1000   508   492 0.508
## 1215 1000   498   502 0.498
## 1216 1000   499   501 0.499
## 1217 1000   489   511 0.489
## 1218 1000   505   495 0.505
## 1219 1000   509   491 0.509
## 1220 1000   501   499 0.501
## 1221 1000   496   504 0.496
## 1222 1000   496   504 0.496
## 1223 1000   504   496 0.504
## 1224 1000   491   509 0.491
## 1225 1000   500   500 0.500
## 1226 1000   523   477 0.523
## 1227 1000   499   501 0.499
## 1228 1000   489   511 0.489
## 1229 1000   486   514 0.486
## 1230 1000   515   485 0.515
## 1231 1000   494   506 0.494
## 1232 1000   496   504 0.496
## 1233 1000   496   504 0.496
## 1234 1000   486   514 0.486
## 1235 1000   533   467 0.533
## 1236 1000   487   513 0.487
## 1237 1000   485   515 0.485
## 1238 1000   503   497 0.503
## 1239 1000   508   492 0.508
## 1240 1000   510   490 0.510
## 1241 1000   496   504 0.496
## 1242 1000   497   503 0.497
## 1243 1000   504   496 0.504
## 1244 1000   470   530 0.470
## 1245 1000   512   488 0.512
## 1246 1000   526   474 0.526
## 1247 1000   487   513 0.487
## 1248 1000   508   492 0.508
## 1249 1000   505   495 0.505
## 1250 1000   519   481 0.519
## 1251 1000   490   510 0.490
## 1252 1000   475   525 0.475
## 1253 1000   479   521 0.479
## 1254 1000   509   491 0.509
## 1255 1000   500   500 0.500
## 1256 1000   479   521 0.479
## 1257 1000   529   471 0.529
## 1258 1000   518   482 0.518
## 1259 1000   510   490 0.510
## 1260 1000   482   518 0.482
## 1261 1000   498   502 0.498
## 1262 1000   478   522 0.478
## 1263 1000   498   502 0.498
## 1264 1000   521   479 0.521
## 1265 1000   501   499 0.501
## 1266 1000   489   511 0.489
## 1267 1000   502   498 0.502
## 1268 1000   509   491 0.509
## 1269 1000   502   498 0.502
## 1270 1000   455   545 0.455
## 1271 1000   486   514 0.486
## 1272 1000   524   476 0.524
## 1273 1000   510   490 0.510
## 1274 1000   492   508 0.492
## 1275 1000   484   516 0.484
## 1276 1000   480   520 0.480
## 1277 1000   520   480 0.520
## 1278 1000   486   514 0.486
## 1279 1000   506   494 0.506
## 1280 1000   492   508 0.492
## 1281 1000   512   488 0.512
## 1282 1000   522   478 0.522
## 1283 1000   525   475 0.525
## 1284 1000   494   506 0.494
## 1285 1000   500   500 0.500
## 1286 1000   499   501 0.499
## 1287 1000   522   478 0.522
## 1288 1000   494   506 0.494
## 1289 1000   525   475 0.525
## 1290 1000   506   494 0.506
## 1291 1000   496   504 0.496
## 1292 1000   524   476 0.524
## 1293 1000   475   525 0.475
## 1294 1000   465   535 0.465
## 1295 1000   495   505 0.495
## 1296 1000   517   483 0.517
## 1297 1000   502   498 0.502
## 1298 1000   494   506 0.494
## 1299 1000   518   482 0.518
## 1300 1000   479   521 0.479
## 1301 1000   513   487 0.513
## 1302 1000   522   478 0.522
## 1303 1000   494   506 0.494
## 1304 1000   499   501 0.499
## 1305 1000   493   507 0.493
## 1306 1000   535   465 0.535
## 1307 1000   495   505 0.495
## 1308 1000   507   493 0.507
## 1309 1000   509   491 0.509
## 1310 1000   500   500 0.500
## 1311 1000   480   520 0.480
## 1312 1000   524   476 0.524
## 1313 1000   489   511 0.489
## 1314 1000   504   496 0.504
## 1315 1000   516   484 0.516
## 1316 1000   521   479 0.521
## 1317 1000   532   468 0.532
## 1318 1000   518   482 0.518
## 1319 1000   500   500 0.500
## 1320 1000   502   498 0.502
## 1321 1000   491   509 0.491
## 1322 1000   529   471 0.529
## 1323 1000   513   487 0.513
## 1324 1000   489   511 0.489
## 1325 1000   496   504 0.496
## 1326 1000   515   485 0.515
## 1327 1000   498   502 0.498
## 1328 1000   495   505 0.495
## 1329 1000   459   541 0.459
## 1330 1000   521   479 0.521
## 1331 1000   515   485 0.515
## 1332 1000   491   509 0.491
## 1333 1000   496   504 0.496
## 1334 1000   514   486 0.514
## 1335 1000   497   503 0.497
## 1336 1000   515   485 0.515
## 1337 1000   483   517 0.483
## 1338 1000   497   503 0.497
## 1339 1000   496   504 0.496
## 1340 1000   495   505 0.495
## 1341 1000   497   503 0.497
## 1342 1000   499   501 0.499
## 1343 1000   515   485 0.515
## 1344 1000   520   480 0.520
## 1345 1000   520   480 0.520
## 1346 1000   513   487 0.513
## 1347 1000   504   496 0.504
## 1348 1000   528   472 0.528
## 1349 1000   489   511 0.489
## 1350 1000   512   488 0.512
## 1351 1000   527   473 0.527
## 1352 1000   503   497 0.503
## 1353 1000   471   529 0.471
## 1354 1000   478   522 0.478
## 1355 1000   501   499 0.501
## 1356 1000   491   509 0.491
## 1357 1000   504   496 0.504
## 1358 1000   502   498 0.502
## 1359 1000   471   529 0.471
## 1360 1000   492   508 0.492
## 1361 1000   488   512 0.488
## 1362 1000   494   506 0.494
## 1363 1000   531   469 0.531
## 1364 1000   473   527 0.473
## 1365 1000   487   513 0.487
## 1366 1000   503   497 0.503
## 1367 1000   494   506 0.494
## 1368 1000   530   470 0.530
## 1369 1000   496   504 0.496
## 1370 1000   517   483 0.517
## 1371 1000   526   474 0.526
## 1372 1000   515   485 0.515
## 1373 1000   488   512 0.488
## 1374 1000   455   545 0.455
## 1375 1000   503   497 0.503
## 1376 1000   494   506 0.494
## 1377 1000   527   473 0.527
## 1378 1000   503   497 0.503
## 1379 1000   472   528 0.472
## 1380 1000   511   489 0.511
## 1381 1000   488   512 0.488
## 1382 1000   493   507 0.493
## 1383 1000   520   480 0.520
## 1384 1000   524   476 0.524
## 1385 1000   508   492 0.508
## 1386 1000   515   485 0.515
## 1387 1000   519   481 0.519
## 1388 1000   490   510 0.490
## 1389 1000   477   523 0.477
## 1390 1000   508   492 0.508
## 1391 1000   515   485 0.515
## 1392 1000   520   480 0.520
## 1393 1000   489   511 0.489
## 1394 1000   500   500 0.500
## 1395 1000   519   481 0.519
## 1396 1000   493   507 0.493
## 1397 1000   509   491 0.509
## 1398 1000   489   511 0.489
## 1399 1000   494   506 0.494
## 1400 1000   508   492 0.508
## 1401 1000   513   487 0.513
## 1402 1000   514   486 0.514
## 1403 1000   516   484 0.516
## 1404 1000   502   498 0.502
## 1405 1000   496   504 0.496
## 1406 1000   483   517 0.483
## 1407 1000   516   484 0.516
## 1408 1000   502   498 0.502
## 1409 1000   510   490 0.510
## 1410 1000   469   531 0.469
## 1411 1000   487   513 0.487
## 1412 1000   518   482 0.518
## 1413 1000   499   501 0.499
## 1414 1000   463   537 0.463
## 1415 1000   521   479 0.521
## 1416 1000   483   517 0.483
## 1417 1000   469   531 0.469
## 1418 1000   493   507 0.493
## 1419 1000   496   504 0.496
## 1420 1000   482   518 0.482
## 1421 1000   477   523 0.477
## 1422 1000   536   464 0.536
## 1423 1000   507   493 0.507
## 1424 1000   505   495 0.505
## 1425 1000   511   489 0.511
## 1426 1000   517   483 0.517
## 1427 1000   510   490 0.510
## 1428 1000   486   514 0.486
## 1429 1000   520   480 0.520
## 1430 1000   493   507 0.493
## 1431 1000   497   503 0.497
## 1432 1000   491   509 0.491
## 1433 1000   520   480 0.520
## 1434 1000   494   506 0.494
## 1435 1000   514   486 0.514
## 1436 1000   479   521 0.479
## 1437 1000   506   494 0.506
## 1438 1000   492   508 0.492
## 1439 1000   474   526 0.474
## 1440 1000   501   499 0.501
## 1441 1000   504   496 0.504
## 1442 1000   507   493 0.507
## 1443 1000   482   518 0.482
## 1444 1000   512   488 0.512
## 1445 1000   506   494 0.506
## 1446 1000   516   484 0.516
## 1447 1000   504   496 0.504
## 1448 1000   508   492 0.508
## 1449 1000   504   496 0.504
## 1450 1000   499   501 0.499
## 1451 1000   520   480 0.520
## 1452 1000   484   516 0.484
## 1453 1000   504   496 0.504
## 1454 1000   499   501 0.499
## 1455 1000   499   501 0.499
## 1456 1000   500   500 0.500
## 1457 1000   503   497 0.503
## 1458 1000   488   512 0.488
## 1459 1000   474   526 0.474
## 1460 1000   504   496 0.504
## 1461 1000   510   490 0.510
## 1462 1000   498   502 0.498
## 1463 1000   510   490 0.510
## 1464 1000   523   477 0.523
## 1465 1000   525   475 0.525
## 1466 1000   475   525 0.475
## 1467 1000   496   504 0.496
## 1468 1000   482   518 0.482
## 1469 1000   506   494 0.506
## 1470 1000   468   532 0.468
## 1471 1000   500   500 0.500
## 1472 1000   486   514 0.486
## 1473 1000   508   492 0.508
## 1474 1000   517   483 0.517
## 1475 1000   507   493 0.507
## 1476 1000   518   482 0.518
## 1477 1000   508   492 0.508
## 1478 1000   482   518 0.482
## 1479 1000   504   496 0.504
## 1480 1000   483   517 0.483
## 1481 1000   521   479 0.521
## 1482 1000   506   494 0.506
## 1483 1000   510   490 0.510
## 1484 1000   500   500 0.500
## 1485 1000   473   527 0.473
## 1486 1000   516   484 0.516
## 1487 1000   505   495 0.505
## 1488 1000   486   514 0.486
## 1489 1000   467   533 0.467
## 1490 1000   522   478 0.522
## 1491 1000   515   485 0.515
## 1492 1000   495   505 0.495
## 1493 1000   476   524 0.476
## 1494 1000   497   503 0.497
## 1495 1000   514   486 0.514
## 1496 1000   490   510 0.490
## 1497 1000   518   482 0.518
## 1498 1000   508   492 0.508
## 1499 1000   480   520 0.480
## 1500 1000   501   499 0.501
## 1501 1000   490   510 0.490
## 1502 1000   475   525 0.475
## 1503 1000   493   507 0.493
## 1504 1000   498   502 0.498
## 1505 1000   541   459 0.541
## 1506 1000   484   516 0.484
## 1507 1000   508   492 0.508
## 1508 1000   453   547 0.453
## 1509 1000   530   470 0.530
## 1510 1000   491   509 0.491
## 1511 1000   496   504 0.496
## 1512 1000   520   480 0.520
## 1513 1000   508   492 0.508
## 1514 1000   504   496 0.504
## 1515 1000   524   476 0.524
## 1516 1000   510   490 0.510
## 1517 1000   500   500 0.500
## 1518 1000   490   510 0.490
## 1519 1000   505   495 0.505
## 1520 1000   509   491 0.509
## 1521 1000   525   475 0.525
## 1522 1000   493   507 0.493
## 1523 1000   511   489 0.511
## 1524 1000   497   503 0.497
## 1525 1000   479   521 0.479
## 1526 1000   489   511 0.489
## 1527 1000   528   472 0.528
## 1528 1000   515   485 0.515
## 1529 1000   492   508 0.492
## 1530 1000   498   502 0.498
## 1531 1000   518   482 0.518
## 1532 1000   484   516 0.484
## 1533 1000   485   515 0.485
## 1534 1000   502   498 0.502
## 1535 1000   515   485 0.515
## 1536 1000   535   465 0.535
## 1537 1000   529   471 0.529
## 1538 1000   481   519 0.481
## 1539 1000   505   495 0.505
## 1540 1000   492   508 0.492
## 1541 1000   478   522 0.478
## 1542 1000   514   486 0.514
## 1543 1000   491   509 0.491
## 1544 1000   494   506 0.494
## 1545 1000   498   502 0.498
## 1546 1000   487   513 0.487
## 1547 1000   494   506 0.494
## 1548 1000   511   489 0.511
## 1549 1000   510   490 0.510
## 1550 1000   488   512 0.488
## 1551 1000   491   509 0.491
## 1552 1000   544   456 0.544
## 1553 1000   514   486 0.514
## 1554 1000   501   499 0.501
## 1555 1000   506   494 0.506
## 1556 1000   485   515 0.485
## 1557 1000   505   495 0.505
## 1558 1000   490   510 0.490
## 1559 1000   502   498 0.502
## 1560 1000   500   500 0.500
## 1561 1000   485   515 0.485
## 1562 1000   503   497 0.503
## 1563 1000   483   517 0.483
## 1564 1000   517   483 0.517
## 1565 1000   509   491 0.509
## 1566 1000   510   490 0.510
## 1567 1000   488   512 0.488
## 1568 1000   491   509 0.491
## 1569 1000   526   474 0.526
## 1570 1000   484   516 0.484
## 1571 1000   494   506 0.494
## 1572 1000   498   502 0.498
## 1573 1000   481   519 0.481
## 1574 1000   520   480 0.520
## 1575 1000   504   496 0.504
## 1576 1000   512   488 0.512
## 1577 1000   510   490 0.510
## 1578 1000   503   497 0.503
## 1579 1000   501   499 0.501
## 1580 1000   495   505 0.495
## 1581 1000   497   503 0.497
## 1582 1000   533   467 0.533
## 1583 1000   521   479 0.521
## 1584 1000   492   508 0.492
## 1585 1000   496   504 0.496
## 1586 1000   484   516 0.484
## 1587 1000   487   513 0.487
## 1588 1000   495   505 0.495
## 1589 1000   476   524 0.476
## 1590 1000   483   517 0.483
## 1591 1000   520   480 0.520
## 1592 1000   502   498 0.502
## 1593 1000   497   503 0.497
## 1594 1000   495   505 0.495
## 1595 1000   510   490 0.510
## 1596 1000   500   500 0.500
## 1597 1000   517   483 0.517
## 1598 1000   513   487 0.513
## 1599 1000   491   509 0.491
## 1600 1000   475   525 0.475
## 1601 1000   498   502 0.498
## 1602 1000   516   484 0.516
## 1603 1000   493   507 0.493
## 1604 1000   485   515 0.485
## 1605 1000   504   496 0.504
## 1606 1000   496   504 0.496
## 1607 1000   480   520 0.480
## 1608 1000   498   502 0.498
## 1609 1000   530   470 0.530
## 1610 1000   470   530 0.470
## 1611 1000   516   484 0.516
## 1612 1000   514   486 0.514
## 1613 1000   500   500 0.500
## 1614 1000   469   531 0.469
## 1615 1000   495   505 0.495
## 1616 1000   489   511 0.489
## 1617 1000   503   497 0.503
## 1618 1000   475   525 0.475
## 1619 1000   492   508 0.492
## 1620 1000   504   496 0.504
## 1621 1000   488   512 0.488
## 1622 1000   492   508 0.492
## 1623 1000   516   484 0.516
## 1624 1000   479   521 0.479
## 1625 1000   502   498 0.502
## 1626 1000   490   510 0.490
## 1627 1000   493   507 0.493
## 1628 1000   517   483 0.517
## 1629 1000   509   491 0.509
## 1630 1000   498   502 0.498
## 1631 1000   517   483 0.517
## 1632 1000   497   503 0.497
## 1633 1000   519   481 0.519
## 1634 1000   493   507 0.493
## 1635 1000   500   500 0.500
## 1636 1000   501   499 0.501
## 1637 1000   486   514 0.486
## 1638 1000   502   498 0.502
## 1639 1000   500   500 0.500
## 1640 1000   505   495 0.505
## 1641 1000   464   536 0.464
## 1642 1000   500   500 0.500
## 1643 1000   502   498 0.502
## 1644 1000   488   512 0.488
## 1645 1000   480   520 0.480
## 1646 1000   491   509 0.491
## 1647 1000   529   471 0.529
## 1648 1000   490   510 0.490
## 1649 1000   487   513 0.487
## 1650 1000   494   506 0.494
## 1651 1000   527   473 0.527
## 1652 1000   493   507 0.493
## 1653 1000   512   488 0.512
## 1654 1000   512   488 0.512
## 1655 1000   481   519 0.481
## 1656 1000   486   514 0.486
## 1657 1000   459   541 0.459
## 1658 1000   487   513 0.487
## 1659 1000   481   519 0.481
## 1660 1000   544   456 0.544
## 1661 1000   479   521 0.479
## 1662 1000   513   487 0.513
## 1663 1000   501   499 0.501
## 1664 1000   480   520 0.480
## 1665 1000   489   511 0.489
## 1666 1000   491   509 0.491
## 1667 1000   503   497 0.503
## 1668 1000   527   473 0.527
## 1669 1000   506   494 0.506
## 1670 1000   487   513 0.487
## 1671 1000   506   494 0.506
## 1672 1000   506   494 0.506
## 1673 1000   485   515 0.485
## 1674 1000   525   475 0.525
## 1675 1000   520   480 0.520
## 1676 1000   490   510 0.490
## 1677 1000   508   492 0.508
## 1678 1000   488   512 0.488
## 1679 1000   505   495 0.505
## 1680 1000   485   515 0.485
## 1681 1000   508   492 0.508
## 1682 1000   473   527 0.473
## 1683 1000   503   497 0.503
## 1684 1000   526   474 0.526
## 1685 1000   496   504 0.496
## 1686 1000   524   476 0.524
## 1687 1000   498   502 0.498
## 1688 1000   540   460 0.540
## 1689 1000   486   514 0.486
## 1690 1000   491   509 0.491
## 1691 1000   499   501 0.499
## 1692 1000   521   479 0.521
## 1693 1000   496   504 0.496
## 1694 1000   501   499 0.501
## 1695 1000   485   515 0.485
## 1696 1000   482   518 0.482
## 1697 1000   510   490 0.510
## 1698 1000   488   512 0.488
## 1699 1000   499   501 0.499
## 1700 1000   486   514 0.486
## 1701 1000   496   504 0.496
## 1702 1000   504   496 0.504
## 1703 1000   499   501 0.499
## 1704 1000   484   516 0.484
## 1705 1000   489   511 0.489
## 1706 1000   491   509 0.491
## 1707 1000   515   485 0.515
## 1708 1000   476   524 0.476
## 1709 1000   508   492 0.508
## 1710 1000   485   515 0.485
## 1711 1000   483   517 0.483
## 1712 1000   529   471 0.529
## 1713 1000   552   448 0.552
## 1714 1000   483   517 0.483
## 1715 1000   511   489 0.511
## 1716 1000   479   521 0.479
## 1717 1000   496   504 0.496
## 1718 1000   511   489 0.511
## 1719 1000   530   470 0.530
## 1720 1000   501   499 0.501
## 1721 1000   505   495 0.505
## 1722 1000   527   473 0.527
## 1723 1000   495   505 0.495
## 1724 1000   496   504 0.496
## 1725 1000   494   506 0.494
## 1726 1000   486   514 0.486
## 1727 1000   495   505 0.495
## 1728 1000   503   497 0.503
## 1729 1000   493   507 0.493
## 1730 1000   475   525 0.475
## 1731 1000   493   507 0.493
## 1732 1000   501   499 0.501
## 1733 1000   511   489 0.511
## 1734 1000   487   513 0.487
## 1735 1000   480   520 0.480
## 1736 1000   471   529 0.471
## 1737 1000   482   518 0.482
## 1738 1000   527   473 0.527
## 1739 1000   494   506 0.494
## 1740 1000   500   500 0.500
## 1741 1000   527   473 0.527
## 1742 1000   521   479 0.521
## 1743 1000   498   502 0.498
## 1744 1000   487   513 0.487
## 1745 1000   488   512 0.488
## 1746 1000   534   466 0.534
## 1747 1000   492   508 0.492
## 1748 1000   491   509 0.491
## 1749 1000   516   484 0.516
## 1750 1000   496   504 0.496
## 1751 1000   496   504 0.496
## 1752 1000   497   503 0.497
## 1753 1000   508   492 0.508
## 1754 1000   488   512 0.488
## 1755 1000   526   474 0.526
## 1756 1000   495   505 0.495
## 1757 1000   510   490 0.510
## 1758 1000   504   496 0.504
## 1759 1000   496   504 0.496
## 1760 1000   501   499 0.501
## 1761 1000   562   438 0.562
## 1762 1000   505   495 0.505
## 1763 1000   493   507 0.493
## 1764 1000   513   487 0.513
## 1765 1000   506   494 0.506
## 1766 1000   517   483 0.517
## 1767 1000   499   501 0.499
## 1768 1000   489   511 0.489
## 1769 1000   488   512 0.488
## 1770 1000   516   484 0.516
## 1771 1000   479   521 0.479
## 1772 1000   494   506 0.494
## 1773 1000   506   494 0.506
## 1774 1000   497   503 0.497
## 1775 1000   485   515 0.485
## 1776 1000   482   518 0.482
## 1777 1000   518   482 0.518
## 1778 1000   483   517 0.483
## 1779 1000   496   504 0.496
## 1780 1000   480   520 0.480
## 1781 1000   487   513 0.487
## 1782 1000   511   489 0.511
## 1783 1000   507   493 0.507
## 1784 1000   474   526 0.474
## 1785 1000   506   494 0.506
## 1786 1000   493   507 0.493
## 1787 1000   497   503 0.497
## 1788 1000   507   493 0.507
## 1789 1000   535   465 0.535
## 1790 1000   501   499 0.501
## 1791 1000   514   486 0.514
## 1792 1000   528   472 0.528
## 1793 1000   486   514 0.486
## 1794 1000   482   518 0.482
## 1795 1000   484   516 0.484
## 1796 1000   503   497 0.503
## 1797 1000   528   472 0.528
## 1798 1000   507   493 0.507
## 1799 1000   478   522 0.478
## 1800 1000   536   464 0.536
## 1801 1000   500   500 0.500
## 1802 1000   489   511 0.489
## 1803 1000   527   473 0.527
## 1804 1000   487   513 0.487
## 1805 1000   515   485 0.515
## 1806 1000   481   519 0.481
## 1807 1000   496   504 0.496
## 1808 1000   489   511 0.489
## 1809 1000   524   476 0.524
## 1810 1000   513   487 0.513
## 1811 1000   503   497 0.503
## 1812 1000   493   507 0.493
## 1813 1000   495   505 0.495
## 1814 1000   506   494 0.506
## 1815 1000   513   487 0.513
## 1816 1000   485   515 0.485
## 1817 1000   498   502 0.498
## 1818 1000   483   517 0.483
## 1819 1000   502   498 0.502
## 1820 1000   501   499 0.501
## 1821 1000   498   502 0.498
## 1822 1000   505   495 0.505
## 1823 1000   495   505 0.495
## 1824 1000   517   483 0.517
## 1825 1000   504   496 0.504
## 1826 1000   499   501 0.499
## 1827 1000   496   504 0.496
## 1828 1000   499   501 0.499
## 1829 1000   481   519 0.481
## 1830 1000   496   504 0.496
## 1831 1000   488   512 0.488
## 1832 1000   492   508 0.492
## 1833 1000   495   505 0.495
## 1834 1000   528   472 0.528
## 1835 1000   520   480 0.520
## 1836 1000   516   484 0.516
## 1837 1000   496   504 0.496
## 1838 1000   493   507 0.493
## 1839 1000   511   489 0.511
## 1840 1000   491   509 0.491
## 1841 1000   469   531 0.469
## 1842 1000   487   513 0.487
## 1843 1000   490   510 0.490
## 1844 1000   475   525 0.475
## 1845 1000   491   509 0.491
## 1846 1000   510   490 0.510
## 1847 1000   491   509 0.491
## 1848 1000   512   488 0.512
## 1849 1000   503   497 0.503
## 1850 1000   485   515 0.485
## 1851 1000   508   492 0.508
## 1852 1000   497   503 0.497
## 1853 1000   512   488 0.512
## 1854 1000   511   489 0.511
## 1855 1000   506   494 0.506
## 1856 1000   516   484 0.516
## 1857 1000   499   501 0.499
## 1858 1000   499   501 0.499
## 1859 1000   490   510 0.490
## 1860 1000   488   512 0.488
## 1861 1000   499   501 0.499
## 1862 1000   522   478 0.522
## 1863 1000   464   536 0.464
## 1864 1000   487   513 0.487
## 1865 1000   512   488 0.512
## 1866 1000   504   496 0.504
## 1867 1000   504   496 0.504
## 1868 1000   501   499 0.501
## 1869 1000   526   474 0.526
## 1870 1000   534   466 0.534
## 1871 1000   503   497 0.503
## 1872 1000   496   504 0.496
## 1873 1000   497   503 0.497
## 1874 1000   517   483 0.517
## 1875 1000   508   492 0.508
## 1876 1000   501   499 0.501
## 1877 1000   482   518 0.482
## 1878 1000   498   502 0.498
## 1879 1000   510   490 0.510
## 1880 1000   503   497 0.503
## 1881 1000   502   498 0.502
## 1882 1000   476   524 0.476
## 1883 1000   507   493 0.507
## 1884 1000   500   500 0.500
## 1885 1000   493   507 0.493
## 1886 1000   507   493 0.507
## 1887 1000   500   500 0.500
## 1888 1000   509   491 0.509
## 1889 1000   510   490 0.510
## 1890 1000   500   500 0.500
## 1891 1000   512   488 0.512
## 1892 1000   527   473 0.527
## 1893 1000   484   516 0.484
## 1894 1000   458   542 0.458
## 1895 1000   497   503 0.497
## 1896 1000   502   498 0.502
## 1897 1000   496   504 0.496
## 1898 1000   505   495 0.505
## 1899 1000   513   487 0.513
## 1900 1000   543   457 0.543
## 1901 1000   506   494 0.506
## 1902 1000   508   492 0.508
## 1903 1000   528   472 0.528
## 1904 1000   472   528 0.472
## 1905 1000   492   508 0.492
## 1906 1000   493   507 0.493
## 1907 1000   482   518 0.482
## 1908 1000   501   499 0.501
## 1909 1000   504   496 0.504
## 1910 1000   504   496 0.504
## 1911 1000   499   501 0.499
## 1912 1000   491   509 0.491
## 1913 1000   507   493 0.507
## 1914 1000   463   537 0.463
## 1915 1000   499   501 0.499
## 1916 1000   486   514 0.486
## 1917 1000   483   517 0.483
## 1918 1000   515   485 0.515
## 1919 1000   475   525 0.475
## 1920 1000   495   505 0.495
## 1921 1000   495   505 0.495
## 1922 1000   504   496 0.504
## 1923 1000   484   516 0.484
## 1924 1000   523   477 0.523
## 1925 1000   491   509 0.491
## 1926 1000   472   528 0.472
## 1927 1000   498   502 0.498
## 1928 1000   514   486 0.514
## 1929 1000   473   527 0.473
## 1930 1000   485   515 0.485
## 1931 1000   502   498 0.502
## 1932 1000   491   509 0.491
## 1933 1000   499   501 0.499
## 1934 1000   498   502 0.498
## 1935 1000   492   508 0.492
## 1936 1000   502   498 0.502
## 1937 1000   477   523 0.477
## 1938 1000   518   482 0.518
## 1939 1000   520   480 0.520
## 1940 1000   469   531 0.469
## 1941 1000   500   500 0.500
## 1942 1000   509   491 0.509
## 1943 1000   482   518 0.482
## 1944 1000   519   481 0.519
## 1945 1000   488   512 0.488
## 1946 1000   488   512 0.488
## 1947 1000   517   483 0.517
## 1948 1000   510   490 0.510
## 1949 1000   519   481 0.519
## 1950 1000   486   514 0.486
## 1951 1000   496   504 0.496
## 1952 1000   503   497 0.503
## 1953 1000   503   497 0.503
## 1954 1000   528   472 0.528
## 1955 1000   506   494 0.506
## 1956 1000   484   516 0.484
## 1957 1000   504   496 0.504
## 1958 1000   494   506 0.494
## 1959 1000   492   508 0.492
## 1960 1000   487   513 0.487
## 1961 1000   518   482 0.518
## 1962 1000   475   525 0.475
## 1963 1000   498   502 0.498
## 1964 1000   473   527 0.473
## 1965 1000   509   491 0.509
## 1966 1000   459   541 0.459
## 1967 1000   508   492 0.508
## 1968 1000   499   501 0.499
## 1969 1000   514   486 0.514
## 1970 1000   511   489 0.511
## 1971 1000   504   496 0.504
## 1972 1000   490   510 0.490
## 1973 1000   518   482 0.518
## 1974 1000   487   513 0.487
## 1975 1000   498   502 0.498
## 1976 1000   515   485 0.515
## 1977 1000   521   479 0.521
## 1978 1000   492   508 0.492
## 1979 1000   522   478 0.522
## 1980 1000   498   502 0.498
## 1981 1000   510   490 0.510
## 1982 1000   495   505 0.495
## 1983 1000   529   471 0.529
## 1984 1000   483   517 0.483
## 1985 1000   505   495 0.505
## 1986 1000   497   503 0.497
## 1987 1000   493   507 0.493
## 1988 1000   491   509 0.491
## 1989 1000   525   475 0.525
## 1990 1000   490   510 0.490
## 1991 1000   498   502 0.498
## 1992 1000   524   476 0.524
## 1993 1000   506   494 0.506
## 1994 1000   485   515 0.485
## 1995 1000   502   498 0.502
## 1996 1000   491   509 0.491
## 1997 1000   479   521 0.479
## 1998 1000   524   476 0.524
## 1999 1000   505   495 0.505
## 2000 1000   507   493 0.507
```


```r
mean(coin_flips_2000_1000$heads)
```

```
## [1] 499.9055
```


```r
ggplot(coin_flips_2000_1000, aes(x = heads)) +
    geom_histogram(binwidth = 10, boundary = 500)
```

<img src="08-intro_to_randomization_1-web_files/figure-html/unnamed-chunk-19-1.png" width="672" />

And now the same histogram, but with proportions:


```r
ggplot(coin_flips_2000_1000, aes(x = prop)) +
    geom_histogram(binwidth = 0.01, boundary = 0.5)
```

<img src="08-intro_to_randomization_1-web_files/figure-html/unnamed-chunk-20-1.png" width="672" />


##### Exercise 4 {-}

Comment on the histogram above. Describe its shape using the vocabulary of the three important features (modes, symmetry, outliers). Why do you think it's shaped like this?

::: {.answer}

Please write up your answer here.

:::

##### Exercise 5 {-}

Given the amount of randomness involved (each person is tossing coins which randomly come up heads or tails), why do we see so much structure and orderliness in the histograms?

::: {.answer}

Please write up your answer here.

:::


## But who cares about coin flips? {#randomization1-who-cares}

It's fair to ask why we go to all this trouble to talk about coin flips. The most pressing research questions of our day do not involve people sitting around and flipping coins, either physically or virtually.

But now substitute "heads" and "tails" with "cancer" and "no cancer". Or "guilty" and "not guilty". Or "shot" and "not shot". The fact is that many important issues are measured as variables with two possible outcomes. There is some underlying "probability" of seeing one outcome over the other. (It doesn't have to be 50% like the coin.) Statistical methods---including simulation---can say a lot about what we "expect" to see if these outcomes are truly random. More importantly, when we see outcomes that *aren't* consistent with our simulations, we may wonder if there is some underlying mechanism that may be not so random after all. It may not look like it on first blush, but this idea is at the core of the scientific method.

For example, let's suppose that 85% of U.S. adults support some form of background checks for gun buyers.^[This is likely close to the truth. See this article: https://iop.harvard.edu/get-involved/harvard-political-review/vast-majority-americans-support-universal-background-checks] Now, imagine we went out and surveyed a random group of people and asked them a simple yes/no question about their support for background checks. What might we see?

Let's simulate. Imagine flipping a coin, but instead of coming up heads 50% of the time, suppose it were possible for the coin to come up heads 85% of the time.^[The idea of a "weighted" coin that can do this comes up all the time in probability and statistics courses, but it seems that it's not likely one could actually manufacture a coin that came up heads more or less than 50% of the time when flipped. See this paper for more details: http://www.stat.columbia.edu/~gelman/research/published/diceRev2.pdf] A sequence of heads and tails with this weird coin would be much like randomly surveying people and asking them about background checks.

We can make a "virtual" weird coin with the `rflip` command by specifying how often we want heads to come up.


```r
set.seed(1234)
rflip(1, prob = 0.85)
```

```
## 
## Flipping 1 coin [ Prob(Heads) = 0.85 ] ...
## 
## H
## 
## Number of Heads: 1 [Proportion Heads: 1]
```

If we flip our weird coin a bunch of times, we can see that our coin is not fair. Indeed, it appears to come up heads way more often than not:


```r
set.seed(1234)
rflip(100, prob = 0.85)
```

```
## 
## Flipping 100 coins [ Prob(Heads) = 0.85 ] ...
## 
## H H H H T H H H H H H H H T H H H H H H H H H H H H H T H H H H H H H H
## H H T H H H H H H H H H H H H H H H H H H H H H T H H H H H H H H H H T
## H H H H H H H H T H H H H T H H H T H T H H H H H H H H
## 
## Number of Heads: 90 [Proportion Heads: 0.9]
```

The results from the above code can be thought of as a survey of 100 random U.S. adults about their support for background checks for purchasing guns. "Heads" means "supports" and "tails" means "opposes." If the majority of Americans support background checks, then we will come across more people in our survey who tell us they support background checks. This shows up in our simulation as the appearance of more heads than tails.

Note that there is no guarantee that our sample will have exactly 85% heads. In fact, it doesn't; it has 90% heads.

Again, keep in mind that we're simulating the act of obtaining a random sample of 100 U.S. adults. If we get a different sample, we'll get different results. (We set a different seed here. That ensures that this code chunk is randomly different from the one above.)


```r
set.seed(123456)
rflip(100, prob = 0.85)
```

```
## 
## Flipping 100 coins [ Prob(Heads) = 0.85 ] ...
## 
## H H H H H H H H T H H H T T T T T H H H H H H H H H T T T H H T H H H H
## T T H H H H T H H H H H H H H H H T H T H H H H H H H H H H H H H H H H
## T H H H T H H H H H H T H H H H H H H H H H H H T H H H
## 
## Number of Heads: 81 [Proportion Heads: 0.81]
```

See, this time, only 81% came up heads, even though we expected 85%. That's how randomness works.

##### Exercise 6(a) {-}

Now imagine that 2000 people all go out and conduct surveys of 100 random U.S. adults, asking them about their support for background checks. Write some R code that simulates this. Plot a histogram of the results. (Hint: you'll need `do(2000) *` in there.) Use the proportion of supporters (`prop`), not the raw count of supporters (`heads`).

::: {.answer}


```r
set.seed(1234)
# Add code here to simulate 2000 surveys of 100 U.S. adults.
```


```r
# Plot the results in a histogram using proportions.
```

:::

##### Exercise 6(b) {-}

Run another simulation, but this time, have each person survey 1000 adults and not just 100.

::: {.answer}


```r
set.seed(1234)
# Add code here to simulate 2000 surveys of 1000 U.S. adults.
```


```r
# Plot the results in a histogram using proportions.
```

:::

##### Exercise 6(c) {-}

What changed when you surveyed 1000 people instead of 100?

::: {.answer}

Please write up your answer here.

:::


## Sampling variability {#randomization1-sampling-var}

We've seen that taking repeated samples (using the `do` command) leads to lots of different outcomes. That is randomness in action. We don't expect the results of each survey to be exactly the same every time the survey is administered.

But despite this randomness, there is an interesting pattern that we can observe. It has to do with the number of times we flip the coin. Since we're using coin flips to simulate the act of conducting a survey, the number of coin flips is playing the role of the *sample size*. In other words, if we want to simulate a survey of U.S. adults with a sample size of 100, we simulate that by flipping 100 coins.

##### Exercise 7 {-}

Go back and look at all the examples above. What do you notice about the range of values on the x-axis when the sample size is small versus large? (In other words, in what way are the histograms different when using `rflip(10, prob = ...)` or `rflip(100, prob = ...)` versus `rflip(1000, prob = ...)`? It's easier to compare histograms one to another when looking at the proportions instead of the raw head counts because proportions are always on the same scale from 0 to 1.)

::: {.answer}

Please write up your answer here.

:::


## Conclusion {#randomization1-conclusion}

Simulation is a tool for understanding what happens when a statistical process is repeated many times in a randomized way. The availability of fast computer processing makes simulation easy and accessible. Eventually, the goal will be to use simulation to answer important questions about data and the processes in the world that generate data. This is possible because, despite the ubiquitous presence of randomness, a certain order emerges when the number of samples is large enough. Even though there is sampling variability (different random outcomes each time we sample), there are patterns in that variability that can be exploited to make predictions.

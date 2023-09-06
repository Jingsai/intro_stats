# Manipulating data {#manipulating}

<!-- Please don't mess with the next few lines! -->
<style>h5{font-size:2em;color:#0000FF}h6{font-size:1.5em;color:#0000FF}div.answer{margin-left:5%;border:1px solid #0000FF;border-left-width:10px;padding:25px} div.summary{background-color:rgba(30,144,255,0.1);border:3px double #0000FF;padding:25px}</style><p style="color:#ffffff">2.0</p>
<!-- Please don't mess with the previous few lines! -->

::: {.summary}

### Functions introduced in this chapter {-}

`read_csv`, `select`, `rename`, `rm`, `filter`, `slice`, `arrange`, `mutate`, `all.equal`, `ifelse`, `transmute`, `summarise`, `group_by`, `%>%`, `count`

:::


## Introduction {#manipulating-intro}

This tutorial will import some data from the web and then explore it using the amazing `dplyr` package, a package which is quickly becoming the *de facto* standard among R users for manipulating data. It's part of the `tidyverse` that we've already used in several chapters.

### Install new packages {#manipulating-install}

There are no new packages used in this chapter.

### Download the R notebook file {#manipulating-download}

Check the upper-right corner in RStudio to make sure you're in your `intro_stats` project. Then click on the following link to download this chapter as an R notebook file (`.Rmd`).

<a href = "https://jingsai.github.io/intro_stats/chapter_downloads/05-manipulating_data.Rmd" download>https://jingsai.github.io/intro_stats/chapter_downloads/05-manipulating_data.Rmd</a>

Once the file is downloaded, move it to your project folder in RStudio and open it there.

### Restart R and run all chunks {#manipulating-restart}

In RStudio, select "Restart R and Run All Chunks" from the "Run" menu.

### Load packages {#manipulating-load}

We load the `tidyverse` package as usual, but this time it is to give us access to the `dplyr` package, which is loaded alongside our other `tidyverse` packages like `ggplot2`. The `tidyverse` also has a package called `readr` that will allow us to import data from an external source (in this case, a web site).


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


## Importing CSV data {#manipulating-csv}

For most of the chapters, we use data sets that are either included in base R or included in a package that can be loaded into R. But it is useful to see how to get a data set from outside the R ecosystem. This depends a lot on the format of the data file, but a common format is a "comma-separated values" file, or CSV file. If you have a data set that is not formatted as a CSV file, it is usually pretty easy to open it in something like Google Spreadsheets or Microsoft Excel and then re-save it as a CSV file.

The file we'll import is a random sample from all the commercial domestic flights that departed from Houston, Texas, in 2011.

We use the `read_csv` command to import a CSV file. In this case, we're grabbing the file from a web page where the file is hosted. If you have a file on your computer, you can also put the file into your project directory and import it from there. Put the URL (for a web page) or the filename (for a file in your project directory) in quotes inside the `read_csv`command. We also need to assign the output to a tibble, so we've called it `hf` for "Houston flights".


```r
hf <- read_csv("https://jingsai.github.io/intro_stats/data/hf.csv")
```

```
## Rows: 22758 Columns: 21
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr  (5): UniqueCarrier, TailNum, Origin, Dest, CancellationCode
## dbl (16): Year, Month, DayofMonth, DayOfWeek, DepTime, ArrTime, FlightNum, A...
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```


```r
hf
```

```
## # A tibble: 22,758 × 21
##     Year Month DayofMo…¹ DayOf…² DepTime ArrTime Uniqu…³ Fligh…⁴ TailNum Actua…⁵
##    <dbl> <dbl>     <dbl>   <dbl>   <dbl>   <dbl> <chr>     <dbl> <chr>     <dbl>
##  1  2011     1        12       3    1419    1515 AA          428 N577AA       56
##  2  2011     1        17       1    1530    1634 AA          428 N518AA       64
##  3  2011     1        24       1    1356    1513 AA          428 N531AA       77
##  4  2011     1         9       7     714     829 AA          460 N586AA       75
##  5  2011     1        18       2     721     827 AA          460 N558AA       66
##  6  2011     1        22       6     717     829 AA          460 N499AA       72
##  7  2011     1        11       2    1953    2051 AA          533 N505AA       58
##  8  2011     1        14       5    2119    2229 AA          533 N549AA       70
##  9  2011     1        26       3    2009    2103 AA          533 N403AA       54
## 10  2011     1        14       5    1629    1734 AA         1121 N551AA       65
## # … with 22,748 more rows, 11 more variables: AirTime <dbl>, ArrDelay <dbl>,
## #   DepDelay <dbl>, Origin <chr>, Dest <chr>, Distance <dbl>, TaxiIn <dbl>,
## #   TaxiOut <dbl>, Cancelled <dbl>, CancellationCode <chr>, Diverted <dbl>, and
## #   abbreviated variable names ¹​DayofMonth, ²​DayOfWeek, ³​UniqueCarrier,
## #   ⁴​FlightNum, ⁵​ActualElapsedTime
```


```r
glimpse(hf)
```

```
## Rows: 22,758
## Columns: 21
## $ Year              <dbl> 2011, 2011, 2011, 2011, 2011, 2011, 2011, 2011, 2011…
## $ Month             <dbl> 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1…
## $ DayofMonth        <dbl> 12, 17, 24, 9, 18, 22, 11, 14, 26, 14, 18, 20, 3, 12…
## $ DayOfWeek         <dbl> 3, 1, 1, 7, 2, 6, 2, 5, 3, 5, 2, 4, 1, 3, 6, 4, 1, 3…
## $ DepTime           <dbl> 1419, 1530, 1356, 714, 721, 717, 1953, 2119, 2009, 1…
## $ ArrTime           <dbl> 1515, 1634, 1513, 829, 827, 829, 2051, 2229, 2103, 1…
## $ UniqueCarrier     <chr> "AA", "AA", "AA", "AA", "AA", "AA", "AA", "AA", "AA"…
## $ FlightNum         <dbl> 428, 428, 428, 460, 460, 460, 533, 533, 533, 1121, 1…
## $ TailNum           <chr> "N577AA", "N518AA", "N531AA", "N586AA", "N558AA", "N…
## $ ActualElapsedTime <dbl> 56, 64, 77, 75, 66, 72, 58, 70, 54, 65, 135, 144, 64…
## $ AirTime           <dbl> 41, 48, 43, 51, 46, 47, 44, 45, 39, 47, 114, 111, 46…
## $ ArrDelay          <dbl> 5, 84, 3, -6, -8, -6, -29, 69, -17, -11, 39, -1, -2,…
## $ DepDelay          <dbl> 19, 90, -4, -6, 1, -3, -12, 74, 4, -1, 44, -5, -1, 1…
## $ Origin            <chr> "IAH", "IAH", "IAH", "IAH", "IAH", "IAH", "IAH", "IA…
## $ Dest              <chr> "DFW", "DFW", "DFW", "DFW", "DFW", "DFW", "DFW", "DF…
## $ Distance          <dbl> 224, 224, 224, 224, 224, 224, 224, 224, 224, 224, 96…
## $ TaxiIn            <dbl> 4, 8, 6, 11, 7, 18, 3, 5, 9, 8, 7, 20, 5, 8, 8, 7, 4…
## $ TaxiOut           <dbl> 11, 8, 28, 13, 13, 7, 11, 20, 6, 10, 14, 13, 13, 10,…
## $ Cancelled         <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0…
## $ CancellationCode  <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ Diverted          <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0…
```

The one disadvantage of a file imported from the internet or your computer is that it does not come with a help file. (Only packages in R have help files.) Hopefully you have access to some kind of information about the data you're importing. In this case, we get lucky because the full Houston flights data set happens to be available in a package called `hflights`.

##### Exercise 1 {-}

Go to the help tab in RStudio and search for `hflights`. Of the several options that appear, click the one from the `hflights` package (listed as `hflights::hflights`). Review the help file so you know what all the variables mean. Report below how many cases are in the original `hflights` data. What fraction of the original data has been sampled in the CSV file we imported above?

::: {.answer}

Please write up your answer here.

:::


## Introduction to `dplyr` {#manipulating-dplyr}

The `dplyr` package (pronounced "dee-ply-er") contains tools for manipulating the rows and columns of tibbles. The key to using `dplyr` is to familiarize yourself with the "key verbs":

- `select` (and `rename`)
- `filter` (and `slice`)
- `arrange`
- `mutate` (and `transmute`)
- `summarise` (with `group_by`)

We'll consider these one by one. We won't have time to cover every aspect of these functions. More information appears in the help files, as well as this very helpful "cheat sheet": [https://raw.githubusercontent.com/rstudio/cheatsheets/main/data-transformation.pdf](https://raw.githubusercontent.com/rstudio/cheatsheets/main/data-transformation.pdf)


## `select` {#manipulating-select}

The `select` verb is very easy. It just selects some subset of variables (the columns of your data set).

The `select` command from the `dplyr` package illustrates one of the common issues R users face. Because the word "select" is pretty common, and selecting things is a common task, there are multiple packages that have a function called `select`. Depending on the order in which packages were loaded, R might get confused as to which version of `select` you want and try to apply the wrong one. One way to get the correct version is to specify the package in the syntax. Instead of typing `select`, we can type `dplyr::select` to ensure we get the version from the `dplyr` package. We'll do this in all future uses of the `select` function. (The other functions in this chapter don't cause us trouble because we don't use any other packages whose functions conflict like this.)

Suppose all we wanted to see was the carrier, origin, and destination. We would type


```r
hf_select <- dplyr::select(hf, UniqueCarrier, Origin, Dest)
hf_select
```

```
## # A tibble: 22,758 × 3
##    UniqueCarrier Origin Dest 
##    <chr>         <chr>  <chr>
##  1 AA            IAH    DFW  
##  2 AA            IAH    DFW  
##  3 AA            IAH    DFW  
##  4 AA            IAH    DFW  
##  5 AA            IAH    DFW  
##  6 AA            IAH    DFW  
##  7 AA            IAH    DFW  
##  8 AA            IAH    DFW  
##  9 AA            IAH    DFW  
## 10 AA            IAH    DFW  
## # … with 22,748 more rows
```

A brief but important aside here: there is nothing special about the variable name `hf_select`. I could have typed

`beef_gravy <- dplyr::select(hf, UniqueCarrier, Origin, Dest)`

and it would work just as well. Generally speaking, though, you want to give variables a name that reflects the intent of your analysis.

Also, **it is important to assign the result to a new variable**. If I had typed

`hf <- dplyr::select(hf, UniqueCarrier, Origin, Dest)`

this would have overwritten the original tibble `hf` with this new version with only three variables. I want to preserve `hf` because I want to do other things with the entire data set later. The take-home message here is this: **Major modifications to your data should generally be given a new variable name.** There are caveats here, though. Every time you create a new variable, you also fill up more memory with your creation. If you check your Global Environment, you'll see that both `hf` and `hf_select` are sitting in there. We'll have more to say about this in a moment.

Okay, back to the `select` function. The first argument of `select` is the tibble. After that, just list all the names of the variables you want to select.

If you don't like the names of the variables, you can change them as part of the select process.


```r
hf_select <- dplyr::select(hf,
                           carrier = UniqueCarrier,
                           origin = Origin,
                           dest = Dest)
hf_select
```

```
## # A tibble: 22,758 × 3
##    carrier origin dest 
##    <chr>   <chr>  <chr>
##  1 AA      IAH    DFW  
##  2 AA      IAH    DFW  
##  3 AA      IAH    DFW  
##  4 AA      IAH    DFW  
##  5 AA      IAH    DFW  
##  6 AA      IAH    DFW  
##  7 AA      IAH    DFW  
##  8 AA      IAH    DFW  
##  9 AA      IAH    DFW  
## 10 AA      IAH    DFW  
## # … with 22,748 more rows
```

(Note here that I am overwriting `hf_select` which had been defined slightly differently before. However, these two versions of `hf_select` are basically the same object, so no need to keep two copies here.)

There are a few notational shortcuts. For example, see what the following do.


```r
hf_select2 <- dplyr::select(hf, DayOfWeek:UniqueCarrier)
hf_select2
```

```
## # A tibble: 22,758 × 4
##    DayOfWeek DepTime ArrTime UniqueCarrier
##        <dbl>   <dbl>   <dbl> <chr>        
##  1         3    1419    1515 AA           
##  2         1    1530    1634 AA           
##  3         1    1356    1513 AA           
##  4         7     714     829 AA           
##  5         2     721     827 AA           
##  6         6     717     829 AA           
##  7         2    1953    2051 AA           
##  8         5    2119    2229 AA           
##  9         3    2009    2103 AA           
## 10         5    1629    1734 AA           
## # … with 22,748 more rows
```


```r
hf_select3 <- dplyr::select(hf, starts_with("Taxi"))
hf_select3
```

```
## # A tibble: 22,758 × 2
##    TaxiIn TaxiOut
##     <dbl>   <dbl>
##  1      4      11
##  2      8       8
##  3      6      28
##  4     11      13
##  5      7      13
##  6     18       7
##  7      3      11
##  8      5      20
##  9      9       6
## 10      8      10
## # … with 22,748 more rows
```

##### Exercise 2 {-}

What is contained in the new tibbles `hf_select2` and `hf_select3`? In other words, what does the colon (:) appear to do and what does `starts_with` appear to do in the `select` function?

::: {.answer}

Please write up your answer here.

:::

*****

The cheat sheet shows a lot more of these "helper functions" if you're interested.

The other command that's related to `select` is `rename`. The only difference is that `select` will throw away any columns you don't select (which is what you want and expect, typically), whereas `rename` will keep all the columns, but rename those you designate.

##### Exercise 3 {-}

Putting a minus sign in front of a variable name in the `select` command will remove the variable. Create a tibble called `hf_select4` that removes `Year`, `DayofMonth`, `DayOfWeek`, `FlightNum`, and `Diverted`. (Be careful with the unusual---and inconsistent!---capitalization in those variable names.) In the second part of the code chunk below, type `hf_select4` so that the tibble prints to the screen (just like in all the above examples).

::: {.answer}


```r
# Add code here to define hf_select4.
# Add code here to print hf_select4.
```

:::


## The `rm` command {#manipulating-rm}

Recall that earlier we mentioned the pros and cons of creating a new tibble every time we make a change. On one hand, making a new tibble instead of overwriting the original one will keep the original one available so that we can run different commands on it. On the other hand, making a new tibble does eat up a lot of memory.

One way to get rid of an object once we are done with it is the `rm` command, where `rm` is short for "remove". When you run the code chunk below, you'll see that all the tibbles we created with `select` will disappear from your Global Environment.


```r
rm(hf_select, hf_select2, hf_select3)
```

If you need one these tibbles back later, you can always go back and re-run the code chunk that defined it.

We'll use `rm` at the end of some of the following sections so that we don't use up too much memory.

##### Exercise 4 {-}

Remove `hf_select4` (that you created in Exercise 3) from the Global Environment.

::: {.answer}


```r
# Add code here to remove hf_select4.
```

:::


## `filter` {#manipulating-filter}

The `filter` verb works a lot like `select`, but for rows instead of columns.

For example, let's say we only want to see Delta flights. We use `filter`:


```r
hf_filter <- filter(hf, UniqueCarrier == "DL")
hf_filter
```

```
## # A tibble: 265 × 21
##     Year Month DayofMo…¹ DayOf…² DepTime ArrTime Uniqu…³ Fligh…⁴ TailNum Actua…⁵
##    <dbl> <dbl>     <dbl>   <dbl>   <dbl>   <dbl> <chr>     <dbl> <chr>     <dbl>
##  1  2011     1         4       2    1834    2134 DL           54 N969DL      120
##  2  2011     1         5       3    1606    1903 DL            8 N985DL      117
##  3  2011     1         5       3     543     834 DL         1248 N332NB      111
##  4  2011     1         7       5    1603    1902 DL            8 N958DL      119
##  5  2011     1         7       5    1245    1539 DL         1204 N907DE      114
##  6  2011     1         7       5     933    1225 DL         1590 N768NC      112
##  7  2011     1         8       6     921    1210 DL         1590 N770NC      109
##  8  2011     1        12       3      NA      NA DL         1590 N776NC       NA
##  9  2011     1        13       4     928    1224 DL         1590 N760NC      116
## 10  2011     1        13       4     656     947 DL         1900 N977DL      111
## # … with 255 more rows, 11 more variables: AirTime <dbl>, ArrDelay <dbl>,
## #   DepDelay <dbl>, Origin <chr>, Dest <chr>, Distance <dbl>, TaxiIn <dbl>,
## #   TaxiOut <dbl>, Cancelled <dbl>, CancellationCode <chr>, Diverted <dbl>, and
## #   abbreviated variable names ¹​DayofMonth, ²​DayOfWeek, ³​UniqueCarrier,
## #   ⁴​FlightNum, ⁵​ActualElapsedTime
```

In the printout of the tibble above, if you can't see the `UniqueCarrier` column, click the black arrow on the right to scroll through the columns until you can see it. You can click "Next" at the bottom to scroll through the rows.

##### Exercise 5 {-}

How many rows did we get in the `hf_filter` tibble? What do you notice about the `UniqueCarrier` of all those rows?

::: {.answer}

Please write up your answer here.

:::

*****

Just like `select`, the first argument of `filter` is the name of the tibble. Following that, you must specify some condition. Only rows meeting that condition will be included in the output.

One thing that is unusual here is the double equal sign (`UniqueCarrier == "DL"`). This won't be a mystery to people with programming experience, but it tends to be a sticking point for the rest of us. A single equals sign represents assignment. If I type `x = 3`, what I mean is, "Take the letter x and assign it the value 3." In R, we would also write `x <- 3` to mean the same thing. The first line of the code chunk below assigns `x` to be 3. Therefore, the following line that just says `x` creates the output "3".


```r
x = 3
x
```

```
## [1] 3
```

On the other hand, `x == 3` means something completely different. This is a logical statement that is either true or false. Either `x` is 3, in which case we get `TRUE` or `x` is not 3, and we get `FALSE`.


```r
x == 3
```

```
## [1] TRUE
```

(It's true because we just assigned `x` to be 3 in the previous code chunk!)

In the above `filter` command, we are saying, "Give me the rows where the value of `UniqueCarrier` is `"DL"`, or, in other words, where the statement `UniqueCarrier == "DL"` is true.

As another example, suppose we wanted to find out all flights that leave before 6:00 a.m.


```r
hf_filter2 <- filter(hf, DepTime < 600)
hf_filter2
```

```
## # A tibble: 230 × 21
##     Year Month DayofMo…¹ DayOf…² DepTime ArrTime Uniqu…³ Fligh…⁴ TailNum Actua…⁵
##    <dbl> <dbl>     <dbl>   <dbl>   <dbl>   <dbl> <chr>     <dbl> <chr>     <dbl>
##  1  2011     1        20       4     556     912 AA         1994 N3FTAA      136
##  2  2011     1        21       5     555     822 CO          446 N37252      147
##  3  2011     1        18       2     555     831 CO          446 N12216      156
##  4  2011     1        16       7     556     722 CO          199 N79279      146
##  5  2011     1         5       3     558    1009 CO           89 N16709      191
##  6  2011     1         1       6     558    1006 CO           89 N73406      188
##  7  2011     1         5       3     543     834 DL         1248 N332NB      111
##  8  2011     1         3       1     555     749 US          270 N156AW      174
##  9  2011     1         6       4     556     801 US          270 N313AW      185
## 10  2011     1        13       4     552     713 US          270 N334AW      141
## # … with 220 more rows, 11 more variables: AirTime <dbl>, ArrDelay <dbl>,
## #   DepDelay <dbl>, Origin <chr>, Dest <chr>, Distance <dbl>, TaxiIn <dbl>,
## #   TaxiOut <dbl>, Cancelled <dbl>, CancellationCode <chr>, Diverted <dbl>, and
## #   abbreviated variable names ¹​DayofMonth, ²​DayOfWeek, ³​UniqueCarrier,
## #   ⁴​FlightNum, ⁵​ActualElapsedTime
```

##### Exercise 6 {-}

Look at the help file for `hflights` again. Why do we have to use the number 600 in the command above? (Read the description of the `DepTime` variable.)

::: {.answer}

Please write up your answer here.

:::

*****

If we need two or more conditions, we use `&` for "and" and `|` for "or". The following will give us only the Delta flights that departed before 6:00 a.m.


```r
hf_filter3 <- filter(hf, UniqueCarrier == "DL" & DepTime < 600)
hf_filter3
```

```
## # A tibble: 30 × 21
##     Year Month DayofMo…¹ DayOf…² DepTime ArrTime Uniqu…³ Fligh…⁴ TailNum Actua…⁵
##    <dbl> <dbl>     <dbl>   <dbl>   <dbl>   <dbl> <chr>     <dbl> <chr>     <dbl>
##  1  2011     1         5       3     543     834 DL         1248 N332NB      111
##  2  2011     1        16       7     542     834 DL         1248 N364NB      112
##  3  2011     1        19       3     538     844 DL         1248 N370NB      126
##  4  2011     1        22       6     540     850 DL         1248 N315NB      130
##  5  2011     1        26       3     540     851 DL         1248 N321NB      131
##  6  2011     2        12       6     538     823 DL         1248 N361NB      105
##  7  2011     2        15       2     539     840 DL         1248 N327NB      121
##  8  2011     2        16       3     540     829 DL         1248 N326NB      109
##  9  2011     2        21       1     552     856 DL         1248 N779NC      124
## 10  2011     3         2       3     557     902 DL         2375 N777NC      125
## # … with 20 more rows, 11 more variables: AirTime <dbl>, ArrDelay <dbl>,
## #   DepDelay <dbl>, Origin <chr>, Dest <chr>, Distance <dbl>, TaxiIn <dbl>,
## #   TaxiOut <dbl>, Cancelled <dbl>, CancellationCode <chr>, Diverted <dbl>, and
## #   abbreviated variable names ¹​DayofMonth, ²​DayOfWeek, ³​UniqueCarrier,
## #   ⁴​FlightNum, ⁵​ActualElapsedTime
```

Again, check the cheat sheet for more complicated condition-checking if needed.

##### Exercise 7(a) {-}

The symbol `!=` means "not equal to" in R. Use the `filter` command to create a tibble called `hf_filter4` that finds all flights *except* those flying into Salt Lake City ("SLC"). As before, print the output to the screen.

::: {.answer}


```r
# Add code here to define hf_filter4.
# Add code here to print hf_filter4.
```

:::

##### Exercise 7(b) {-}

Based on the output of the previous part, how many flights were there flying into SLC? (In other words, how many rows were removed from the original `hf` tibble to produce `hf_filter4`?)

::: {.answer}

Please write up your answer here.

:::

##### Exercise 8 {-}

Use the `rm` command to remove all the extra tibbles you created in this section with `filter`.

::: {.answer}


```r
# Add code here to remove all filtered tibbles.
```

:::

*****

The `slice` command is related, but fairly useless in practice. It will allow you to extract rows by position. So `slice(hf, 1:10)` will give you the first 10 rows. As a general rule, the information available in a tibble should never depend on the order in which the rows appear. Therefore, no function you run should make any assumptions about the ordering of your data. The only reason one might want to think about the order of data is for convenience in presenting that data visually for someone to inspect. And that brings us to...


## `arrange` {#manipulating-arrange}

This just re-orders the rows, sorting on the values of one or more specified columns. As I mentioned before, in most data analyses you work with summaries of the data that do not depend on the order of the rows, so this is not quite as interesting as some of the other verbs. In fact, since the re-ordering is usually for the visual benefit of the reader, there is often no need to store the output in a new variable. We'll just print the output to the screen.


```r
arrange(hf, ActualElapsedTime)
```

```
## # A tibble: 22,758 × 21
##     Year Month DayofMo…¹ DayOf…² DepTime ArrTime Uniqu…³ Fligh…⁴ TailNum Actua…⁵
##    <dbl> <dbl>     <dbl>   <dbl>   <dbl>   <dbl> <chr>     <dbl> <chr>     <dbl>
##  1  2011    10         5       3    1656    1731 WN         2493 N510SW       35
##  2  2011     4        13       3    1207    1243 WN         2025 N738CB       36
##  3  2011     7        19       2    1043    1119 CO         1583 N16713       36
##  4  2011     2        22       2    1426    1503 WN         1773 N526SW       37
##  5  2011     3        19       6    1629    1706 WN         3805 N644SW       37
##  6  2011     5        31       2    1937    2014 WN          819 N378SW       37
##  7  2011     7        16       6    1632    1709 WN          912 N679AA       37
##  8  2011     8        22       1    1708    1745 WN         1754 N909WN       37
##  9  2011     9        30       5    1955    2032 WN         1959 N608SW       37
## 10  2011     9         1       4    1735    1812 WN         1754 N739GB       37
## # … with 22,748 more rows, 11 more variables: AirTime <dbl>, ArrDelay <dbl>,
## #   DepDelay <dbl>, Origin <chr>, Dest <chr>, Distance <dbl>, TaxiIn <dbl>,
## #   TaxiOut <dbl>, Cancelled <dbl>, CancellationCode <chr>, Diverted <dbl>, and
## #   abbreviated variable names ¹​DayofMonth, ²​DayOfWeek, ³​UniqueCarrier,
## #   ⁴​FlightNum, ⁵​ActualElapsedTime
```

Scroll over to the `ActualElapsedTime` variable in the output above (using the black right arrow) to see that these are now sorted in ascending order.

##### Exercise 9 {-}

How long is the shortest actual elapsed time? Why is this flight so short? (Hint: look at the destination.) Which airline flies that route? You may have to use your best friend Google to look up airport and airline codes.

::: {.answer}

Please write up your answer here.

:::

*****

If you want descending order, do this:


```r
arrange(hf, desc(ActualElapsedTime))
```

```
## # A tibble: 22,758 × 21
##     Year Month DayofMo…¹ DayOf…² DepTime ArrTime Uniqu…³ Fligh…⁴ TailNum Actua…⁵
##    <dbl> <dbl>     <dbl>   <dbl>   <dbl>   <dbl> <chr>     <dbl> <chr>     <dbl>
##  1  2011     2         4       5     941    1428 CO            1 N77066      527
##  2  2011    11         8       2     937    1417 CO            1 N59053      520
##  3  2011    11        11       5     930    1408 CO            1 N66057      518
##  4  2011    12        30       5     936    1413 CO            1 N69063      517
##  5  2011    12         8       4     935    1410 CO            1 N66051      515
##  6  2011    10        17       1     938    1311 CO            1 N69063      513
##  7  2011     6        27       1     936    1308 CO            1 N76064      512
##  8  2011     3        24       4     926    1256 CO            1 N69063      510
##  9  2011    12        27       2     935    1405 CO            1 N76065      510
## 10  2011     3         9       3     933    1402 CO            1 N69063      509
## # … with 22,748 more rows, 11 more variables: AirTime <dbl>, ArrDelay <dbl>,
## #   DepDelay <dbl>, Origin <chr>, Dest <chr>, Distance <dbl>, TaxiIn <dbl>,
## #   TaxiOut <dbl>, Cancelled <dbl>, CancellationCode <chr>, Diverted <dbl>, and
## #   abbreviated variable names ¹​DayofMonth, ²​DayOfWeek, ³​UniqueCarrier,
## #   ⁴​FlightNum, ⁵​ActualElapsedTime
```

##### Exercise 10 {-}

How long is the longest actual elapsed time? Why is this flight so long? Which airline flies that route? Again, you may have to use your best friend Google to look up airport and airline codes.

::: {.answer}

Please write up your answer here.

:::

##### Exercise 11(a) {-}

You can sort by multiple columns. The first column listed will be the first in the sort order, and then within each level of that first variable, the next column will be sorted, etc. Print a tibble that sorts first by destination (`Dest`) and then by arrival time (`ArrTime`), both in the default ascending order.

::: {.answer}


```r
# Add code here to sort hf first by Dest and then by ArrTime.
```

:::

##### Exercise 11(b) {-}

Based on the output of the previous part, what is the first airport code alphabetically and to what city does it correspond? (Use Google if you need to link the airport code to a city name.) At what time did the earliest flight to that city arrive? 

::: {.answer}

Please write up your answer here.

:::


## `mutate` {#manipulating-mutate}

Frequently, we want to create new variables that combine information from one or more existing variables. We use `mutate` for this. For example, suppose we wanted to find the total time of the flight. We might do this by adding up the minutes from several variables: `TaxiOut`, `AirTime`, and `TaxiIn`, and assigning that sum to a new variable called `total`. Scroll all the way to the right in the output below (using the black right arrow) to see the new `total` variable.


```r
hf_mutate <- mutate(hf, total = TaxiOut + AirTime + TaxiIn)
hf_mutate
```

```
## # A tibble: 22,758 × 22
##     Year Month DayofMo…¹ DayOf…² DepTime ArrTime Uniqu…³ Fligh…⁴ TailNum Actua…⁵
##    <dbl> <dbl>     <dbl>   <dbl>   <dbl>   <dbl> <chr>     <dbl> <chr>     <dbl>
##  1  2011     1        12       3    1419    1515 AA          428 N577AA       56
##  2  2011     1        17       1    1530    1634 AA          428 N518AA       64
##  3  2011     1        24       1    1356    1513 AA          428 N531AA       77
##  4  2011     1         9       7     714     829 AA          460 N586AA       75
##  5  2011     1        18       2     721     827 AA          460 N558AA       66
##  6  2011     1        22       6     717     829 AA          460 N499AA       72
##  7  2011     1        11       2    1953    2051 AA          533 N505AA       58
##  8  2011     1        14       5    2119    2229 AA          533 N549AA       70
##  9  2011     1        26       3    2009    2103 AA          533 N403AA       54
## 10  2011     1        14       5    1629    1734 AA         1121 N551AA       65
## # … with 22,748 more rows, 12 more variables: AirTime <dbl>, ArrDelay <dbl>,
## #   DepDelay <dbl>, Origin <chr>, Dest <chr>, Distance <dbl>, TaxiIn <dbl>,
## #   TaxiOut <dbl>, Cancelled <dbl>, CancellationCode <chr>, Diverted <dbl>,
## #   total <dbl>, and abbreviated variable names ¹​DayofMonth, ²​DayOfWeek,
## #   ³​UniqueCarrier, ⁴​FlightNum, ⁵​ActualElapsedTime
```

As it turns out, that was wasted effort because that variable already exists in `ActualElapsedTime`. The `all.equal` command below checks that both specified columns contain the exact same values.


```r
all.equal(hf_mutate$total, hf$ActualElapsedTime)
```

```
## [1] TRUE
```

Perhaps we want a variable that just classifies a flight as arriving late or not. Scroll all the way to the right in the output below to see the new `late` variable.


```r
hf_mutate2 <- mutate(hf, late = (ArrDelay > 0))
hf_mutate2
```

```
## # A tibble: 22,758 × 22
##     Year Month DayofMo…¹ DayOf…² DepTime ArrTime Uniqu…³ Fligh…⁴ TailNum Actua…⁵
##    <dbl> <dbl>     <dbl>   <dbl>   <dbl>   <dbl> <chr>     <dbl> <chr>     <dbl>
##  1  2011     1        12       3    1419    1515 AA          428 N577AA       56
##  2  2011     1        17       1    1530    1634 AA          428 N518AA       64
##  3  2011     1        24       1    1356    1513 AA          428 N531AA       77
##  4  2011     1         9       7     714     829 AA          460 N586AA       75
##  5  2011     1        18       2     721     827 AA          460 N558AA       66
##  6  2011     1        22       6     717     829 AA          460 N499AA       72
##  7  2011     1        11       2    1953    2051 AA          533 N505AA       58
##  8  2011     1        14       5    2119    2229 AA          533 N549AA       70
##  9  2011     1        26       3    2009    2103 AA          533 N403AA       54
## 10  2011     1        14       5    1629    1734 AA         1121 N551AA       65
## # … with 22,748 more rows, 12 more variables: AirTime <dbl>, ArrDelay <dbl>,
## #   DepDelay <dbl>, Origin <chr>, Dest <chr>, Distance <dbl>, TaxiIn <dbl>,
## #   TaxiOut <dbl>, Cancelled <dbl>, CancellationCode <chr>, Diverted <dbl>,
## #   late <lgl>, and abbreviated variable names ¹​DayofMonth, ²​DayOfWeek,
## #   ³​UniqueCarrier, ⁴​FlightNum, ⁵​ActualElapsedTime
```

This one is a little tricky. Keep in mind that `ArrDelay > 0` is a logical condition that is either true or false, so that truth value is what is recorded in the `late` variable. If the arrival delay is a positive number of minutes, the flight is considered "late", and if the arrival delay is zero or negative, it's not late.

If we would rather see more descriptive words than `TRUE` or `FALSE`, we have to do something even more tricky. Look at the `late` variable in the output below.


```r
hf_mutate3 <- mutate(hf,
                     late = as_factor(ifelse(ArrDelay > 0, 
                                             "Late", "On time")))
hf_mutate3
```

```
## # A tibble: 22,758 × 22
##     Year Month DayofMo…¹ DayOf…² DepTime ArrTime Uniqu…³ Fligh…⁴ TailNum Actua…⁵
##    <dbl> <dbl>     <dbl>   <dbl>   <dbl>   <dbl> <chr>     <dbl> <chr>     <dbl>
##  1  2011     1        12       3    1419    1515 AA          428 N577AA       56
##  2  2011     1        17       1    1530    1634 AA          428 N518AA       64
##  3  2011     1        24       1    1356    1513 AA          428 N531AA       77
##  4  2011     1         9       7     714     829 AA          460 N586AA       75
##  5  2011     1        18       2     721     827 AA          460 N558AA       66
##  6  2011     1        22       6     717     829 AA          460 N499AA       72
##  7  2011     1        11       2    1953    2051 AA          533 N505AA       58
##  8  2011     1        14       5    2119    2229 AA          533 N549AA       70
##  9  2011     1        26       3    2009    2103 AA          533 N403AA       54
## 10  2011     1        14       5    1629    1734 AA         1121 N551AA       65
## # … with 22,748 more rows, 12 more variables: AirTime <dbl>, ArrDelay <dbl>,
## #   DepDelay <dbl>, Origin <chr>, Dest <chr>, Distance <dbl>, TaxiIn <dbl>,
## #   TaxiOut <dbl>, Cancelled <dbl>, CancellationCode <chr>, Diverted <dbl>,
## #   late <fct>, and abbreviated variable names ¹​DayofMonth, ²​DayOfWeek,
## #   ³​UniqueCarrier, ⁴​FlightNum, ⁵​ActualElapsedTime
```

The `as_factor` command tells R that `late` should be a categorical variable. Without it, the variable would be a "character" variable, meaning a list of character strings. It won't matter for us here, but in any future analysis, you want categorical data to be treated as such by R.

The main focus here is on the `ifelse` construction. The `ifelse` function takes a condition as its first argument. If the condition is true, it returns the value in the second slot, and if it's false (the "else" part of if/else), it returns the value in the third slot. In other words, if `ArrDelay > 0`, this means the flight is late, so the new `late` variable should say "Late"; whereas, if `ArrDelay` is not greater than zero (so either zero or possibly negative if the flight arrived early), then the new variable should say "On Time".

Having said that, I would generally recommend that you leave these kinds of variables as logical types. It's much easier to summarize such variables in R, namely because R treats `TRUE` as 1 and `FALSE` as 0, allowing us to do things like this:


```r
mean(hf_mutate2$late, na.rm = TRUE)
```

```
## [1] 0.4761522
```

This gives us the proportion of late flights.

Note that we needed `na.rm` as you've seen in previous chapter. For example, look at the 93rd row of the tibble:


```r
slice(hf_mutate2, 93)
```

```
## # A tibble: 1 × 22
##    Year Month DayofMonth DayOf…¹ DepTime ArrTime Uniqu…² Fligh…³ TailNum Actua…⁴
##   <dbl> <dbl>      <dbl>   <dbl>   <dbl>   <dbl> <chr>     <dbl> <chr>     <dbl>
## 1  2011     1         27       4      NA      NA CO          258 <NA>         NA
## # … with 12 more variables: AirTime <dbl>, ArrDelay <dbl>, DepDelay <dbl>,
## #   Origin <chr>, Dest <chr>, Distance <dbl>, TaxiIn <dbl>, TaxiOut <dbl>,
## #   Cancelled <dbl>, CancellationCode <chr>, Diverted <dbl>, late <lgl>, and
## #   abbreviated variable names ¹​DayOfWeek, ²​UniqueCarrier, ³​FlightNum,
## #   ⁴​ActualElapsedTime
```

Notice that all the times are missing. There are a bunch of rows like this. Since there is not always an arrival delay listed, the `ArrDelay` variable doesn't always have a value, and if `ArrDelay` is `NA`, the `late` variable will be too. So if we try to calculate the mean with just the `mean` command, this happens:


```r
mean(hf_mutate2$late)
```

```
## [1] NA
```

##### Exercise 12 {-}

Why does taking the mean of a bunch of zeros and ones give us the proportion of ones? (Think about the formula for the mean. What happens when we take the sum of all the zeros and ones, and what happens when we divide by the total?)

::: {.answer}

Please write up your answer here.

:::

##### Exercise 13 {-}

Create a new tibble called `hf_mutate4` that uses the `mutate` command to create a new variable called `dist_k` which measures the flight distance in kilometers instead of miles. (Hint: to get from miles to kilometers, multiply the distance by 1.60934.) Print the output to the screen.

::: {.answer}


```r
# Add code here to define hf_mutate4.
# Add code here to print hf_mutate4.
```

:::

*****

A related verb is `transmute`. The only difference between `mutate` and `transmute` is that `mutate` creates the new column(s) and keeps all the old ones too, whereas `transmute` will throw away all the columns except the newly created ones. This is not something that you generally want to do, but there are exceptions. For example, if I was preparing a report and I needed only to talk about flights being late or not, it would do no harm (and would save some memory) to throw away everything except the `late` variable.

Before moving on to the next section, we'll clean up the extra tibbles lying around. You'll need to manually click the run button in the next code chunk since you have defined `hf_mutate4`.


```r
rm(hf_mutate, hf_mutate2, hf_mutate3, hf_mutate4)
```

```
## Warning in rm(hf_mutate, hf_mutate2, hf_mutate3, hf_mutate4): object
## 'hf_mutate4' not found
```


## `summarise` (with `group_by`) {#manipulating-summ-group}

First, before you mention that `summarise` is spelled wrong...well, the author of the `dplyr` package is named Hadley Wickham (same author as the `ggplot2` package) and he is from New Zealand. So that's the way he spells it. He was nice enough to include the `summarize` function as an alias if you need to use it 'cause this is 'Murica!

The `summarise` function, by itself, is kind of boring, and doesn't do anything that couldn't be done more easily with base R functions.


```r
summarise(hf, mean(Distance))
```

```
## # A tibble: 1 × 1
##   `mean(Distance)`
##              <dbl>
## 1             791.
```


```r
mean(hf$Distance)
```

```
## [1] 790.5861
```

Where `summarise` shines is in combination with `group_by`. For example, let's suppose that we want to see average flight distances, but broken down by airline. We can do the following:


```r
hf_summ_grouped <- group_by(hf, UniqueCarrier)
hf_summ <- summarise(hf_summ_grouped, mean(Distance))
hf_summ
```

```
## # A tibble: 15 × 2
##    UniqueCarrier `mean(Distance)`
##    <chr>                    <dbl>
##  1 AA                        470.
##  2 AS                       1874 
##  3 B6                       1428 
##  4 CO                       1097.
##  5 DL                        723.
##  6 EV                        788.
##  7 F9                        883 
##  8 FL                        686.
##  9 MQ                        701.
## 10 OO                        823.
## 11 UA                       1204.
## 12 US                        982.
## 13 WN                        613.
## 14 XE                        590.
## 15 YV                        982.
```

### Piping {#manipulating-piping}

This is a good spot to introduce a time-saving and helpful device called "piping", denoted by the symbol `%>%`. We've seen this weird combination of symbols in past chapters, but we haven't really explained what they do.

Piping always looks more complicated than it really is. The technical definition is that

`x %>% f(y)`

is equivalent to 

`f(x, y)`.

As a simple example, we could add two numbers like this:


```r
sum(2, 3)
```

```
## [1] 5
```

Or using the pipe, we could do it like this:


```r
2 %>% sum(3)
```

```
## [1] 5
```

All this is really saying is that the pipe takes the thing on its left, and plugs it into the first slot of the function on its right. So why do we care?

Let's revisit the combination `group_by`/`summarise` example above. There are two ways to do this without pipes, and both are a little ugly. One way is above, where you have to keep reassigning the output to new variables (in the case above, to `hf_summ_grouped` and then `hf_summ`). The other way is to nest the functions:


```r
summarise(group_by(hf, UniqueCarrier), mean(Distance))
```

```
## # A tibble: 15 × 2
##    UniqueCarrier `mean(Distance)`
##    <chr>                    <dbl>
##  1 AA                        470.
##  2 AS                       1874 
##  3 B6                       1428 
##  4 CO                       1097.
##  5 DL                        723.
##  6 EV                        788.
##  7 F9                        883 
##  8 FL                        686.
##  9 MQ                        701.
## 10 OO                        823.
## 11 UA                       1204.
## 12 US                        982.
## 13 WN                        613.
## 14 XE                        590.
## 15 YV                        982.
```

This requires a lot of brain power to parse. In part, this is because the function is inside-out: first you group `hf` by `UniqueCarrier`, and then the result of that is summarized. Here's how the pipe fixes it:


```r
hf %>%
    group_by(UniqueCarrier) %>%
    summarise(mean(Distance))
```

```
## # A tibble: 15 × 2
##    UniqueCarrier `mean(Distance)`
##    <chr>                    <dbl>
##  1 AA                        470.
##  2 AS                       1874 
##  3 B6                       1428 
##  4 CO                       1097.
##  5 DL                        723.
##  6 EV                        788.
##  7 F9                        883 
##  8 FL                        686.
##  9 MQ                        701.
## 10 OO                        823.
## 11 UA                       1204.
## 12 US                        982.
## 13 WN                        613.
## 14 XE                        590.
## 15 YV                        982.
```

Look at the `group_by` line. The `group_by` function should take two arguments, the tibble, and then the grouping variable. It appears to have only one argument. But look at the previous line. The pipe says to insert whatever is on its left (`hf`) into the first slot of the function on its right (`group_by`). So the net effect is still to evaluate the function `group_by(hf, UniqueCarrier)`.

Now look at the `summarise` line. Again, `summarise` is a function of two inputs, but all we see is the part that finds the mean. The pipe at the end of the previous line tells the `summarise` function to insert the stuff already computed (the grouped tibble returned by `group_by(hf, UniqueCarrier)`) into the first slot of the `summarise` function.

Piping takes a little getting used to, but once you're good at it, you'll never go back. It's just makes more sense semantically. When I read the above set of commands, I see a set of instructions in chronological order:

- Start with the tibble `hf`.
- Next, group by the carrier. 
- Next, summarize each group using the mean distance.

Now we can assign the result of all that to the new variable `hf_summ`:


```r
hf_summ <- hf %>%
    group_by(UniqueCarrier) %>%
    summarise(mean(Distance))
hf_summ
```

```
## # A tibble: 15 × 2
##    UniqueCarrier `mean(Distance)`
##    <chr>                    <dbl>
##  1 AA                        470.
##  2 AS                       1874 
##  3 B6                       1428 
##  4 CO                       1097.
##  5 DL                        723.
##  6 EV                        788.
##  7 F9                        883 
##  8 FL                        686.
##  9 MQ                        701.
## 10 OO                        823.
## 11 UA                       1204.
## 12 US                        982.
## 13 WN                        613.
## 14 XE                        590.
## 15 YV                        982.
```

Some people even take this one step further. The result of all the above is assigned to a new variable `hf_summ` that currently appears as the first command (`hf_summ <- ...`) But you could write this as


```r
hf %>%
    group_by(UniqueCarrier) %>%
    summarise(mean(Distance)) -> hf_summ
```

Now it says the following:

- Start with the tibble `hf`.
- Next, group by the carrier. 
- Next, summarize each group using the mean distance.
- *Finally*, assign the result to a new variable called `hf_summ`.

In other words, the arrow operator for assignment works both directions!

Let's try some counting. This one is common enough that `dplyr` doesn't even make us use `group_by` and `summarise`. We can just use the command `count`. What if we wanted to know how many flights correspond to each carrier?


```r
hf_summ2 <- hf %>%
    count(UniqueCarrier)
hf_summ2
```

```
## # A tibble: 15 × 2
##    UniqueCarrier     n
##    <chr>         <int>
##  1 AA              325
##  2 AS               37
##  3 B6               70
##  4 CO             7004
##  5 DL              265
##  6 EV              221
##  7 F9               84
##  8 FL              214
##  9 MQ              465
## 10 OO             1607
## 11 UA              208
## 12 US              409
## 13 WN             4535
## 14 XE             7306
## 15 YV                8
```

Also note that we can give summary columns a new name if we wish. In `hf_summ`, we didn't give the new column an explicit name, so it showed up in our tibble as a column called `mean(Distance)`. If we want to change it, we can do this:


```r
hf_summ <- hf %>%
    group_by(UniqueCarrier) %>%
    summarise(mean_dist = mean(Distance))
hf_summ
```

```
## # A tibble: 15 × 2
##    UniqueCarrier mean_dist
##    <chr>             <dbl>
##  1 AA                 470.
##  2 AS                1874 
##  3 B6                1428 
##  4 CO                1097.
##  5 DL                 723.
##  6 EV                 788.
##  7 F9                 883 
##  8 FL                 686.
##  9 MQ                 701.
## 10 OO                 823.
## 11 UA                1204.
## 12 US                 982.
## 13 WN                 613.
## 14 XE                 590.
## 15 YV                 982.
```

Look at the earlier version of `hf_summ` and compare it to the one above. Make sure you see that the name of the second column changed.

The new count column of `hf_summ2` is just called `n`. That's okay, but if we insist on giving it a more user-friendly name, we can do so as follows:


```r
hf_summ2 <- hf %>%
    count(UniqueCarrier, name = "total_count")
hf_summ2
```

```
## # A tibble: 15 × 2
##    UniqueCarrier total_count
##    <chr>               <int>
##  1 AA                    325
##  2 AS                     37
##  3 B6                     70
##  4 CO                   7004
##  5 DL                    265
##  6 EV                    221
##  7 F9                     84
##  8 FL                    214
##  9 MQ                    465
## 10 OO                   1607
## 11 UA                    208
## 12 US                    409
## 13 WN                   4535
## 14 XE                   7306
## 15 YV                      8
```

This is a little different because it requires us to use a `name` argument and put the new name in quotes.

##### Exercise 14(a) {-}

Create a tibble called `hf_summ3` that lists the total count of flights for each day of the week. Be sure to use the pipe as above. Print the output to the screen. (You don't need to give the count column a new name.)

::: {.answer}


```r
# Add code here to define hf_summ3.
# Add code here to print hf_summ3.
```

:::

##### Exercise 14(b) {-}

According to the output in the previous part, what day of the week had the fewest flights? (Assume 1 = Monday.)

::: {.answer}

Please write up your answer here.

:::

*****

The tibbles created in this section are all just a few rows each. They don't take up much memory, so we don't really need to remove them. You can if you want, but it's not necessary.


## Putting it all together {#manipulating-all-together}

Often we need more than one of these verbs. In many data analyses, we need to do a sequence of operations to get at the answer we seek. This is most easily accomplished using a more complicated sequence of pipes.

Here's a example of multi-step piping. Let's say that we only care about Delta flights, and even then, we only want to know about the month of the flight and the departure delay. From there, we wish to group by month so we can find the maximum departure delay by month. Here is a solution, piping hot and ready to go. [groan]


```r
hf_grand_finale <- hf %>%
    filter(UniqueCarrier == "DL") %>%
    dplyr::select(Month, DepDelay) %>%
    group_by(Month) %>%
    summarise(max_delay = max(DepDelay, na.rm = TRUE))
hf_grand_finale
```

```
## # A tibble: 12 × 2
##    Month max_delay
##    <dbl>     <dbl>
##  1     1        26
##  2     2       460
##  3     3       202
##  4     4        23
##  5     5       127
##  6     6       184
##  7     7       360
##  8     8        48
##  9     9       292
## 10    10        90
## 11    11        10
## 12    12        14
```

Go through each line of code carefully and translate it into English:

- We define a variable called `hf_grand_finale` that starts with the original `hf` data.
- We `filter` this data so that only Delta flights will be analyzed.
- We `select` the variables `Month` and `DepDelay`, throwing away all other variables that are not of interest to us. (Don't forget to use the `dplyr::select` syntax to make sure we get the right function!)
- We `group_by` month so that the results will be displayed by month.
- We `summarise` each month by listing the maximum value of `DepDelay` that appears within each month.
- We print the result to the screen.

Notice in the `summarise` line, we again took advantage of `dplyr`'s ability to rename any variable along the way, assigning our computation to the new variable `max_delay`. Also note the need for `na.rm = TRUE` so that the `max` command ignores any missing values.

A minor simplification results from the realization that `summarise` must throw away any variables it doesn't need. (Think about why for a second: what would `summarise` do with, say, `ArrTime` if we've only asked it to calculate the maximum value of `DepDelay` for each month?) So you could write this instead, removing the `select` clause:


```r
hf_grand_finale <- hf %>%
    filter(UniqueCarrier == "DL") %>%
    group_by(Month) %>%
    summarise(max_delay = max(DepDelay, na.rm = TRUE))
hf_grand_finale
```

```
## # A tibble: 12 × 2
##    Month max_delay
##    <dbl>     <dbl>
##  1     1        26
##  2     2       460
##  3     3       202
##  4     4        23
##  5     5       127
##  6     6       184
##  7     7       360
##  8     8        48
##  9     9       292
## 10    10        90
## 11    11        10
## 12    12        14
```

Check that you get the same result. With *massive* data sets, it's possible that the selection and sequence of these verbs matter, but you don't see an appreciable difference here, even with 22758 rows. (There are ways of benchmarking performance in R, but that is a more advanced topic.)

##### Exercise 15 {-}

Summarize in your own words what information is contained in the `hf_grand_finale` tibble. In other words, what are the numbers in the `max_delay` column telling us? Be specific!

::: {.answer}

Please write up your answer here.

:::

The remaining exercises are probably the most challenging you've seen so far in the course. Take each slowly. Read the instructions carefully. Go back through the chapter and identify which "verb" needs to be used for each part of the task. Examine the sample code in those sections carefully to make sure you get the syntax right. Create a sequence of "pipes" to do each task, one-by-one. Copy and paste pieces of code from earlier and make minor changes to adapt the code to the given task.

##### Exercise 16 {-}

Create a tibble that counts the flights to LAX grouped by day of the week. (Hint: you need to `filter` to get flights to LAX. Then you'll need to `count` using `DayOfWeek` as a grouping variable. Because you're using `count`, you don't need `group_by` or `summarise`.) Print the output to the screen.

::: {.answer}


```r
# Add code here to count the flights to LAX
# grouped by day of the week.
# Print the output to the screen.
```

:::

##### Exercise 17 {-}

Create a tibble that finds the median distance flight for each airline. Sort the resulting tibble from highest distance to lowest. (Hint: You'll need to `group_by` carrier and `summarise` using the `median` function. Finally, you'll need to `arrange` the result according to the median distance variable that you just created.) Print the output to the screen.

::: {.answer}


```r
# Add code here to find the median distance by airline.
# Print the output to the screen.
```

:::


## Conclusion {#manipulating-conclusion}

Raw data often doesn't come in the right form for us to run our analyses. The `dplyr` verbs are powerful tools for manipulating tibbles until they are in the right form.

### Preparing and submitting your assignment {#manipulating-prep}

1. From the "Run" menu, select "Restart R and Run All Chunks".
2. Deal with any code errors that crop up. Repeat steps 1–-2 until there are no more code errors.
3. Spell check your document by clicking the icon with "ABC" and a check mark.
4. Hit the "Preview" button one last time to generate the final draft of the `.nb.html` file.
5. Proofread the HTML file carefully. If there are errors, go back and fix them, then repeat steps 1--5 again.

If you have completed this chapter as part of a statistics course, follow the directions you receive from your professor to submit your assignment.



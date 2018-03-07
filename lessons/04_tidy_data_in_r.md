

# Tidy Data in R

In this lesson we will cover the basics of data in R and will do so from a somewhat opinionated viewpoint of "Tidy Data".  There are other paradigms and other ways to work with data in R, but focusing on Tidy Data concepts and tools (a.k.a., The Tidyverse) gets people to a productive place the quickest.  For more on the data analysis using the Tidyverse, the best resource I know of is [R for Data Science](http://r4ds.had.co.nz).  The approaches we will cover are very much inspired by this book.

## Lesson Outline
- [Data in R: The data frame](#data-in-r-the-data-frame)
- [Reading in data](#reading-in-data)
- [Tidy data](#tidy-data)

## Exercises
- [Excercise 4.1](#exercise-41)

## Data in R: The data frame

Simply put, a data structure is a way for programming languages to handle storing information.  Like most languages, R has several structures (vectors, matrix, lists, etc.) but since R is built for data analysis the data frame, a spreadsheet like structure with rows and columns, is the most widely used and useful to learn first.  In addition, the data frame (or is it data.frame) is the basis for many modern R pacakges (e.g. the tidyverse) and getting used to it will allow you to quickly build your R skills.

*Note:* It is useful to know more about the different data structures such as vectors, lists, and factors (a weird one that is for catergorical data).  But that is beyond what we have time for.  You can look at some of our [old materials](data_in_r.md) or even better look at what I think is the best source on this information, Hadley Wickham's [Data Structures Chapter in Advanced R](http://adv-r.had.co.nz/Data-structures.html).

### Build a data frame
Best way to learn what a data frame is is to look at one.  Let's now build a simple data frame from scratch with the `data.frame()` function.  This is mostly a teaching excercise as we will use the function very little in the excercises to come.  


```r
# Our first data frame

my_df <- data.frame(names = c("joe","jenny","bob","sue"), 
                    age = c(45, 27, 38,51), 
                    knows_r = c(FALSE, TRUE, TRUE,FALSE))
my_df
```

```
##   names age knows_r
## 1   joe  45   FALSE
## 2 jenny  27    TRUE
## 3   bob  38    TRUE
## 4   sue  51   FALSE
```

That created a data frame with 3 columns (names, age, knows_r) and four rows.  For each row we have some information on the name of an individual (stored as a character/string), their age (stored as a numeric value), and a column indicating if they know R or not (stored as a boolean/logical).

If you've worked with data before in a spreadsheet or from a table in a database, this rectangular structure should look somewhat familiar.   One way (there are many!) we can access the different parts of the data frame is like:


```r
# Use the dollar sign to get a column
my_df$age
```

```
## [1] 45 27 38 51
```

```r
# Grab a row with indexing
my_df[2,]
```

```
##   names age knows_r
## 2 jenny  27    TRUE
```

At this point, we have:

- built a data frame from scratch
- seen rows and columns
- heard about "rectangular" structure
- seen how to get a row and a column

The purpose of all this was to introduce the concept of the data frame.  Moving forward we will use other tools to read in data, but the end result will be the same: a data frame with rows (i.e. observations) and columns (i.e. variables).

## Reading in data

Completely creating a data frame from scratch is useful (especially when you start writing your own functions), but more often than not data is stored in an external file that you need to read into R.  These may be delimited text files, spreadsheets, relational databases, SAS files ...  You get the idea.  Instead of treating this subject exhaustively, we will focus just on a single file type, `.csv` that is very commonly encountered and (usually) easy to create from other file types.  For this, we will use the Tidyverse way to do this and use  `read_csv()` from the `readr` pacakge.

The `read_csv()` is a re-imagined version of the base R fucntion, `read.csv()`.  This command assumes a header row with column names and that the delimiter is a comma. The expected no data value is NA and by default, strings are NOT converted to factors.  This is a big benefit to using `read_csv()` as opposed to `read.csv()`.  Additionally, `read_csv()` has some performance enhancements that make it preferrable when working with larger data sets.  In my limited experience it is about 45% faster than the base R options.  For instance a ~200 MB file with hundreds of columns and a couple hundred thousand rows took ~14 seconds to read in with `read_csv()` and about 24 seconds with `read.csv()`.  As a comparison at 45 seconds Excel had only opened 25% of the file!

Source files for `read_csv()` can either be on a local hard drive or, and this is pretty cool, on the web. We will be using the former for our examples and exercises. If you had a file available from a URL it would be accessed like `mydf <- read.csv("https://example.com/my_cool_file.csv")`. As an aside, paths and the use of forward vs back slash is important. R is looking for forward slashes ("/"), or unix-like paths. You can use these in place of the back slash and be fine. You can use a back slash but it needs to be a double back slash ("\"). This is becuase the single backslash in an escape character that is used to indicate things like newlines or tabs. 

For today's workshop we will focus on grabbing data from a local file, and we already have an example of this in our `yale_markdown.Rmd`.  In that file look for the code chunk labeled "read_csv".

For your convenience, it looks like:


```r
gap_gdp <- read_csv("gapminder_gdp.csv")
```

And now we can take a look at our data frame


```r
gap_gdp
```

```
## # A tibble: 270 x 53
##    `GDP (constant 2~    `1960`   `1961`  `1962`   `1963`   `1964`   `1965`
##    <chr>                 <dbl>    <dbl>   <dbl>    <dbl>    <dbl>    <dbl>
##  1 Abkhazia           NA       NA       NA      NA       NA       NA      
##  2 Afghanistan        NA       NA       NA      NA       NA       NA      
##  3 Akrotiri and Dhe~  NA       NA       NA      NA       NA       NA      
##  4 Albania            NA       NA       NA      NA       NA       NA      
##  5 Algeria             1.38e10  1.19e10  9.60e9  1.29e10  1.36e10  1.45e10
##  6 American Samoa     NA       NA       NA      NA       NA       NA      
##  7 Andorra            NA       NA       NA      NA       NA       NA      
##  8 Angola             NA       NA       NA      NA       NA       NA      
##  9 Anguilla           NA       NA       NA      NA       NA       NA      
## 10 Antigua and Barb~  NA       NA       NA      NA       NA       NA      
## # ... with 260 more rows, and 46 more variables: `1966` <dbl>,
## #   `1967` <dbl>, `1968` <dbl>, `1969` <dbl>, `1970` <dbl>, `1971` <dbl>,
## #   `1972` <dbl>, `1973` <dbl>, `1974` <dbl>, `1975` <dbl>, `1976` <dbl>,
## #   `1977` <dbl>, `1978` <dbl>, `1979` <dbl>, `1980` <dbl>, `1981` <dbl>,
## #   `1982` <dbl>, `1983` <dbl>, `1984` <dbl>, `1985` <dbl>, `1986` <dbl>,
## #   `1987` <dbl>, `1988` <dbl>, `1989` <dbl>, `1990` <dbl>, `1991` <dbl>,
## #   `1992` <dbl>, `1993` <dbl>, `1994` <dbl>, `1995` <dbl>, `1996` <dbl>,
## #   `1997` <dbl>, `1998` <dbl>, `1999` <dbl>, `2000` <dbl>, `2001` <dbl>,
## #   `2002` <dbl>, `2003` <dbl>, `2004` <dbl>, `2005` <dbl>, `2006` <dbl>,
## #   `2007` <dbl>, `2008` <dbl>, `2009` <dbl>, `2010` <dbl>, `2011` <dbl>
```

### Other ways to read in data

There are many ways to read in data with R.  If you have questions about this, please let Jeff know.  He's happy to chat more about it.  For this workhsop we will point out one other way we did it in the example R Markdown file and will show one last way to read in Excel files here.

Google Sheets are becoming more and more ubiquitous.  Luckily there is an R package for that!  Look at the `yale_markdown.Rmd` file and find the code chunk labeled "gs_read".  This shows us how to access a Google Sheet.

Lastly, since Excel spreadsheets are so ubiquitous we need a reliable way to read in data stored in an excel spreadsheet.  There are a variety of packages that provide this capability, but by far the best is `readxl` which is part of the Tidyverse.  This is how we read in an File:


```r
# You'll very likely need to install it first!!!  How'd we do that?
library(readxl)
gap_gdp_percap <- read_excel("gapminder_gdp_percap.xlsx")
```

This is the simplest case, but lets dig into the options to see what's possible


```
## function (path, sheet = NULL, range = NULL, col_names = TRUE, 
##     col_types = NULL, na = "", trim_ws = TRUE, skip = 0, n_max = Inf, 
##     guess_max = min(1000, n_max)) 
## NULL
```

## Tidy data

We have learned about data frames, how to create them, and about several ways to read in external data into a data.frame.   At this point there have been only a few rules applied to our data frames (which already separates them from spreadsheets) and that is our datasets must by rectangular.  Beyond that we havent disscussed how best to organize that data so that subsequent analyses are easier to accomplish. This is, in my opinion, the biggest decision we make as data analysts and it takes a lot of time to think about how best to organize data and to actually re-organize that data.  Luckily, we can use an existing concept for this that will help guide our decisions and re-organization.  The best concept I know of to do this is the concept of [tidy data](http://r4ds.had.co.nz/tidy-data.html).  The essence of which can be summed up as:

1. Each column is a variable
2. Each row is an observation
3. Each cell has a single value
4. The data must be rectangular

Lastly, if you want to read more about this there are several good sources:

- The previously linked R4DS Chapter on [tidy data](http://r4ds.had.co.nz/tidy-data.html)
- The [original paper by Hadley Wickham](https://www.jstatsoft.org/article/view/v059i10)
- The [Tidy Data Vignette](http://tidyr.tidyverse.org/articles/tidy-data.html)
- Really anything on the [Tidyverse page](https://www.tidyverse.org/)

Let's now see some of the basic tools for tidying data using the `tidyr` and `dplyr` packages.

### Spread and Gather with `tidyr`

Two of the function I use the most in `tidyr` are `spread()` and `gather()`.  They are somewhat similar to pivot tables in spreadsheets and allow us combine columns together or spread them back out.  I'll admit it still sometimes feels a bit like magic.  So, abracadabra!

Load up the library:


```r
library(tidyr)
```

### gather

Let's build an untidy data frame.  This is made up, but let's say we want to grab some monthly stats on number of visitors (in thousands) per state...


```r
dirty_df <- data_frame(state = c("CT","RI","MA"), jan = c(3.1,8.9,9.3), feb = c(1.0,4.2,8.6), march = c(2.9,3.1,12.5), april = c(4.4,5.6,14.2))
dirty_df
```

```
## # A tibble: 3 x 5
##   state   jan   feb march april
##   <chr> <dbl> <dbl> <dbl> <dbl>
## 1 CT     3.10  1.00  2.90  4.40
## 2 RI     8.90  4.20  3.10  5.60
## 3 MA     9.30  8.60 12.5  14.2
```

What would be a tidy way to represent this same data set?

They way I would do this is to gather the month columns into two new columns that reperesnet month and number of vistors/


```r
tidy_df <- gather(dirty_df, month, vistors, jan:april) 
tidy_df
```

```
## # A tibble: 12 x 3
##    state month vistors
##    <chr> <chr>   <dbl>
##  1 CT    jan      3.10
##  2 RI    jan      8.90
##  3 MA    jan      9.30
##  4 CT    feb      1.00
##  5 RI    feb      4.20
##  6 MA    feb      8.60
##  7 CT    march    2.90
##  8 RI    march    3.10
##  9 MA    march   12.5 
## 10 CT    april    4.40
## 11 RI    april    5.60
## 12 MA    april   14.2
```

### spread

Here's another possibility from my world.  We have data collected at multiple sampling locations and we are measuring multiple water quality parameters.


```r
long_df <- data_frame(station = rep(c("A","A","B","B"),3), 
                      month = c(rep("june",4),rep("july",4),rep("aug", 4)), 
                      parameter = rep(c("chla","temp"), 6),
                      value = c(18,23,3,22,19.5,24,3.5,22.25,32,26.7,4.2,23))
long_df
```

```
## # A tibble: 12 x 4
##    station month parameter value
##    <chr>   <chr> <chr>     <dbl>
##  1 A       june  chla      18.0 
##  2 A       june  temp      23.0 
##  3 B       june  chla       3.00
##  4 B       june  temp      22.0 
##  5 A       july  chla      19.5 
##  6 A       july  temp      24.0 
##  7 B       july  chla       3.50
##  8 B       july  temp      22.2 
##  9 A       aug   chla      32.0 
## 10 A       aug   temp      26.7 
## 11 B       aug   chla       4.20
## 12 B       aug   temp      23.0
```

We might want to have this in a wide as opposed to long format.  That can be accomplished with `spread()`


```r
wide_df <- spread(long_df,parameter,value)
wide_df
```

```
## # A tibble: 6 x 4
##   station month  chla  temp
##   <chr>   <chr> <dbl> <dbl>
## 1 A       aug   32.0   26.7
## 2 A       july  19.5   24.0
## 3 A       june  18.0   23.0
## 4 B       aug    4.20  23.0
## 5 B       july   3.50  22.2
## 6 B       june   3.00  22.0
```

### Data manipulation with `dplyr`

There are a lot of different ways to manipulate data in R, but one that is a fairly recent addition and that is at the core of the Tidyverse is `dplyr`.  In particular, we are going to look at selecting columns, filtering data, adding new columns, grouping data, and summarizing data.  

#### select

Often we get datasets that have many columns or we might what to re-order those columns.  We can accomplish both of these with select.  Here's a quick example with the `iris` dataset.  We will also be introducing the concept of the pipe: `%>%` which we will be using going forward.


```r
iris_petals <- iris %>%
  select(Species, Petal.Width, Petal.Length)
as_tibble(iris_petals)
```

```
## # A tibble: 150 x 3
##    Species Petal.Width Petal.Length
##    <fct>         <dbl>        <dbl>
##  1 setosa        0.200         1.40
##  2 setosa        0.200         1.40
##  3 setosa        0.200         1.30
##  4 setosa        0.200         1.50
##  5 setosa        0.200         1.40
##  6 setosa        0.400         1.70
##  7 setosa        0.300         1.40
##  8 setosa        0.200         1.50
##  9 setosa        0.200         1.40
## 10 setosa        0.100         1.50
## # ... with 140 more rows
```

The end result of this is a data frame, `iris_petals` that has three columns: Species, Petal.Width and Petal.Length in the order that we specified.

#### filter

The `filter()` function allows us to select out data that meets certain criteria.  For instance we might want to further manipulate our 3 column data frame with only one species of Iris and Petals greater than the .


```r
iris_petals_virginica <- iris %>%
  select(species = Species, petal_width = Petal.Width, petal_length = Petal.Length) %>%
  filter(species == "virginica") %>%
  filter(petal_width >= median(petal_width))
as_tibble(iris_petals_virginica)  
```

```
## # A tibble: 29 x 3
##    species   petal_width petal_length
##    <fct>           <dbl>        <dbl>
##  1 virginica        2.50         6.00
##  2 virginica        2.10         5.90
##  3 virginica        2.20         5.80
##  4 virginica        2.10         6.60
##  5 virginica        2.50         6.10
##  6 virginica        2.00         5.10
##  7 virginica        2.10         5.50
##  8 virginica        2.00         5.00
##  9 virginica        2.40         5.10
## 10 virginica        2.30         5.30
## # ... with 19 more rows
```

#### mutate

Now say we have some research that suggest the ratio of the petal width and length is imporant.  We might want to add that as a new column in our data set, but we want to do this now for all or our species and all sizes.


```r
iris_petals_ratio <- iris %>%
  select(species = Species, petal_width = Petal.Width, petal_length = Petal.Length) %>%
  mutate(petal_ratio = petal_width/petal_length)
as_tibble(iris_petals_ratio)
```

```
## # A tibble: 150 x 4
##    species petal_width petal_length petal_ratio
##    <fct>         <dbl>        <dbl>       <dbl>
##  1 setosa        0.200         1.40      0.143 
##  2 setosa        0.200         1.40      0.143 
##  3 setosa        0.200         1.30      0.154 
##  4 setosa        0.200         1.50      0.133 
##  5 setosa        0.200         1.40      0.143 
##  6 setosa        0.400         1.70      0.235 
##  7 setosa        0.300         1.40      0.214 
##  8 setosa        0.200         1.50      0.133 
##  9 setosa        0.200         1.40      0.143 
## 10 setosa        0.100         1.50      0.0667
## # ... with 140 more rows
```

#### group_by and summarize

Lastly, we might want to get some summary statistics of our important petal ratio metric for each of the species.


```r
iris_petal_ratio_species <- iris %>%
  select(species = Species, petal_width = Petal.Width, petal_length = Petal.Length) %>%
  mutate(petal_ratio = petal_width/petal_length) %>%
  group_by(species) %>%
  summarize(mean_petal_ratio = mean(petal_ratio),
            sd_petal_ratio = sd(petal_ratio),
            median_petal_ratio = median(petal_ratio))
```

## Exercise 4.1
The best way to really understand this is to look at some more hands on examples.  For this exercise we will dig into our datasets and find ways to tidy them up.  We will do this together via examples in the [`yale_markdown.Rmd`](https://raw.githubusercontent.com/jhollist/yale_markdown/master/yale_markdown.Rmd) file.  Our goal will be to add in the the GDP per capita values and then calculate an estimate of the population for each country.  To do this we will need to tidy up our data, join datasets together and add in a new column with the population estimates.  

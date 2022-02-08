Activity 6
================
Nathan Vandermeer

## Data and packages

Again, we will load all of the `{tidyverse}` for this activity.

``` r
knitr::opts_chunk$set(error = TRUE)

library(tidyverse)
```

In this activity we will explore the differences between human-readable
datasets (untidy data) and computer-readable datasets (tidy data).
Before making any visualizations, it is important to understand whether
the data we have is tidy or not.

In this activity you will be working with data from the popular tv
sitcom Friends. Transcripts for all 10 seasons of the show were parsed
and all utterances are broken down by season, episode, and scene. The
`by_speaker` dataset summarizes the total number of words spoken by main
character for each season.

![FRIENDS tv show
logo](https://upload.wikimedia.org/wikipedia/commons/thumb/b/bc/Friends_logo.svg/1186px-Friends_logo.svg.png)

``` r
by_speaker <- read_csv(here::here("data","by_speaker.csv"))
by_speaker
```

    ## # A tibble: 6 x 11
    ##   speaker  `Season 1` `Season 2` `Season 3` `Season 4` `Season 5` `Season 6`
    ##   <chr>         <dbl>      <dbl>      <dbl>      <dbl>      <dbl>      <dbl>
    ## 1 Chandler       8796       8248       8952       9695       8611      10764
    ## 2 Joey           6195       7207       8504       8541       9731      10923
    ## 3 Monica         7851       7822       8301       7897       8703       8784
    ## 4 Phoebe         6522       8713       9177       8560       8695       8372
    ## 5 Rachel         8854       8600       8809       9896      10352      11013
    ## 6 Ross          10569       9382      10725       9178       9234      10307
    ## # … with 4 more variables: Season 7 <dbl>, Season 8 <dbl>, Season 9 <dbl>,
    ## #   Season 10 <dbl>

In the `load_data` chunk, notice that I am using a new function: the
`here` function from `{here}` (typed as `here::here` or in
`<package>::<function>` format). This function is a another great tool
to use for sharing projects with others when their file structure might
be different than yours. Read Jenny Bryan’s [Ode to the here
package](https://github.com/jennybc/here_here) and/or read more about
`{here}`… [here](https://here.r-lib.org/articles/here.html). Also,
Martin Chan provides a guide of [RStudio Projects and Working
Directories](https://martinctc.github.io/blog/rstudio-projects-and-working-directories-a-beginner's-guide/)
and Jenny Richmond gives an overview on [How to use the `here`
package](http://jenrichmond.rbind.io/post/how-to-use-the-here-package/).

In short, folder structures vary from [computer-to-computer and
user-to-user](https://r4ds.had.co.nz/workflow-projects.html#paths-and-directories).
RStudio Projects are one great way to make sure that others that use
your code won’t have to worry about the path to files within the Project
folder. Then, `{here}` will always recognizes the top-level directory of
your RStudio project! Type `here::here()` in your **Console** to verify
that it is recognizing your current project is located at
`.../activity06-pivoting` (your front matter will be different than
mine, but it should end with this project folder).

![Illustration by Allison
Horst](https://raw.githubusercontent.com/allisonhorst/stats-illustrations/master/rstats-artwork/here.png)

Art by \[@allison\_horst\](<https://twitter.com/allison_horst>)

Now, why is `by_speaker` untidy data? How would this data be organized
if it were tidy? Do not use any of your new `{tidyr}` skills yet, only
describe it.

**Response**: Having the speakers as the columns and the seasons as the
rows could be more appealing than vice versa. It will also make it
potentially easier to work with to look at speakers instead of seasons.

![](README-img/noun_pause.png) **Planned Pause Point**: If you have any
questions, contact your instructor. Otherwise feel free to continue on.

## Analysis

### Gather the… I mean `pivot_longer` the data

From before, I hope that it was evident that the variable *Season* is
spread across multiple columns and *Words* count is stored within the
cell values for these columns. Therefore, to “tidy-fy” these data we
need to reshape the information contained in columns `Season 1` through
`Season 10`. One way to do this is as follows:

``` r
friends_tidy <- by_speaker %>% 
  pivot_longer(
    cols = `Season 1`:`Season 10`,
    names_to = "Season",
    values_to = "Words"
  )

friends_tidy
```

    ## # A tibble: 60 x 3
    ##    speaker  Season    Words
    ##    <chr>    <chr>     <dbl>
    ##  1 Chandler Season 1   8796
    ##  2 Chandler Season 2   8248
    ##  3 Chandler Season 3   8952
    ##  4 Chandler Season 4   9695
    ##  5 Chandler Season 5   8611
    ##  6 Chandler Season 6  10764
    ##  7 Chandler Season 7   8712
    ##  8 Chandler Season 8   6262
    ##  9 Chandler Season 9  10980
    ## 10 Chandler Season 10  6589
    ## # … with 50 more rows

Notice that the columns for each season have a space in them:
“Season”SPACE“Number”. To be able to use column names with spaces, we
need to encapsulate them in backticks (the tickmark above your Tab key).
Also notice that I am directing the names of these old columns
(`Season 1` through `Season 10`) to a new column called `Season`. Since
I am creating this new column, I need to encapsulate the name in
quotation marks. Similarly with directing the values to a new `Words`
column.

There are a number of helper functions to help with selecting columns.

1.  Try reproducing the pivoted data in the `pivot_season_longer` code
    chunk using either the `contains` or `starts_with` helper functions
    from `{tidyselect}` (this is loaded with the `{tidyverse}` by
    default). Remember that you can view the help documentation by using
    the `?` method in your **Console**.
2.  Assign your reformatted dataset to `friends_tidy`, and
3.  Name your code chunk either `pivot_contains` or `pivot_ends`
    depending on which helper function you used.

``` r
friends_tidy2 <- by_speaker %>% 
  select(contains("spe"))
friends_tidy2
```

    ## # A tibble: 6 x 1
    ##   speaker 
    ##   <chr>   
    ## 1 Chandler
    ## 2 Joey    
    ## 3 Monica  
    ## 4 Phoebe  
    ## 5 Rachel  
    ## 6 Ross

### Save the dataset

Now that we have this tidy dataset, we might want to share this with a
collaborator or use in alternative analysis or visualization tools. We
can do this by writing it to a file within our directory.

1.  Using the `write_csv` and `here::here` functions, write the
    `friends_tidy` dataset to the `data` folder.
2.  Call the file that is being created “`friends_tidy.csv`”.
3.  Name your code chunk `writing_file`

``` r
write_csv(friends_tidy,here::here('data','friends_tidy.csv'))
```

Inspect that this file was created by looking in the `data` project
folder in the **Files** pane. You should see three files:
`by_speaker.csv`, `by_season.csv`, and `friends_tidy.csv`. Simply
verifying that it exists is sufficient.

<img src="README-img/noun_pause.png" alt="pause" width = "20"/>
<b>Planned Pause Point</b>: If you feel that you have a good
understanding of these commands, feel free to skip to the **Preparing
tables** section. The remainder of this activity will help to expand
these commands.

### Try it another way

While exploring the `data` project folder, there should have also been
an additional data files: `by_season.csv`. This is the same information
that we were just worked with, but structured in a different way.

1.  Write the code that reads in the data file.
2.  Assign this data file into meaningful objects.
3.  Name this code chunk `read_season_file`.

``` r
by_season <- read_csv(here::here("data","by_season.csv"))
by_season
```

    ## # A tibble: 10 x 7
    ##    season    Chandler  Joey Monica Phoebe Rachel  Ross
    ##    <chr>        <dbl> <dbl>  <dbl>  <dbl>  <dbl> <dbl>
    ##  1 Season 1      8796  6195   7851   6522   8854 10569
    ##  2 Season 2      8248  7207   7822   8713   8600  9382
    ##  3 Season 3      8952  8504   8301   9177   8809 10725
    ##  4 Season 4      9695  8541   7897   8560   9896  9178
    ##  5 Season 5      8611  9731   8703   8695  10352  9234
    ##  6 Season 6     10764 10923   8784   8372  11013 10307
    ##  7 Season 7      8712  9777   9397   7910  11813  8960
    ##  8 Season 8      6262 10695   8768   7688  11650 10295
    ##  9 Season 9     10980  9318   9400   9397   9915 10482
    ## 10 Season 10     6589  6886   6958   7420   7960  8289

Now,

1.  Pivot the `by_season` dataset so that it is now tidy,
2.  Assign it to an object called `speaker_tidy`, and
3.  Name the code chunk `pivot_speaker`.

``` r
speaker_tidy <- by_season %>% 
    pivot_longer(
    cols = 'Chandler':'Ross',
    names_to = "Speaker",
    values_to = "Words"
    )
speaker_tidy
```

    ## # A tibble: 60 x 3
    ##    season   Speaker  Words
    ##    <chr>    <chr>    <dbl>
    ##  1 Season 1 Chandler  8796
    ##  2 Season 1 Joey      6195
    ##  3 Season 1 Monica    7851
    ##  4 Season 1 Phoebe    6522
    ##  5 Season 1 Rachel    8854
    ##  6 Season 1 Ross     10569
    ##  7 Season 2 Chandler  8248
    ##  8 Season 2 Joey      7207
    ##  9 Season 2 Monica    7822
    ## 10 Season 2 Phoebe    8713
    ## # … with 50 more rows

#### Challenge: Total words spoken

If you were to use `by_season` and your skills from past activities,
describe (or code) how you would obtain the total number of words spoken
by each season and the total words spoken across the series (challenge,
can you display this information in one table?).

**Response**:

``` r
by_season %>% 
  mutate(Total=Chandler+Joey+Monica+Phoebe+Rachel+Ross,GrandTotal=sum(Total))
```

    ## # A tibble: 10 x 9
    ##    season    Chandler  Joey Monica Phoebe Rachel  Ross Total GrandTotal
    ##    <chr>        <dbl> <dbl>  <dbl>  <dbl>  <dbl> <dbl> <dbl>      <dbl>
    ##  1 Season 1      8796  6195   7851   6522   8854 10569 48787     538004
    ##  2 Season 2      8248  7207   7822   8713   8600  9382 49972     538004
    ##  3 Season 3      8952  8504   8301   9177   8809 10725 54468     538004
    ##  4 Season 4      9695  8541   7897   8560   9896  9178 53767     538004
    ##  5 Season 5      8611  9731   8703   8695  10352  9234 55326     538004
    ##  6 Season 6     10764 10923   8784   8372  11013 10307 60163     538004
    ##  7 Season 7      8712  9777   9397   7910  11813  8960 56569     538004
    ##  8 Season 8      6262 10695   8768   7688  11650 10295 55358     538004
    ##  9 Season 9     10980  9318   9400   9397   9915 10482 59492     538004
    ## 10 Season 10     6589  6886   6958   7420   7960  8289 44102     538004

If you were to use `by_speaker` and your skills from past activities,
describe (or code) how you would obtain the total number of words spoken
by each season and the total words spoken across the series (challenge,
can you display this information in one table?).

**Response**:

``` r
by_speaker %>% 
  mutate(s1Total=sum(`Season 1`)) %>% 
  select(s1Total)
```

    ## # A tibble: 6 x 1
    ##   s1Total
    ##     <dbl>
    ## 1   48787
    ## 2   48787
    ## 3   48787
    ## 4   48787
    ## 5   48787
    ## 6   48787

If you were to use `friends_tidy` object and your skills from past
activities, describe (or code) how you would obtain the total number of
words spoken by each season and the total words spoken across the series
(challenge, can you display this information in one table?).

**Response**:

``` r
friends_tidy %>% 
  mutate(GrandTotal=sum(Words)) %>% 
  filter(Season=="Season 1") %>% 
  mutate(s1Total=sum(Words))
```

    ## # A tibble: 6 x 5
    ##   speaker  Season   Words GrandTotal s1Total
    ##   <chr>    <chr>    <dbl>      <dbl>   <dbl>
    ## 1 Chandler Season 1  8796     538004   48787
    ## 2 Joey     Season 1  6195     538004   48787
    ## 3 Monica   Season 1  7851     538004   48787
    ## 4 Phoebe   Season 1  6522     538004   48787
    ## 5 Rachel   Season 1  8854     538004   48787
    ## 6 Ross     Season 1 10569     538004   48787

How does the code for `friends_tidy` compare to the two `by_` code?
Reflect on the process of writing this code and on the code itself.
Which is easier to write? Which is easier to read?

**Response**: Working with friends\_tidy was a lot easier to think
about.

#### Preparing tables

So far, we have taken a variable spread across multiple variables (e.g.,
word counts for each character) and pivoted so that we have tidy data
where,

1.  Each observation is its own row,
2.  Each variable is its own column, and
3.  Each value has its own cell.

Now we will use `pivot_wider` to “untidy-fy” data. You may be wondering,
“but you just said untidy data is bad…” However, like the tables that we
started off with, untidy data is useful at the end of an analysis to
create human-readable tables. Also, sometimes the analysis tool/function
that we want to use needs data organized in this format.

The code below takes our tidy dataset, then creates one variable for
each *Season* (like we started with).

``` r
friends_tidy %>% 
  pivot_wider(names_from = Season, values_from = Words)
```

    ## # A tibble: 6 x 11
    ##   speaker  `Season 1` `Season 2` `Season 3` `Season 4` `Season 5` `Season 6`
    ##   <chr>         <dbl>      <dbl>      <dbl>      <dbl>      <dbl>      <dbl>
    ## 1 Chandler       8796       8248       8952       9695       8611      10764
    ## 2 Joey           6195       7207       8504       8541       9731      10923
    ## 3 Monica         7851       7822       8301       7897       8703       8784
    ## 4 Phoebe         6522       8713       9177       8560       8695       8372
    ## 5 Rachel         8854       8600       8809       9896      10352      11013
    ## 6 Ross          10569       9382      10725       9178       9234      10307
    ## # … with 4 more variables: Season 7 <dbl>, Season 8 <dbl>, Season 9 <dbl>,
    ## #   Season 10 <dbl>

Write the code to take the tidy dataset and create one variable for each
*speaker*. Call this code chunk `speaker_wide`.

``` r
friends_tidy %>% 
  pivot_wider(names_from = speaker, values_from = Words)
```

    ## # A tibble: 10 x 7
    ##    Season    Chandler  Joey Monica Phoebe Rachel  Ross
    ##    <chr>        <dbl> <dbl>  <dbl>  <dbl>  <dbl> <dbl>
    ##  1 Season 1      8796  6195   7851   6522   8854 10569
    ##  2 Season 2      8248  7207   7822   8713   8600  9382
    ##  3 Season 3      8952  8504   8301   9177   8809 10725
    ##  4 Season 4      9695  8541   7897   8560   9896  9178
    ##  5 Season 5      8611  9731   8703   8695  10352  9234
    ##  6 Season 6     10764 10923   8784   8372  11013 10307
    ##  7 Season 7      8712  9777   9397   7910  11813  8960
    ##  8 Season 8      6262 10695   8768   7688  11650 10295
    ##  9 Season 9     10980  9318   9400   9397   9915 10482
    ## 10 Season 10     6589  6886   6958   7420   7960  8289

<img src="README-img/noun_pause.png" alt="pause" width = "20"/>
<b>Planned Pause Point</b>: If you feel that you have a good
understanding of these commands, feel free to start working on your
project. The remainder of this activity will help to expand these
commands.

### Dressing up tables

Explore the
[`knitr::kable`](https://bookdown.org/yihui/rmarkdown-cookbook/kable.html)
function and [`{kableExtra}`](https://haozhu233.github.io/kableExtra/).

A note before installing new packages: The HPC support team has [created
a
guide](https://hpcsupport.atlassian.net/servicedesk/customer/portal/3/article/562429973)
to help you avoid issues when working within the RStudio Server. The
information here is useful to read **prior** to experiencing issues and
I encourage you to read it now (1-2 minutes).

Use `knitr::kable` and `{kableExtra}` to dress up the `speaker_wide`
table.

### The Curb Cut Effect

> When you design well for disability, you design well for everybody
> else.

[Gary Karp on “The Curb Cut
Effect”](https://www.youtube.com/watch?v=iuIkFRZKfCU)

The curb cut is the ramp section of a side walk:

![Illustration of curb cuts by Jono
Hey](https://images.prismic.io/sketchplanations/f103d71e-0ecb-4e78-bc02-e6dce18d0ed8_SP+718+-+The+curb-cut+effect.png?auto=format&ixlib=react-9.0.3&h=1887.9781420765028&w=1600&q=75&dpr=1)

Summary tables are great ways to present information. However, there is
a big disparity in what sighted users see and what a screen reader goes
through for default tables.

Here is a short video (4:44) that showcases how screen readers interpret
elements on a webpage:

![Screen reader demo for digital accessibility by UC San
Fran](https://www.youtube.com/watch?v=dEbl5jvLKGQ)

**Update** your table in the previous section to add a *table caption*.

## Challenge: One more `pivot_wider`

While you used `pivot_wider` in this activity to make data “untidy”,
that is not the only time this function is useful. Sometimes multiple
variable names are stored in one column. Data from the government
usually has this issue. For example, see:

``` r
tidyr::us_rent_income
```

    ## # A tibble: 104 x 5
    ##    GEOID NAME       variable estimate   moe
    ##    <chr> <chr>      <chr>       <dbl> <dbl>
    ##  1 01    Alabama    income      24476   136
    ##  2 01    Alabama    rent          747     3
    ##  3 02    Alaska     income      32940   508
    ##  4 02    Alaska     rent         1200    13
    ##  5 04    Arizona    income      27517   148
    ##  6 04    Arizona    rent          972     4
    ##  7 05    Arkansas   income      23789   165
    ##  8 05    Arkansas   rent          709     5
    ##  9 06    California income      29454   109
    ## 10 06    California rent         1358     3
    ## # … with 94 more rows

1.  Restructure `us_rent_income` to have the following column names:

-   `GEOID`

-   `NAME`

-   `income_estimate`

-   `income_moe`

-   `rent_estimate`

-   `rent_moe`

    Hint: There are some additional arguments in `pivot_wider` that can
    assist with this. Look at the **Examples** in the help
    documentation.

2.  Assign this to `us_wide`

Now,

1.  Re-restructure so that you have the original column names of:

-   `GEOID`

-   `NAME`

-   `variable`

-   `estimate`

-   `moe`

    Hint: There are some additional arguments in `pivot_longer` that can
    assist with this. Look at the **Examples** in the help
    documentation.

2.  Assign this to `us_long`

## Attribution

This Activity is inspired by materials from Jenny Bryan’s [lotr
tidy](https://github.com/jennybc/lotr-tidy) repository.

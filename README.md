Activity 6 - Pivot!
================

It is assumed that you have read Chapters 9 and 10, and 12 from R4DS and
completed the Derive Information with dplyr Primer.

In this activity, you will:

-   Locate and read data or other files into R using the `here::here`
    function.
-   Restructure datasets into longer and wider formats using `{tidyr}`.
-   Separate and unite columns into a tidy format.
-   Dress up data tables to help with human readability.

## ☑️ Task 1: The Workflow

Remember that more detailed directions can be found in [Task 1 of
Activity
4](https://github.com/gvsu-sta518/activity04-data-pipelines#%EF%B8%8F-task-1-the-workflow).

![fork](README-img/fork-icon.png) **Fork** this repo and clone it to a
new [RStudio Project](https://rstudio.gvsu.edu/)

<img src="README-img/noun_pause.png" alt="pause" width = "20"/>
<b>Planned Pause Point</b>: If you have any questions, contact your
instructor or another group. We will complete this Activity during our
next class session

## `gather` and `spread` vs `pivot_longer` and `pivot_wider`

In the Primer, you learned about the `tidyr::gather` and `tidyr::spread`
functions. These two functions still exist within the **tidyverse**;
however, they have been superseded ![superseded
badge](README-img/lifecycle-superseded.png) and will no longer receive
new features. You will often see these [“lifecycle”
badges](https://lifecycle.r-lib.org/articles/stages.html) on
`{tidyverse}` packages and are meant to provide additional information
to users.

Hadley’s introduction to the new pivoting functions can be found at the
[tidyverse blog](https://tidyr.tidyverse.org/dev/articles/pivot.html).

In the `tidyr::gather` help documentation (i.e., type `?gather` in the
**Console**), we can see how to do similar calls using
`tidyr::pivot_longer`.

> `df %>% gather("key", "value", x, y, z)` is equivalent to
> `df %>% pivot_longer(cols = c(x, y, z), names_to = "key", values_to = "value")`

Similarly, in the `tidyr::spread` help documentation, we can see the
similarities to `tidyr::pivot_longer`.

> `df %>% spread(key, value)` is equivalent to
> `df %>% pivot_wider(names_from = key, values_from = value)`

An important thing to recognize between the two `pivot_` functions is
when we use quotation marks `"..."`.

-   For `pivot_wider`, we are specifying columns that **already exist**
    to restructure that information across new columns. Therefore, we do
    not need to use quotation marks.
-   For `pivot_longer`, we are specifying **new columns** to restructure
    the information into. Therefore, we need to use quotation marks.

For this Activity (and in STA 418/518) we will use `pivot_longer` and
`pivot_wider`.

## ☑️ Task 2: Complete the RMarkdown File

The `activity06-pivoting.Rmd` file contains the directions for this
activity. For the rest of this class period, you will complete the
RMarkdown document with your neighbor(s). Your instructor will be
circling and be available to help when needed.

Note that each person is working in their own repo. We are not worrying
about collaborating for the time being and instead will be working on
being more comfortable with the workflow for working between RStudio and
GitHub.

However, do not continue in this README document until you and your
neighbor(s) have completed your `.Rmd` files.

![Ross from the TV show FRIENDS yelling “Pivot!” while trying to move a
couch up a set of
stairs](https://media.giphy.com/media/oCjCwnuLpiWbfMb1UA/giphy.gif)

## ☑️ Task 3: Reflection

Take 5 minutes to draw a concept map that connects what you have learned
about `pivot_longer` and `pivot_wider`. For example, here is a concept
map for `dplyr::select`:

![RStudio concept map for the select function from the dplyr package in
R](https://raw.githubusercontent.com/rstudio/concept-maps/master/en/select.svg)

**Next**: Activity 7 will focus on combining information from multiple
datasets as well and creating maps!

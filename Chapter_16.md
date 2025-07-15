# Chapter_16_Factors


\#Factors are used for categorical variables, variables that have a
fixed and known set of possible values, or when you want to display
character vectors in a non-alphabetical order.

``` r
library(tidyverse)
```

    ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ✔ dplyr     1.1.4     ✔ readr     2.1.5
    ✔ forcats   1.0.0     ✔ stringr   1.5.1
    ✔ ggplot2   3.5.2     ✔ tibble    3.3.0
    ✔ lubridate 1.9.4     ✔ tidyr     1.3.1
    ✔ purrr     1.0.4     
    ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ✖ dplyr::filter() masks stats::filter()
    ✖ dplyr::lag()    masks stats::lag()
    ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

# Factor Basics

``` r
x1 <- c("Dec", "Apr", "Jan", "Mar")
```

``` r
x2 <- c("Dec", "Apr", "Jam", "Mar")
```

``` r
sort(x1)
```

    [1] "Apr" "Dec" "Jan" "Mar"

\#To create a factor you must start by creating a list of the valid
levels:

``` r
month_levels <- c(
  "Jan", "Feb", "Mar", "Apr", "May", "Jun",
  "Jul", "Aug", "Sep", "Oct", "Nov", "Dec"
)
```

``` r
y1 <- factor(x1, levels = month_levels)
y1
```

    [1] Dec Apr Jan Mar
    Levels: Jan Feb Mar Apr May Jun Jul Aug Sep Oct Nov Dec

``` r
sort(y1)
```

    [1] Jan Mar Apr Dec
    Levels: Jan Feb Mar Apr May Jun Jul Aug Sep Oct Nov Dec

``` r
y2 <- factor(x2, levels = month_levels)
y2
```

    [1] Dec  Apr  <NA> Mar 
    Levels: Jan Feb Mar Apr May Jun Jul Aug Sep Oct Nov Dec

``` r
factor(x1)
```

    [1] Dec Apr Jan Mar
    Levels: Apr Dec Jan Mar

``` r
fct(x1)
```

    [1] Dec Apr Jan Mar
    Levels: Dec Apr Jan Mar

``` r
levels(y2)
```

     [1] "Jan" "Feb" "Mar" "Apr" "May" "Jun" "Jul" "Aug" "Sep" "Oct" "Nov" "Dec"

``` r
csv <- "
month,value
Jan,12
Feb,56
Mar,12"

df <- read_csv(csv, col_types = cols(month = col_factor(month_levels)))
df$month
```

    [1] Jan Feb Mar
    Levels: Jan Feb Mar Apr May Jun Jul Aug Sep Oct Nov Dec

# General Social Survey

``` r
gss_cat
```

    # A tibble: 21,483 × 9
        year marital         age race  rincome        partyid    relig denom tvhours
       <int> <fct>         <int> <fct> <fct>          <fct>      <fct> <fct>   <int>
     1  2000 Never married    26 White $8000 to 9999  Ind,near … Prot… Sout…      12
     2  2000 Divorced         48 White $8000 to 9999  Not str r… Prot… Bapt…      NA
     3  2000 Widowed          67 White Not applicable Independe… Prot… No d…       2
     4  2000 Never married    39 White Not applicable Ind,near … Orth… Not …       4
     5  2000 Divorced         25 White Not applicable Not str d… None  Not …       1
     6  2000 Married          25 White $20000 - 24999 Strong de… Prot… Sout…      NA
     7  2000 Never married    36 White $25000 or more Not str r… Chri… Not …       3
     8  2000 Divorced         44 White $7000 to 7999  Ind,near … Prot… Luth…      NA
     9  2000 Married          44 White $25000 or more Not str d… Prot… Other       0
    10  2000 Married          47 White $25000 or more Strong re… Prot… Sout…       3
    # ℹ 21,473 more rows

``` r
gss_cat |>
  count(race)
```

    # A tibble: 3 × 2
      race      n
      <fct> <int>
    1 Other  1959
    2 Black  3129
    3 White 16395

# Exercises pt 1 of 3

# Question 1

``` r
ggplot(gss_cat, mapping = aes(x = rincome)) +
  geom_bar(width = 1 )
```

![](Chapter_16_files/figure-commonmark/unnamed-chunk-14-1.png)

\#Overlapping categories make this plot hard to read. \#Improved:

``` r
gss_cat %>%
  mutate(rincome = fct_collapse(rincome,
    "No answer" = c("No answer", "Don't know", "Refused"),
    "$0 to 4999" = c("Lt $1000", "$1000 to 2999", "$3000 to 3999", "$4000 to 4999"),
    "$5000 to 9999" = c("$5000 to 5999", "$6000 to 6999", "$7000 to 7999", "$8000 to 9999")
  )) %>%
  ggplot(aes(x = rincome)) +
  geom_bar() +
  coord_flip() +
  labs(x = "Reported Income", y = "Count")
```

![](Chapter_16_files/figure-commonmark/unnamed-chunk-15-1.png)

# Question 2

``` r
ggplot(gss_cat, aes(x = relig)) +
  geom_bar() +
  coord_flip() +
  labs(y = "Count", x = "Religion")
```

![](Chapter_16_files/figure-commonmark/unnamed-chunk-16-1.png)

\#Protestant is the most popular religion

``` r
ggplot(gss_cat, aes(x = partyid)) +
  geom_bar() +
  coord_flip() +
  labs(y = "Count", x = "Party Identity")
```

![](Chapter_16_files/figure-commonmark/unnamed-chunk-17-1.png)

\#Independent is the most common party identity \#mQuestion 3

``` r
library(dplyr)
gss_cat %>%
  count(denom, relig)
```

    # A tibble: 47 × 3
       denom           relig          n
       <fct>           <fct>      <int>
     1 No answer       No answer     93
     2 No answer       Christian      2
     3 No answer       Protestant    22
     4 Don't know      Christian     11
     5 Don't know      Protestant    41
     6 No denomination Christian    452
     7 No denomination Other          7
     8 No denomination Protestant  1224
     9 Other           Protestant  2534
    10 Episcopal       Protestant   397
    # ℹ 37 more rows

# Modifying Factor Order

``` r
relig_summary <- gss_cat |>
  group_by(relig) |>
  summarize(
    tvhours = mean(tvhours, na.rm = TRUE),
    n = n()
  )

ggplot(relig_summary, aes(x = tvhours, y = relig)) +
  geom_point()
```

![](Chapter_16_files/figure-commonmark/unnamed-chunk-19-1.png)

\#fct_reorder() takes three arguments: \#“.f”, the factor whose levels
you want to modify. \#“.x”, a numeric vector that you want to use to
reorder the levels. \#Optionally, “.fun”, a function that’s used if
there are multiple values of “.x” for each value of “.f”. The default
value is median.

``` r
ggplot(relig_summary, aes(x = tvhours, y = fct_reorder(relig, tvhours))) +
  geom_point()
```

![](Chapter_16_files/figure-commonmark/unnamed-chunk-20-1.png)

``` r
relig_summary |>
  mutate(
    relig = fct_reorder(relig, tvhours)
  ) |>
  ggplot(aes(x = tvhours, y = relig)) +
  geom_point()
```

![](Chapter_16_files/figure-commonmark/unnamed-chunk-21-1.png)

``` r
rincome_summary <- gss_cat |>
  group_by(rincome) |>
  summarize(
    age = mean(age, na.rm = TRUE),
    n = n()
  )

ggplot(rincome_summary, aes(x = age, y = fct_reorder(rincome, age))) +
  geom_point()
```

![](Chapter_16_files/figure-commonmark/unnamed-chunk-22-1.png)

``` r
ggplot(rincome_summary, aes(x = age, y = fct_relevel(rincome, "Not applicable"))) +
  geom_point()
```

![](Chapter_16_files/figure-commonmark/unnamed-chunk-23-1.png)

``` r
by_age <- gss_cat |>
  filter(!is.na(age)) |>
  count(age, marital) |>
  group_by(age) |>
  mutate(
    prop = n / sum(n)
  )

ggplot(by_age, aes(x = age, y = prop, color = marital)) +
  geom_line(linewidth = 1) +
  scale_color_brewer(palette = "Set1")
```

![](Chapter_16_files/figure-commonmark/unnamed-chunk-24-1.png)

``` r
ggplot(by_age, aes(x = age, y = prop, color = fct_reorder2(marital, age, prop))) +
  geom_line(linewidth = 1) +
  scale_color_brewer(palette = "Set1") +
  labs(color = "marital")
```

![](Chapter_16_files/figure-commonmark/unnamed-chunk-24-2.png)

``` r
gss_cat |>
  mutate(marital = marital |> fct_infreq() |> fct_rev()) |>
  ggplot(aes(x = marital)) +
  geom_bar()
```

![](Chapter_16_files/figure-commonmark/unnamed-chunk-25-1.png)

# Exercises pt 2 of 3

# Question 1

``` r
ggplot(gss_cat, aes(x = tvhours)) +
  geom_bar()
```

    Warning: Removed 10146 rows containing non-finite outside the scale range
    (`stat_count()`).

![](Chapter_16_files/figure-commonmark/unnamed-chunk-26-1.png)

\#Because there are high outliers in the tv hour data, the mean would
likely not be a good summary as it would be skewed upward. \# Question 2
\#marital = arbitrary, rae = arbitrary, rincome = principled, partyid =
pricipled, relgi = arbitrary, denom = arbitrary

# Question 3

\#Labels start from the axis of the graph and go upwards.

# Modifying Factor Levels

``` r
gss_cat |> count(partyid)
```

    # A tibble: 10 × 2
       partyid                n
       <fct>              <int>
     1 No answer            154
     2 Don't know             1
     3 Other party          393
     4 Strong republican   2314
     5 Not str republican  3032
     6 Ind,near rep        1791
     7 Independent         4119
     8 Ind,near dem        2499
     9 Not str democrat    3690
    10 Strong democrat     3490

``` r
gss_cat |>
  mutate(
    partyid = fct_recode(partyid,
      "Republican, strong"    = "Strong republican",
      "Republican, weak"      = "Not str republican",
      "Independent, near rep" = "Ind,near rep",
      "Independent, near dem" = "Ind,near dem",
      "Democrat, weak"        = "Not str democrat",
      "Democrat, strong"      = "Strong democrat"
    )
  ) |>
  count(partyid)
```

    # A tibble: 10 × 2
       partyid                   n
       <fct>                 <int>
     1 No answer               154
     2 Don't know                1
     3 Other party             393
     4 Republican, strong     2314
     5 Republican, weak       3032
     6 Independent, near rep  1791
     7 Independent            4119
     8 Independent, near dem  2499
     9 Democrat, weak         3690
    10 Democrat, strong       3490

``` r
gss_cat |>
  mutate(
    partyid = fct_recode(partyid,
      "Republican, strong"    = "Strong republican",
      "Republican, weak"      = "Not str republican",
      "Independent, near rep" = "Ind,near rep",
      "Independent, near dem" = "Ind,near dem",
      "Democrat, weak"        = "Not str democrat",
      "Democrat, strong"      = "Strong democrat",
      "Other"                 = "No answer",
      "Other"                 = "Don't know",
      "Other"                 = "Other party"
    )
  )
```

    # A tibble: 21,483 × 9
        year marital         age race  rincome        partyid    relig denom tvhours
       <int> <fct>         <int> <fct> <fct>          <fct>      <fct> <fct>   <int>
     1  2000 Never married    26 White $8000 to 9999  Independe… Prot… Sout…      12
     2  2000 Divorced         48 White $8000 to 9999  Republica… Prot… Bapt…      NA
     3  2000 Widowed          67 White Not applicable Independe… Prot… No d…       2
     4  2000 Never married    39 White Not applicable Independe… Orth… Not …       4
     5  2000 Divorced         25 White Not applicable Democrat,… None  Not …       1
     6  2000 Married          25 White $20000 - 24999 Democrat,… Prot… Sout…      NA
     7  2000 Never married    36 White $25000 or more Republica… Chri… Not …       3
     8  2000 Divorced         44 White $7000 to 7999  Independe… Prot… Luth…      NA
     9  2000 Married          44 White $25000 or more Democrat,… Prot… Other       0
    10  2000 Married          47 White $25000 or more Republica… Prot… Sout…       3
    # ℹ 21,473 more rows

``` r
gss_cat |>
  mutate(
    partyid = fct_collapse(partyid,
      "other" = c("No answer", "Don't know", "Other party"),
      "rep" = c("Strong republican", "Not str republican"),
      "ind" = c("Ind,near rep", "Independent", "Ind,near dem"),
      "dem" = c("Not str democrat", "Strong democrat")
    )
  ) |>
  count(partyid)
```

    # A tibble: 4 × 2
      partyid     n
      <fct>   <int>
    1 other     548
    2 rep      5346
    3 ind      8409
    4 dem      7180

``` r
gss_cat |>
  mutate(relig = fct_lump_lowfreq(relig)) |>
  count(relig)
```

    # A tibble: 2 × 2
      relig          n
      <fct>      <int>
    1 Protestant 10846
    2 Other      10637

``` r
gss_cat |>
  mutate(relig = fct_lump_n(relig, n = 10)) |>
  count(relig, sort = TRUE)
```

    # A tibble: 10 × 2
       relig                       n
       <fct>                   <int>
     1 Protestant              10846
     2 Catholic                 5124
     3 None                     3523
     4 Christian                 689
     5 Other                     458
     6 Jewish                    388
     7 Buddhism                  147
     8 Inter-nondenominational   109
     9 Moslem/islam              104
    10 Orthodox-christian         95

# Exercises pt 3 of 3

# Question 1

``` r
gss_cat %>%
  mutate(
    partyid = fct_collapse(
      partyid,
      Democrat = c("Strong democrat", "Not str democrat"),
      Republican = c("Strong republican", "Not str republican"),
      Independent = c("Ind,near rep", "Independent", "Ind,near dem"),
      Other = c("No answer", "Don't know", "Other party")
    )
  ) %>%
  count(year, partyid) %>%
  group_by(year) %>%
  mutate(prop = n / sum(n)) %>%
  ggplot(aes(x = year, y = prop, color = partyid)) +
  geom_line(size = 1.1) +
  labs(
    x = "Year",
    y = "Proportion",
    color = "Party ID"
  )
```

    Warning: Using `size` aesthetic for lines was deprecated in ggplot2 3.4.0.
    ℹ Please use `linewidth` instead.

![](Chapter_16_files/figure-commonmark/unnamed-chunk-33-1.png)

\#No noticable trend?

# Question 2

``` r
gss_cat %>%
  mutate(
    rincome = fct_collapse(
      rincome,
      Other = c("No answer", "Don't know", "Refused", "Not applicable"),
      `15,000+` = c("$15000 - 19999", "$20000 - 24999", "25000 or more"),
      `5,000 or less` = c("Lt $1000", "$1000 to 2999", "$3000 to 3999", "$4000 to 4999"),
      `5,000 - 15,000` = c("$5000 to 5999", "$6000 to 6999", "$7000 to 7999", "$8000 to 9999", "$10000 - 14999")
    )
  )
```

    Warning: There was 1 warning in `mutate()`.
    ℹ In argument: `rincome = fct_collapse(...)`.
    Caused by warning:
    ! Unknown levels in `f`: 25000 or more

    # A tibble: 21,483 × 9
        year marital         age race  rincome        partyid    relig denom tvhours
       <int> <fct>         <int> <fct> <fct>          <fct>      <fct> <fct>   <int>
     1  2000 Never married    26 White 5,000 - 15,000 Ind,near … Prot… Sout…      12
     2  2000 Divorced         48 White 5,000 - 15,000 Not str r… Prot… Bapt…      NA
     3  2000 Widowed          67 White Other          Independe… Prot… No d…       2
     4  2000 Never married    39 White Other          Ind,near … Orth… Not …       4
     5  2000 Divorced         25 White Other          Not str d… None  Not …       1
     6  2000 Married          25 White 15,000+        Strong de… Prot… Sout…      NA
     7  2000 Never married    36 White $25000 or more Not str r… Chri… Not …       3
     8  2000 Divorced         44 White 5,000 - 15,000 Ind,near … Prot… Luth…      NA
     9  2000 Married          44 White $25000 or more Not str d… Prot… Other       0
    10  2000 Married          47 White $25000 or more Strong re… Prot… Sout…       3
    # ℹ 21,473 more rows

# Question 3

\#fct_lump_n() lumps ALL levels except for the n most frequent (or least
frequent if n \< 0)

# Ordered Factors

``` r
ordered(c("a", "b", "c"))
```

    [1] a b c
    Levels: a < b < c

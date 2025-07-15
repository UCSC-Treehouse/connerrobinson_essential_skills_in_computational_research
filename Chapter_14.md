# Chapter_14


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

``` r
library(babynames)
```

# Strings

``` r
string1 <- "This is a string"
string2 <- 'If I want to include a "quote" inside a string, I use single quotes'
```

\#str_view(): To see the raw contents of the string

# Escapes

``` r
double_quote <- "\"" # or '"'
single_quote <- '\'' # or "'"
backslash <- "\\"
```

``` r
x2 <- c(single_quote, double_quote, backslash)
x2
```

    [1] "'"  "\"" "\\"

``` r
str_view(x2)
```

    [1] │ '
    [2] │ "
    [3] │ \

# Raw Strings

``` r
tricky <- "double_quote <- \"\\\"\" # or '\"'
single_quote <- '\\'' # or \"'\""
str_view(tricky)
```

    [1] │ double_quote <- "\"" # or '"'
        │ single_quote <- '\'' # or "'"

``` r
tricky <- r"(double_quote <- "\"" # or '"'
single_quote <- '\'' # or "'")"
str_view(tricky)
```

    [1] │ double_quote <- "\"" # or '"'
        │ single_quote <- '\'' # or "'"

# Other Special Characters

``` r
x1 <- c("one\ntwo", "one\ttwo", "\u00b5", "\U0001f604")
x1
```

    [1] "one\ntwo" "one\ttwo" "µ"        "😄"      

``` r
str_view(x1)
```

    [1] │ one
        │ two
    [2] │ one{\t}two
    [3] │ µ
    [4] │ 😄

\#You can see the complete list of other special characters in ?Quotes.

# Exercises pt 1 of 3

# Question 1

``` r
string_1 <- "He said \"That’s amazing!\""
string_2 <- "\\"
string_3 <- ''
str_view(string_1)
```

    [1] │ He said "That’s amazing!"

``` r
str_view(string_2)
```

    [1] │ \

``` r
str_view(string_3)
```

    [1] │ 

# Question 2

``` r
t <- "This\u00a0is\u00a0tricky"
print(t)
```

    [1] "This is tricky"

``` r
str_view(t)
```

    [1] │ This{\u00a0}is{\u00a0}tricky

\#0a0 is a Unicode escape sequence, representing the non-breaking space
character

# str_c()

\#str_c() takes any number of vectors as arguments and returns a
character vector

``` r
str_c("x", "y")
```

    [1] "xy"

``` r
str_c("x", "y", "z")
```

    [1] "xyz"

``` r
str_c("Hello ", c("John", "Susan"))
```

    [1] "Hello John"  "Hello Susan"

``` r
df <- tibble(name = c("Flora", "David", "Terra", NA))
df |> mutate(greeting = str_c("Hi ", name, "!"))
```

    # A tibble: 4 × 2
      name  greeting 
      <chr> <chr>    
    1 Flora Hi Flora!
    2 David Hi David!
    3 Terra Hi Terra!
    4 <NA>  <NA>     

``` r
df |> 
  mutate(
    greeting1 = str_c("Hi ", coalesce(name, "you"), "!"),
    greeting2 = coalesce(str_c("Hi ", name, "!"), "Hi!")
  )
```

    # A tibble: 4 × 3
      name  greeting1 greeting2
      <chr> <chr>     <chr>    
    1 Flora Hi Flora! Hi Flora!
    2 David Hi David! Hi David!
    3 Terra Hi Terra! Hi Terra!
    4 <NA>  Hi you!   Hi!      

# str_glue()

# give it a single string that has a special feature: anything inside {} will be evaluated like it’s outside of the quotes:

``` r
df |> mutate(greeting = str_glue("Hi {name}!"))
```

    # A tibble: 4 × 2
      name  greeting 
      <chr> <glue>   
    1 Flora Hi Flora!
    2 David Hi David!
    3 Terra Hi Terra!
    4 <NA>  Hi NA!   

``` r
df |> mutate(greeting = str_glue("{{Hi {name}!}}"))
```

    # A tibble: 4 × 2
      name  greeting   
      <chr> <glue>     
    1 Flora {Hi Flora!}
    2 David {Hi David!}
    3 Terra {Hi Terra!}
    4 <NA>  {Hi NA!}   

# str_flatten()

\#str_flatten(): takes a character vector and combines each element of
the vector into a single string:

``` r
str_flatten(c("x", "y", "z"))
```

    [1] "xyz"

``` r
str_flatten(c("x", "y", "z"), ", ")
```

    [1] "x, y, z"

``` r
str_flatten(c("x", "y", "z"), ", ", last = ", and ")
```

    [1] "x, y, and z"

``` r
df <- tribble(
  ~ name, ~ fruit,
  "Carmen", "banana",
  "Carmen", "apple",
  "Marvin", "nectarine",
  "Terence", "cantaloupe",
  "Terence", "papaya",
  "Terence", "mandarin"
)
df |>
  group_by(name) |> 
  summarize(fruits = str_flatten(fruit, ", "))
```

    # A tibble: 3 × 2
      name    fruits                      
      <chr>   <chr>                       
    1 Carmen  banana, apple               
    2 Marvin  nectarine                   
    3 Terence cantaloupe, papaya, mandarin

# Exercises pt 2 of 3

# Question 1

``` r
str_c("hi ", NA)
```

    [1] NA

``` r
paste0("hi ", NA)
```

    [1] "hi NA"

``` r
paste0(letters[1:2], letters[1:3])
```

    [1] "aa" "bb" "ac"

\#str_c() returns only NA if in input

# Question 2

\#paste(): joins strings together, inserting a space between them
\#paste0(): joins strings together without a space between them
\#str_c(), like paste0() does not add a space. To mimic paste() the sep
argument can be used at the end of the string.

``` r
str_c("a", "b", "c", sep = " ")
```

    [1] "a b c"

# Question 3

``` r
food <- "cookie"
price <- 1.00
str_glue("The price of {food} is {price}")
```

    The price of cookie is 1

``` r
age <- 20
country <- "United States"
str_c("I’m ", age, " years old and live in ", country)
```

    [1] "I’m 20 years old and live in United States"

``` r
title <- "Manager"
str_glue("\\section{{{title}}}")
```

    \section{Manager}

# Extracting Data from Strings

\#Four tiydr functions: \#df \|\> separate_longer_delim(col, delim) \#df
\|\> separate_longer_position(col, width) \#df \|\>
separate_wider_delim(col, delim, names) \#df \|\>
separate_wider_position(col, widths)

# Separating into Rows

\#use “longer” in separate_wider_delim()

``` r
df1 <- tibble(x = c("a,b,c", "d,e", "f"))
df1 |> 
  separate_longer_delim(x, delim = ",")
```

    # A tibble: 6 × 1
      x    
      <chr>
    1 a    
    2 b    
    3 c    
    4 d    
    5 e    
    6 f    

``` r
df2 <- tibble(x = c("1211", "131", "21"))
df2 |> 
  separate_longer_position(x, width = 1)
```

    # A tibble: 9 × 1
      x    
      <chr>
    1 1    
    2 2    
    3 1    
    4 1    
    5 1    
    6 3    
    7 1    
    8 2    
    9 1    

\#delim = delimiter

# Separating into Columns

\#use “wider” in separate_wider_delim(0

``` r
df3 <- tibble(x3 = c("a10.1.2022", "b10.2.2011", "e15.1.2015"))
df3 |> 
  separate_wider_delim(
    x3,
    delim = ".",
    names = c("code", "edition", "year")
  )
```

    # A tibble: 3 × 3
      code  edition year 
      <chr> <chr>   <chr>
    1 a10   1       2022 
    2 b10   2       2011 
    3 e15   1       2015 

``` r
df4 <- tibble(x4 = c("202215TX", "202122LA", "202325CA")) 
df4 |> 
  separate_wider_position(
    x4,
    widths = c(year = 4, age = 2, state = 2)
  )
```

    # A tibble: 3 × 3
      year  age   state
      <chr> <chr> <chr>
    1 2022  15    TX   
    2 2021  22    LA   
    3 2023  25    CA   

# Diagnosing Widening Problems

\#separate_wider_delim() requires a fixed and known set of columns. What
happens if some of the rows don’t have the expected number of pieces?

\#x_pieces tells us how many pieces were found, compared to the expected
3

df \|\> separate_wider_delim( x, delim = “-”, names = c(“x”, “y”, “z”),
too_few = “align_start”

# Letters: Length

\#str_length() tells you the number of letters in the string

``` r
str_length(c("a", "R for data science", NA))
```

    [1]  1 18 NA

``` r
babynames |>
  count(length = str_length(name), wt = n)
```

    # A tibble: 14 × 2
       length        n
        <int>    <int>
     1      2   338150
     2      3  8589596
     3      4 48506739
     4      5 87011607
     5      6 90749404
     6      7 72120767
     7      8 25404066
     8      9 11926551
     9     10  1306159
    10     11  2135827
    11     12    16295
    12     13    10845
    13     14     3681
    14     15      830

``` r
babynames |> 
  filter(str_length(name) == 15) |> 
  count(name, wt = n, sort = TRUE)
```

    # A tibble: 34 × 2
       name                n
       <chr>           <int>
     1 Franciscojavier   123
     2 Christopherjohn   118
     3 Johnchristopher   118
     4 Christopherjame   108
     5 Christophermich    52
     6 Ryanchristopher    45
     7 Mariadelosangel    28
     8 Jonathanmichael    25
     9 Christianjoseph    22
    10 Christopherjose    22
    # ℹ 24 more rows

# Letters: Subsetting

\#You can extract parts of a string using str_sub(string, start, end)

``` r
x <- c("Apple", "Banana", "Pear")
str_sub(x, 1, 3)
```

    [1] "App" "Ban" "Pea"

``` r
str_sub(x, -3, -1)
```

    [1] "ple" "ana" "ear"

``` r
str_sub("a", 1, 5)
```

    [1] "a"

``` r
babynames |> 
  mutate(
    first = str_sub(name, 1, 1),
    last = str_sub(name, -1, -1)
  )
```

    # A tibble: 1,924,665 × 7
        year sex   name          n   prop first last 
       <dbl> <chr> <chr>     <int>  <dbl> <chr> <chr>
     1  1880 F     Mary       7065 0.0724 M     y    
     2  1880 F     Anna       2604 0.0267 A     a    
     3  1880 F     Emma       2003 0.0205 E     a    
     4  1880 F     Elizabeth  1939 0.0199 E     h    
     5  1880 F     Minnie     1746 0.0179 M     e    
     6  1880 F     Margaret   1578 0.0162 M     t    
     7  1880 F     Ida        1472 0.0151 I     a    
     8  1880 F     Alice      1414 0.0145 A     e    
     9  1880 F     Bertha     1320 0.0135 B     a    
    10  1880 F     Sarah      1288 0.0132 S     h    
    # ℹ 1,924,655 more rows

# Exercises pt 3 of 3

# Question 1

\#The arugment wt = n in the count() function plotted the count or
weight of baby names in the “n” column.

# Question 2

\#Use name_lengths/2 \#Question 3

``` r
babynames <- babynames %>%
  mutate(name_length = str_length(name))
```

``` r
avg_length <- babynames %>%
  group_by(year) %>%
  summarise(avg_length = weighted.mean(name_length, n))

ggplot(avg_length, aes(x = year, y = avg_length)) +
  geom_line(color = "steelblue") +
  labs(x = "Year", y = "Average Name Length")
```

![](Chapter_14_files/figure-commonmark/unnamed-chunk-34-1.png)

\#The average length of baby names has gradually increased over time,
peaking in the late 1900s.

``` r
babynames <- babynames %>%
  mutate(first_letter = str_sub(name, 1, 1))

first_letters <- babynames %>%
  group_by(year, first_letter) %>%
  summarise(total = sum(n), .groups = "drop")

# Plot for a few key letters (e.g., A, J, M)
letters_to_plot <- c("A", "J", "M")
ggplot(filter(first_letters, first_letter %in% letters_to_plot),
       aes(x = year, y = total, color = first_letter)) +
  geom_line() +
  labs(x = "Year", y = "Number of Babies")
```

![](Chapter_14_files/figure-commonmark/unnamed-chunk-35-1.png)

\#The first letters A, J, and M have remained the most popular over
time.

``` r
babynames <- babynames %>%
  mutate(last_letter = str_sub(name, -1, -1))

last_letters <- babynames %>%
  group_by(year, last_letter) %>%
  summarise(total = sum(n), .groups = "drop")

letters_to_plot <- c("a", "n", "e")
ggplot(filter(last_letters, last_letter %in% letters_to_plot),
       aes(x = year, y = total, color = last_letter)) +
  geom_line() +
  labs(x = "Year", y = "Number of Babies")
```

![](Chapter_14_files/figure-commonmark/unnamed-chunk-36-1.png)

\#The last letters a, e, and n have increased in popularity over time
for baby names.

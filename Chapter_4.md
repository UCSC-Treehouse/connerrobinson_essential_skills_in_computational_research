# Chapter_4


# install core packages (run once)

renv::install(c(“nycflights13”, “tidyverse”)) install.packages(“styler”)
library(tidyverse) library(nycflights13)

# Pipes

\#Pipes should alwasy have a space before it and should be the last
thing on a line flights \|\>  
filter(!is.na(arr_delay), !is.na(tailnum)) \|\> count(dest)

# ggplot2

\#Use + the same way as a pipe flights \|\> group_by(month) \|\>
summarize( delay = mean(arr_delay, na.rm = TRUE) ) \|\> ggplot(aes(x =
month, y = delay)) + geom_point() + geom_line()

# Exercise 1

flights \|\> filter(dest == “IAH”) \|\> group_by(year, month,
day)\|\>summarize( n = n(), delay = mean(arr_delay,na.rm = TRUE)) \|\>
filter(n\>10)

flights \|\> filter(carrier == “UA”, dest%in%c(“IAH”,“HOU”),
sched_dep_time\> 0900, sched_arr_time\<2000) \|\> group_by(flight) \|\>
summarize(delay = mean( arr_delay, na.rm = TRUE),cancelled =
sum(is.na(arr_delay)), n = n()) \|\> filter(n \> 10)

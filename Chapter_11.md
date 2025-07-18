# Chapter_11


library(tidyverse) library(scales) install.packages(“ggrepel”)
install.packages(“patchwork”) library(ggrepel) library(patchwork)

# Labels

ggplot(mpg, aes(x = displ, y = hwy)) + geom_point(aes(color = class)) +
geom_smooth(se = FALSE) + labs( x = “Engine displacement (L)”, y =
“Highway fuel economy (mpg)”, color = “Car type”, title = “Fuel
efficiency generally decreases with engine size”, subtitle = “Two
seaters (sports cars) are an exception because of their light weight”,
caption = “Data from fueleconomy.gov” ) \#Plotting Math Equations use
?plotmath; ex: df \<- tibble( x = 1:10, y = cumsum(x^2) )

ggplot(df, aes(x, y)) + geom_point() + labs( x = quote(x\[i\]), y =
quote(sum(x\[i\] ^ 2, i == 1, n)) )

# Exercises pt 1 of 5

# Question 1

ggplot(mpg, aes(x = displ, y = hwy)) + geom_point(aes(color = class)) +
geom_smooth(se = FALSE) + labs( x = “x-axis”, y = “y-axis”, color = “Car
Type”, title = “Bad Naming”, subtitle = “haha” )

# Question 2

ggplot(mpg, aes(x = cty, y = hwy)) + geom_point(aes(color = drv, shape =
drv)) + labs( x = “City MPG”, y = “Highway MPG”, color = “Type of drive
train” ) \# Annotations \#geom_text() is similar to geom_point(), but it
has an additional aesthetic: label. This makes it possible to add
textual labels to your plots. label_info \<- mpg \|\> group_by(drv) \|\>
arrange(desc(displ)) \|\> slice_head(n = 1) \|\> mutate( drive_type =
case_when( drv == “f” ~ “front-wheel drive”, drv == “r” ~ “rear-wheel
drive”, drv == “4” ~ “4-wheel drive” ) ) \|\> select(displ, hwy, drv,
drive_type)

label_info

ggplot(mpg, aes(x = displ, y = hwy, color = drv)) + geom_point(alpha =
0.3) + geom_smooth(se = FALSE) + geom_text( data = label_info, aes(x =
displ, y = hwy, label = drive_type), fontface = “bold”, size = 5, hjust
= “right”, vjust = “bottom” ) + theme(legend.position = “none”)

ggplot(mpg, aes(x = displ, y = hwy, color = drv)) + geom_point(alpha =
0.3) + geom_smooth(se = FALSE) + geom_label_repel( data = label_info,
aes(x = displ, y = hwy, label = drive_type), fontface = “bold”, size =
5, nudge_y = 2 ) + theme(legend.position = “none”)

potential_outliers \<- mpg \|\> filter(hwy \> 40 \| (hwy \> 20 & displ
\> 5))

ggplot(mpg, aes(x = displ, y = hwy)) + geom_point() +
geom_text_repel(data = potential_outliers, aes(label = model)) +
geom_point(data = potential_outliers, color = “red”) + geom_point( data
= potential_outliers, color = “red”, size = 3, shape = “circle open” )

trend_text \<- “Larger engine sizes tend to have lower fuel economy.”
\|\> str_wrap(width = 30) trend_text

ggplot(mpg, aes(x = displ, y = hwy)) + geom_point() + annotate( geom =
“label”, x = 3.5, y = 38, label = trend_text, hjust = “left”, color =
“red” ) + annotate( geom = “segment”, x = 3, y = 35, xend = 5, yend =
25, color = “red”, arrow = arrow(type = “closed”) )

# Exercises pt 2 of 5

# Question 1 and 2

corners \<- data.frame( label = c(“Top Left”, “Top Right”, “Bottom
Left”, “Bottom Right”), x = c(-Inf, Inf, -Inf, Inf), y = c( Inf, Inf,
-Inf, -Inf) ) ggplot(mpg, aes(x = displ, y = hwy, color = drv)) +
geom_point() geom_text(data = corners, aes(x = x, y = y, label = label),
hjust = c(0, 1, 0, 1), \# Align text to the plot edges vjust = c(1, 1,
0, 0) ) + annotate( “point”, x = mean(range(mpg$displ)),
      y = mean(range(mpg$hwy)), shape = 17,  
size = 5,  
color = “red” ) \#Question 3 \#When you use geom_text() in a faceted
plot, the labels will appear in every facet unless your label data
includes the faceting variable(s)

# Question 4

# The arguments: fill, label.size, colour, label.padding, label.r

# Default Scales

ggplot(mpg, aes(x = displ, y = hwy)) + geom_point(aes(color = class))
\#ggplot2 automatically adds default scales behind the scenes, adding
scales can allow you to change the axes or parameters

# Axis Ticks and Legend Keys

\#Breaks controls the position of the ticks, or the values associated
with the keys. Labels controls the text label associated with each
tick/key. \#labels = NULL can supress labels \#labels = label_dollar()
adds a dollar sign \#labels = label_percent() adds percentage sign

# Legend Layout

\#theme() controls the overall position of the legend \#legend.position
= “none” to suppress the display of the legend altogether \#guide90
controls the diplay of indivdual legends with guide_legend() or
guide_colorbar()

# Replacing a Scale

\#Types of scales: continous position and color scales \#
scale_x/y_log10() can help better display a relationship \#ColorBrewer
scales work better for color blind people; ex.
scale_color_brewer(palette = “Set1”) \# specific colors can be used with
scale_color_manual; ex. scale_color_manual(values = c(Republican =
“\#E81B23”, Democratic = “\#00AEF3”))

# Zooming

\#There are three ways to control the plot limits: \#1.Adjusting what
data are plotted. \#2.Setting the limits in each scale. \#3.Setting xlim
and ylim in coord_cartesian()

# Exercises pt 3 of 5

# Question 1

\#geom_hex() uses the fill aesthetic to represent the bin counts (or
densities), not the color aesthetic; you should use
scale_fill_gradient()

# Question 2

\#The first argument to every scale function is the axis or legend title

# Question 4

ggplot(diamonds, aes(x = carat, y = price)) + geom_point(aes(color =
cut), alpha = 1/20) + guides(color = guide_legend(override.aes =
list(alpha = 1, size = 3)))

# Themes

\#theme() + elements (legend.position, legend.direction,
legend.box.background, plot.title, plot. title.position, etc) can be
used

# Exercises pt 4 of 5

# Question 1

install.packages(‘ggthemes’, dependencies = TRUE) library(“ggthemes”)

ggplot(mpg,aes(x = displ, y = hwy, color = drv)) + geom_point() + labs(
title = “Larger engine sizes tend to have lower fuel economy”, caption =
“Source: https://fueleconomy.gov.” )+ theme_economist() \# Question 2
ggplot(mpg,aes(x = displ, y = hwy, color = drv)) + geom_point() + labs(
title = “Larger engine sizes tend to have lower fuel economy”, caption =
“Source: https://fueleconomy.gov.” )+ theme( axis.title.x =
element_text(color = “blue”, face = “bold”), axis.title.y =
element_text(color = “blue”, face = “bold”) )

# Layout

\#When you have multiple plots you want to have a certain layout you
need to create the plots and save them as objects (ex. p1 and p2) then
run a function to position them on the larger plot map \#ex. p1 \<-
ggplot(mpg, aes(x = displ, y = hwy)) + geom_point() + labs(title = “Plot
1”) p2 \<- ggplot(mpg, aes(x = drv, y = hwy)) + geom_boxplot() +
labs(title = “Plot 2”) p1 + p2 \#In the following, \| places the p1 and
p3 next to each other and / moves p2 to the next line. \#ex. p3 \<-
ggplot(mpg, aes(x = cty, y = hwy)) + geom_point() + labs(title = “Plot
3”) (p1 \| p3) / p2

# Exercises pt 5 of 5

# Question 1

# Question 2

p1 / (p2 \| p3)

---
layout: post
title: Tidy Tuesday with The Economist
---

Hi everyone! Here's my first #TidyTuesday project. I spent around 2-3 hours working on this. The data is based on Sarah Leo, a PHD
student and writer for The Economist, [revisiting bad graphs and re-doing them](https://medium.economist.com/mistakes-weve-drawn-a-few-8cdd8a42d368). This is the data that was a challenge to reinterpret,
mostly because there was a lot of categorical data that needed to share one discrete point of information. 

![The Economist Comparison](https://cdn-images-1.medium.com/max/1400/1*7GJIxnYsyFSGgQV537Ittg.png "")

[Science remains male-dominated was the title of the March 11th 2017 article](https://www.economist.com/science-and-technology/2017/03/11/science-remains-male-dominated).
So, the data itself should show us a compelling argument of male domination. Given the countries and fields, it would be helpful see how they compare. 

Initially, I wanted to contrast male vs female publishing by adding in the percentage of men who publish. I wrote a -(x-1) function, merged it, 
and used the ```molten``` package for the first time on the data set. Everything was still crowded. So I tried to facet out the country and field categories
and it looked like a chaotic mess. Bar chart and line/rectangular charts were out of the question. Decided to eliminate the guys, but not for the matriarchy, just for the simplicity. 

So I revisited the data later and asked what else I could do to show a strong difference? The aspiration was to show women publishing in
STEM with a wide equality gap. So I decided to asses the gap of (x -.5) and plot it. Everything to the right shows women lagging in publishing. 
Everything to the left shows women leading in publishing. 

While I could probably get the size down to the size of the orignial data visualization, I have other things to work on and this works fine.
Let me know if you have suggestions on how to improve this graph!

![Plotted by Gap](https://github.com/AdriMichelson/adrimichelson.github.io/blob/master/images/Rplot01.png "")



```R
#Packages
library(tidyverse)
library(ggthemes)
library(reshape2)
#Load Data
women_research <- readr::read_csv("https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2019/2019-04-16/women_research.csv")

# Calculate gap from equitable research of .5
what_number <- function(x){(.5-x)}

gap<- map_dbl(women_research$percent_women, what_number)

all_research <- women_research %>%
  mutate(gap)

#Plotting Fun
h <- ggplot(all_research, mapping = aes(gap, country, color = field, size = 8, )) + 
  geom_jitter(width = NULL , height = .1)+
  theme_economist(base_size = 7, dkpanel = TRUE)  + scale_size( guide="none") 

h <- h + theme(strip.background = element_blank(),
                strip.text.y = element_blank()) + 
    labs(title = "Closing the Female Publishing Gap", 
       subtitle = "The Gap of Equitable Publishing by Field, 2011-2015", 
       caption = "@80Data, #TidyTuesday Project") + 
    xlim(-.25, .5) +   
    geom_vline(xintercept=c(0), linetype="dotted")

h
...

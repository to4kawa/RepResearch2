---
title: "The most damaging event in STORM DATA"
subtitle: "Reproducible Research Course Project 2 Assignment"
auther: "Toshihisa Kawamata"
data: "2019/08/24"
output:
  html_document:
    keep_md: yes
    toc: yes
    toc_float: true
---

## Files
1. CourseProject2.Rmd  
2. CourseProject2.md  
3. README.md  

## Require library
1. library(dplyr)  
2. library(tidyr)  
3. library(ggplot2)  
4. library(R.utils)  
5. library(data.table)  

## Data

The data for this assignment come in the form of a comma-separated-value file compressed via the bzip2 algorithm to reduce its size. You can download the file from the course web site:

[Storm Data](https://d396qusza40orc.cloudfront.net/repdata%2Fdata%2FStormData.csv.bz2) [47Mb]
There is also some documentation of the database available. Here you will find how some of the variables are constructed/defined.

National Weather Service [Storm Data Documentation](https://d396qusza40orc.cloudfront.net/repdata%2Fpeer2_doc%2Fpd01016005curr.pdf)
National Climatic Data Center Storm Events [FAQ](https://d396qusza40orc.cloudfront.net/repdata%2Fpeer2_doc%2FNCDC%20Storm%20Events-FAQ%20Page.pdf)
The events in the database start in the year 1950 and end in November 2011. In the earlier years of the database there are generally fewer events recorded, most likely due to a lack of good records. More recent years should be considered more complete.

## Program description
This program is data analysis program that address the following questions:

1. Across the United States, which types of events (as indicated in the <span style="color: red; ">EVTYPE</span> variable) are most harmful with respect to population health?

2. Across the United States, which types of events have the greatest economic consequences?

## URL to reference

http://rpubs.com/tuenguyends/521422


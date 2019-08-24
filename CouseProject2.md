---
title: "The most damaging event in STORM DATA"
subtitle: "Reproducible Research Course Project 2 Assignment"
auther: "Toshihisa Kawamata"
data: "2019/08/24"
output:
  html_document:
    keep_md: yes
---

## Synopsis
 As a result of this analysis, it was found that TORNAD has the greatest human damage and FLOOD has the most damage.

## Data

The data for this assignment come in the form of a comma-separated-value file compressed via the bzip2 algorithm to reduce its size. You can download the file from the course web site:

[Storm Data](https://d396qusza40orc.cloudfront.net/repdata%2Fdata%2FStormData.csv.bz2) [47Mb]
There is also some documentation of the database available. Here you will find how some of the variables are constructed/defined.

National Weather Service [Storm Data Documentation](https://d396qusza40orc.cloudfront.net/repdata%2Fpeer2_doc%2Fpd01016005curr.pdf)
National Climatic Data Center Storm Events [FAQ](https://d396qusza40orc.cloudfront.net/repdata%2Fpeer2_doc%2FNCDC%20Storm%20Events-FAQ%20Page.pdf)
The events in the database start in the year 1950 and end in November 2011. In the earlier years of the database there are generally fewer events recorded, most likely due to a lack of good records. More recent years should be considered more complete.

## Task

### Assignment
The basic goal of this assignment is to explore the NOAA Storm Database and answer some basic questions about severe weather events. You must use the database to answer the questions below and show the code for your entire analysis. Your analysis can consist of tables, figures, or other summaries. You may use any R package you want to support your analysis.

### Questions
Your data analysis must address the following questions:

1. Across the United States, which types of events (as indicated in the <span style="color: red; ">EVTYPE</span> variable) are most harmful with respect to population health?
2. Across the United States, which types of events have the greatest economic consequences?

Consider writing your report as if it were to be read by a government or municipal manager who might be responsible for preparing for severe weather events and will need to prioritize resources for different types of events. However, there is no need to make any specific recommendations in your report.

### Requirements
For this assignment you will need some specific tools

RStudio: You will need RStudio to publish your completed analysis document to RPubs. You can also use RStudio to edit/write your analysis.
knitr: You will need the knitr package in order to compile your R Markdown document and convert it to HTML

### Document Layout
Language: Your document should be written in English.

Title: Your document should have a title that briefly summarizes your data analysis

Synopsis: Immediately after the title, there should be a synopsis which describes and summarizes your analysis in at most 10 complete sentences.

There should be a section titled Data Processing which describes (in words and code) how the data were loaded into R and processed for analysis.   
In particular, your analysis must start from the raw CSV file containing the data.  
You cannot do any preprocessing outside the document.   
If preprocessing is time-consuming you may consider using the <span style="color: red; ">cache=TRUE</span> option for certain code chunks.  
There should be a section titled Results in which your results are presented.  
You may have other sections in your analysis, but Data Processing and Results are required.
The analysis document must have at least one figure containing a plot.  
Your analysis must have no more than three figures. Figures may have multiple plots in them (i.e. panel plots), but there cannot be more than three figures total.  
You must show all your code for the work in your analysis document.   
This may make the document a bit verbose, but that is okay. In general, you should ensure that <span style="color: red; ">echo=TRUE</span> for every code chunk (this is the default setting in knitr).




## Read Data and Create data frame


```r
library(dplyr)
library(tidyr)
library(ggplot2)
library(R.utils)
library(data.table)
```

```r
url <- "https://d396qusza40orc.cloudfront.net/repdata%2Fdata%2FStormData.csv.bz2"
if(!file.exists("StormData.csv.bz2")) download.file(url,"StormData.csv.bz2")
```


```r
if(!any(ls() %in% "SD_df")) SD_df <- fread("StormData.csv.bz2")
head(SD_df)
```

```
##    STATE__           BGN_DATE BGN_TIME TIME_ZONE COUNTY COUNTYNAME STATE
## 1:       1  4/18/1950 0:00:00     0130       CST     97     MOBILE    AL
## 2:       1  4/18/1950 0:00:00     0145       CST      3    BALDWIN    AL
## 3:       1  2/20/1951 0:00:00     1600       CST     57    FAYETTE    AL
## 4:       1   6/8/1951 0:00:00     0900       CST     89    MADISON    AL
## 5:       1 11/15/1951 0:00:00     1500       CST     43    CULLMAN    AL
## 6:       1 11/15/1951 0:00:00     2000       CST     77 LAUDERDALE    AL
##     EVTYPE BGN_RANGE BGN_AZI BGN_LOCATI END_DATE END_TIME COUNTY_END
## 1: TORNADO         0                                               0
## 2: TORNADO         0                                               0
## 3: TORNADO         0                                               0
## 4: TORNADO         0                                               0
## 5: TORNADO         0                                               0
## 6: TORNADO         0                                               0
##    COUNTYENDN END_RANGE END_AZI END_LOCATI LENGTH WIDTH F MAG FATALITIES
## 1:         NA         0                      14.0   100 3   0          0
## 2:         NA         0                       2.0   150 2   0          0
## 3:         NA         0                       0.1   123 2   0          0
## 4:         NA         0                       0.0   100 2   0          0
## 5:         NA         0                       0.0   150 2   0          0
## 6:         NA         0                       1.5   177 2   0          0
##    INJURIES PROPDMG PROPDMGEXP CROPDMG CROPDMGEXP WFO STATEOFFIC ZONENAMES
## 1:       15    25.0          K       0                                    
## 2:        0     2.5          K       0                                    
## 3:        2    25.0          K       0                                    
## 4:        2     2.5          K       0                                    
## 5:        2     2.5          K       0                                    
## 6:        6     2.5          K       0                                    
##    LATITUDE LONGITUDE LATITUDE_E LONGITUDE_ REMARKS REFNUM
## 1:     3040      8812       3051       8806              1
## 2:     3042      8755          0          0              2
## 3:     3340      8742          0          0              3
## 4:     3458      8626          0          0              4
## 5:     3412      8642          0          0              5
## 6:     3450      8748          0          0              6
```


```r
summary(SD_df)
```

```
##     STATE__       BGN_DATE           BGN_TIME          TIME_ZONE        
##  Min.   : 1.0   Length:902297      Length:902297      Length:902297     
##  1st Qu.:19.0   Class :character   Class :character   Class :character  
##  Median :30.0   Mode  :character   Mode  :character   Mode  :character  
##  Mean   :31.2                                                           
##  3rd Qu.:45.0                                                           
##  Max.   :95.0                                                           
##                                                                         
##      COUNTY       COUNTYNAME           STATE              EVTYPE         
##  Min.   :  0.0   Length:902297      Length:902297      Length:902297     
##  1st Qu.: 31.0   Class :character   Class :character   Class :character  
##  Median : 75.0   Mode  :character   Mode  :character   Mode  :character  
##  Mean   :100.6                                                           
##  3rd Qu.:131.0                                                           
##  Max.   :873.0                                                           
##                                                                          
##    BGN_RANGE          BGN_AZI           BGN_LOCATI       
##  Min.   :   0.000   Length:902297      Length:902297     
##  1st Qu.:   0.000   Class :character   Class :character  
##  Median :   0.000   Mode  :character   Mode  :character  
##  Mean   :   1.484                                        
##  3rd Qu.:   1.000                                        
##  Max.   :3749.000                                        
##                                                          
##    END_DATE           END_TIME           COUNTY_END COUNTYENDN    
##  Length:902297      Length:902297      Min.   :0    Mode:logical  
##  Class :character   Class :character   1st Qu.:0    NA's:902297   
##  Mode  :character   Mode  :character   Median :0                  
##                                        Mean   :0                  
##                                        3rd Qu.:0                  
##                                        Max.   :0                  
##                                                                   
##    END_RANGE          END_AZI           END_LOCATI       
##  Min.   :  0.0000   Length:902297      Length:902297     
##  1st Qu.:  0.0000   Class :character   Class :character  
##  Median :  0.0000   Mode  :character   Mode  :character  
##  Mean   :  0.9862                                        
##  3rd Qu.:  0.0000                                        
##  Max.   :925.0000                                        
##                                                          
##      LENGTH              WIDTH                F               MAG         
##  Min.   :   0.0000   Min.   :   0.000   Min.   :0.0      Min.   :    0.0  
##  1st Qu.:   0.0000   1st Qu.:   0.000   1st Qu.:0.0      1st Qu.:    0.0  
##  Median :   0.0000   Median :   0.000   Median :1.0      Median :   50.0  
##  Mean   :   0.2301   Mean   :   7.503   Mean   :0.9      Mean   :   46.9  
##  3rd Qu.:   0.0000   3rd Qu.:   0.000   3rd Qu.:1.0      3rd Qu.:   75.0  
##  Max.   :2315.0000   Max.   :4400.000   Max.   :5.0      Max.   :22000.0  
##                                         NA's   :843563                    
##    FATALITIES          INJURIES            PROPDMG       
##  Min.   :  0.0000   Min.   :   0.0000   Min.   :   0.00  
##  1st Qu.:  0.0000   1st Qu.:   0.0000   1st Qu.:   0.00  
##  Median :  0.0000   Median :   0.0000   Median :   0.00  
##  Mean   :  0.0168   Mean   :   0.1557   Mean   :  12.06  
##  3rd Qu.:  0.0000   3rd Qu.:   0.0000   3rd Qu.:   0.50  
##  Max.   :583.0000   Max.   :1700.0000   Max.   :5000.00  
##                                                          
##   PROPDMGEXP           CROPDMG         CROPDMGEXP       
##  Length:902297      Min.   :  0.000   Length:902297     
##  Class :character   1st Qu.:  0.000   Class :character  
##  Mode  :character   Median :  0.000   Mode  :character  
##                     Mean   :  1.527                     
##                     3rd Qu.:  0.000                     
##                     Max.   :990.000                     
##                                                         
##      WFO             STATEOFFIC         ZONENAMES            LATITUDE   
##  Length:902297      Length:902297      Length:902297      Min.   :   0  
##  Class :character   Class :character   Class :character   1st Qu.:2802  
##  Mode  :character   Mode  :character   Mode  :character   Median :3540  
##                                                           Mean   :2875  
##                                                           3rd Qu.:4019  
##                                                           Max.   :9706  
##                                                           NA's   :47    
##    LONGITUDE        LATITUDE_E     LONGITUDE_       REMARKS         
##  Min.   :-14451   Min.   :   0   Min.   :-14455   Length:902297     
##  1st Qu.:  7247   1st Qu.:   0   1st Qu.:     0   Class :character  
##  Median :  8707   Median :   0   Median :     0   Mode  :character  
##  Mean   :  6940   Mean   :1452   Mean   :  3509                     
##  3rd Qu.:  9605   3rd Qu.:3549   3rd Qu.:  8735                     
##  Max.   : 17124   Max.   :9706   Max.   :106220                     
##                   NA's   :40                                        
##      REFNUM      
##  Min.   :     1  
##  1st Qu.:225575  
##  Median :451149  
##  Mean   :451149  
##  3rd Qu.:676723  
##  Max.   :902297  
## 
```

## Create an appropriate data frame

There are two problems with this assignment.I define these as follows:  
1. The most harmful with respect to population health: Defined as having caused the most deaths.  
2. the greatest economic consequences: The sum of physical damage and crop damage is defined as the maximum.

According to the following document,National Weather Service [Storm Data Documentation](https://d396qusza40orc.cloudfront.net/repdata%2Fpeer2_doc%2Fpd01016005curr.pdf)  

> 2.7 Damage.   
> If the values provided are rough estimates, then this should be stated as such in the narrative. Estimates should be rounded to three significant digits, followed by an alphabetical character signifying the magnitude of the number,  
>i.e., 1.55B for $1,550,000,000. Alphabetical characters used to signify magnitude
include “K” for thousands, “M” for millions, and “B” for billions. If additional precision is available, it may be provided in the narrative part of the entry. 

Therefore, Three metric prefixes (B, M, K) are used for the damage amount, and the others are not used due to errors.


```r
SD_df %>% 
  mutate(propexp = dplyr::case_when(PROPDMGEXP %in% c("B","b") ~ 1000000000,
                                    PROPDMGEXP %in% c("M","m") ~ 1000000,
                                    PROPDMGEXP %in% c("K","k") ~ 1000,
                                    TRUE ~ 1) ) %>%
  mutate(cropexp = dplyr::case_when(CROPDMGEXP %in% c("B","b") ~ 1000000000,
                                    CROPDMGEXP %in% c("M","m") ~ 1000000,
                                    CROPDMGEXP %in% c("K","k") ~ 1000,
                                    TRUE ~ 1) ) %>%
  group_by(EVTYPE) %>%
  summarise(Fatalities=sum(FATALITIES),
            Injuries=sum(INJURIES),
            DamageAmount=sum(PROPDMG * propexp + CROPDMG * cropexp)) -> result_df
```

### Analyzing aggregate data

Rearrange the aggregate data in the order of the number of deaths and the amount of damage, and confirm the disaster with the greatest damage.


```r
result_df %>%
  dplyr::arrange(-Fatalities) %>%
  dplyr::mutate(rank=row_number()) %>% 
  dplyr::filter(rank <= 5) -> result_f

result_df %>%
  dplyr::arrange(-DamageAmount) %>%
  dplyr::mutate(rank=row_number()) %>% 
  dplyr::filter(rank <= 5) -> result_d
```

Event with most harmful: **TORNADO **  
Event with greatest economic consequences: **FLOOD**  

These are the answers to this assignment.

## Plot aggregate data

Plot the top 5 events by number and amount of victims


```r
rbind(result_f,result_d) %>%
  tidyr::gather(key= "damage" ,value ="count",Fatalities,DamageAmount,Injuries) %>%
  select(-rank) %>% 
  transform(damage = factor(damage,
                            levels = c("Fatalities","DamageAmount","Injuries"))) %>%
  ggplot(.) + 
  geom_col(aes(x=reorder(EVTYPE,-count),y=count)) +
  facet_grid(rows=vars(damage),scales="free") +
  scale_y_continuous(name="Number/Amount",
                     labels = scales::comma) +
  labs(title="Top 5 event") + 
  xlab("Event") +
  theme(axis.text.x = element_text(angle = 45, hjust = 1),
        plot.title = element_text(hjust = 0.5)) 
```

![](CouseProject2_files/figure-html/plot data-1.png)<!-- -->

## Conclusion

Overall, tornado are devastating in USA.


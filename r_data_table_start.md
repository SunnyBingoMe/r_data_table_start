Get Started With data.table (DT)
================
SunnyBingoMe
2016-06-29

-   [DT Key Ideas](#dt-key-ideas)
-   [DT Basic Commands](#dt-basic-commands)
    -   [prepare data for commands](#prepare-data-for-commands)
    -   [subset](#subset)
    -   [order/sort](#ordersort)
    -   [column actions](#column-actions)
        -   [add (or unconditional update of all rows in a column):](#add-or-unconditional-update-of-all-rows-in-a-column)
        -   [update (with WHERE condition):](#update-with-where-condition)
        -   [delete:](#delete)
        -   [combine all tasks (just need to put `[]`s together):](#combine-all-tasks-just-need-to-put-s-together)
-   [DT Aggregate](#dt-aggregate)
-   [Performance DT vs dplyr](#performance-dt-vs-dplyr)
    -   [io performance](#io-performance)
-   [DT Other Useful Features](#dt-other-useful-features)
-   [OBS](#obs)
    -   [cat/paste in `[]`](#catpaste-in)
-   [Ref](#ref)

See also [Notes of Practical Machine Learning - Coursera](http://tdd.nu/pml-notes) for ML and ggplot related commands.

DT Key Ideas
============

1.  `data.table` is an extension of `data.frame`, it is capable of df
2.  DT's syntax is similar to SQL: `DT[i, j, by]` = sql query `[where, select, group_by]` i.e. `[WHERE_condition_of_rows, SELECT_of_columns, GROUP_BY_a_categorical_variable]`

DT Basic Commands
=================

prepare data for commands
-------------------------

``` r
library(data.table)
library(dplyr)
```

``` r
system.time((dt = fread("D:/hNow/Dropbox/Github/r_data_table_start/gb_code.csv")))
```

    ## 
    Read 17.5% of 1716366 rows
    Read 31.5% of 1716366 rows
    Read 43.1% of 1716366 rows
    Read 55.3% of 1716366 rows
    Read 67.6% of 1716366 rows
    Read 81.6% of 1716366 rows
    Read 93.2% of 1716366 rows
    Read 1716366 rows and 12 (of 12) columns from 0.187 GB file in 00:00:10

    ##    user  system elapsed 
    ##    9.16    0.06    9.30

``` r
summary(dt)
```

    ##       V1                 V2                 V3           
    ##  Length:1716366     Length:1716366     Length:1716366    
    ##  Class :character   Class :character   Class :character  
    ##  Mode  :character   Mode  :character   Mode  :character  
    ##                                                          
    ##                                                          
    ##                                                          
    ##                                                          
    ##       V4                 V5                 V6           
    ##  Length:1716366     Length:1716366     Length:1716366    
    ##  Class :character   Class :character   Class :character  
    ##  Mode  :character   Mode  :character   Mode  :character  
    ##                                                          
    ##                                                          
    ##                                                          
    ##                                                          
    ##       V7                 V8                 V9                 V10       
    ##  Length:1716366     Length:1716366     Length:1716366     Min.   :49.90  
    ##  Class :character   Class :character   Class :character   1st Qu.:51.49  
    ##  Mode  :character   Mode  :character   Mode  :character   Median :52.40  
    ##                                                           Mean   :52.71  
    ##                                                           3rd Qu.:53.56  
    ##                                                           Max.   :60.81  
    ##                                                           NA's   :27431  
    ##       V11              V12       
    ##  Min.   :-7.540   Min.   :1.000  
    ##  1st Qu.:-2.613   1st Qu.:6.000  
    ##  Median :-1.551   Median :6.000  
    ##  Mean   :-1.604   Mean   :5.961  
    ##  3rd Qu.:-0.347   3rd Qu.:6.000  
    ##  Max.   : 1.760   Max.   :6.000  
    ##  NA's   :27431    NA's   :2

``` r
head(dt)
```

    ##    V1       V2                     V3       V4  V5            V6 V7
    ## 1: GB     AB10               Aberdeen Scotland SCT Aberdeenshire   
    ## 2: GB AB10 1AA George St/Harbour Ward Scotland SCT                 
    ## 3: GB AB10 1AB George St/Harbour Ward Scotland SCT                 
    ## 4: GB AB10 1AF George St/Harbour Ward Scotland SCT                 
    ## 5: GB AB10 1AG George St/Harbour Ward Scotland SCT                 
    ## 6: GB AB10 1AH George St/Harbour Ward Scotland SCT                 
    ##               V8        V9      V10       V11 V12
    ## 1:                               NA        NA   4
    ## 2: Aberdeen City S12000033 57.14823 -2.096648   6
    ## 3: Aberdeen City S12000033 57.14960 -2.096916   6
    ## 4: Aberdeen City S12000033 57.14870 -2.097806   6
    ## 5: Aberdeen City S12000033 57.14823 -2.096648   6
    ## 6: Aberdeen City S12000033 57.14808 -2.094664   6

subset
------

``` r
# subset rows
subset_rows = dt[V4 == "England" & V3 == "Beswick" & V10 < 53.93]
subset_rows
```

    ##     V1       V2      V3      V4  V5 V6 V7                       V8
    ##  1: GB YO25 9AR Beswick England ENG       East Riding of Yorkshire
    ##  2: GB YO25 9AS Beswick England ENG       East Riding of Yorkshire
    ##  3: GB YO25 9AT Beswick England ENG       East Riding of Yorkshire
    ##  4: GB YO25 9AU Beswick England ENG       East Riding of Yorkshire
    ##  5: GB YO25 9AX Beswick England ENG       East Riding of Yorkshire
    ##  6: GB YO25 9BQ Beswick England ENG       East Riding of Yorkshire
    ##  7: GB YO25 9HZ Beswick England ENG       East Riding of Yorkshire
    ##  8: GB YO25 9JA Beswick England ENG       East Riding of Yorkshire
    ##  9: GB YO25 9JB Beswick England ENG       East Riding of Yorkshire
    ## 10: GB YO25 9TT Beswick England ENG       East Riding of Yorkshire
    ##            V9      V10        V11 V12
    ##  1: E06000011 53.92682 -0.4586771   6
    ##  2: E06000011 53.91990 -0.4601350   6
    ##  3: E06000011 53.92142 -0.4594243   6
    ##  4: E06000011 53.91834 -0.4599795   6
    ##  5: E06000011 53.91854 -0.4399179   6
    ##  6: E06000011 53.91618 -0.4012392   6
    ##  7: E06000011 53.92791 -0.4536871   6
    ##  8: E06000011 53.92365 -0.4391790   6
    ##  9: E06000011 53.92856 -0.4592069   6
    ## 10: E06000011 53.92171 -0.4989768   6

``` r
# subset cols
subset_cols = dt[ , .(V2,V4)]
head(subset_cols)
```

    ##          V2       V4
    ## 1:     AB10 Scotland
    ## 2: AB10 1AA Scotland
    ## 3: AB10 1AB Scotland
    ## 4: AB10 1AF Scotland
    ## 5: AB10 1AG Scotland
    ## 6: AB10 1AH Scotland

``` r
nrow(subset_cols)
```

    ## [1] 1716366

wrong example using `:`

``` r
subset_cols = dt[ , .(V2:V4)] # wrong !!!
```

order/sort
----------

``` r
dt_order = dt[order(V4, -V8)] # radix sort, so it is fast
```

column actions
--------------

add, update, delete, etc. tip: `:=`

### add (or unconditional update of all rows in a column):

``` r
dt[ , V18 := V10 + V11]
head(dt)
```

    ##    V1       V2                     V3       V4  V5            V6 V7
    ## 1: GB     AB10               Aberdeen Scotland SCT Aberdeenshire   
    ## 2: GB AB10 1AA George St/Harbour Ward Scotland SCT                 
    ## 3: GB AB10 1AB George St/Harbour Ward Scotland SCT                 
    ## 4: GB AB10 1AF George St/Harbour Ward Scotland SCT                 
    ## 5: GB AB10 1AG George St/Harbour Ward Scotland SCT                 
    ## 6: GB AB10 1AH George St/Harbour Ward Scotland SCT                 
    ##               V8        V9      V10       V11 V12      V18
    ## 1:                               NA        NA   4       NA
    ## 2: Aberdeen City S12000033 57.14823 -2.096648   6 55.05158
    ## 3: Aberdeen City S12000033 57.14960 -2.096916   6 55.05269
    ## 4: Aberdeen City S12000033 57.14870 -2.097806   6 55.05090
    ## 5: Aberdeen City S12000033 57.14823 -2.096648   6 55.05158
    ## 6: Aberdeen City S12000033 57.14808 -2.094664   6 55.05341

``` r
dt[ , V19 := 'this-is-NEW.col']
head(dt)
```

    ##    V1       V2                     V3       V4  V5            V6 V7
    ## 1: GB     AB10               Aberdeen Scotland SCT Aberdeenshire   
    ## 2: GB AB10 1AA George St/Harbour Ward Scotland SCT                 
    ## 3: GB AB10 1AB George St/Harbour Ward Scotland SCT                 
    ## 4: GB AB10 1AF George St/Harbour Ward Scotland SCT                 
    ## 5: GB AB10 1AG George St/Harbour Ward Scotland SCT                 
    ## 6: GB AB10 1AH George St/Harbour Ward Scotland SCT                 
    ##               V8        V9      V10       V11 V12      V18             V19
    ## 1:                               NA        NA   4       NA this-is-NEW.col
    ## 2: Aberdeen City S12000033 57.14823 -2.096648   6 55.05158 this-is-NEW.col
    ## 3: Aberdeen City S12000033 57.14960 -2.096916   6 55.05269 this-is-NEW.col
    ## 4: Aberdeen City S12000033 57.14870 -2.097806   6 55.05090 this-is-NEW.col
    ## 5: Aberdeen City S12000033 57.14823 -2.096648   6 55.05158 this-is-NEW.col
    ## 6: Aberdeen City S12000033 57.14808 -2.094664   6 55.05341 this-is-NEW.col

``` r
system.time(
dt[ , V20 := paste(V18, V19, sep = "_")] # expensive, system.time = 16s. see also 'OBS' below
)
```

    ##    user  system elapsed 
    ##   15.36    0.08   15.49

``` r
head(dt)
```

    ##    V1       V2                     V3       V4  V5            V6 V7
    ## 1: GB     AB10               Aberdeen Scotland SCT Aberdeenshire   
    ## 2: GB AB10 1AA George St/Harbour Ward Scotland SCT                 
    ## 3: GB AB10 1AB George St/Harbour Ward Scotland SCT                 
    ## 4: GB AB10 1AF George St/Harbour Ward Scotland SCT                 
    ## 5: GB AB10 1AG George St/Harbour Ward Scotland SCT                 
    ## 6: GB AB10 1AH George St/Harbour Ward Scotland SCT                 
    ##               V8        V9      V10       V11 V12      V18             V19
    ## 1:                               NA        NA   4       NA this-is-NEW.col
    ## 2: Aberdeen City S12000033 57.14823 -2.096648   6 55.05158 this-is-NEW.col
    ## 3: Aberdeen City S12000033 57.14960 -2.096916   6 55.05269 this-is-NEW.col
    ## 4: Aberdeen City S12000033 57.14870 -2.097806   6 55.05090 this-is-NEW.col
    ## 5: Aberdeen City S12000033 57.14823 -2.096648   6 55.05158 this-is-NEW.col
    ## 6: Aberdeen City S12000033 57.14808 -2.094664   6 55.05341 this-is-NEW.col
    ##                                 V20
    ## 1:               NA_this-is-NEW.col
    ## 2:   55.05158022833_this-is-NEW.col
    ## 3: 55.0526863914458_this-is-NEW.col
    ## 4: 55.0508972975867_this-is-NEW.col
    ## 5:   55.05158022833_this-is-NEW.col
    ## 6: 55.0534126323586_this-is-NEW.col

### update (with WHERE condition):

``` r
dt[V8 == "Aberdeen City", V8 := "updated city name"]
head(dt)
```

    ##    V1       V2                     V3       V4  V5            V6 V7
    ## 1: GB     AB10               Aberdeen Scotland SCT Aberdeenshire   
    ## 2: GB AB10 1AA George St/Harbour Ward Scotland SCT                 
    ## 3: GB AB10 1AB George St/Harbour Ward Scotland SCT                 
    ## 4: GB AB10 1AF George St/Harbour Ward Scotland SCT                 
    ## 5: GB AB10 1AG George St/Harbour Ward Scotland SCT                 
    ## 6: GB AB10 1AH George St/Harbour Ward Scotland SCT                 
    ##                   V8        V9      V10       V11 V12      V18
    ## 1:                                   NA        NA   4       NA
    ## 2: updated city name S12000033 57.14823 -2.096648   6 55.05158
    ## 3: updated city name S12000033 57.14960 -2.096916   6 55.05269
    ## 4: updated city name S12000033 57.14870 -2.097806   6 55.05090
    ## 5: updated city name S12000033 57.14823 -2.096648   6 55.05158
    ## 6: updated city name S12000033 57.14808 -2.094664   6 55.05341
    ##                V19                              V20
    ## 1: this-is-NEW.col               NA_this-is-NEW.col
    ## 2: this-is-NEW.col   55.05158022833_this-is-NEW.col
    ## 3: this-is-NEW.col 55.0526863914458_this-is-NEW.col
    ## 4: this-is-NEW.col 55.0508972975867_this-is-NEW.col
    ## 5: this-is-NEW.col   55.05158022833_this-is-NEW.col
    ## 6: this-is-NEW.col 55.0534126323586_this-is-NEW.col

### delete:

``` r
# delete one col
dt[ , V18 := NULL]
head(dt)
```

    ##    V1       V2                     V3       V4  V5            V6 V7
    ## 1: GB     AB10               Aberdeen Scotland SCT Aberdeenshire   
    ## 2: GB AB10 1AA George St/Harbour Ward Scotland SCT                 
    ## 3: GB AB10 1AB George St/Harbour Ward Scotland SCT                 
    ## 4: GB AB10 1AF George St/Harbour Ward Scotland SCT                 
    ## 5: GB AB10 1AG George St/Harbour Ward Scotland SCT                 
    ## 6: GB AB10 1AH George St/Harbour Ward Scotland SCT                 
    ##                   V8        V9      V10       V11 V12             V19
    ## 1:                                   NA        NA   4 this-is-NEW.col
    ## 2: updated city name S12000033 57.14823 -2.096648   6 this-is-NEW.col
    ## 3: updated city name S12000033 57.14960 -2.096916   6 this-is-NEW.col
    ## 4: updated city name S12000033 57.14870 -2.097806   6 this-is-NEW.col
    ## 5: updated city name S12000033 57.14823 -2.096648   6 this-is-NEW.col
    ## 6: updated city name S12000033 57.14808 -2.094664   6 this-is-NEW.col
    ##                                 V20
    ## 1:               NA_this-is-NEW.col
    ## 2:   55.05158022833_this-is-NEW.col
    ## 3: 55.0526863914458_this-is-NEW.col
    ## 4: 55.0508972975867_this-is-NEW.col
    ## 5:   55.05158022833_this-is-NEW.col
    ## 6: 55.0534126323586_this-is-NEW.col

``` r
# delete multi cols (must use their names)
dt[ , c('V19', 'V20') := NULL]
head(dt)
```

    ##    V1       V2                     V3       V4  V5            V6 V7
    ## 1: GB     AB10               Aberdeen Scotland SCT Aberdeenshire   
    ## 2: GB AB10 1AA George St/Harbour Ward Scotland SCT                 
    ## 3: GB AB10 1AB George St/Harbour Ward Scotland SCT                 
    ## 4: GB AB10 1AF George St/Harbour Ward Scotland SCT                 
    ## 5: GB AB10 1AG George St/Harbour Ward Scotland SCT                 
    ## 6: GB AB10 1AH George St/Harbour Ward Scotland SCT                 
    ##                   V8        V9      V10       V11 V12
    ## 1:                                   NA        NA   4
    ## 2: updated city name S12000033 57.14823 -2.096648   6
    ## 3: updated city name S12000033 57.14960 -2.096916   6
    ## 4: updated city name S12000033 57.14870 -2.097806   6
    ## 5: updated city name S12000033 57.14823 -2.096648   6
    ## 6: updated city name S12000033 57.14808 -2.094664   6

wrong format of delete:

``` r
dt[ , .(V11, V12) := NULL] # wrong !!!
dt[ , c(V11, V12) := NULL] # wrong !!!
```

### combine all tasks (just need to put `[]`s together):

``` r
dt[V8 == "Aberdeen City", V8 := "updated_city"][ , V_New := V10 + V11][ , c("V6","V7") := NULL]
head(dt)
```

    ##    V1       V2                     V3       V4  V5                V8
    ## 1: GB     AB10               Aberdeen Scotland SCT                  
    ## 2: GB AB10 1AA George St/Harbour Ward Scotland SCT updated city name
    ## 3: GB AB10 1AB George St/Harbour Ward Scotland SCT updated city name
    ## 4: GB AB10 1AF George St/Harbour Ward Scotland SCT updated city name
    ## 5: GB AB10 1AG George St/Harbour Ward Scotland SCT updated city name
    ## 6: GB AB10 1AH George St/Harbour Ward Scotland SCT updated city name
    ##           V9      V10       V11 V12    V_New
    ## 1:                 NA        NA   4       NA
    ## 2: S12000033 57.14823 -2.096648   6 55.05158
    ## 3: S12000033 57.14960 -2.096916   6 55.05269
    ## 4: S12000033 57.14870 -2.097806   6 55.05090
    ## 5: S12000033 57.14823 -2.096648   6 55.05158
    ## 6: S12000033 57.14808 -2.094664   6 55.05341

DT Aggregate
============

(compute functions while grouping)

``` r
summary(dt$V_New)
```

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max.    NA's 
    ##   43.55   50.45   51.30   51.10   52.04   59.99   27431

``` r
unique(dt$V4)
```

    ## [1] "Scotland"         "England"          "Northern Ireland"
    ## [4] "Wales"            ""

``` r
# calculate a "func"
dt[V_New >= 50, mean(V10), by = V4]
```

    ##          V4       V1
    ## 1: Scotland 56.20603
    ## 2:  England 52.60817
    ## 3:    Wales 53.13679

``` r
# named calculation in "func", instead of auto-named as 'V1', 'V2' ...
# OBS:  .()  is necessary, otherwise confusion with arg of dt
dt[V_New >= 50, .(averaged_result = mean(V10)), by = V4]
```

    ##          V4 averaged_result
    ## 1: Scotland        56.20603
    ## 2:  England        52.60817
    ## 3:    Wales        53.13679

``` r
# multi-calculation
dt[V_New >= 50, .(sum(V10), mean(V10), .N), by = V4]
```

    ##          V4         V1       V2       N
    ## 1: Scotland  8720365.2 56.20603  155150
    ## 2:  England 63449185.9 52.60817 1206071
    ## 3:    Wales   365687.4 53.13679    6882

``` r
# multi-calculation with names in "func" and func in "by"
dt[V_New >= 50, .(sumTotal = sum(V10), avr = mean(V10), countNr = .N), by = substr(V4, 1,1)]
```

    ##    substr   sumTotal      avr countNr
    ## 1:      S  8720365.2 56.20603  155150
    ## 2:      E 63449185.9 52.60817 1206071
    ## 3:      W   365687.4 53.13679    6882

Performance DT vs dplyr
=======================

``` r
# data.table
system.time((
dt[V_New >= 50, .(sumTotal = sum(V10), avr = mean(V10), countNr = .N), by = substr(V4, 1,1)]
))
```

    ##    user  system elapsed 
    ##    0.12    0.00    0.12

``` r
# base::aggregate
system.time({
mainDataDf = subset(dt, V_New >= 50)
summary_t = as.data.frame(as.list(aggregate(
    cbind(V10)
    ~ substr(V4, 1,1)
    ,data = mainDataDf
    ,FUN=function(mainDataDf) c(sumTotal = sum(mainDataDf), avr = mean(mainDataDf), countNr = length(mainDataDf))
)))
})
```

    ##    user  system elapsed 
    ##   12.99    0.16   13.28

``` r
summary_t
```

    ##   substr.V4..1..1. V10.sumTotal  V10.avr V10.countNr
    ## 1                E   63449185.9 52.60817     1206071
    ## 2                S    8720365.2 56.20603      155150
    ## 3                W     365687.4 53.13679        6882

``` r
# dplyr::summarise
system.time({
mainDataDf = subset(dt, V_New >= 50)
mainDataDf$t = substr(mainDataDf$V4, 1,1)
summary_t = summarise(group_by(mainDataDf, t),
                      sumTotal = sum(V10),
                      avr = mean(V10),
                      countNr = n())
mainDataDf$t = NULL
})
```

    ##    user  system elapsed 
    ##    0.67    0.25    0.94

``` r
summary_t
```

    ## Source: local data table [3 x 4]
    ## 
    ##   t   sumTotal      avr countNr
    ## 1 E 63449185.9 52.60817 1206071
    ## 2 S  8720365.2 56.20603  155150
    ## 3 W   365687.4 53.13679    6882

``` r
# pip-ed dplyr::summarise
system.time({
summary_t = dt %>%
    filter(V_New >= 50) %>%
    mutate(t = substr(V4, 1,1)) %>%
    group_by(t) %>%
    summarise(sumTotal = sum(V10),
              avr = mean(V10),
              countNr = n())
})
```

    ##    user  system elapsed 
    ##    0.25    0.09    0.34

``` r
summary_t
```

    ## Source: local data table [3 x 4]
    ## 
    ##   t   sumTotal      avr countNr
    ## 1 E 63449185.9 52.60817 1206071
    ## 2 S  8720365.2 56.20603  155150
    ## 3 W   365687.4 53.13679    6882

Note: the results are the same, but rwos in different order. To make exactly the same, we can use `base::order()` and `dplyr::arrange()`.

io performance
--------------

-   `dt = fread("data.csv")` 10x faster than `read.csv("data.csv")`.
-   `fwrite()` is faster than `write.csv()`.

DT Other Useful Features
========================

-   automatic indexing
-   rolling joins
-   overlapping range joins
-   and a lot more ...

OBS
===

cat/paste in `[]`
-----------------

`paste` works fine. wrong usage of `cat`:

``` r
system.time(dt[ , V20 := cat(V18, V19)])  # wrong !!!
# this will print all cat-results in console, and NOT adding/updating any column
```

Ref
===

[ref](https://www.evernote.com/shard/s144/sh/4b76e1d6-20e0-476c-8c40-aaf32a62a75e/b9f375de89da3229)

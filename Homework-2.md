Homework 2
================
Bingkun Luo
9/26/2019

\#Problem
1

``` r
Mr_Trash_Wheel_data = read_excel(path = "C:/Users/61693/Desktop/data Science/P8105_Hw2_bl2789/Trash-Wheel-Collection-Totals-8-6-19.xlsx",
                        sheet = 1, range = "A2:N336") %>%

  janitor::clean_names()%>%
  drop_na(dumpster)

sports_balls_integer = as.integer(pull(Mr_Trash_Wheel_data,sports_balls))  
mutate(Mr_Trash_Wheel_data,sports_balls = sports_balls_integer)
```

    ## # A tibble: 285 x 14
    ##    dumpster month  year date                weight_tons volume_cubic_ya~
    ##       <dbl> <chr> <dbl> <dttm>                    <dbl>            <dbl>
    ##  1        1 May    2014 2014-05-16 00:00:00        4.31               18
    ##  2        2 May    2014 2014-05-16 00:00:00        2.74               13
    ##  3        3 May    2014 2014-05-16 00:00:00        3.45               15
    ##  4        4 May    2014 2014-05-17 00:00:00        3.1                15
    ##  5        5 May    2014 2014-05-17 00:00:00        4.06               18
    ##  6        6 May    2014 2014-05-20 00:00:00        2.71               13
    ##  7        7 May    2014 2014-05-21 00:00:00        1.91                8
    ##  8        8 May    2014 2014-05-28 00:00:00        3.7                16
    ##  9        9 June   2014 2014-06-05 00:00:00        2.52               14
    ## 10       10 June   2014 2014-06-11 00:00:00        3.76               18
    ## # ... with 275 more rows, and 8 more variables: plastic_bottles <dbl>,
    ## #   polystyrene <dbl>, cigarette_butts <dbl>, glass_bottles <dbl>,
    ## #   grocery_bags <dbl>, chip_bags <dbl>, sports_balls <int>,
    ## #   homes_powered <dbl>

\#b

``` r
precipitation_2018 = read_excel(path = "C:/Users/61693/Desktop/data Science/P8105_Hw2_bl2789/Trash-Wheel-Collection-Totals-8-6-19.xlsx",
                        sheet = 5,range = "A2:B15") %>%
   janitor::clean_names()%>%
   drop_na(month)

precipitation_2018 = mutate(precipitation_2018,year = 2018)%>%
  select(year, precipitation_in_month = month,total)
```

``` r
precipitation_2017 = read_excel(path = "C:/Users/61693/Desktop/data Science/P8105_Hw2_bl2789/Trash-Wheel-Collection-Totals-8-6-19.xlsx",
                        sheet = 6,range = "A2:B15") %>%
   janitor::clean_names()%>%
   drop_na(month)

 precipitation_2017 = mutate(precipitation_2017,year = 2017)%>%
  select(year, precipitation_in_month = month,total) 
```

\#Problem 2

\#Problem 3

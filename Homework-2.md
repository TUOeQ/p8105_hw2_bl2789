Homework 2
================
Bingkun Luo
9/26/2019

# Problem 1

``` r
Mr_Trash_Wheel_data = read_excel(path = "C:/Users/61693/Desktop/data Science/P8105_Hw2_bl2789/Trash-Wheel-Collection-Totals-8-6-19.xlsx",
                        sheet = 1, range = "A2:N408") %>%

  janitor::clean_names()%>%
  drop_na(dumpster)

sports_balls_integer = as.integer(pull(Mr_Trash_Wheel_data,sports_balls))  
mutate(Mr_Trash_Wheel_data,sports_balls = sports_balls_integer)
```

    ## # A tibble: 344 x 14
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
    ## # ... with 334 more rows, and 8 more variables: plastic_bottles <dbl>,
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

``` r
##precipitation = rbind(precipitation_2017,precipitation_2018)

precipitation = full_join(precipitation_2017,precipitation_2018)
```

    ## Joining, by = c("year", "precipitation_in_month", "total")

``` r
month_name = month.name[pull(precipitation,precipitation_in_month)]

Final_precipitation = mutate(precipitation, month_name)%>%
  select(year,month_name,total)
```

  - The number of the observations in the **Mr.Trash Wheel** dataset is
    334 and 14 variables and the key varibles are shown below.

  - **Mr.Trash
    Wheel**

<!-- end list -->

    ## Classes 'tbl_df', 'tbl' and 'data.frame':    344 obs. of  14 variables:
    ##  $ dumpster          : num  1 2 3 4 5 6 7 8 9 10 ...
    ##  $ month             : chr  "May" "May" "May" "May" ...
    ##  $ year              : num  2014 2014 2014 2014 2014 ...
    ##  $ date              : POSIXct, format: "2014-05-16" "2014-05-16" ...
    ##  $ weight_tons       : num  4.31 2.74 3.45 3.1 4.06 2.71 1.91 3.7 2.52 3.76 ...
    ##  $ volume_cubic_yards: num  18 13 15 15 18 13 8 16 14 18 ...
    ##  $ plastic_bottles   : num  1450 1120 2450 2380 980 1430 910 3580 2400 1340 ...
    ##  $ polystyrene       : num  1820 1030 3100 2730 870 2140 1090 4310 2790 1730 ...
    ##  $ cigarette_butts   : num  126000 91000 105000 100000 120000 90000 56000 112000 98000 130000 ...
    ##  $ glass_bottles     : num  72 42 50 52 72 46 32 58 49 75 ...
    ##  $ grocery_bags      : num  584 496 1080 896 368 ...
    ##  $ chip_bags         : num  1162 874 2032 1971 753 ...
    ##  $ sports_balls      : num  7.2 5.2 6 6 7.2 5.2 3.2 6.4 5.6 7.2 ...
    ##  $ homes_powered     : num  0 0 0 0 0 0 0 0 0 0 ...

  - The number of the observations in the **Final precipitation**
    dataset is 24 and 3 variables and the key varibles are shown below.

  - **Final
    precipitation**

<!-- end list -->

    ## Classes 'tbl_df', 'tbl' and 'data.frame':    24 obs. of  3 variables:
    ##  $ year      : num  2017 2017 2017 2017 2017 ...
    ##  $ month_name: chr  "January" "February" "March" "April" ...
    ##  $ total     : num  2.34 1.46 3.57 3.99 5.64 1.4 7.09 4.44 1.95 0 ...

  - The total precipitation in 2018 is **70.33**
  - The median number of sports balls in a dumpster in 2017 is
**8**

# Problem 2

## a

``` r
pols_month = read_csv(file = "C:/Users/61693/Desktop/data Science/fivethirtyeight_datasets/pols-month.csv")%>% 
  separate(mon,into = c('year','month','day'),'-')%>% 
  mutate(month = month.name[as.integer(month)],
         president = recode(as.character(prez_gop),'1'='gop','0'='dem'))%>%
  select(-prez_dem,-prez_gop,-day)
```

    ## Parsed with column specification:
    ## cols(
    ##   mon = col_date(format = ""),
    ##   prez_gop = col_double(),
    ##   gov_gop = col_double(),
    ##   sen_gop = col_double(),
    ##   rep_gop = col_double(),
    ##   prez_dem = col_double(),
    ##   gov_dem = col_double(),
    ##   sen_dem = col_double(),
    ##   rep_dem = col_double()
    ## )

## b

\#Problem 3

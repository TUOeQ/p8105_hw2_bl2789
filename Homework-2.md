Homework 2
================
Bingkun Luo
9/26/2019

# Problem 1

## a

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

## b

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

``` r
snp = read_csv(file = "C:/Users/61693/Desktop/data Science/fivethirtyeight_datasets/snp.csv")%>% 
  separate(date,into = c('month','day','year'),'/')%>%
  mutate(month = month.name[as.integer(month)])%>%
  select('year','month','close')
```

    ## Parsed with column specification:
    ## cols(
    ##   date = col_character(),
    ##   close = col_double()
    ## )

## c

``` r
unemployment = read_csv(file = "C:/Users/61693/Desktop/data Science/fivethirtyeight_datasets/unemployment.csv")
```

    ## Parsed with column specification:
    ## cols(
    ##   Year = col_double(),
    ##   Jan = col_double(),
    ##   Feb = col_double(),
    ##   Mar = col_double(),
    ##   Apr = col_double(),
    ##   May = col_double(),
    ##   Jun = col_double(),
    ##   Jul = col_double(),
    ##   Aug = col_double(),
    ##   Sep = col_double(),
    ##   Oct = col_double(),
    ##   Nov = col_double(),
    ##   Dec = col_double()
    ## )

``` r
unemployment = pivot_longer(unemployment,
               Jan:Dec,
               names_to = 'month',
               values_to = 'unemployment')%>% 
   mutate(month = as.character(factor(month, labels = month.name)),
          year = as.character(Year))%>%
  select(year,month,unemployment)
```

## d

``` r
Final = full_join(pols_month,snp)%>%
 drop_na(gov_gop)
```

    ## Joining, by = c("year", "month")

``` r
Result = full_join(Final,unemployment)%>%
 drop_na(gov_gop)
```

    ## Joining, by = c("year", "month")

``` r
str(Result)
```

    ## Classes 'tbl_df', 'tbl' and 'data.frame':    822 obs. of  11 variables:
    ##  $ year        : chr  "1947" "1947" "1947" "1947" ...
    ##  $ month       : chr  "January" "February" "March" "April" ...
    ##  $ gov_gop     : num  23 23 23 23 23 23 23 23 23 23 ...
    ##  $ sen_gop     : num  51 51 51 51 51 51 51 51 51 51 ...
    ##  $ rep_gop     : num  253 253 253 253 253 253 253 253 253 253 ...
    ##  $ gov_dem     : num  23 23 23 23 23 23 23 23 23 23 ...
    ##  $ sen_dem     : num  45 45 45 45 45 45 45 45 45 45 ...
    ##  $ rep_dem     : num  198 198 198 198 198 198 198 198 198 198 ...
    ##  $ president   : chr  "dem" "dem" "dem" "dem" ...
    ##  $ close       : num  NA NA NA NA NA NA NA NA NA NA ...
    ##  $ unemployment: num  NA NA NA NA NA NA NA NA NA NA ...

  - From table above, we can see that the **Result** dataset has 822
    observations and 11 variables, including year, month, gov\_gop,
    sen\_gop, rep\_gop, gov\_dem, sen\_dem, rep\_dem, president, close,
    unemployment.

\#Problem 3

``` r
firstup <- function(x) {
  substr(x, 1, 1) <- toupper(substr(x, 1, 1))
  x
}

Popular_Baby_Names = read_csv(file = "C:/Users/61693/Desktop/data Science/P8105_Hw2_bl2789/Popular_Baby_Names.csv")%>%
  janitor::clean_names()%>%
  mutate(gender = toupper(gender),
         ethnicity = recode(firstup(tolower(ethnicity)),
         "Asian and paci"="Asian and pacific islander",
         "Black non hisp"="Black non hispanic",
         "White non hisp"="White non hispanic"),
         childs_first_name = firstup(tolower(childs_first_name)))
```

    ## Parsed with column specification:
    ## cols(
    ##   `Year of Birth` = col_double(),
    ##   Gender = col_character(),
    ##   Ethnicity = col_character(),
    ##   `Child's First Name` = col_character(),
    ##   Count = col_double(),
    ##   Rank = col_double()
    ## )

``` r
# Remove duplicate rows of the dataframe
Popular_Baby_Names = distinct(Popular_Baby_Names)
```

## a

``` r
A = filter(Popular_Baby_Names,
           childs_first_name == "Olivia",
           gender == "FEMALE")
 
pivot_wider(A, 
  names_from = "year_of_birth", 
  values_from = "rank",
  id_cols = "ethnicity")
```

    ## # A tibble: 4 x 7
    ##   ethnicity                  `2016` `2015` `2014` `2013` `2012` `2011`
    ##   <chr>                       <dbl>  <dbl>  <dbl>  <dbl>  <dbl>  <dbl>
    ## 1 Asian and pacific islander      1      1      1      3      3      4
    ## 2 Black non hispanic              8      4      8      6      8     10
    ## 3 Hispanic                       13     16     16     22     22     18
    ## 4 White non hispanic              1      1      1      1      4      2

## b

``` r
B = filter(Popular_Baby_Names,
           gender == "MALE",
           rank == which.min(rank))
 
pivot_wider(B, 
  names_from = "year_of_birth", 
  values_from = "childs_first_name",
  id_cols = "ethnicity")
```

    ## # A tibble: 4 x 7
    ##   ethnicity                  `2016` `2015` `2014` `2013` `2012` `2011` 
    ##   <chr>                      <chr>  <chr>  <chr>  <chr>  <chr>  <chr>  
    ## 1 Asian and pacific islander Ethan  Jayden Jayden Jayden Ryan   Ethan  
    ## 2 Black non hispanic         Noah   Noah   Ethan  Ethan  Jayden Jayden 
    ## 3 Hispanic                   Liam   Liam   Liam   Jayden Jayden Jayden 
    ## 4 White non hispanic         Joseph David  Joseph David  Joseph Michael

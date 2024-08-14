# Import raw data


## Data source

Data is downloaded from
https://ftp.cdc.gov/pub/Health_Statistics/NCHS/Datasets/NSFG/ on
8/14/2024 including:

- 2011_2019_FemaleWgtData.dat
- 2017_2019_FemPregData.dat

National Center for Health Statistics (NCHS). (2020). 2017-2019 National
Survey of Family Growth Public-Use Data and Documentation. Hyattsville,
MD: CDC National Center for Health Statistics. Retrieved from
http://www.cdc.gov/nchs/nsfg/nsfg_2017_2019_puf.htm.

## Import data

``` r
library(tidyverse)
```

    ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ✔ dplyr     1.1.4     ✔ readr     2.1.5
    ✔ forcats   1.0.0     ✔ stringr   1.5.1
    ✔ ggplot2   3.5.1     ✔ tibble    3.2.1
    ✔ lubridate 1.9.3     ✔ tidyr     1.3.1
    ✔ purrr     1.0.2     
    ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ✖ dplyr::filter() masks stats::filter()
    ✖ dplyr::lag()    masks stats::lag()
    ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

``` r
library(tidylog)
```


    Attaching package: 'tidylog'

    The following objects are masked from 'package:dplyr':

        add_count, add_tally, anti_join, count, distinct, distinct_all,
        distinct_at, distinct_if, filter, filter_all, filter_at, filter_if,
        full_join, group_by, group_by_all, group_by_at, group_by_if,
        inner_join, left_join, mutate, mutate_all, mutate_at, mutate_if,
        relocate, rename, rename_all, rename_at, rename_if, rename_with,
        right_join, sample_frac, sample_n, select, select_all, select_at,
        select_if, semi_join, slice, slice_head, slice_max, slice_min,
        slice_sample, slice_tail, summarise, summarise_all, summarise_at,
        summarise_if, summarize, summarize_all, summarize_at, summarize_if,
        tally, top_frac, top_n, transmute, transmute_all, transmute_at,
        transmute_if, ungroup

    The following objects are masked from 'package:tidyr':

        drop_na, fill, gather, pivot_longer, pivot_wider, replace_na,
        separate_wider_delim, separate_wider_position,
        separate_wider_regex, spread, uncount

    The following object is masked from 'package:stats':

        filter

``` r
surv_wt_in <- read_fwf(
  here::here("raw-data", "2011_2019_FemaleWgtData.dat"),
  col_positions = fwf_positions(
    start=c(1, 6, 22, 38, 54, 70, 86), 
    end=c(5, 21, 37, 53, 69, 85, 101),
    col_names=c("CASEID", "WGT2011_2015", "WGT2013_2017", "WGT2015_2019", "WGT2011_2017", "WGT2013_2019", "WGT2011_2019")),
  col_types="n")

summary(surv_wt_in)
```

         CASEID       WGT2011_2015      WGT2013_2017      WGT2015_2019    
     Min.   :50002   Min.   :  880.6   Min.   :  889.4   Min.   :  822.4  
     1st Qu.:60686   1st Qu.: 2156.3   1st Qu.: 2178.5   1st Qu.: 2220.5  
     Median :70972   Median : 3484.5   Median : 3542.5   Median : 3747.8  
     Mean   :71013   Mean   : 5421.5   Mean   : 5813.1   Mean   : 6192.4  
     3rd Qu.:81447   3rd Qu.: 6243.6   3rd Qu.: 6625.9   3rd Qu.: 7082.6  
     Max.   :92062   Max.   :39574.8   Max.   :44776.6   Max.   :45701.2  
                     NA's   :11695     NA's   :12405     NA's   :11300    
      WGT2011_2017      WGT2013_2019      WGT2011_2019  
     Min.   :  576.3   Min.   :  573.3   Min.   :  394  
     1st Qu.: 1442.2   1st Qu.: 1437.3   1st Qu.: 1059  
     Median : 2347.9   Median : 2415.2   Median : 1781  
     Mean   : 3792.4   Mean   : 3855.5   Mean   : 2848  
     3rd Qu.: 4329.9   3rd Qu.: 4440.8   3rd Qu.: 3272  
     Max.   :29365.5   Max.   :28202.2   Max.   :21350  
     NA's   :6804      NA's   :6987      NA's   :1386   

``` r
glimpse(surv_wt_in)
```

    Rows: 22,995
    Columns: 7
    $ CASEID       <dbl> 50002, 50004, 50005, 50008, 50013, 50015, 50018, 50022, 5…
    $ WGT2011_2015 <dbl> 1137.966, 1462.100, 3842.586, 1536.542, 3286.332, 2038.25…
    $ WGT2013_2017 <dbl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, N…
    $ WGT2015_2019 <dbl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, N…
    $ WGT2011_2017 <dbl> 789.1947, 1044.0912, 2581.6531, 1104.0147, 2163.3483, 137…
    $ WGT2013_2019 <dbl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, N…
    $ WGT2011_2019 <dbl> 593.0939, 831.8113, 1950.0663, 728.5286, 1717.5160, 1082.…

``` r
preg_dat_in <- read_fwf(
  here::here("raw-data", "2017_2019_FemPregData.dat"),
  col_positions = fwf_positions(
    start=c(1, 6, 8, 9, 10, 45, 102, 103, 105, 106, 138, 221, 222, 248), 
    end=  c(5, 7, 8, 9, 10, 45, 102, 103, 105, 107, 139, 221, 224, 251),
    col_names=c("CASEID", "PREGORDR", "MOSCURRP", "PREGEND1", "PREGEND2", "NBRNLV_S", "GEST_LB", "GEST_OTHR", "OUTCOME", "BIRTHORD", "AGER", "SECU", "SEST", "INTVWYEAR")),
  col_types="n")

summary(preg_dat_in)
```

         CASEID         PREGORDR         MOSCURRP        PREGEND1    
     Min.   :80719   Min.   : 1.000   Min.   :0.000   Min.   :1.000  
     1st Qu.:83439   1st Qu.: 1.000   1st Qu.:3.000   1st Qu.:3.000  
     Median :86362   Median : 2.000   Median :5.000   Median :6.000  
     Mean   :86362   Mean   : 2.388   Mean   :4.951   Mean   :4.624  
     3rd Qu.:89266   3rd Qu.: 3.000   3rd Qu.:7.000   3rd Qu.:6.000  
     Max.   :92062   Max.   :14.000   Max.   :9.000   Max.   :9.000  
                                      NA's   :9685    NA's   :194    
        PREGEND2        NBRNLV_S        GEST_LB        GEST_OTHR    
     Min.   :1.000   Min.   :1.000   Min.   :1.000   Min.   :1.000  
     1st Qu.:3.000   1st Qu.:1.000   1st Qu.:3.000   1st Qu.:1.000  
     Median :5.000   Median :1.000   Median :3.000   Median :1.000  
     Mean   :4.308   Mean   :1.016   Mean   :2.935   Mean   :1.202  
     3rd Qu.:5.000   3rd Qu.:1.000   3rd Qu.:3.000   3rd Qu.:1.000  
     Max.   :6.000   Max.   :2.000   Max.   :4.000   Max.   :3.000  
     NA's   :10202   NA's   :3016    NA's   :3016    NA's   :7199   
        OUTCOME         BIRTHORD           AGER            SECU            SEST    
     Min.   :1.000   Min.   : 1.000   Min.   :15.00   Min.   :1.000   Min.   :302  
     1st Qu.:1.000   1st Qu.: 1.000   1st Qu.:31.00   1st Qu.:1.000   1st Qu.:327  
     Median :1.000   Median : 2.000   Median :36.00   Median :2.000   Median :346  
     Mean   :1.741   Mean   : 1.899   Mean   :36.31   Mean   :2.418   Mean   :341  
     3rd Qu.:2.000   3rd Qu.: 2.000   3rd Qu.:43.00   3rd Qu.:3.000   3rd Qu.:356  
     Max.   :6.000   Max.   :11.000   Max.   :50.00   Max.   :4.000   Max.   :371  
                     NA's   :3016                                                  
       INTVWYEAR   
     Min.   :2017  
     1st Qu.:2018  
     Median :2018  
     Mean   :2018  
     3rd Qu.:2019  
     Max.   :2019  
                   

``` r
glimpse(preg_dat_in)
```

    Rows: 10,215
    Columns: 14
    $ CASEID    <dbl> 88819, 88819, 83055, 83055, 92062, 92062, 92062, 84933, 8493…
    $ PREGORDR  <dbl> 1, 2, 1, 2, 1, 2, 3, 1, 2, 1, 2, 1, 2, 3, 1, 2, 3, 1, 2, 1, …
    $ MOSCURRP  <dbl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
    $ PREGEND1  <dbl> 6, 5, 5, 5, 6, 2, 6, 5, 5, 1, 5, 3, 1, 6, 6, 6, 6, 6, 6, 1, …
    $ PREGEND2  <dbl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
    $ NBRNLV_S  <dbl> 1, 1, 1, 1, 1, NA, 1, 1, 1, NA, 1, NA, NA, 1, 1, 1, 1, 1, 1,…
    $ GEST_LB   <dbl> 4, 3, 4, 3, 3, NA, 3, 3, 3, NA, 3, NA, NA, 3, 4, 4, 3, 4, 3,…
    $ GEST_OTHR <dbl> NA, NA, NA, NA, NA, 2, NA, NA, NA, 1, NA, 1, 1, NA, NA, NA, …
    $ OUTCOME   <dbl> 1, 1, 1, 1, 1, 3, 1, 1, 1, 4, 1, 2, 4, 1, 1, 1, 1, 1, 1, 4, …
    $ BIRTHORD  <dbl> 1, 2, 1, 2, 1, NA, 2, 1, 2, NA, 1, NA, NA, 1, 1, 2, 3, 1, 2,…
    $ AGER      <dbl> 43, 43, 41, 41, 41, 41, 41, 38, 38, 40, 40, 45, 45, 45, 43, …
    $ SECU      <dbl> 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, …
    $ SEST      <dbl> 354, 354, 354, 354, 354, 354, 354, 354, 354, 354, 354, 354, …
    $ INTVWYEAR <dbl> 2018, 2018, 2018, 2018, 2018, 2018, 2018, 2018, 2018, 2018, …

``` r
dat_tog <- preg_dat_in %>%
  left_join(select(surv_wt_in, CASEID, WGT2011_2019), by="CASEID")
```

    select: dropped 5 variables (WGT2011_2015, WGT2013_2017, WGT2015_2019, WGT2011_2017, WGT2013_2019)
    left_join: added one column (WGT2011_2019)
               > rows only in x                               0
               > rows only in select(surv_wt_in, CASE.. (19,286)
               > matched rows                            10,215
               >                                        ========
               > rows total                              10,215

``` r
nrow(preg_dat_in)
```

    [1] 10215

``` r
nrow(dat_tog)
```

    [1] 10215

``` r
dat_tog %>% filter(is.na(WGT2011_2019)) %>% summary()
```

    filter: removed 8,355 rows (82%), 1,860 rows remaining

         CASEID         PREGORDR         MOSCURRP       PREGEND1        PREGEND2   
     Min.   :80742   Min.   : 1.000   Min.   : NA    Min.   :1.000   Min.   : NA   
     1st Qu.:83209   1st Qu.: 1.000   1st Qu.: NA    1st Qu.:3.000   1st Qu.: NA   
     Median :86266   Median : 2.000   Median : NA    Median :6.000   Median : NA   
     Mean   :86203   Mean   : 2.545   Mean   :NaN    Mean   :4.627   Mean   :NaN   
     3rd Qu.:89084   3rd Qu.: 3.000   3rd Qu.: NA    3rd Qu.:6.000   3rd Qu.: NA   
     Max.   :92046   Max.   :13.000   Max.   : NA    Max.   :9.000   Max.   : NA   
                                      NA's   :1860                   NA's   :1860  
        NBRNLV_S        GEST_LB        GEST_OTHR        OUTCOME     
     Min.   :1.000   Min.   :1.000   Min.   :1.000   Min.   :1.000  
     1st Qu.:1.000   1st Qu.:3.000   1st Qu.:1.000   1st Qu.:1.000  
     Median :1.000   Median :3.000   Median :1.000   Median :1.000  
     Mean   :1.016   Mean   :2.918   Mean   :1.141   Mean   :1.656  
     3rd Qu.:1.000   3rd Qu.:3.000   3rd Qu.:1.000   3rd Qu.:2.000  
     Max.   :2.000   Max.   :4.000   Max.   :3.000   Max.   :5.000  
     NA's   :525     NA's   :525     NA's   :1335                   
        BIRTHORD           AGER            SECU            SEST      
     Min.   : 1.000   Min.   :45.00   Min.   :1.000   Min.   :302.0  
     1st Qu.: 1.000   1st Qu.:46.00   1st Qu.:1.000   1st Qu.:320.0  
     Median : 2.000   Median :47.00   Median :2.000   Median :346.0  
     Mean   : 2.014   Mean   :47.03   Mean   :2.415   Mean   :339.6  
     3rd Qu.: 3.000   3rd Qu.:48.00   3rd Qu.:4.000   3rd Qu.:356.0  
     Max.   :10.000   Max.   :50.00   Max.   :4.000   Max.   :371.0  
     NA's   :525                                                     
       INTVWYEAR     WGT2011_2019 
     Min.   :2017   Min.   : NA   
     1st Qu.:2018   1st Qu.: NA   
     Median :2018   Median : NA   
     Mean   :2018   Mean   :NaN   
     3rd Qu.:2019   3rd Qu.: NA   
     Max.   :2019   Max.   : NA   
                    NA's   :1860  

Those age 45-50 do not have weights so the whole set has a consistent
age. This older group was only added in later years.

``` r
dat_tog_wt <- dat_tog %>%
  mutate(
    WGT2011_2019=replace_na(WGT2011_2019, 0)
  )
```

    mutate: changed 1,860 values (18%) of 'WGT2011_2019' (1,860 fewer NAs)

``` r
write_rds(dat_tog_wt, here::here("Data", "d01_preg_dat.rds"))
```

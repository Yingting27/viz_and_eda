Viz part 1
================
Yingting Zhang
2023-10-08

``` r
library(tidyverse)
```

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ dplyr     1.1.3     ✔ readr     2.1.4
    ## ✔ forcats   1.0.0     ✔ stringr   1.5.0
    ## ✔ ggplot2   3.4.3     ✔ tibble    3.2.1
    ## ✔ lubridate 1.9.2     ✔ tidyr     1.3.0
    ## ✔ purrr     1.0.2     
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

``` r
library(ggridges)
```

Get the data for plotting today

``` r
weather_df = 
  rnoaa::meteo_pull_monitors(
    c("USW00094728", "USW00022534", "USS0023B17S"),
    var = c("PRCP", "TMIN", "TMAX"), 
    date_min = "2021-01-01",
    date_max = "2022-12-31") |>
  mutate(
    name = recode(
      id, 
      USW00094728 = "CentralPark_NY", 
      USW00022534 = "Molokai_HI",
      USS0023B17S = "Waterhole_WA"),
    tmin = tmin / 10,
    tmax = tmax / 10) |>
  select(name, id, everything())
```

    ## using cached file: /Users/demiwang/Library/Caches/org.R-project.R/R/rnoaa/noaa_ghcnd/USW00094728.dly

    ## date created (size, mb): 2023-10-08 19:41:14.184812 (0.343)

    ## file min/max dates: 2021-01-01 / 2023-10-31

    ## using cached file: /Users/demiwang/Library/Caches/org.R-project.R/R/rnoaa/noaa_ghcnd/USW00022534.dly

    ## date created (size, mb): 2023-10-08 19:41:14.836161 (0.282)

    ## file min/max dates: 2021-01-01 / 2023-10-31

    ## using cached file: /Users/demiwang/Library/Caches/org.R-project.R/R/rnoaa/noaa_ghcnd/USS0023B17S.dly

    ## date created (size, mb): 2023-10-08 19:41:15.23604 (0.122)

    ## file min/max dates: 2021-01-01 / 2023-10-31

``` r
#the things in c is monitor
```

Let’s make a plot!

``` r
ggplot(weather_df, aes(x = tmin, y = tmax)) + geom_point()
```

    ## Warning: Removed 17 rows containing missing values (`geom_point()`).

![](viz_part-1_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->

``` r
#before geom, this just a picture without any plot or data in it, because we dont show what types of plot we want
```

Pipes and stuff

``` r
weather_df |> 
  filter(name == "CentralPark_NY") |> 
ggplot(aes(x = tmin, y = tmax)) + geom_point()
```

![](viz_part-1_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

``` r
ggp_nyc_weather = 
  weather_df |> 
    filter(name == "CentralPark_NY") |> 
ggplot(aes(x = tmin, y = tmax)) + geom_point()
#this doesnt show the plot, just create a plot data, and need to type ggp_nyc_weather to show the plot
ggp_nyc_weather
```

![](viz_part-1_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->

## Fancy plot

``` r
ggplot(weather_df, aes(x= tmin, y = tmax, color = name)) + geom_point() + geom_smooth()
```

    ## `geom_smooth()` using method = 'loess' and formula = 'y ~ x'

    ## Warning: Removed 17 rows containing non-finite values (`stat_smooth()`).

    ## Warning: Removed 17 rows containing missing values (`geom_point()`).

![](viz_part-1_files/figure-gfm/unnamed-chunk-6-1.png)<!-- -->

``` r
# name variable with color
#geom_smooth: fix smooth line with plots (create smooth line)
```

``` r
ggplot(weather_df, aes(x= tmin, y = tmax)) + geom_point(aes(color = name)) + geom_smooth()
```

    ## `geom_smooth()` using method = 'gam' and formula = 'y ~ s(x, bs = "cs")'

    ## Warning: Removed 17 rows containing non-finite values (`stat_smooth()`).

    ## Warning: Removed 17 rows containing missing values (`geom_point()`).

![](viz_part-1_files/figure-gfm/unnamed-chunk-7-1.png)<!-- -->

``` r
#mention particular color lines
#the difference between these two codes: the first one is to establish smooth lines everywhere in plots, both to the points and the scatter plot geometry that was making and to the smooth curve that was going through it (have three separate lines with three separate colors). the second one, only the points are being highlighted by color and the smooth is not
```

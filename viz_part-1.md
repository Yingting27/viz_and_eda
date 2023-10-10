Viz part 2
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
knitr::opts_chunk$set(
  fig.width = 6,
  fig.asp = .6,
  out.width = "90%"
)
#set the figure width to be six everywhere and the aspect ratio to be 0.6. And this out width means that it's going to take up 90% of the width of like an html document or a word document.
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

## Same plot from last time

``` r
weather_df |> 
  ggplot(aes(x = tmin, y = tmax, color = name)) + geom_point(alpha=0.5)+ labs(
    title = "Temperature plot",
    x = "Min daily temp (Degrees C)",
    y ="Min daily temp",
    color = "Location",
    caption = "Max vs. min daily temp in three locations; data from rnoaa"
)
```

    ## Warning: Removed 17 rows containing missing values (`geom_point()`).

<img src="viz_part-1_files/figure-gfm/unnamed-chunk-3-1.png" width="90%" />

``` r
# do the label: labs function to x,y and color
#adding message below the x: caption function
#adding title: title function
```

``` r
weather_df |> 
  ggplot(aes(x = tmin, y = tmax, color = name)) + geom_point(alpha=0.5)+ labs(
    title = "Temperature plot",
    x = "Min daily temp (Degrees C)",
    y ="Min daily temp",
    color = "Location",
    caption = "Max vs. min daily temp in three locations; data from rnoaa"
)+
  scale_x_continuous(
    breaks = c(-15,0,15),
    labels=c("-15 C", "0", "15")
  )+
  scale_y_continuous(
    position = "right",
    trans = "sqrt"
  )
```

    ## Warning in self$trans$transform(x): NaNs produced

    ## Warning: Transformation introduced infinite values in continuous y-axis

    ## Warning: Removed 142 rows containing missing values (`geom_point()`).

<img src="viz_part-1_files/figure-gfm/unnamed-chunk-4-1.png" width="90%" />

``` r
#scale:try to modify whatever the default mapping between a variable and the aesthetic that it's mapped onto sort of how those behave

#breaks: where you want to break as exist

#position: move to the place you want (right/left)

#trans function: transfer the numbers to what types of format, like sqrt

#limit function: limit the range you want or you can filter the range by using filter to limit the range value of variables
#like:   filter(tmax >=20, tmax <=30)
```

what about colors…

``` r
weather_df |> 
  ggplot(aes(x = tmin, y = tmax, color = name)) + geom_point(alpha=0.5)+ labs(
    title = "Temperature plot",
    x = "Min daily temp (Degrees C)",
    y ="Min daily temp",
    color = "Location",
    caption = "Max vs. min daily temp in three") + 
  viridis::scale_color_viridis(discrete = TRUE)
```

    ## Warning: Removed 17 rows containing missing values (`geom_point()`).

<img src="viz_part-1_files/figure-gfm/unnamed-chunk-5-1.png" width="90%" />

``` r
 # use viridis color package 
```

## Themes

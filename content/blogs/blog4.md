---
categories:
- ""
- ""
date: "2017-10-31T22:42:51-05:00"
description: Nullam et orci eu lorem consequat tincidunt vivamus et sagittis magna sed nunc rhoncus condimentum sem. In efficitur ligula tate urna. Maecenas massa sed magna lacinia magna pellentesque lorem ipsum dolor. Nullam et orci eu lorem consequat tincidunt. Vivamus et sagittis tempus.
draft: false
image: pic07.jpg
keywords: ""
slug: aliquam
title: Aliquam
---


```{r, setup, echo=FALSE}
knitr::opts_chunk$set(
  message = FALSE, 
  warning = FALSE, 
  tidy=FALSE,     # display code as typed
  size="small")   # slightly smaller font for code
options(digits = 3)

# default figure size
knitr::opts_chunk$set(
  fig.width=6.75, 
  fig.height=6.75,
  fig.align = "center"
)
```


```{r load-libraries,echo=FALSE}
library(tidyverse)  # Load ggplot2, dplyr, and all the other tidyverse packages
library(skimr)
library(mosaic)
library(ggthemes)
library(lubridate)
library(here)
library(janitor)
library(httr)
library(readxl)
library(vroom)
#remotes::install_github("kjhealy/socviz")
library(socviz)
```
# Climate change and temperature anomalies

```{r weather_data, cache=TRUE}
weather <- 
  read_csv("https://data.giss.nasa.gov/gistemp/tabledata_v3/NH.Ts+dSST.csv", 
           skip = 1, 
           na = "***")
```

```{r tidyweather}
tidyweather <- weather %>% select(Year,Jan,Feb,Mar,May,Jun,Jul,Aug,Sep,Oct,Nov,Dec) %>% 
  gather("month","delta",2:12)
```

## Plotting Information


Let us plot the data using a time-series scatter plot, and add a trendline. 

```{r scatter_plot, eval=TRUE}

tidyweather <- tidyweather %>%
  mutate(date = ymd(paste(as.character(Year), month, "1")),
         month = month(date, label=TRUE),
         year = year(date))

ggplot(tidyweather, aes(x=date, y = delta))+
  geom_point()+
  geom_smooth(color="red") +
  theme_bw() +
  labs (
    title = "Weather Anomalies"
  )

```
# Who Is the 'Worst' Bachelor

We're scraping imdb again and looking at the worst bachelor (in way too much depth) using ggplot2 charts

[Full article on DataCritics](https://datacritics.com/2018/09/06/exploring-the-data-who-was-the-worst-bachelor/)

### Prerequisites

```
library(rvest)
library(tidyverse)
library(magrittr)
library(scales)
library(knitr)
library(lubridate)
```

### Markdown file and RData 

Get thebachelor.Rmd and thebachelor.RData from the Code & Environment folder

### Web-Scraper
```{r}
url <- "https://www.imdb.com/title/tt0313038/episodes?season="

timevalues <- 13:22

unitedata <- function(x){
  full_url <- paste0(url, x)
  full_url
}

finalurl <- unitedata(timevalues)

imdbScrape <- function(x){
  page <- x
  name <- page %>% read_html() %>% html_nodes('#episodes_content strong a') %>% html_text() %>% as.data.frame()
  rating <- page %>% read_html() %>% html_nodes('.ipl-rating-widget') %>% html_text() %>% as.data.frame()
  details <- page %>% read_html() %>% html_nodes('.zero-z-index div') %>% html_text() %>% as.data.frame()
  
  chart <- cbind(name, rating, details)
  names(chart) <- c("Name", "Rating", "Details")
  chart <- as.tibble(chart)
  return(chart)
  Sys.sleep(5)
}

bachelor <- map_df(finalurl, imdbScrape)
head(bachelor)
```
![Scrape Outputs](https://github.com/imjakedaniels/thebachelor/blob/master/Code%20and%20Environment/scrape_output.PNG?raw=true)

### Visualizations
![Visual Outputs](https://github.com/imjakedaniels/thebachelor/blob/master/Visual%20Outputs/Bachelor%20Season%20Averages.jpeg?raw=true)
![Visual Outputs](https://github.com/imjakedaniels/thebachelor/blob/master/Visual%20Outputs/Arie%20Chart.jpeg?raw=true)
![Visual Outputs](https://github.com/imjakedaniels/thebachelor/blob/master/Visual%20Outputs/Juan%20Pablo%20Chart.jpeg?raw=true)
![Visual Outputs](https://github.com/imjakedaniels/thebachelor/blob/master/Visual%20Outputs/Linear%20Comparison.jpeg?raw=true)

## Authors

* **Jake Daniels** - *Visuals & Writing* - [LinkedIn](https://www.linkedin.com/in/imjakedaniels/)
* **Leila Taweel** - *Writing* - [LinkedIn](https://www.linkedin.com/in/leilataweel/)

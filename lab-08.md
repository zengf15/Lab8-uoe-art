Lab 08 - University of Edinburgh Art Collection
================
Fanyi Zeng
03/11/22

### Load packages and data

``` r
library(robotstxt)
library(rvest)
library(tidyverse)
library(dplyr)
library(skimr)
```

``` r
paths_allowed("https://collections.ed.ac.uk/art)")
```

### Read the page

``` r
# set url
first_url <- "https://collections.ed.ac.uk/art/search/*:*/Collection:%22edinburgh+college+of+art%7C%7C%7CEdinburgh+College+of+Art%22?offset=0"
# read html page
page <- read_html(first_url)
```

### Save the titles

``` r
titles <- page %>%
  html_nodes(".iteminfo") %>%
  html_node("h3 a") %>%
  html_text() %>%
  str_squish()
titles
```

    ##  [1] "Portrait of a Young Woman (1965)"                          
    ##  [2] "Drawing of a Man's Head (1955)"                            
    ##  [3] "Self Portrait against Window (Circa 1983)"                 
    ##  [4] "Untitled - Figured Drawing of Nude Woman on a Couch (1963)"
    ##  [5] "Seated Female Nude (Circa 1968)"                           
    ##  [6] "Male Nude Leaning on a Table (verso)"                      
    ##  [7] "Sienna Cathedral (1982)"                                   
    ##  [8] "Plate"                                                     
    ##  [9] "Virgin and Child"                                          
    ## [10] "Unknown (1950)"

### Save the links

``` r
links <- page %>%
  html_nodes(".iteminfo") %>%   # same nodes
  html_node("h3 a") %>%         # as before
  html_attr("href") %>%         # but get href attribute instead of text
  str_replace(".", "https://collections.ed.ac.uk/art")
links
```

    ##  [1] "https://collections.ed.ac.uk/art/record/21292?highlight=*:*" 
    ##  [2] "https://collections.ed.ac.uk/art/record/20804?highlight=*:*" 
    ##  [3] "https://collections.ed.ac.uk/art/record/22593?highlight=*:*" 
    ##  [4] "https://collections.ed.ac.uk/art/record/99329?highlight=*:*" 
    ##  [5] "https://collections.ed.ac.uk/art/record/20748?highlight=*:*" 
    ##  [6] "https://collections.ed.ac.uk/art/record/22650?highlight=*:*" 
    ##  [7] "https://collections.ed.ac.uk/art/record/102660?highlight=*:*"
    ##  [8] "https://collections.ed.ac.uk/art/record/53674?highlight=*:*" 
    ##  [9] "https://collections.ed.ac.uk/art/record/20571?highlight=*:*" 
    ## [10] "https://collections.ed.ac.uk/art/record/53557?highlight=*:*"

### Save the artists

``` r
artists <- page %>%
  html_nodes(".iteminfo") %>%
  html_node(".artist") %>%
  html_text()
artists
```

    ##  [1] "Irene Scott"      "James Robertson"  "Gwen W. Hardie"   "Gordon Bryce"    
    ##  [5] "Alan T. Johnston" "Unknown"          "Ann Oram"         "Emma Gillies"    
    ##  [9] "Unknown"          "Zygmunt Bukowski"

### Create a dataframe

``` r
first_ten <- tibble(
  title = titles,
  artist = artists,
  link = links
) 
first_ten  
```

    ## # A tibble: 10 x 3
    ##    title                                                      artist  link      
    ##    <chr>                                                      <chr>   <chr>     
    ##  1 Portrait of a Young Woman (1965)                           Irene ~ https://c~
    ##  2 Drawing of a Man's Head (1955)                             James ~ https://c~
    ##  3 Self Portrait against Window (Circa 1983)                  Gwen W~ https://c~
    ##  4 Untitled - Figured Drawing of Nude Woman on a Couch (1963) Gordon~ https://c~
    ##  5 Seated Female Nude (Circa 1968)                            Alan T~ https://c~
    ##  6 Male Nude Leaning on a Table (verso)                       Unknown https://c~
    ##  7 Sienna Cathedral (1982)                                    Ann Or~ https://c~
    ##  8 Plate                                                      Emma G~ https://c~
    ##  9 Virgin and Child                                           Unknown https://c~
    ## 10 Unknown (1950)                                             Zygmun~ https://c~

### Scrape the next page

``` r
# set url
second_url <- "https://collections.ed.ac.uk/art/search/*:*/Collection:%22edinburgh+college+of+art%7C%7C%7CEdinburgh+College+of+Art%22?offset=10"
# read html page
page2 <- read_html(second_url)
```

### Save the titles

``` r
titles2 <- page2 %>%
  html_nodes(".iteminfo") %>%
  html_node("h3 a") %>%
  html_text() %>%
  str_squish()
titles2
```

    ##  [1] "Portrait of Head of a Woman (1952)"              
    ##  [2] "Woman with Fish on her Head"                     
    ##  [3] "Portrait of a Seated Woman (1958)"               
    ##  [4] "etc (2006)"                                      
    ##  [5] "Racing Circuit (1987-1988)"                      
    ##  [6] "Standing Male Nude (1958)"                       
    ##  [7] "Still Life with Jug of Flowers, Goblet and Fruit"
    ##  [8] "Unknown (1952)"                                  
    ##  [9] "Seated Nude (1953)"                              
    ## [10] "Portrait of Woman in White Blouse (1962)"

### Save the links

``` r
links2 <- page2 %>%
  html_nodes(".iteminfo") %>%   # same nodes
  html_node("h3 a") %>%         # as before
  html_attr("href") %>%         # but get href attribute instead of text
  str_replace(".", "https://collections.ed.ac.uk/art")
links2
```

    ##  [1] "https://collections.ed.ac.uk/art/record/20846?highlight=*:*"
    ##  [2] "https://collections.ed.ac.uk/art/record/21530?highlight=*:*"
    ##  [3] "https://collections.ed.ac.uk/art/record/20865?highlight=*:*"
    ##  [4] "https://collections.ed.ac.uk/art/record/99865?highlight=*:*"
    ##  [5] "https://collections.ed.ac.uk/art/record/21456?highlight=*:*"
    ##  [6] "https://collections.ed.ac.uk/art/record/22716?highlight=*:*"
    ##  [7] "https://collections.ed.ac.uk/art/record/20753?highlight=*:*"
    ##  [8] "https://collections.ed.ac.uk/art/record/53513?highlight=*:*"
    ##  [9] "https://collections.ed.ac.uk/art/record/21121?highlight=*:*"
    ## [10] "https://collections.ed.ac.uk/art/record/22714?highlight=*:*"

### Save the artists

``` r
artists2 <- page2 %>%
  html_nodes(".iteminfo") %>%
  html_node(".artist") %>%
  html_text()
artists2
```

    ##  [1] "Frances Walker"             "John Bellany"              
    ##  [3] "Peter Standen"              "Catherine Robbie"          
    ##  [5] "Jane V Hyslop"              "Andrew P. W. Conkie"       
    ##  [7] "Sir William George Gillies" "Rosemary Bagshaw"          
    ##  [9] "Robert L Thompson"          "Elizabeth G. Kellar"

### Create a dataframe

``` r
second_ten <- tibble(
  title = titles2,
  artist = artists2,
  link = links2
) 
second_ten  
```

    ## # A tibble: 10 x 3
    ##    title                                            artist      link            
    ##    <chr>                                            <chr>       <chr>           
    ##  1 Portrait of Head of a Woman (1952)               Frances Wa~ https://collect~
    ##  2 Woman with Fish on her Head                      John Bella~ https://collect~
    ##  3 Portrait of a Seated Woman (1958)                Peter Stan~ https://collect~
    ##  4 etc (2006)                                       Catherine ~ https://collect~
    ##  5 Racing Circuit (1987-1988)                       Jane V Hys~ https://collect~
    ##  6 Standing Male Nude (1958)                        Andrew P. ~ https://collect~
    ##  7 Still Life with Jug of Flowers, Goblet and Fruit Sir Willia~ https://collect~
    ##  8 Unknown (1952)                                   Rosemary B~ https://collect~
    ##  9 Seated Nude (1953)                               Robert L T~ https://collect~
    ## 10 Portrait of Woman in White Blouse (1962)         Elizabeth ~ https://collect~

### Create a function

``` r
scrape_page <- function(url){
   page <- read_html(url)
titles <- page %>%
  html_nodes(".iteminfo") %>%
  html_node("h3 a") %>%
  html_text() %>%
  str_squish()
links <- page %>%
  html_nodes(".iteminfo") %>%   
  html_node("h3 a") %>%         
  html_attr("href") %>%         
  str_replace(".", "https://collections.ed.ac.uk/art")
artists <- page %>%
  html_nodes(".iteminfo") %>%
  html_node(".artist") %>%
  html_text()
ten <- tibble(
  title = titles,
  artist = artists,
  link = links)
}
```

### Test the function

It works!!

``` r
first_page <- scrape_page(first_url)
second_page <- scrape_page(second_url)
```

### Iteration, mapping, & saving as csv

``` r
urls <- paste0("https://collections.ed.ac.uk/art/search/*:*/Collection:%22edinburgh+college+of+art%7C%7C%7CEdinburgh+College+of+Art%22?offset=",seq(0,2980,by=10))
```

I am converting the following code into comments because it is going to
take a long time knitting. I already have the dataframe saved in my
folder.

``` r
#uoe_art <- map_dfr(urls, scrape_page)
#uoe_art
```

``` r
#write.csv(uoe_art, "uoe_art.csv",row.names = F)
```

### Read my saved data

``` r
uoe_art <- read_csv("uoe_art.csv")
```

    ## Rows: 2986 Columns: 3

    ## -- Column specification --------------------------------------------------------
    ## Delimiter: ","
    ## chr (3): title, artist, link

    ## 
    ## i Use `spec()` to retrieve the full column specification for this data.
    ## i Specify the column types or set `show_col_types = FALSE` to quiet this message.

### Separating title and date

``` r
uoe_art <- uoe_art %>%
  separate(title, into = c("title", "date"), sep = "\\(") %>%
  mutate(year = str_remove(date, "\\)") %>% as.numeric()) %>%
  select(title, artist, year, link)
```

    ## Warning in gregexpr(pattern, x, perl = TRUE): PCRE error
    ##  'UTF-8 error: byte 2 top bits not 0x80'
    ##  for element 281

    ## Warning in gregexpr(pattern, x, perl = TRUE): PCRE error
    ##  'UTF-8 error: byte 2 top bits not 0x80'
    ##  for element 735

    ## Warning in gregexpr(pattern, x, perl = TRUE): PCRE error
    ##  'UTF-8 error: isolated byte with 0x80 bit set'
    ##  for element 943

    ## Warning in gregexpr(pattern, x, perl = TRUE): PCRE error
    ##  'UTF-8 error: byte 2 top bits not 0x80'
    ##  for element 981

    ## Warning in gregexpr(pattern, x, perl = TRUE): PCRE error
    ##  'UTF-8 error: isolated byte with 0x80 bit set'
    ##  for element 1469

    ## Warning in gregexpr(pattern, x, perl = TRUE): PCRE error
    ##  'UTF-8 error: isolated byte with 0x80 bit set'
    ##  for element 1823

    ## Warning in gregexpr(pattern, x, perl = TRUE): PCRE error
    ##  'UTF-8 error: byte 2 top bits not 0x80'
    ##  for element 1824

    ## Warning in gregexpr(pattern, x, perl = TRUE): PCRE error
    ##  'UTF-8 error: byte 2 top bits not 0x80'
    ##  for element 2002

    ## Warning in gregexpr(pattern, x, perl = TRUE): PCRE error
    ##  'UTF-8 error: byte 2 top bits not 0x80'
    ##  for element 2254

    ## Warning in gregexpr(pattern, x, perl = TRUE): PCRE error
    ##  'UTF-8 error: byte 2 top bits not 0x80'
    ##  for element 2326

    ## Warning in gregexpr(pattern, x, perl = TRUE): PCRE error
    ##  'UTF-8 error: byte 2 top bits not 0x80'
    ##  for element 2957

    ## Warning: Expected 2 pieces. Additional pieces discarded in 39 rows [53, 135,
    ## 160, 209, 373, 567, 647, 662, 699, 729, 736, 772, 870, 877, 1010, 1116, 1165,
    ## 1330, 1441, 1491, ...].

    ## Warning: Expected 2 pieces. Missing pieces filled with `NA` in 645 rows [8, 9,
    ## 12, 17, 27, 30, 33, 37, 45, 47, 49, 56, 57, 58, 60, 66, 67, 77, 81, 82, ...].

    ## Warning in str_remove(date, "\\)") %>% as.numeric(): NAs introduced by coercion

The warnings tell us three things: 1. there are redundant art pieces
(with the same title and year); 2. there are art pieces with missing
titles or artists (which are then filled with “NA”); 3. there are art
pieces with missing years (which are then filled with “NA”).

### Analysis

115 pieces are missing artist info, and 1411 pieces missing year info.

``` r
skim(uoe_art)
```

|                                                  |         |
|:-------------------------------------------------|:--------|
| Name                                             | uoe_art |
| Number of rows                                   | 2986    |
| Number of columns                                | 4       |
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_   |         |
| Column type frequency:                           |         |
| character                                        | 3       |
| numeric                                          | 1       |
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_ |         |
| Group variables                                  | None    |

Data summary

**Variable type: character**

| skim_variable | n_missing | complete_rate | min | max | empty | n_unique | whitespace |
|:--------------|----------:|--------------:|----:|----:|------:|---------:|-----------:|
| title         |         1 |          1.00 |   0 |  95 |     8 |     1371 |          0 |
| artist        |       115 |          0.96 |   2 |  55 |     0 |     1115 |          0 |
| link          |         0 |          1.00 |  57 |  60 |     0 |     2986 |          0 |

**Variable type: numeric**

| skim_variable | n_missing | complete_rate |    mean |    sd |  p0 |  p25 |  p50 |    p75 | p100 | hist  |
|:--------------|----------:|--------------:|--------:|------:|----:|-----:|-----:|-------:|-----:|:------|
| year          |      1411 |          0.53 | 1964.37 | 55.65 |   2 | 1953 | 1961 | 1978.5 | 2020 | ▁▁▁▁▇ |

There is an art piece out of the ordinary - it has a year of “2”. This
happened because the art piece has two numbers, “2” and “1964”. We will
need to correct its year to “1964”.

``` r
uoe_art %>%
  ggplot(aes(x=year)) +
  geom_histogram()
```

    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

    ## Warning: Removed 1411 rows containing non-finite values (stat_bin).

![](lab-08_files/figure-gfm/bar-1.png)<!-- -->

There are two ways to correct the year: if_else and case_when.

Using if_else:

``` r
uoe_art_2 <- uoe_art %>%
  mutate(year=if_else(year==2, 1964, year, NA_real_))
```

Using case_when:

``` r
uoe_art_new <- uoe_art %>%
  mutate(year = case_when(
    year == 2 ~ 1964,
    year != 2 ~ year))
```

I limit the years to between 1820 (min) and 2020 (max) and make a new
histogram. Either uoe_art_new or uoe_art_2 will work. They are the same.

``` r
uoe_art_new %>%
  ggplot(aes(x=year)) +
  geom_histogram(binwidth=10) +
  xlim(1819,2020) +
  labs(title = "University of Edinburgh Artwork Collection (1819~2020)", subtitle = "As of 03/13/2022", x = "Year of Artwork Pieces", y = "Number of Artwork Pieces")
```

    ## Warning: Removed 1411 rows containing non-finite values (stat_bin).

    ## Warning: Removed 2 rows containing missing values (geom_bar).

![](lab-08_files/figure-gfm/newbar-1.png)<!-- -->

The most featured known artist is Emma Gillies. From this website
(<https://www.ed.ac.uk/news/2014/emma-gillies-031214>), I learned that
her family was renown in the art field in 20th century and very closely
related to the U of Edinburgh. Emma was a ceramic student there, and her
brother William was a teacher for 40 years as well as the Principal for
6 tears.

``` r
uoe_art_new %>%
  count(artist) %>%
  arrange(desc(n))
```

    ## # A tibble: 1,116 x 2
    ##    artist               n
    ##    <chr>            <int>
    ##  1 Unknown            357
    ##  2 Emma Gillies       148
    ##  3 <NA>               115
    ##  4 John Bellany        22
    ##  5 Ann F Ward          19
    ##  6 Boris Bucan         17
    ##  7 Marjorie Wallace    17
    ##  8 Zygmunt Bukowski    17
    ##  9 Gordon Bryce        16
    ## 10 William Gillon      16
    ## # ... with 1,106 more rows

11 pieces have the word “child” in them.

``` r
uoe_art_new %>%
  filter(
  str_detect(title, "child")|
  str_detect(title, "Child")
  )
```

    ## # A tibble: 11 x 4
    ##    title                           artist          year link                    
    ##    <chr>                           <chr>          <dbl> <chr>                   
    ##  1 "Virgin and Child"              Unknown           NA https://collections.ed.~
    ##  2 "Virgin and Child "             Unknown           NA https://collections.ed.~
    ##  3 "The Children's Hour "          Eduardo Luigi~    NA https://collections.ed.~
    ##  4 "Woman with Child and Still Li~ Catherine I. ~  1938 https://collections.ed.~
    ##  5 "Untitled - Children Playing "  Monika L I Ue~  1963 https://collections.ed.~
    ##  6 "Child's collar. Chinese"       Unknown           NA https://collections.ed.~
    ##  7 "Child's chinese headdress"     Unknown           NA https://collections.ed.~
    ##  8 "Virgin and Child "             Unknown           NA https://collections.ed.~
    ##  9 "Figure Composition with Nurse~ Edward A. Gage    NA https://collections.ed.~
    ## 10 "The Sun Dissolves while Man L~ Eduardo Luigi~    NA https://collections.ed.~
    ## 11 "Untitled - Portrait of a Woma~ William Gillon    NA https://collections.ed.~

coca_trial
================
Ben Kubacki
2024-11-07

- [Preview](#preview)
  - [Read in data](#read-in-data)
    - [w_news_2012.txt](#w_news_2012txt)
    - [wlp_news_2012.txt](#wlp_news_2012txt)
    - [subgenre text](#subgenre-text)
  - [Data in focus: text_news_2012](#data-in-focus-text_news_2012)
    - [Summarize text_news_2012](#summarize-text_news_2012)
    - [Separate columns into 2 to isolate the number of the document and
      the text, then Rename
      columns:](#separate-columns-into-2-to-isolate-the-number-of-the-document-and-the-text-then-rename-columns)
    - [Format & Description](#format--description)
- [web-scraping vocab lists](#web-scraping-vocab-lists)
  - [Data Sharing](#data-sharing)
  - [References](#references)
- [Session Info](#session-info)

# Preview

In this Rmd, I will have snippets of my code and analysis of the data
for a portion of the intended dataset. See Data Sharing at the end of
this document.

## Read in data

### w_news_2012.txt

This is the text of each document per row, with the document identifying
number in the beginning of the string after two `##`.

``` r
text_news_2012 <- read_tsv("../Context-Word-Embeddings/data_private/text_newspaper_lsp/w_news_2012.txt", col_names = F, show_col_types = F) 

text_news_2012 %>% summary()
```

          X1           
     Length:1559       
     Class :character  
     Mode  :character  

``` r
head(text_news_2012) %>%
  substring(first = 1, last = 300L) %>% 
  writeLines()
```

    c("##4113706 The top U.S. general , visiting Israel at a delicate and dangerous moment in the global standoff with Tehran , is expected to press for restraint amid fears that the Jewish state is nearing a decision to attack Iran 's nuclear program . # Thursday 's arrival of Army Gen. Martin Dempsey 

### wlp_news_2012.txt

This is the same text but lemmatized and tokenized with one word per row
and columns showing word, lemma, and POS.

``` r
wlp_news_2012 <- read_tsv("../Context-Word-Embeddings/data_private/wlp_newspaper_lsp/wlp_news_2012.txt", col_names=F, show_col_types = F) %>% rename(word = X1, lemma = X2, POS = X3)
```

    Warning: One or more parsing issues, call `problems()` on your data frame for details,
    e.g.:
      dat <- vroom(...)
      problems(dat)

``` r
wlp_news_2012 %>% 
  summary()
```

         word              lemma               POS           
     Length:1371886     Length:1371886     Length:1371886    
     Class :character   Class :character   Class :character  
     Mode  :character   Mode  :character   Mode  :character  

``` r
head(wlp_news_2012) %>%
  slice(1:20)
```

    # A tibble: 6 × 3
      word      lemma   POS   
      <chr>     <chr>   <chr> 
    1 ##4113706 <NA>    <NA>  
    2 The       the     at    
    3 top       top     jj_nn1
    4 U.S.      us      np1   
    5 general   general nn1_jj
    6 ,         ,       y     

### subgenre text

This is a dataset listing the subgenre of each source in the data,
filtered to only showing the subgenres of the Newspaper genre.

``` r
subgenres_news <- read_delim("../Context-Word-Embeddings/data_private/subgenreCodes.txt", col_names = F) %>% 
  rename(code = X1, subgenre = X2) %>%  
  filter(code %in% 135:142)
```

    Rows: 42 Columns: 2
    ── Column specification ────────────────────────────────────────────────────────
    Delimiter: "\t"
    chr (1): X2
    dbl (1): X1

    ℹ Use `spec()` to retrieve the full column specification for this data.
    ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
subgenres_news
```

    # A tibble: 8 × 2
       code subgenre       
      <dbl> <chr>          
    1   135 NEWS:Misc      
    2   136 NEWS:News_Intl 
    3   137 NEWS:News_Natl 
    4   138 NEWS:News_Local
    5   139 NEWS:Money     
    6   140 NEWS:Life      
    7   141 NEWS:Sports    
    8   142 NEWS:Editorial 

## Data in focus: text_news_2012

### Summarize text_news_2012

There are 1559 rows, or documents, and only one column. Below is a
sample of the text inside one of the cells, including a break \#.

``` r
summary(text_news_2012)
```

          X1           
     Length:1559       
     Class :character  
     Mode  :character  

``` r
head(text_news_2012) %>%
  substring(first = 1, last = 300L)
```

    [1] "c(\"##4113706 The top U.S. general , visiting Israel at a delicate and dangerous moment in the global standoff with Tehran , is expected to press for restraint amid fears that the Jewish state is nearing a decision to attack Iran 's nuclear program . # Thursday 's arrival of Army Gen. Martin Dempsey "

Count the words with `str_split()`

Method from [Sanderson,
2024](https://www.r-bloggers.com/2024/05/counting-words-in-a-string-in-r-a-comprehensive-guide/)

Seems the result is 1331 words, which is safe for sharing.

``` r
row1 <- text_news_2012[1, ]

row1_words <- str_split(row1$X1, "\\s+")[[1]]

word_count_row1 <- length(row1_words)

word_count_row1
```

    [1] 1331

### Separate columns into 2 to isolate the number of the document and the text, then Rename columns:

1559 documents in this file.

``` r
text_news_2012 <- text_news_2012 %>% 
  select(document = X1, text = X1) 

text_news_2012$document <- text_news_2012$document %>% 
  str_extract("^.{9}") %>% 
  str_remove("^..") 

text_news_2012$text <- text_news_2012$text %>% 
  str_remove("^.{9}") 

# text_news_2012 %>% 
#   head()                 ## 3706
# text_news_2012 %>% 
#   tail()                 ## 5379
  
text_news_2012$document %>% 
  unique() %>% 
  length()
```

    [1] 1559

### Format & Description

- \# symbol indicates paragraph breaks  
- punctuation marks have a space on either side  
- @ symbol indicates redacted text? or unreadable text?

Two methods of counting words, but the second comes from the R-Blogger
source above, so may be more reliable than mine.

``` r
text_news_2012_row1 <- text_news_2012[1, ]

## Character count, word count per document
text_news_2012_row1 <- text_news_2012_row1 %>% 
  mutate(chr_count = str_count(text_news_2012_row1$text), word_count = str_count(text_news_2012_row1$text, "\\s\\b"), word_count2 = str_count(text_news_2012_row1$text, "\\s+"))

head(text_news_2012_row1)
```

    # A tibble: 1 × 5
      document text                                 chr_count word_count word_count2
      <chr>    <chr>                                    <int>      <int>       <int>
    1 4113706  " The top U.S. general , visiting I…      7347       1104        1330

# web-scraping vocab lists

This list from
[Gorick](https://www.gorick.com/blog/workplace-jargon-dictionary) is a
starting point for a target word list.

``` r
gorick_list <- read_html("https://www.gorick.com/blog/workplace-jargon-dictionary")

gorick_list <- gorick_list %>% 
  html_elements("h3") %>% 
  html_text() 

gorick_list <- gorick_list %>% 
  str_remove("^.") %>% 
  str_remove(".$") 

## Remove strings with acronyms and maybe numbers
gorick_list <- gorick_list %>% 
  str_remove_all("^[ABCDEFGHIJKLMNOPQRSTUVWXYZ]{2,9}$")

gorick_list <- gorick_list %>%
  tolower()

gorick_tbl <- tibble(word = gorick_list) 

gorick_tbl %>% slice(1:10)
```

    # A tibble: 10 × 1
       word              
       <chr>             
     1 "2.0"             
     2 "30,000-feet view"
     3 "80/20"           
     4 "action item"     
     5 "actionable"      
     6 "add value"       
     7 "adjourn"         
     8 "agenda"          
     9 "align upon"      
    10 ""                

### Data Sharing

I will include `slice()`d data used in this trial analysis (and later in
the full analysis) in the `data/` folder of this repo. The stipulations
of the license for the COCA prohibit distributing data of 50,000 or more
words, so to be safe, I am only including slices of a sample of rows.
This will allow viewers and users to understand the raw data source from
which I am drawing, while preserving the protection of the data. The
names of the data sources are maintained so that replicators can easily
locate and reproduce my results.

### References

<https://www.corpusdata.org>

Davies, Mark. (2008-) The Corpus of Contemporary American English
(COCA). Available online at <https://www.english-corpora.org/coca/>

# Session Info

``` r
sessionInfo()
```

    R version 4.4.2 (2024-10-31 ucrt)
    Platform: x86_64-w64-mingw32/x64
    Running under: Windows 11 x64 (build 26100)

    Matrix products: default


    locale:
    [1] LC_COLLATE=English_United States.utf8 
    [2] LC_CTYPE=English_United States.utf8   
    [3] LC_MONETARY=English_United States.utf8
    [4] LC_NUMERIC=C                          
    [5] LC_TIME=English_United States.utf8    

    time zone: America/New_York
    tzcode source: internal

    attached base packages:
    [1] stats     graphics  grDevices utils     datasets  methods   base     

    other attached packages:
     [1] tidytext_0.4.2  rvest_1.0.4     lubridate_1.9.3 forcats_1.0.0  
     [5] stringr_1.5.1   dplyr_1.1.4     purrr_1.0.2     readr_2.1.5    
     [9] tidyr_1.3.1     tibble_3.2.1    ggplot2_3.5.1   tidyverse_2.0.0

    loaded via a namespace (and not attached):
     [1] janeaustenr_1.0.0 utf8_1.2.4        generics_0.1.3    xml2_1.3.6       
     [5] stringi_1.8.4     lattice_0.22-6    hms_1.1.3         digest_0.6.37    
     [9] magrittr_2.0.3    evaluate_1.0.1    grid_4.4.2        timechange_0.3.0 
    [13] fastmap_1.2.0     Matrix_1.7-1      httr_1.4.7        selectr_0.4-2    
    [17] fansi_1.0.6       scales_1.3.0      cli_3.6.3         crayon_1.5.3     
    [21] rlang_1.1.4       tokenizers_0.3.0  bit64_4.5.2       munsell_0.5.1    
    [25] withr_3.0.2       yaml_2.3.10       parallel_4.4.2    tools_4.4.2      
    [29] tzdb_0.4.0        colorspace_2.1-1  curl_5.2.3        vctrs_0.6.5      
    [33] R6_2.5.1          lifecycle_1.0.4   bit_4.5.0         vroom_1.6.5      
    [37] pkgconfig_2.0.3   pillar_1.9.0      gtable_0.3.6      glue_1.8.0       
    [41] Rcpp_1.0.13-1     xfun_0.49         tidyselect_1.2.1  rstudioapi_0.17.1
    [45] knitr_1.48        htmltools_0.5.8.1 SnowballC_0.7.1   rmarkdown_2.29   
    [49] compiler_4.4.2   

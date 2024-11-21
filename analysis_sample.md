Analysis Sample
================
Ben Kubacki
2024-11-07

- [Read in data](#read-in-data)
  - [Plan: Write sample data with approx 4,400
    words](#plan-write-sample-data-with-approx-4400-words)
- [Preview](#preview)
  - [Explanation of creation of data
    sample](#explanation-of-creation-of-data-sample)
  - [Read in data](#read-in-data-1)
    - [w_news_2012.txt](#w_news_2012txt)
    - [wlp_news_2012.txt](#wlp_news_2012txt)
  - [Data in focus: text_news_2012](#data-in-focus-text_news_2012)
    - [Summarize text_news_2012](#summarize-text_news_2012)
    - [Separate columns into 2 to isolate the number of the document and
      the text, then Rename
      columns:](#separate-columns-into-2-to-isolate-the-number-of-the-document-and-the-text-then-rename-columns)
    - [Format & Description](#format--description)
    - [Data Sharing](#data-sharing)
    - [References](#references)
- [Session Info](#session-info)

# Read in data

- Files: 1 of 22 in (analysis.Rmd)\[analysis.Rmd\] and
  (analysis.nb.Rmd)\[analysis.nd.Rmd\]  
- Type: text  
- Contents: Document number, text

Method

- Assign to object `news_sample`  
- Use `c()` to combine all relative paths of the files as strings in
  character vector  
- Read in using `read_lines()`  
- Use `map()` to iterate `read_lines()` over all 22 files at once  
- Use `unlist()` to turn from list containing character vectors (each
  doc’s text) into character vector containing elements (each doc’s
  text)  
- Read in from `data_private/` folder within this repo but ignored with
  `.gitignore`

## Plan: Write sample data with approx 4,400 words

``` r
# news_sample_2012 <- read_lines("data_private/text_newspaper_lsp/w_news_2012.txt")
# 
# news_sample <- c("data_private/text_newspaper_lsp/w_news_2011.txt",
#                           "data_private/text_newspaper_lsp/w_news_2012.txt") %>% 
#   map(read_lines) %>% unlist()
# 
# news_sample %>% str_split("\\s+") %>% length()
# 
# news_sample_2012 %>% str_split("\\s+") %>% length()
# 
# news_sample_2012 <- news_sample_2012 %>% unlist()
# 
# news_sample_2012 <- tibble(news_sample_2012)
# 
# news_sample_2012 <- news_sample_2012 %>%
#   slice(1:10) 
# 
# news_sample_2012 <- news_sample_2012 %>% 
#   rename(document_id = news_sample_2012)
# 
# news_sample_2012 <- news_sample_2012 %>% 
#   select(document_id = document_id, text = document_id)
# 
# news_sample_2012 <- news_sample_2012 %>% mutate(document_id = str_sub(document_id, 3,9), text = str_sub(text, start = 10)) 
# 
# news_sample_2012 %>% 
#   map(text[1], str_split("\\s+"))

# word_count_row1 <- length(row1_words)

# %>%
#   write_csv("../data/data_sample_text.csv")
```

# Preview

In this Rmd, I have snippets of my code and analysis of the data for a
portion of the intended dataset. See Data Sharing at the end of this
document.

## Explanation of creation of data sample

Here.

## Read in data

### w_news_2012.txt

This is the text of each document per row, with the document identifying
number in the beginning of the string after two `##`.

``` r
text_news_2012 <- read_csv("data/data_sample_text.csv", col_names = F, show_col_types = F) 

head(text_news_2012) %>%
  substring(first = 1, last = 300L) %>% 
  writeLines()
```

    c("##4113706 The top U.S. general , visiting Israel at a delicate and dangerous moment in the global standoff with Tehran , is expected to press for restraint amid fears that the Jewish state is nearing a decision to attack Iran 's nuclear program . # Thursday 's arrival of Army Gen. Martin Dempsey 

### wlp_news_2012.txt

This is the same text but lemmatized and tokenized with one word per row
and columns showing word, lemma, and POS.

``` r
wlp_news_2012 <- read_csv("data/data_sample_tokenized.csv", col_names=F, show_col_types = F) %>% rename(word = X1, lemma = X2, POS = X3)
```

``` r
wlp_news_2012 %>% 
  summary()
```

         word              lemma               POS           
     Length:50001       Length:50001       Length:50001      
     Class :character   Class :character   Class :character  
     Mode  :character   Mode  :character   Mode  :character  

``` r
head(wlp_news_2012) %>%
  slice(1:20)
```

    # A tibble: 6 × 3
      word      lemma   POS   
      <chr>     <chr>   <chr> 
    1 ##4113706 ...2    ...3  
    2 The       the     at    
    3 top       top     jj_nn1
    4 U.S.      us      np1   
    5 general   general nn1_jj
    6 ,         ,       y     

## Data in focus: text_news_2012

### Summarize text_news_2012

There are 1559 rows, or documents, and only one column. Below is a
sample of the text inside one of the cells, including a break \#.

``` r
summary(text_news_2012)
```

          X1           
     Length:6          
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

1559 documents in the original file.

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

    [1] 6

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
  mutate(chr_count = str_count(text_news_2012_row1$text), word_count2 = str_count(text_news_2012_row1$text, "\\s+"))

head(text_news_2012_row1)
```

    # A tibble: 1 × 4
      document text                                            chr_count word_count2
      <chr>    <chr>                                               <int>       <int>
    1 4113706  " The top U.S. general , visiting Israel at a …      7347        1330

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
     [1] tm_0.7-14       NLP_0.3-0       cluster_2.1.6   Rling_1.0      
     [5] text2vec_0.6.4  tidytext_0.4.2  rvest_1.0.4     readxl_1.4.3   
     [9] lubridate_1.9.3 forcats_1.0.0   stringr_1.5.1   dplyr_1.1.4    
    [13] purrr_1.0.2     readr_2.1.5     tidyr_1.3.1     tibble_3.2.1   
    [17] ggplot2_3.5.1   tidyverse_2.0.0

    loaded via a namespace (and not attached):
     [1] gtable_0.3.6        xfun_0.49           lattice_0.22-6     
     [4] tzdb_0.4.0          vctrs_0.6.5         tools_4.4.2        
     [7] generics_0.1.3      parallel_4.4.2      fansi_1.0.6        
    [10] janeaustenr_1.0.0   pkgconfig_2.0.3     tokenizers_0.3.0   
    [13] Matrix_1.7-1        data.table_1.16.2   lifecycle_1.0.4    
    [16] compiler_4.4.2      munsell_0.5.1       RhpcBLASctl_0.23-42
    [19] htmltools_0.5.8.1   SnowballC_0.7.1     yaml_2.3.10        
    [22] pillar_1.9.0        crayon_1.5.3        rsparse_0.5.2      
    [25] tidyselect_1.2.1    digest_0.6.37       slam_0.1-54        
    [28] stringi_1.8.4       fastmap_1.2.0       grid_4.4.2         
    [31] colorspace_2.1-1    cli_3.6.3           magrittr_2.0.3     
    [34] utf8_1.2.4          withr_3.0.2         scales_1.3.0       
    [37] bit64_4.5.2         float_0.3-2         timechange_0.3.0   
    [40] rmarkdown_2.29      httr_1.4.7          bit_4.5.0          
    [43] mlapi_0.1.1         cellranger_1.1.0    hms_1.1.3          
    [46] evaluate_1.0.1      knitr_1.48          rlang_1.1.4        
    [49] Rcpp_1.0.13-1       glue_1.8.0          xml2_1.3.6         
    [52] vroom_1.6.5         rstudioapi_0.17.1   lgr_0.4.4          
    [55] R6_2.5.1           

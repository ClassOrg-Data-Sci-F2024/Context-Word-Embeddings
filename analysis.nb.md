Analysis of Workplace-Themed Term Co-Occurrence Matrices
================
Ben Kubacki
20 November 2024

- [Load packages in hidden setup chunk (see list
  below)](#load-packages-in-hidden-setup-chunk-see-list-below)
- [Read in data](#read-in-data)
  - [Continue with TCM matrix](#continue-with-tcm-matrix)
  - [Test a DTM matrix with the rest the
    same.](#test-a-dtm-matrix-with-the-rest-the-same)
- [Session info](#session-info)

# Load packages in hidden setup chunk (see list below)

``` r
# library(tidyverse)
# library(dplyr)
# library(purrr)
# library(stringr)
# library(readxl)
# 
# # web scraping
# library(rvest)
# 
# # tokenizer
# library(tidytext)
# 
# # word embeddings
# library(text2vec)
# library(Rling)
# library(cluster)
# library(tm)
```

> Not intended for general audience. See analysis.md

# Read in data

- Files: 22  
- Type: text  
- Contents: Document number, text  
- Text components: spaces, `#` paragraph or sentence separators

Method

- Assign to object `news_raw`  
- Use `c()` to combine all relative paths of the files as strings in
  character vector  
- Read in using `read_lines()`  
- Use `map()` to iterate `read_lines()` over all 22 files at once  
- Use `unlist()` to turn from list containing character vectors (each
  doc’s text) into character vector containing elements (each doc’s
  text)  
- Read in from `data_private/` folder within this repo but ignored with
  `.gitignore`

``` r
news <- c("data_private/text_newspaper_lsp/w_news_1990.txt",
           "data_private/text_newspaper_lsp/w_news_1991.txt",
           "data_private/text_newspaper_lsp/w_news_1992.txt",
           "data_private/text_newspaper_lsp/w_news_1993.txt",
           "data_private/text_newspaper_lsp/w_news_1994.txt",
           "data_private/text_newspaper_lsp/w_news_1995.txt",
           "data_private/text_newspaper_lsp/w_news_1996.txt",
           "data_private/text_newspaper_lsp/w_news_1997.txt",
           "data_private/text_newspaper_lsp/w_news_1998.txt",
           "data_private/text_newspaper_lsp/w_news_1999.txt",
           "data_private/text_newspaper_lsp/w_news_2000.txt",
           "data_private/text_newspaper_lsp/w_news_2001.txt",
           "data_private/text_newspaper_lsp/w_news_2002.txt",
           "data_private/text_newspaper_lsp/w_news_2003.txt",
           "data_private/text_newspaper_lsp/w_news_2004.txt",
           "data_private/text_newspaper_lsp/w_news_2005.txt",
           "data_private/text_newspaper_lsp/w_news_2006.txt",
           "data_private/text_newspaper_lsp/w_news_2007.txt",
           "data_private/text_newspaper_lsp/w_news_2008.txt",
           "data_private/text_newspaper_lsp/w_news_2009.txt",
           "data_private/text_newspaper_lsp/w_news_2010.txt",
           "data_private/text_newspaper_lsp/w_news_2011.txt",
           "data_private/text_newspaper_lsp/w_news_2012.txt") %>% 
  map(read_lines) %>% unlist() 

# news %>% head()
# 
# news %>% summary()

news <- tibble(news)

news <- news %>% 
  rename(document_id = news)

# news %>% slice(1:10)

# news <- news %>% 
#  rename(document_id = news)
# select(document_id = news) %>% 
#   separate(into = c("doc", "text"), sep = "^.{9}", remove = F)
```

Create test data frame

``` r
news_test <- news %>% head()

# news_test %>% 
#   str_extract("^{0}")

news_test <- tibble(news_test)

news_test <- news_test %>% 
  select(document_id = document_id, text = document_id)
```

``` r
news_test <- news_test %>% mutate(document_id = str_sub(document_id, 3,9), text = str_sub(text, start = 10))
```

``` r
## Test Method 1


# news_test$document_id[] <- news_test$document_id[-1] %>%
#   str_extract("^.{9}") %>%
#   str_remove("^..") 
# 
# news_test$text <- news_test$text %>%
#   str_remove("^.{9}")
# 
# news_test$document_id %>%
#   unique() %>%
#   length()

## Method 1

# news <- tibble(news) %>% 
#   select(document_id = news, text = news)
# 
# news$document_id <- news$document_id %>% 
#   str_extract("^.{9}") %>% 
#   str_remove("^..") 
# 
# news$text <- news$text %>% 
#   str_remove("^.{9}") 
#   
# news$document_id %>% 
#   unique() %>% 
#   length()
```

Method 2: use separate() to separate columns

``` r
# news_test %>% 
#   separate(col = document_id, into = c("document_id", "text"), sep = "^.{9}", remove = F)
# news_test %>% 
#   separate_wider_delim(cols = document_id, delim = regex("^.{9}"), names = c("document_id", "text"))
# news_test %>% 
#   separate_wider_position(cols = document_id, widths = 9)
# news_test %>% 
#   separate_wider_regex(cols = document_id, patterns = )
# 
# ## test
# 
# head(news) %>% 
#   rename(document_id = news)
# 
# news %>% 
#   rename(document_id = news)
# select(document_id = news) %>% 
#   separate(into = c("doc", "text"), sep = "^.{9}", remove = F)
```

method 3 (original)

``` r
# news_test <- news_test %>% 
#   select(document_id = document_id, text = document_id)
# 
# news_test %>% 
#   str_subset()
```

## Continue with TCM matrix

Source of demo, including object names, code, and comments inside code:
(Selivanov,
2023)\[<https://cran.r-project.org/web/packages/text2vec/vignettes/glove.html>\]

“You can achieve much better results by experimenting with
skip_grams_window and the parameters of the GloVe class (including word
vectors size and the number of iterations).”

``` r
# Create iterator over tokens
tokens <- space_tokenizer(news_test$text)

# Create vocabulary. Terms will be unigrams (simple words).
it <- itoken(tokens, progressbar = FALSE)
vocab <- create_vocabulary(it)
```

Use source’s vocab list first, change later to mine.

Term count min: Minimum number of token counts for a term to be included
in the corpus.

Was set at 5. **Set at 2**

``` r
vocab <- prune_vocabulary(vocab, term_count_min = 2L)
```

Skip Gram Window: Size of context window around the target word

**Set at 5**

``` r
# Use our filtered vocabulary
vectorizer <- vocab_vectorizer(vocab)

# use window of 5 for context words
tcm <- create_tcm(it, vectorizer, skip_grams_window = 5L)
```

May need to specify the number of cores, according to author.

``` r
glove = GlobalVectors$new(rank = 50, x_max = 10)
wv_main = glove$fit_transform(tcm, n_iter = 10, convergence_tol = 0.01, n_threads = 8)
```

    INFO  [22:48:45.031] epoch 1, loss 0.1820
    INFO  [22:48:45.076] epoch 2, loss 0.0950
    INFO  [22:48:45.089] epoch 3, loss 0.0684
    INFO  [22:48:45.097] epoch 4, loss 0.0533
    INFO  [22:48:45.106] epoch 5, loss 0.0432
    INFO  [22:48:45.117] epoch 6, loss 0.0358
    INFO  [22:48:45.125] epoch 7, loss 0.0303
    INFO  [22:48:45.133] epoch 8, loss 0.0259
    INFO  [22:48:45.141] epoch 9, loss 0.0223
    INFO  [22:48:45.149] epoch 10, loss 0.0194

``` r
wv_context = glove$components
word_vectors = wv_main + t(wv_context)
```

``` r
# test <- word_vectors["meeting", , drop = FALSE] -
#   word_vectors["meet", , drop = FALSE] +
#   word_vectors["conference", , drop = FALSE]
# cos_sim = sim2(x = word_vectors, y = test, method = "cosine", norm = "l2")
# head(sort(cos_sim[,1], decreasing = TRUE), 5)


by <- word_vectors["and", , drop = FALSE] -
  word_vectors["to", , drop = FALSE] +
  word_vectors["with", , drop = FALSE]
cos_sim = sim2(x = word_vectors, y = by, method = "cosine", norm = "l2")
head(sort(cos_sim[,1], decreasing = TRUE), 5)
```

          and      with         ,        by       the 
    0.7418695 0.6398215 0.4458370 0.4427143 0.4218288 

## Test a DTM matrix with the rest the same.

``` r
## Create iterator over tokens

# tokens <- space_tokenizer(news_test$text)

## Create vocabulary. Terms will be unigrams (simple words).

# it <- itoken(tokens, progressbar = FALSE)
# vocab <- create_vocabulary(it)
```

Use source’s vocab list first, change later to mine.

Term count min: Minimum number of token counts for a term to be included
in the corpus.

Was set at 5. **Set at 2**

Use arguments to determine term and doc count mins and maxes, as well as
overall vocab max.

``` r
# vocab <- prune_vocabulary(vocab, term_count_min = 2L)
```

Skip Gram Window: Size of context window around the target word

**Set at 5**

``` r
## Use our filtered vocabulary

# vectorizer <- vocab_vectorizer(vocab)

## create DTM

dtm <- create_dtm(it, vectorizer, type= "dgCMatrix")
```

Try TfIdf
(p. 32)[CRAN](https://cran.r-project.org/web/packages/text2vec/text2vec.pdf)

``` r
# ## Creates tfidf model
# 
# tfidf <- TfIdf$new(smooth_idf = TRUE, norm = c("l1", "l2", "none"), sublinear_tf = FALSE) 
# 
# ## and then transforms it.
# 
# ## fit model to an input sparse matrix (preferably in "dgCMatrix" format) 
# 
# tfidf$fit_transform(dtm)
# 
# ## transform new data x using tf-idf from train data
# 
# tfidf$transform(dtm) 
```

Try TfIdf with other `create_dtm()` method
(p. 33)[CRAN](https://cran.r-project.org/web/packages/text2vec/text2vec.pdf)

``` r
tokens2 <- word_tokenizer(tolower(news_test$text))

dtm <- create_dtm(itoken(tokens2), hash_vectorizer())

model_tfidf <- TfIdf$new()

dtm_tfidf <- model_tfidf$fit_transform(dtm)
```

``` r
## Creates tfidf model

tfidf <- TfIdf$new(smooth_idf = TRUE, norm = c("l1", "l2", "none"), sublinear_tf = FALSE) 

## and then transforms it.

## fit model to an input sparse matrix (preferably in "dgCMatrix" format) 

tfidf$fit_transform(dtm)
```

    6 x 262144 sparse Matrix of class "dgCMatrix"
                                                                                
    1 . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . ......
    2 . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . ......
    3 . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . ......
    4 . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . ......
    5 . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . ......
    6 . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . ......

     .....suppressing 262110 columns in show(); maybe adjust options(max.print=, width=)
     ..............................

``` r
## transform new data x using tf-idf from train data

tfidf$transform(dtm) 
```

    6 x 262144 sparse Matrix of class "dgCMatrix"
                                                                                
    1 . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . ......
    2 . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . ......
    3 . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . ......
    4 . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . ......
    5 . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . ......
    6 . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . ......

     .....suppressing 262110 columns in show(); maybe adjust options(max.print=, width=)
     ..............................

May need to specify the number of cores, according to author.

``` r
# glove = GlobalVectors$new(rank = 50, x_max = 10)
# wv_main = glove$fit_transform(dtm, n_iter = 10, convergence_tol = 0.01, n_threads = 8)
```

``` r
# wv_context = glove$components
# word_vectors = wv_main + t(wv_context)
```

``` r
# test <- word_vectors["meeting", , drop = FALSE] -
#   word_vectors["meet", , drop = FALSE] +
#   word_vectors["conference", , drop = FALSE]
# cos_sim = sim2(x = word_vectors, y = test, method = "cosine", norm = "l2")
# head(sort(cos_sim[,1], decreasing = TRUE), 5)


# by <- word_vectors["and", , drop = FALSE] -
#   word_vectors["to", , drop = FALSE] +
#   word_vectors["with", , drop = FALSE]
# cos_sim = sim2(x = word_vectors, y = by, method = "cosine", norm = "l2")
# head(sort(cos_sim[,1], decreasing = TRUE), 5)
```

# Session info

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

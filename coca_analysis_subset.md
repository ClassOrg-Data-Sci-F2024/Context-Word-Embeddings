COCA Corpus Data Analysis
================
Ben Kubacki
28 Oct 2024

- [Initial steps](#initial-steps)
  - [Sample 1990 file (Word Lemma
    POS)](#sample-1990-file-word-lemma-pos)
    - [Read the 1990 file of word-lemma-POS data from the Spoken genre
      of the
      COCA](#read-the-1990-file-of-word-lemma-pos-data-from-the-spoken-genre-of-the-coca)
    - [Change column names to word, lemma,
      pos](#change-column-names-to-word-lemma-pos)
    - [Size and makeup](#size-and-makeup)
    - [Add column with collapsed verb-noun-adjective-etc. POS
      tags](#add-column-with-collapsed-verb-noun-adjective-etc-pos-tags)
    - [Attempt at seed word analysis using
      “work”](#attempt-at-seed-word-analysis-using-work)
  - [Aggregated files](#aggregated-files)
- [Session info](#session-info)

# Initial steps

## Sample 1990 file (Word Lemma POS)

### Read the 1990 file of word-lemma-POS data from the Spoken genre of the COCA

``` r
wlp_1990 <- read_tsv("../Context-Word-Embeddings/data/wlp_spoken_kde/wlp_spok_1990.txt", col_names = FALSE)
```

    Warning: One or more parsing issues, call `problems()` on your data frame for details,
    e.g.:
      dat <- vroom(...)
      problems(dat)

    Rows: 3000787 Columns: 3
    ── Column specification ────────────────────────────────────────────────────────
    Delimiter: "\t"
    chr (3): X1, X2, X3

    ℹ Use `spec()` to retrieve the full column specification for this data.
    ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

### Change column names to word, lemma, pos

``` r
wlp_1990 <- wlp_1990 %>% 
  rename(word = X1, lemma = X2, pos = X3) 
```

### Size and makeup

Some information about the data (see below):

- Rows: 3 million and 787  
- Type: character vectors  
- Unique values of Part of Speech (POS): 3435

The POS column `$pos` can be used to create a new simpler column with a
small number of values: noun, verb, adjective, adverb, determiner, etc.
As it currently is, that column contains detailed POS information that
may or may not be relevant for this project.

There is more information about the data in the other data files (`text`
and `db`) that may help determine how the strings are separated. For
example, the first rows appear to be connected in a single utterance
starting with “Good evening…” How are the data organized?

The text file may be interesting to explore. How is the text file
different from the WLP data?

``` r
wlp_1990 %>% 
  summary()
```

         word              lemma               pos           
     Length:3000787     Length:3000787     Length:3000787    
     Class :character   Class :character   Class :character  
     Mode  :character   Mode  :character   Mode  :character  

``` r
wlp_1990 %>% 
  nrow()
```

    [1] 3000787

``` r
is.vector(wlp_1990$word)  
```

    [1] TRUE

``` r
## The head() output is commented out until licensing restrictions can be double checked.
# wlp_1990 %>% 
#   head()

wlp_1990$pos %>% 
   unique() %>% 
  length()
```

    [1] 3435

``` r
  # colnames() %>%
  # length()
```

### Add column with collapsed verb-noun-adjective-etc. POS tags

In this section, a new column will be added with mutate() which lists
the basic POS of each word. Metadata about the COCA data will need to be
found that explains the POS values in the current column.

### Attempt at seed word analysis using “work”

The anticipated analysis for this project involves using a seed word
from workplace-related English words (i.e., “work” or “meet” or “job”)
and using word embeddings to find nearby and related words.

As it currently stands, the code below is returning errors that will be
cleared up in the next phase.

``` r
# wlp_1990 %>%
#   filter(pos == "vv0") %>%
#   select(word, lemma, pos) %>%
#   str_view(wlp_1990$lemma, "work")
```

## Aggregated files

In this section, more wlp (word-lemma-POS) files will be added to the
current file in order to join the datasets and analyze the entire Spoken
data. The current file (1990) is one of about 22, one for each year
between 1990 and 2012.

# Session info

``` r
sessionInfo()
```

    R version 4.4.1 (2024-06-14 ucrt)
    Platform: x86_64-w64-mingw32/x64
    Running under: Windows 11 x64 (build 22631)

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
     [1] lubridate_1.9.3 forcats_1.0.0   stringr_1.5.1   dplyr_1.1.4    
     [5] purrr_1.0.2     readr_2.1.5     tidyr_1.3.1     tibble_3.2.1   
     [9] ggplot2_3.5.1   tidyverse_2.0.0

    loaded via a namespace (and not attached):
     [1] bit_4.0.5         gtable_0.3.5      crayon_1.5.3      compiler_4.4.1   
     [5] tidyselect_1.2.1  parallel_4.4.1    scales_1.3.0      yaml_2.3.10      
     [9] fastmap_1.2.0     R6_2.5.1          generics_0.1.3    knitr_1.48       
    [13] munsell_0.5.1     pillar_1.9.0      tzdb_0.4.0        rlang_1.1.4      
    [17] utf8_1.2.4        stringi_1.8.4     xfun_0.47         bit64_4.0.5      
    [21] timechange_0.3.0  cli_3.6.3         withr_3.0.1       magrittr_2.0.3   
    [25] digest_0.6.37     grid_4.4.1        vroom_1.6.5       rstudioapi_0.16.0
    [29] hms_1.1.3         lifecycle_1.0.4   vctrs_0.6.5       evaluate_0.24.0  
    [33] glue_1.7.0        fansi_1.0.6       colorspace_2.1-1  rmarkdown_2.28   
    [37] tools_4.4.1       pkgconfig_2.0.3   htmltools_0.5.8.1

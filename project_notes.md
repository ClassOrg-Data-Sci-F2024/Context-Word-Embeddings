# Notes on Project Progress and Plan

## Plan for data use and sharing to comply with COCA restrictions:

I am restricted in how much of the raw COCA data I can share due to the license restrictions. Even though the corpus can be used online for free, the full data must be accessed with a license. My project will demonstrate the steps of the analysis for future replication by anyone who has access to the full data from the COCA, and will present a subset of the final analysis. To that end, I will have two final Rmd files: One with my analysis using the full text (or rather, the portion of the full text that I select from the COCA), with relative paths accessing the data so that the pipeline could not be cloned and replicated with the same paths by those who do not have the full data; and one with my analysis based on a created sample of the data (< 50,000 words) that will be made available in the repo, so that anyone can clone and replicate the pipeline with that portion of data. The first Rmd will have sample of the final analysis, but I cannot share, for example, and full list of words related to the target words, because this would violate the restrictions.  

### References 

https://www.corpusdata.org 

Davies, Mark. (2008-) The Corpus of Contemporary American English (COCA). Available online at https://www.english-corpora.org/coca/

## Data Endgame (modified from 1st PR)

The data will contain a column with each row containing a single word, another column with the lemma, and another column with the POS. It will contain the words in all 22 files from the Newspaper word-lemma-pos genre, which will require joining data frames. Functions will be used to analyze either words that locally co-occur with the target workplace terms or words that globally co-occur with the target words.    

## General game plan

Goal: Identify and analyze words used in similar contexts as or used in proximity to words frequently used in workplace English 

One tricky thing in my project is that words that co-occur with the target words may not be in workplace contexts. "Meet" may be a word used in the workplace, but "meet" and "friend" may be related in non-workplace contexts. So it may be necessary to find the words in context, too. So first, use GloVe to group words by theme. However, a language learner may be interested in words that co-occur with workplace terms regardless of whether they only co-occur in the workplace. 

- Create functions for making word co-occurrence matrices using either GloVe or Word2Vec  
- Identify a small test list of seed words related to the workplace  
- Run that function and seed words through a trial dataset, such as `wlp_news_2012`  
- Examine the output to assess its effectiveness  
- Tweak the function as needed to get the target the output  
- Combine multiple datasets through relational data methods    
- Refine the seed word list by finding a scholarly source or adapting the online source lists; **consider restricting list to verbs frequently used in job descriptions or workplace contexts** 
- Run the embedding function through the larger combined dataset  
- Interpret the results (words co-occurring locally or globally with the target words) by examining patterns with visualizations and statistics: morphological tendencies of related words, POS proportions of related words compared to POS of input words   
- Restrict the published output to abide by the COCA license requirements, for example, by providing excerpts of the function outputs and clear steps for how to reproduce it with full data access


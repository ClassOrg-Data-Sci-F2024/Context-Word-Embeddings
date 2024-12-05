
# 11/14 class notes

todo list

- look for shared words between target word cosine lists  
- analyze k-bands of the words, using the 100k list or the lextutor  

- Read the full data (all 22 files) in with read_lines and unlist    
- Make into a tibble  
- Mutate with document number  
- Read this into the TCM function somehow  
- Analyze the output  

- Create a sample dataset that matches the format and reading of the full (or modify the existing one)
- Run the above analysis with the sample   

sample data:
- use stringr to find out where wlp and text files overlap - how to match them in content if both are needed for the sample data  
- Count the words in the 2012 text file, figure out how many rows would keep the data under 50,000 words (and be safe by aiming for 30,000)  
- Count the words in the 2012 wlp file, and figure out how to match it with the text file  
- copy my cleaned up work into the sample file and change the paths to read the sample data. 

word list:
- use canonical list of words rather than frequency list for workplace terms, use that as the seed words  
- make a target list with words like "job" so that we see what kinds of words co-occur with those core words. In other words, not "most frequent workplace verbs" but rather "canonical" workplace words that will help us use the words and comprehend them in more contexts  
- if time permits, could also do this with the 625 word list from Fluent Forever (careful of license)  
- Refine the target word list and run the same trial data  

embeddings:
- use text2vec rather than try to do it from scratch?  
- figure out how to make a matrix with my document column, or maybe just make a table with the document number as columns and the words as rows? I don't know.  
- make a new column with the document numbers (got a vector from str_subset)
- Learn how to use and apply the text2vec package to my data  
- Apply the text2vec functions to my 2012 data files using a few target words  
- Analyze the demos from Selivanov and Levshina to determine which version of the method I want for my outcome
- Decide if I should use wlp or full text files ultimately  
- Document term matrix instructions from [text2vec CRAN page pdf](https://cran.r-project.org/web/packages/text2vec/text2vec.pdf) p 12  
- Cite the text2vec package  

full analysis:  
- Combine whatever files I choose, all 22  
- Run the `tcm` or `dtm` function over the combined dataset with a sample of words  
- Do it with the full target word list  
- Analyze the results!  
- Create a 2D map of the results? How else can I get the words with the highest cosine similarity to the target words displayed at the same time?  
- Sort the outcome words by K-Band  
- Try analysis[here](https://cran.r-project.org/web/packages/word2vec/word2vec.pdf) with `word2vec` as well (dense CBOW and skip-gram)


done  

- Share data with instructor through email  
- Gorick list, use str_subset("", negate=TRUE) to get rid of the string rather than just the content of the string leaving it empty  
- 

# to do list

- find the size of the data

Virtual Corpus - make a subcorpus from COCA and search within that. This could be useful for selecting two genres, for example, and going from there.

#### Downloading COCA

**Accessing the corpus**  

- Purchase? Nope - Pitt has a license. Find out how to log in through Pitt. [Instructions here](https://www.english-corpora.org/academic_license.asp?s=userJoinLicense)   
- Ah, remembering I have an account, probably already selected Pitt.  
- After some troubleshooting, I got back into my account. Should be able to access data now.  
- Uh oh. The IP Address does not recognize Pitt's account. Maybe it's because I need to wait for approval for my profile.  
- If this doesn't work, I may need to choose a different corpus to analyze. Na'Rae's course site has free-access corpora listed, such as the Santa Barbara Corpus of Spoken English

**What data do I want to download?**

#### Other corpus options

SBC

I was able to read in a file with `read_tsv()` from this corpus. It's transcript of spoken data. Could be interesting to work with. 

#### Examine the COCA

#### Brainstorm - COCA vs the Office

I think the choices are different primarily in data processing. If I chose COCA, I would learn a lot about how to tidy a dataset, use corpus data, etc. I would then less of a burden on the analysis side of things. I would continue digging into the data, figure out which genre I want, which type of data in that genre, how to tidy and extract what I want from it, and then perform my concordance function on it. This would yield interesting data from "unscripted conversation from more than 150 different TV and radio programs". It would be a lot more data, too, so I would be learning how to figure that out. The license is more restrictive, but would not be impossible to do the project. Just present excerpts of the outcomes. The context of the content in this corpus would not necessarily be work-related, but may touch on various unpredictable topics in American and world life. 

If I chose the Office, I would find interesting results from a scripted fictional show in a particular context: an office workplace. However, topics might go beyond that, and are comedic in genre. Thus, the manipulation of the content will be geared towards comedy. This might change the results I'm looking for: **How are workplace terms used in context, and what other words frequently co-occur with them?** Now, in either case, the workplace terms I search might not be used in a workplace context; for example "work" or "meet" or even "job" could be used in ways that aren't about workplace.

## Continuation

What if I make my own list by examining the COCA sources and deciding one that is workplace related and getting a frequency list from it? Use that as the seed list.

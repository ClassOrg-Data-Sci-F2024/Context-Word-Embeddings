# Progress Report for Context Word Embeddings
**by Ben**

### 7 Oct 2024

**Steps completed:**  

- Created Git repository with short descriptive name and description  
- Created New Project (File > Version Control) titled same as my remote repo and added the URL   
- Added `.gitignore`. Included `private/` directory and content of `Windows.gitignore` template  
- Added `README.md` with brief summary  
- Added `LICENSE.md` with no content  
- Copied `project_ideas`, `project_ideas_personal`, and `project_feedback` from Class-Exercise-Repo into `Context-Word-Embeddings/private/`  
- Added `project_plan.md` and added content from `project_ideas` and polished the file overall for submission by Oct 7 deadline  
- Added `progress_report.md` with this entry

**Next steps:**  

- Find word list(s) for occupational (or spoken) English to collect seed words for the analysis   
- Figure out how to download COCA data    
- Specify what the data should look like for the analysis   
- Identify the R code and packages I will need  


### 1st Progress Report (28 Oct 2024)

**Steps completed:**

- Obtained data through University of Pittsburgh license. Completed and signed data user agreement form. Downloaded data onto my computer: BYU Corpus COCA (Full-Text), BYU COCA 100,000 Word List, and ICLE v2  
- Examined both COCA and The Office (`schrute`) data, and decided to use COCA. I will learn more about reading data, combining files and data frames, tidying datasets, etc. The Office subtitle data was already tidy and in an R package. Given my fairly broad goal of identifying how workplace terms are used in context and what other words frequently co-occur with them, I think it will be best to weight the project more towards COCA data wrangling.  
- Read license agreements and restrictions for COCA. BYU is pretty strict in what can be shared. I will continue referencing the license to make sure I adhere to it.  
- Identified which data of the COCA I will use: the Spoken data, which consists of "unscripted conversation from more than 150 different TV and radio programs" [english-corpora.org](https://www.english-corpora.org/coca/help/coca2020_overview.pdf). See also [this page](https://www.english-corpora.org/coca-spoken.asp). These programs, being unscripted, represent what learners may hear in everyday conversation (though certainly not completely authentic; see the second link).
- Read a subset of the data into R (file `wlp_spok_1990.txt`) which is tab-delimited and contains word-lemma-pos data from 1990. This is one of 22 files of that kind, one per year.   
- Examined the data, including renaming columns and getting some summary information.  
- Created a data endgame: The data will contain a column with each row containing a single word, another column with the lemma, and another column with the POS. It will contain the words in all 22 files from the spoken word-lemma-pos genre (unscripted TV and radio programs), which will require joining data frames. Functions will be used to analyze nearby words (i.e., in preceding and following rows from a target word's row) and/or related words.    
- An alternative to the endgame above is to use the text files from the same spoken data. I encountered an error in reading these files, so I will reformulate the plan depending on the content of those files. If they contain strings of text in vectors rather than one word per row, this may help in identifying nearby words using certain functions.     
- Created a `data/` folder and `.gitignore`-d it so that I can easily access the data without sharing it publicly.

**Next steps:**

- Make a plan for how my repo will adhere to the license requirements of the COCA  
- Locate a target word list with occupational/workplace words  
- Create a temporary word list for trialing and moving the project forward, before a full list is pinned down  
- Explore GloVe and decide whether to use that or train word embeddings with my own data  
- Locate functions to use for word embeddings and creating concordances

### 2nd Progress Report (13 Nov 2024)

**Steps completed:**

- Created Sharing Scheme and License Decision (see below)  
- Created _new replacement_ Rmd script (`../coca_trial.Rmd` and `../coca_trial.nb.Rmd`) using a different dataset from the COCA to run trial analyses, diverging from the previous Rmd. Eventually this Rmd and NB will be replaced with a `coca_analysis.Rmd` and `.md` that will contain the full pipeline  
- Changed name of `data/` folder (my private data folder in `.gitignore`) to `data_private/` and created a new `data/` folder that I can use to share. Updated the `.gitignore` to ignore `data_private/`   
- Changed COCA genre selection from Spoken to Newspaper for the trial runs   
- Created new Rmd Notebook to test Newspaper 2012 text file `coca_trial.nb.Rmd`  
- Format, shape, and size of data: 22 Newspaper non-tokenized text or tokenized word-lemma-POS files (will choose)  
- Located a potential target word list with occupational/workplace jargon (not ideal, but workable) from [Gorick Ng](https://www.gorick.com/blog/workplace-jargon-dictionary) which I extracted via webscraping    
- Located sources to help me decide which approach to use in creating word embeddings: [Medium](https://medium.com/biased-algorithms/word2vec-vs-glove-which-word-embedding-model-is-right-for-you-4dfc161c3f0c),  
- Explored GloVe via text2vec by Selivanov - really good option!  
- Found citation information for the data and corpus [see notes](../Context-Word-Embeddings/project_notes.md)
- Made a [plan](../Context-Word-Embeddings/project_notes.md) for how my repo will adhere to the license requirements of the COCA  
- Created updated [game plan](`../Context-Word-Embeddings/project_notes.md`) with goal and steps

**Next steps:**

- Create a temporary word list for trialing and moving the project forward, before a full list is pinned down  
- Decide whether to use GloVe or train word embeddings with my own data  
- Research GloVe and Word2Vec to decide which approach is appropriate for my project  
- Locate functions to use for word embeddings and creating concordances  
- Create or locate a stop-word list that works for my goals  

#### License Decision & Rationale

I chose the MIT License because I want people to be able to use the code I create to replicate my analysis or do other work with it. What I share and create (in the limited sample size required by the full text COCA license) will be available for the public to use and analyze.

#### Data Sharing Scheme

I am restricted in how much of the raw COCA data I can share due to the [license restrictions](https://www.corpusdata.org/restrictions.asp). Even though the corpus can be used online for free, the full data must be accessed with a license. My project will demonstrate the steps of the analysis for future replication by anyone who has access to the full data from the COCA, and will present a subset of the final analysis. To that end, I will have two final Rmd files: One with my analysis using the full text (or rather, the portion of the full text that I select from the COCA), with relative paths accessing the data so that the pipeline could not be cloned and replicated with the same paths by those who do not have the full data; and one with my analysis based on a created sample of the data (< 50,000 words) that will be made available in the repo, so that anyone can clone and replicate the pipeline with that portion of data. The first Rmd will have sample of the final analysis, but I cannot share, for example, and full list of words related to the target words, because this would violate the restrictions.  




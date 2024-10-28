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





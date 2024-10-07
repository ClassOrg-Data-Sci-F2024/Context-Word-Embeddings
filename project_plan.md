# Project Plan for Context Word Embeddings  
**by Ben**

> Goal 

- My goal is to provide a means of finding similar and related words for a list of target words for a specific context (i.e., occupational or spoken English) for English learners by using embeddings to analyze the COCA corpus.
- This goal aims to meet the needs of adult English learners English-predominant countries (esp. the United States) by helping learners and teachers select target words to make their time spent learning more efficient and meaningful

I will get there by using embeddings in vector semantics. I will locate a list of seed words to use, starting with occupational English words (i.e., broadly related to the workplace). Then I’ll do various analyses (or choose one depending on difficulty) with the COCA to find words-in-context, closely related, or associated words for each target word. The product will be a dataset with the target words and their related words based on the COCA. A visualization of related words in a two-dimensional (t-SNE) projection will be helpful.

> Why I'm interested in the topic

I would like to curate targeted and related word lists for L2 learners of English because as a teacher and prospective administrator, I want to gain practical tools and resources to target students’ needs and goals with appropriate vocabulary instruction. Further, to support communicative learning goals, I want to ensure time spent learning and studying is efficient. English has too many words to memorize, so aside from learning strategies for deciphering word meanings, learners need to spend vocabulary learning time on only the most relevant words. 

> Data 

The data would primarily consist of the COCA and/or BNC (we'll start with the COCA).

Another crucial piece of data will be seed words for occupational (or spoken) English which can be analyzed as described above. This list must be backed by research. 

The data could be sorted by variables such as **lexical item** (with example value "house"), **genre** (example value "occupational"), different features (example  "syllables" > "monosyllabic"), and possibly co-occurring words and/or collocates of the word. 

My vision for the final data frame is roughly as follows:  
- Columns: Target word > Nearby context words 3+- > Semantically similar words > Associated words > Morphemes of the target word > Phons of the target word

> Action Items  

- Collect frequency lists for English occupational contexts  
- Research and find a list of seed words to use, starting with occupational English words  
- Find the right way to conduct vector semantics and embeddings in R  
- Understand the concepts as needed to choose the right method  
- Do various analyses (or choose one depending on difficulty) with the BNC-COCA (for more dialectical diversity) to find words-in-context, closely related, or associated words for each target word  
- Curate a dataset with the target words and their related words based on the BNC-COCA  
- Analyze morphemes of the seed words and/or related words to identify more frequent derivational affixes   
- Analyze other features i.e., number of syllables, consonant clusters, number of possible meanings – homophones, morphological complexity, etc.  
- Produce visualization of related words in a two-dimensional (t-SNE) projection   
- (If needed) Use concordances or genres in COCA to find words that are most often used in these contexts.  
- (If needed) Choose frequent words from 1000 K bands.  
- Use str_view() and other such functions to find words used in conjunction with certain keywords from occupational settings. Search for all instances of safety, safe, or safely in the corpus, then search the concordances and make a data frame with them, then search the data frame for all instances of verbs within a certain distance from those "safe" variants. 

> Contingency Plan for Potential Unknowns & Issues

- Analyzing embeddings via vector semantics to create word lists is a broad task: How will I figure out the best method? How will I find the best R packages for that method?  
- Downloading COCA data - could be complicated!  
- Tidying the data: For example, if I collect 3 words to the right and left of the target word, will I need a column for each word, or will I need to lengthen the data and have more than one row for each target word?  

> Citations  

- Jurafsky & Martin. 2024. Vector Semantics & Embeddings. In _Speech and Language Processing_ (Chapter 6).  
- Handford. 2017. Corpus Linguistics. In _The Routledge Handbook of Language in the Workplace_.  
- Natalie Levshina. _How to Do Linguistics with R_. 
- [English-Corpora.org COCA](https://www.english-corpora.org/coca)  
- [COCA Help Files](https://www.english-corpora.org/help/)
- [COCA: Five minute tour: pdf with images](https://www.english-corpora.org/coca)  
- [Lextutor](https://www.lextutor.ca/vp/comp/) as a tool  
- (May not be relevant anymore) [Fluent Forever 625 word list](https://blog.fluent-forever.com/appendix5/) and others like it  

> Misc

COCA currently has the following genres. Academic (for business English) or Spoken (for communicative needs of adult speakers) genres may be useful in the analysis.

- Spoken (unscripted tv programs and radio)  
- Fiction  
- Magazines  
- Newspapers  
- Academic, incl business  
- Web (Genl)  
- Web (Blog)  
- TV/Movies

Defining terms: 

- context (1): a target setting for which English is learned and taught, that is, learning English for occupational contexts, i.e., the workplace.  
- context (2): the space around a word in a text  
- occupational: broadly, relating to workplace settings    
- survival: relating to contexts that are pivotal or crucial for maintaining health and wellbeing, in which language learners are living in a L2-predominant setting



# Automatic Domain Adaptation of Sentiment Dictionary

- How to predict financial outcomes from text?
- Loughran&McDonald (L&M): Finance dictionary created by **manual inspection of 10,000s of words**
- We show: **a completely automatic method works better!**
- Main reason: Manual labeling of words error-prone due to **experts’ a priori belief about a word’s meaning**
- Example: **God** labeled non-negative by L&M, but it’s negative in finance documents due to main context **Act of God**
- Main lesson: **Always label words in context**

### Approach:
- Download sentiment dictionaries **Harvard Psychosociological Dictionary**, Harvard-IV-4 TagNeg (H4N), and  Loughran and McDonald (**L&M**) from here: https://sraf.nd.edu/textual-analysis/resources/#Master%20Dictionary
- Load **domain specific** word embeddings of interest, e.g. learned from financial 10-K corpus. Words of original dictionary will be represented by these word embeddings
- Train SVM classifier in a 5-fold cross validation setup:
    - using each trained classifier on the 5th fold to predict 20% of the dictionary
    - using dictionary labels, e.g., H4N Dictionary: negative vs. non-negative;  L&M Dictionary: negative vs. non-negative
- Use predicted labels as new labels and save the reclassified dictionary in .csv file 


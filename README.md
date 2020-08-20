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


### Reclassified results 
Let's reclassify negative word list of L&M Dictionary: 
- SVM classifier gets: **negative** L&M wordlist and non-negative L&M wordlist (litigious, uncertainty and positive words)
- We obtain new labels: predicted labels (**negative** vs. non-negative) via 5-fold cross validation
- Results:
```

Reclassified Word   | New Class       Initial L&M Class

uncontracted        | Negative        Litigious      
uncertainly         | Negative        Uncertainty    
possibly            | Negative        Uncertainty    
probation           | Negative        Litigious      
undetectable        | Negative        Uncertainty    
warrantor           | Negative        Litigious      
conceivably         | Negative        Uncertainty    
rescissions         | Negative        Litigious      
resolve             | Negative        Positive       
speculation         | Negative        Uncertainty    
rescinded           | Negative        Litigious      
risking             | Negative        Uncertainty    
recoupments         | Negative        Litigious      
unconstitutional    | Negative        Litigious      
regaining           | Negative        Positive       
unusually           | Negative        Uncertainty    
rebutting           | Negative        Litigious      
contractile         | Negative        Litigious      
unseasonable        | Negative        Uncertainty    
suddenly            | Negative        Uncertainty    
cautiousness        | Negative        Uncertainty    
mediators           | Negative        Litigious      
unusual             | Negative        Uncertainty    
precautionary       | Negative        Uncertainty    
unproved            | Negative        Uncertainty    
sporadically        | Negative        Uncertainty    
suggesting          | Negative        Uncertainty    
excised             | Negative        Litigious      
rectification       | Negative        Litigious      
unenforceability    | Negative        Litigious      
upturns             | Negative        Positive       
permittee           | Negative        Litigious      
constructively      | Negative        Positive       
sudden              | Negative        Uncertainty    
apparently          | Negative        Uncertainty    
recalculations      | Negative        Uncertainty    
defalcations        | Negative        Litigious      
riskiness           | Negative        Uncertainty    
mediated            | Negative        Litigious      
remedied            | Negative        Litigious      
instabilities       | Negative        Uncertainty    
hidden              | Negative        Uncertainty    
solves              | Negative        Positive       
gain                | Negative        Positive       
defalcation         | Negative        Litigious      
rumors              | Negative        Uncertainty      
```

Here we can see that some positive L&M words become negative. Let's discover the meaning of these words:
```
emb_model_fin.most_similar('rebound', topn=9) #rebound is a positive word in L&M dictionary
[('rebounding', 0.5815426707267761),
 ('rebounded', 0.5602948665618896),
 ('soften', 0.5499194860458374),
 ('softening', 0.5360822677612305),
 ('slowdown', 0.533230185508728),
 ('sluggish', 0.5260505080223083),
 ('weak', 0.5184440612792969),
 ('slump', 0.5068018436431885),
 ('downturn', 0.5012232065200806)]
```
The word **rebound** has a negative meaning in financial documents, as well as the word **God**:
```
emb_model_fin.most_similar('god', topn=9)
[('terrorism', 0.5613816976547241),
 ('terror', 0.5098640322685242),
 ('war', 0.4653107523918152),
 ('violence', 0.4387531876564026),
 ('inadvertence', 0.4212065637111664),
 ('aggression', 0.40364134311676025),
 ('vandalism', 0.40329599380493164),
 ('eligibles', 0.39197811484336853),
 ('mandatorily', 0.37775948643684387)]
```
Originally, L&M label the word **rebound** as positive word and the word **God** as common word. 

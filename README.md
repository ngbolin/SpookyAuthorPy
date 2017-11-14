# Spooky Author Identification
_____

![cat eyes](http://www.sciencealert.com/images/articles/processed/catseyes_1024.jpg)

## Introduction
___
In this year's Kaggle Halloween playground competition, we are being challenged to predict the author of excerpts from horror stories by Edgar Allan Poe, Mary Shelley, and HP Lovecraft.

## Approach
___
Using libraries such as [string](https://docs.python.org/2/library/string.html), [re](https://docs.python.org/2/library/re.html) and [nltk](http://www.nltk.org), we begin by cleaning the corpus - removing punctuation marks and numbers, converting all letters to lowercase and removing stopwords. 

With the help of visualisation libraries such as [matplotlib](https://matplotlib.org), [seaborn](http://seaborn.pydata.org) and [wordcloud](https://github.com/amueller/word_cloud), we are able to identify the most frequent terms used by the authors in their writing.

We will also use [nltk's inbuilt Part-of-Speech Tagging function](http://www.nltk.org/book/ch05.html) to classify the words into their parts of speech. The reason for doing so is because we might expect authors to use specific tags relatively more than their counterparts. If this is the case, then the POS tags will be a useful tool to identify which author wrote the specific sentence.

We use [sklearn's](http://scikit-learn.org/stable/modules/generated/sklearn.feature_extraction.text.CountVectorizer.html) `CountVectorizer`  to convert the cleaned corpus into a matrix, with each row being a particular document, and each column a particular term. The term counts (how frequent a term appears in the corpus) for a particular document will be the corresponding in the value in the dataframe. Next, we use [sklearn's](http://scikit-learn.org/dev/modules/generated/sklearn.feature_extraction.text.TfidfVectorizer.html) `TfidfVectorizer` to account for the importance of a particular word term - intutively, words that occur frequently in a document (think 'Messi' in a soccer article) but do not frequently occur in the corpus itself tends to be an important term. On the other hand, words that frequently occur in all documents (think 'and', 'if', 'the') are deemed to have relative low importance.

Following the generation of the term frequency dataframe, we conducting Topic Modelling using [gensim](https://radimrehurek.com/gensim/index.html). By identifying the underlying topics within the corpus, we were able to identify the 'hidden' topics within the corpus. Following the identification of such topics, we then allocate topics to each document depending on the terms present in the document.

Finally, we pulled all our features together, and used a simple [Logistic Regression model](https://en.wikipedia.org/wiki/Logistic_regression) using [sklearn's GridSearchCV](http://scikit-learn.org/stable/modules/generated/sklearn.model_selection.GridSearchCV.html) to search for the best model by varying the regularisation term, C. On the holdout testing dataset, the preliminary model were able to obtain a testing score of 0.445 (as we are using the metric of logarithmic loss, lower is better). After which, we re-fitted the model, using the best parameters we found (C=10), using the whole training dataset.

## Evaluation Metric
___
Similar to Kaggle's evaluation metric, we will use the [multi-class logarithmic loss](https://www.kaggle.com/c/spooky-author-identification#evaluation).


## Afternote
___
Our model scored 0.42827. The score was good enough to place us at 214 of 584 teams (37th percentile). As it turns out, a simple logistic regression model with good features will outperform a sophisticated machine learning algorithm with poor features. 

In the event that you found this useful, please visit the other kernels that were instrumental in helping me formulate hypotheses, generate new features and challenging some of my beliefs:

1. [Abhishek - Approaching (Almost) Any NLP Problem on Kaggle](https://www.kaggle.com/abhishek/approaching-almost-any-nlp-problem-on-kaggle)
2. [Anisotropic - Spooky NLP and Topic Modelling tutorial](https://www.kaggle.com/arthurtok/spooky-nlp-and-topic-modelling-tutorial)
3. [Heads or Tails - Treemap House of Horror: Spooky EDA/LDA/Features](https://www.kaggle.com/headsortails/treemap-house-of-horror-spooky-eda-lda-features)

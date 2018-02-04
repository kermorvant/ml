# Document classification

Automatic text classification is one of the standard Machine Learning applications. The first service based on Machine Learing used by almost every internet users concern text classification : spam filtering.

## A simple example on a toy problem

You want to build a email classification system for your company using a Naive Bayes classifier.
Emails must be classified into 2 different classes : *membership* or *complaint*.

 The classification will be based on the presence or absence of 8 words : 
 `membership, new, IBAN, address, dissatisfied, unacceptable, sales, offers `
 
The training sample is composed of following documents, presented using a bag-of-word representation : 


join | new | IBAN | address | dissatisfied | unacceptable | sales | offers | class 
---|--------|-------|--------|--------|--------|--------|--------|--------
1|0|1|1|1|0|1|1| membership
0|0|1|0|0|0|0|0| membership
1|1|0|1|0|0|0|0| membership
1|1|0|1|0|0|0|1| membership
1|1|0|1|1|0|0|0| membership
0|0|0|1|0|1|0|0| membership
1|1|1|1|1|0|1|0| membership
0|0|0|1|0|1|1|0| complaint
1|0|1|1|1|0|1|0| complaint
0|1|0|0|1|1|0|1| complaint
0|0|0|1|1|1|1|0| complaint
0|0|0|1|1|1|0|0| complaint
1|1|0|0|0|0|0|0| complaint



We denote by  $Y$ the document class and  $X=X_1,\cdots,X_8$ the bag of word features.


Compute the following probabilities :

* P(Y=membership)
* P(Y=complaint)
* $$\forall i \quad P(X_i =1 | Y=membership)$$
* $$\forall i \quad P(X_i =0 | Y=membership)$$
* $$\forall i \quad P(X_i =0 | Y=membership)$$
* $$\forall i \quad P(X_i =1 | Y=complaint)$$
* $$\forall i \quad P(X_i =0 | Y=complaint)$$

Based on these probabilities, predict the class of the following document using a Naives Bayes classifier : 


>Monsieur,
> je vous adresse ce courrier car je suis très mécontent de mon abonnement téléphonique. Je n'arrive jamais à téléphoner ! C'est inadmissible !J'attends un geste commercial de votre part."




## LeMonde2003 Dataset

In this example, we will apply classification algorithms to newspaper articles published in 2003 in *Le Monde*. The dataset can be dowloaded in tab separated CSV format [here](http://data.teklia.com/Texts/LeMonde2003.csv.zip).

These articles concern different subjects but we will consider only articles related to the following subjects : entreprises (ENT), international (INT), arts (ART), société (SOC), France (FRA), sports (SPO), livres (LIV), télévision (TEL) et les articles publiés à la une (UNE).


**Question 1**:

> * Load the CSV file `LeMonde2003.csv` containing the articles using `pd.read_csv`. The field separator is '\t'. 
> * Drop the empty lines using  `dropna`.
> * Keep only the articles of the categories `categories = ['ENT','INT','ART','SOC','FRA','SPO','LIV','TEL','UNE']` using [`isin`](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.isin.html)
> * Plot the frequency histogram of the categories using [`countplot`](https://seaborn.pydata.org/tutorial/categorical.html)

## Bag-of-word representation

In order to apply machine learning algorithms to text, documents must be transformed into vectors. The most simple and standard way to transform a document into a vector is the *bag-of-word* encoding.

The idea is very simple : 

1. define the set of all the possible words that can appear in a document; denote its size by `max_features`.
2. for each document,  encode it with a vector of size `max_features`, with the value of the ith component of the vector equal to the number of time the ith word appears in the document.

See [the wikipedia article on Bag-of-word](https://en.wikipedia.org/wiki/Bag-of-words_model) for an example.

Scikit-learn proposes different methods to encode text into vectors : [CountVectorizer](http://scikit-learn.org/stable/modules/generated/sklearn.feature_extraction.text.CountVectorizer.html) and [TfidfTransformer](http://scikit-learn.org/stable/modules/generated/sklearn.feature_extraction.text.TfidfTransformer.html).

The encoder must first be trained on the train set and applied to the different sets, for example with the 1000  words : 

	vectorizer = CountVectorizer(max_features=1000)
    vectorizer.fit(X_train)
    X_train_counts = vectorizer.transform(X_train)
    X_test_counts = vectorizer.transform(X_test)
        
**Question 2**:

> * Split the dataset LeMonde2003 into train/dev/test set. 
> * For each set, transform the text of the articles into vectors using the `CountVectorizer` with `max_features=1000` words.


## Mutlinomial Naive Bayes classifier

The *Multinomial Naive Bayes* classifier is a statistical classifier that can be used for document classification. This classifier estimates for each class, the probability that a given word appears in a document from this class. For example, in a news article about sport, the word `player` is more likely to appear than the word `profit`. The probablities are estimated simply by counting for each class the number of times each words appears. For more details on the Multinomial Naive Bayes, see [the wikipedia article on Naive Bayes classifier](https://en.wikipedia.org/wiki/Naive_Bayes_classifier#Document_classification) or the [scikit-learn documentation](http://scikit-learn.org/stable/modules/naive_bayes.html)

**Question 3**:

> * Train a multinomial Naive Bayes classifier on the dataset LeMonde. Find the best values for the parameters  `max_features` and `alpha`. Report the train/valid/test error rates.

## TF-IDF representation

The `CountVectorizer` encodes the text using the raw frequencies of the words. However, words that are very frequent and appear in all the documents will have a strong weight whereas they are not discriminative. The *Term-Frequency Inverse-Document-Frequency* weighting scheme take into accound the number of documents in which a given word occurs. A word that appear in many document will have less weight. See [the wikipedia page](https://en.wikipedia.org/wiki/Tf%E2%80%93idf) for more details.

With scikit-learn, the `TfidfTransformer` is applied after the `CountVectorizer` :

	tf_transformer = TfidfTransformer().fit(X_train_counts)
 	X_train_tf = tf_transformer.transform(X_train_counts)
	X_test_tf = tf_transformer.transform(X_test_counts)
	
**Question 4**:

> * Use the TF-IDF representation to train a Multinomial Naive Bayes classifier. Report your best test error rate and the error rates for all the configurations tested.

## Error analysis

The classification error rate give an evaluation of the performance for all the classes. But since the classes are not equally distributed, they may not be equally well modelized. In order to get a better idea of the performance of the classifier, detailed metrics must be used : 

* [metrics.classification_report](http://scikit-learn.org/stable/modules/generated/sklearn.metrics.classification_report.html) provides a detailed analysis per class : the precision (amongst all the example classified as class X, how many are really from the classX) and the recall (amongst all the example that are from the class X, how many are classified as class X) and the F-Score which is as a weighted harmonic mean of the precision and recall.
* [metrics.confusion_matrix](http://scikit-learn.org/stable/modules/generated/sklearn.metrics.confusion_matrix.html) which give the confusions between the classes.

**Question 5**:

> * Report the `classification_report` for your best classifier. Which classes have the best scores ? Why ?
> * Report the `confusion_matrix` for your best classifier. Which classes are the most confused ? Why ?



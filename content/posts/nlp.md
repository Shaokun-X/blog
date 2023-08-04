---
title: "Some notes about NLP"
date: 2021-11-02T10:46:20+02:00
description: "Some notes about NLP"
tags: ["NLP"]
draft: false
---

### stemming and lemmatization
- lemmatization: like stemming but gives human-understandable word

### bag-of-words
- purpose: ML accept only vectors of same length
- after stemming/lemmatization ignoring case etc, build vectors for each sentence, of which each variable corresponds to the score of a word in the vacabulary
- words hashing: use a hash function to map words to a hash space(vector)

### TF(Term Frequency)-IDF

- TF = repetition of a word in a document / number of words in a document
- IDF = log(number of all document / number of document containing the word)
- The final result is a table where for each word, its score is given by the product of the score in IF and the socre in IDF

![TF Example](https://sci2lab.github.io/ml_tutorial/images/tfidf_ex1.png)
![IDF Example](https://sci2lab.github.io/ml_tutorial/images/tfidf_ex2.png)
![Final Result Example](https://sci2lab.github.io/ml_tutorial/images/tfidf_ex3.png)

### Word2Vec

- word embedding represent a word by a vector of features
![](https://jalammar.github.io/images/word2vec/king-analogy-viz.png)
- uses a neural network model to learn word associations from a large corpus of text and produce word embeddings
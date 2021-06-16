# Final-project-SMS-Spam-Detection-
# importing the libraries 

import numpy as np # linear algebra

import pandas as pd # data processing, CSV file I/O (e.g. pd.read_csv)

import nltk

# download and reading the dataset

import pandas
df_sms = pd.read_csv('spam.csv',encoding='latin-1')
df_sms.head()

# Dropping the unwanted coloumns

df_sms = df_sms.drop(["Unnamed: 2", "Unnamed: 3", "Unnamed: 4"], axis=1)

df_sms = df_sms.rename(columns={"v1":"label", "v2":"sms"})

df_sms.head()

# Checking the maximum length of SMS 

print(len(df_sms))

# Number of observations in each label spam and ham

df_sms.label.value_counts()

df_sms.describe()

df_sms['length'] = df_sms['sms'].apply(len)

df_sms.head()


import matplotlib.pyplot as plt

import seaborn as sns

%matplotlib inline


df_sms['length'].plot(bins=50, kind='hist')

df_sms.hist(column='length', by='label', bins=50,figsize=(10,4))

df_sms.loc[:,'label'] = df_sms.label.map({'ham':0, 'spam':1})

print(df_sms.shape)

df_sms.head()

# Implementing the bag of words approach

# Step 1: Convert all strings to their lower case form.

documents = ['Hello, how are you!',

'Win money, win from home.',

'Call me now.',

'Hello, Call hello you tomorrow?']

lower_case_documents = []

lower_case_documents = [d.lower() for d in documents]

print(lower_case_documents)


# Step 2: Removing all punctuations 
sans_punctuation_documents = []

import string

for i in lower_case_documents:

sans_punctuation_documents.append(i.translate(str.maketrans("","", string.punctuation)))



sans_punctuation_documents

# Step 3: Tokenization 

preprocessed_documents = [[w for w in d.split()] for d in sans_punctuation_documents]

preprocessed_documents

# Step 4: Count frequencies

frequency_list = []

import pprint

from collections import Counter



frequency_list = [Counter(d) for d in preprocessed_documents]

pprint.pprint(frequency_list)

# Implementing the bag of words approach in scikit-learn 

from sklearn.feature_extraction.text import CountVectorizer

count_vector = CountVectorizer()

count_vector.fit(documents)

count_vector.get_feature_names()

doc_array = count_vector.transform(documents).toarray()

doc_array

frequency_matrix = pd.DataFrame(doc_array, columns = count_vector.get_feature_names())

frequency_matrix

from sklearn.model_selection import train_test_split



X_train, X_test, y_train, y_test = train_test_split(df_sms['sms'], 

df_sms['label'],test_size=0.20, 

random_state=1)

# Instantiate the CountVectorizer method

count_vector = CountVectorizer()



# Fit the training data and then return the matrix

training_data = count_vector.fit_transform(X_train)



# Transform testing data and return the matrix. 

testing_data = count_vector.transform(X_test)

# Implementation of the Naive Bayes Machine Learning Algorithm 

from sklearn.naive_bayes import MultinomialNB

naive_bayes = MultinomialNB()

naive_bayes.fit(training_data,y_train)

MultinomialNB(alpha=1.0, class_prior=None, fit_prior=True)

predictions = naive_bayes.predict(testing_data)

# evaluating the model- Accuracy, precision, recall(sensitivity) and F1 Score 

from sklearn.metrics import accuracy_score, precision_score, recall_score, f1_score

print('Accuracy score: {}'.format(accuracy_score(y_test, predictions)))

print('Precision score: {}'.format(precision_score(y_test, predictions)))

print('Recall score: {}'.format(recall_score(y_test, predictions)))

print('F1 score: {}'.format(f1_score(y_test, predictions)))

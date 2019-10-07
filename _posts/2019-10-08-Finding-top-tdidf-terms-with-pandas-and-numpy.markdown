---
layout: post
title:  "Web scraping 50 thousand documents from different sources for NLP"
date:   2019-10-08 00:51:12 +0200
---
This text is sort of offshoot from actual project I am currently working on. Recently I took upon myself exploring NLP landscape, in this case topic modelling using TD-IDF algorithm. Before tweaking further my terms extraction I wanted to be really sure that I am storing my data in most accessible way. So I started to play around. This lead me to few simple solutions, each one of them returns TD-IDF matrix in slightly different form, depending on how do I want to query our dataset later. Text will be divided by each script and short discussion.

You can find this project as jupyter notebook on kaggle: <https://www.kaggle.com/neotokio/td-idf-top-terms-for-multiple-documents-3-ways>

This is my first ever kaggle notebook! Enjoy, kind stranger.

**Things we will tackle:**

I will use sample of 300 articles downloaded using newspaper3k. These are financial articles. I also kept false positives which returned nothing (ie. newsletter signup page) to serve as sort of 'reference point'. All of the code handles up to 1000 documents easily.

Creating TD-IDF matrix
Storing TD-IDF matrix
Extracting top terms from TD-IDF matrix

Let's start by importing neccessary libraries.

{% highlight python %}
import pandas as pd
import numpy as np
from itertools import chain
from nltk.corpus import stopwords
from sklearn.feature_extraction.text import CountVectorizer, TfidfVectorizer
{% endhighlight %}


Pandas and numpy serve as a backbone for whole script. Itertools was imported to run few test with flattening numpy arrays. Stopwords are stopwords, obviously. TfidfVectorizer is our main culprit.

**SCRIPT I**

*Return top X terms, without their score for given document. Store them in a list.*

This is actually very popular piece of code. I found it many times while googling around. But let's start from the beginning.

    1.We load csv and do some lightweight processing, changing strings to 'lower' and removing stopwords. Next we call TfidVectorizer to perform 2.vectorization using parameters max/min for returned terms.
    3.We start iterating through dataframe using for loop. We return term matrix for each row.
    4. Single_tdidf is a variable from which we call our function to process TD-IDF matrix to more compact form.

Above three steps will be repeated for each script.

In case of **SCRIPT I**, we perform following operations in tdidf_top function:

    1.Function tdidf_top takes two arguments from our loop.
    2.Get terms (strings) from vectorized array.
    3.Get indicies from array and sort them from descending.
    4.Define variable with amount of top terms you want returned.
    5.Store X top terms in variable.
    6.Append terms to list.

At the end we transform them to DF column. Column will store list of X top terms from TD-IDF, without their frequency score.

{% highlight python %}
df = pd.read_csv('../input/sample.csv', usecols=['article', 'title'])

STOPWORDS = set(stopwords.words('english'))
df['article_tdidf'] = df.article.str.lower()
df['article_tdidf'] = df.article.apply(lambda x: ' '.join([item for item in x.split() if item not in STOPWORDS]))
tfidf_vectorizer = TfidfVectorizer(max_df=1.8, min_df=0.05)

'''Just list with top X tdidf words, no values'''
d = []

def tdidf_top(tfidf_vectorizer, tfidf_matrix):
    f_array = np.array(tfidf_vectorizer.get_feature_names())
    t_sort = np.argsort(tfidf_matrix.toarray()).flatten()[::-1]
    n = 5
    top_n = f_array[t_sort][:n]
    top_n = top_n.tolist()
    d.append(top_n)

for row in df.article_tdidf:
    tfidf_matrix = tfidf_vectorizer.fit_transform([row])
    single_tdidf = tdidf_top(tfidf_vectorizer, tfidf_matrix)

df['tdscore'] = d
print(df['tdscore'][0:10])
{% endhighlight %}

This prints:

{% highlight python %}
0             [cookies, newsletter, we, if, directly]
1                 [funds, prudential, sec, said, ast]
2    [investors, china, shares, participation, share]
{% endhighlight %}

**SCRIPT II**

*Adding frequency value and storing array as dictionary*

This approach gives as additional value of frequency. It gives me additional option to weight terms.

    1.In place of separating terms and indicies to two different variables, we process everything in one variable and store it in DataFrame, already nicely fitted.
    2.Transposing dataframe gives better layout with just 1 column and it's far easier to query.
    3.Define name for new column.
    4.In place of numpy I use pandas .nlargest to return X largest (by frequency value) objects from td-idf matrix.
    5.Transform it to dictionary and append to list.
{% highlight python %}
'''Terms + values stored in dictionary'''
n = []

def tdidf_top(tfidf_vectorizer, tfidf_matrix):
    tdidf_df = pd.DataFrame(tfidf_matrix.toarray(), columns=tfidf_vectorizer.get_feature_names())
    tdidf_df = tdidf_df.T
    tdidf_df.columns = ['value']
    tdidf_df = tdidf_df.nlargest(5, ['value'])
    tdidf_df_dict = tdidf_df.to_dict()
    n.append(tdidf_df_dict)

for row in df.article_tdidf:
    tfidf_matrix = tfidf_vectorizer.fit_transform([row])
    single_tdidf = tdidf_top(tfidf_vectorizer, tfidf_matrix)

df['tdscore'] = n
print(df['tdscore'][0:10])
{% endhighlight %}

This prints:
{% highlight python %}
0    {'value': {'cookies': 0.41702882811414954, 'ne...
1    {'value': {'funds': 0.35529247334746206, 'prud...
2    {'value': {'investors': 0.3342554190045202, 'c...
3    {'value': {'markets': 0.4250522307988794, 'fx'...
{% endhighlight %}

**SCRIPT III**

Most complex but very easy to query later. Here, I will also store TD-IDF matrix in dataframe, but now I will explode our list, storing each word with corresponding frequency in adjacent row. This allows to utilize all of pandas selection tools without writing additional code to access interesting values.

There is one caveat. We will need to change for loop to make it work.

I will present each part of a script in separate, as it involves more steps than just storing terms in list/dict using tdidf_top function.
{% highlight python %}
df = pd.read_csv('../input/sample.csv', usecols=['article', 'title'])

STOPWORDS = set(stopwords.words('english'))
df['article_tdidf'] = df.article.str.lower()
df['article_tdidf'] = df.article.apply(lambda x: ' '.join([item for item in x.split() if item not in STOPWORDS]))
tfidf_vectorizer = TfidfVectorizer(max_df=1.8, min_df=0.05)

def tdidf_top(v, vectors_tfidf):
    tdidf_df = pd.DataFrame(tfidf_matrix.toarray(), columns=v.get_feature_names())
    tdidf_df = tdidf_df.T
    tdidf_df.columns = ['value']
    tdidf_df = tdidf_df.nlargest(5, ['value'])
    tdidf_df_dict = tdidf_df.values.tolist() #We could use that as values column
    words = tdidf_df.index.tolist()
    return words, tdidf_df_dict

w = []
v = []

for row in df.article_tdidf:
    tfidf_matrix = tfidf_vectorizer.fit_transform([row])
    single_tdidf = tdidf_top(tfidf_vectorizer, tfidf_matrix)
    w.append(single_tdidf[0])
    v.append(single_tdidf[1])
    
df['word'] = w
df['score'] = v

df.set_index('title', inplace=True)
df_words = pd.melt(df.word.apply(pd.Series).reset_index(), id_vars='title', value_name='word').set_index('title').drop('variable', axis=1).dropna().sort_index()
df_words.reset_index(inplace=True)
v = [round(x, 5) for x in chain(*chain(*v))]
df_words['score'] = v
print(df_words[0:10])
{% endhighlight %}

This prints:
{% highlight python %}
                                               title        word    score
0  Apollo’s Josh Harris Talks Private Markets at ...      equity  0.41703
1  Apollo’s Josh Harris Talks Private Markets at ...        said  0.41703
2  Apollo’s Josh Harris Talks Private Markets at ...     markets  0.20851
{% endhighlight %}

Steps:

1. Adjust for loop. Create two lists.
2. Tdidf call stays exactly the same, but we need to return terms and array with frequency values and store it inside of a list. With this done, we can proceed to 3rd possibility of extracting top tdidf terms.
3. In place of appending to dictionary (SCRIPT II) we just return array of frequencies as a list and list of terms. Now we can build dataframe, where each row is one word with corresponding frequency. Remember that products od tdidf_top function are now stored in two list - w and v.

**Futher ahead**

4. Store terms and frequency in two separate columns of dataframe. Right now each columns contains a list of 5 top tdidf terms with their frequency.
5. Set title of dataframe to 'title', this will make dataframe more clear to operate later on without a need to store whole text of a document. Remember that dataframes can be easily merged ‘as needed’
6. Explode list to separate rows in dataframe.
7. Explode numpy array to separate values. This is convenient way of fitting dataframe row-by-row with terms. That way we avoid length issues between terms/frequency list.
8. Assign frequency values to new column

**

This solution gives us comfortable option to select rows using pandas condition. Like this:**
{% highlight python %}


print('Conditions with score')
s1 = df_words[(df_words.score > 0.15) & (df_words.score < 0.3)] # Select all rows with TD-IDF frequency higher than 0.15 and lower than 0.30
s2 = df_words[df_words.word.duplicated(keep=False)] #Show high in frequency words which are reappearing across all documents
{% endhighlight %}

That was fun. There is probably more ways to store and manipulate td-idf matrix, depending on your needs and how are you planning to query your data. For me those 3 approaches are more than enough.

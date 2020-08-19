## Introduction to SciKit Learn using kNN

### Introduction to classification

<iframe width="560" height="315" src="https://www.youtube.com/embed/B0i8yBkkM00" frameborder="0" allowfullscreen></iframe>

In this video we continue our exploration of writing queries involving a single table.

### Intro to SciKit Learn

<iframe width="560" height="315" src="https://www.youtube.com/embed/B8NJI1ACqQU" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

The dataset we are going to use is on how the U.S. Congress voted on different bills and we want to see if we can predict what party they belong to (democrat or republican) based on those votes.

FIrst, let's load in the data

```
import pandas as pd
from pandas import DataFrame, Series

```

In the data file there are no column names so we need to add them

```
column_names = ['party', 'handicapped_infants', 'water', 'budget',
                'physician_fee_freeze', 'el_salvador_aid',
                'religious_groups_in_schools', 'anti_satellite_test_ban',
                'aid_to_nicaraguan_contras', 'mx_missile', 'immigration',
                'synfuels_corporation_cutback', 'education_spending',
                'superfund_right_to_sue', 'crime', 'duty_free_exports',
                'south_africa_exports']
len(column_names)

```

    17

and now we can actually load in the data and pass in those column names.

```
votes = pd.read_csv('https://raw.githubusercontent.com/zacharski/ml-class/master/data/house_votes_2.csv', names= column_names )
```

Let's take a look at the data...

```
votes
```

You should see a sample of the dataset.

### How to load in a zip file

Suppose the data wasn't in a csv file, but was contained in a zip file. How do we load in the data?

First, let's get the zip file to our Google Colab machine by using the Linux command `wget`. We can execute an arbitrary Linux command by starting the code cell with a bang `!`

```
!wget https://raw.githubusercontent.com/zacharski/ml-class/master/data/house_votes.zip
```

    --2020-08-04 15:36:09--  https://raw.githubusercontent.com/zacharski/ml-class/master/data/house_votes.zip
    Resolving raw.githubusercontent.com (raw.githubusercontent.com)... 151.101.0.133, 151.101.64.133, 151.101.128.133, ...
    Connecting to raw.githubusercontent.com (raw.githubusercontent.com)|151.101.0.133|:443... connected.
    HTTP request sent, awaiting response... 200 OK
    Length: 1115 (1.1K) [application/zip]
    Saving to: ‘house_votes.zip’

    house_votes.zip     100%[===================>]   1.09K  --.-KB/s    in 0s

    2020-08-04 15:36:10 (29.4 MB/s) - ‘house_votes.zip’ saved [1115/1115]

Now let's unzip the file using the Linux command `unzip`

```
!unzip house_votes.zip
```

    Archive:  house_votes.zip
      inflating: house_votes_2.csv

And see what is in our current directory

```
!ls
```

    house_votes_2.csv  house_votes.zip  sample_data

Now we can load that local file into pandas.

```

votes2 = pd.read_csv('house_votes_2.csv', names= column_name)

```

That was a bit of an aside, but it is a useful thing to know.

### divide the dataset

Okay, we have `votes` the DataFrame with the house vote data. We separate that so 80% goes into a training set and 20% goes into the testing set.

```
from sklearn.model_selection import train_test_split
votes_train, votes_test = train_test_split(votes, test_size = 0.2)
votes_train
```

## Getting labels and features

Next we want to divide the labels -- what we want to predict, from the features -- what we are going to use to make the prediction

```
fColumns = list(votes.columns)
fColumns.remove('party')
votes_train_features = votes_train[fColumns]
votes_test_features = votes_test[fColumns]
votes_train_labels = votes_train[['party']]
votes_test_labels = votes_test[['party']]
votes_test_labels
```

# build a Euclidean kNN classifier with k=3

Finally, we are going to build our kNN classifier. We will use Euclidean distance and a k of 3.

```
from sklearn.neighbors import KNeighborsClassifier
knn = KNeighborsClassifier(n_neighbors=3)
knn.get_params()
```

    {'algorithm': 'auto',
     'leaf_size': 30,
     'metric': 'minkowski',
     'metric_params': None,
     'n_jobs': None,
     'n_neighbors': 3,
     'p': 2,
     'weights': 'uniform'}

# Train the classifier using fit

We will train the classifier on our training dataset

```
knn.fit(votes_train_features, votes_train_labels)
```

    /usr/local/lib/python3.6/dist-packages/ipykernel_launcher.py:1: DataConversionWarning: A column-vector y was passed when a 1d array was expected. Please change the shape of y to (n_samples, ), for example using ravel().
      """Entry point for launching an IPython kernel.





    KNeighborsClassifier(algorithm='auto', leaf_size=30, metric='minkowski',
                         metric_params=None, n_jobs=None, n_neighbors=3, p=2,
                         weights='uniform')

# now use predict to get the predictions

```
predictions = knn.predict(votes_test_features)
predictions
```

    array(['democrat', 'democrat', 'republican', 'republican', 'republican',
           'democrat', 'democrat', 'democrat', 'democrat', 'republican',
           'democrat', 'republican', 'democrat', 'republican', 'republican',
           'republican', 'republican', 'republican', 'republican',
           'republican', 'republican', 'democrat', 'republican', 'democrat',
           'democrat', 'republican', 'democrat', 'democrat', 'democrat',
           'democrat', 'republican', 'republican', 'republican', 'republican',
           'democrat', 'democrat', 'democrat', 'democrat', 'democrat',
           'republican', 'democrat', 'republican', 'republican', 'republican',
           'democrat', 'republican', 'republican'], dtype=object)

# Nice to know the accuracy

we can use accuracy_score

```
from sklearn.metrics import accuracy_score
accuracy_score(votes_test_labels, predictions)
```

    0.9787234042553191

## try to improve accuracy

### p or power -- let's make it Manhattan

```
knn = KNeighborsClassifier(n_neighbors=3, p = 1)
knn.fit(votes_train_features, votes_train_labels)
predictions = knn.predict(votes_test_features)
accuracy_score(votes_test_labels, predictions)
```

    0.9787234042553191

## how is our prediction just using one votes. How about immigration?

```
knn = KNeighborsClassifier(n_neighbors=3)
knn.fit(votes_train[['immigration']], votes_train_labels)
predictions = knn.predict(votes_test[['immigration']])
accuracy_score(votes_test_labels, predictions)
```

    0.48936170212765956

Our prediction on whether someone is a Republican or Democrat based on how they voted on an immigration bill was not very accurate.

### What about how they voted for aid to Nicaraguan Contras?

```

```

```
knn = KNeighborsClassifier(n_neighbors=3)
knn.fit(votes_train[['aid_to_bicaraguan_contras']], votes_train_labels)
predictions = knn.predict(votes_test[['aid_to_bicaraguan_contras']])
accuracy_score(votes_test_labels, predictions)
```

    0.851063829787234

That accuracy is pretty high.

## finally, k=5

```
knn = KNeighborsClassifier(n_neighbors=5, p = 2)
knn.fit(votes_train_features, votes_train_labels)
predictions = knn.predict(votes_test_features)
accuracy_score(votes_test_labels, predictions)
```

    0.9787234042553191

```

```

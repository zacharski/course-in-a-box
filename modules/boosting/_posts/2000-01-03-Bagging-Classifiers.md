# Bagging, Pasting, and Patches

<iframe width="560" height="315" src="https://www.youtube.com/embed/WOhhJ2-uQqY" frameborder="0" allowfullscreen></iframe>




The basic template for using nearly any bagging method is

```
from sklearn.ensemble import BaggingClassifier
clf = tree.DecisionTreeClassifier(criterion='entropy', max_depth=5)
ensemble = BaggingClassifier(clf, n_estimators=50, n_jobs-1)
```

Of course the values of those hyperparameters change, but that is the basic idea.  The hyperparameters that create the different flavors of bagging algorithms are ...


|    algorithm     | max_samples                 | bootstrap                  | max_features                 |
| :--------------: | :-------------------------- | :------------------------- | :--------------------------- |
|     bagging      | `max_samples=num`           | `bootstrap=True` (default) | `max_features=1.0` (default) |
|     pasting      | `max_samples=num`           | `bootstrap=False`          | `max_features=1.0` (default) |
| random subspaces | `max_samples=1.0` (default) | -                          | `max_features=num`           |
|  random patches  | `max_samples=num`           | -                          | `max_features=num`           |


Here are definitions of those column hyperparameters (this from the documentation)

#### max_samples int or float, default=1.0

The number of samples to draw from X to train each base estimator (with replacement by default, see bootstrap for more details).

* If int, then draw max_samples samples.

* If float, then draw max_samples * X.shape[0] samples.


#### max_features int or float, default=1.0

The number of features to draw from X to train each base estimator ( without replacement by default, see bootstrap_features for more details).

* If int, then draw max_features features.
* If float, then draw max_features * X.shape[1] features.


#### bootstrap bool, default=True

Whether samples are drawn with replacement. If False, sampling without replacement is performed.


#### bootstrap_features, bool, default=False

 Whether features are drawn with replacement.


## Clothing

The dataset we will use to explore bagging consists of small 28x28 grayscale image icons of different articles of clothing. There are 60,000 images in the training set and 10,000 in the test set

![](https://raw.githubusercontent.com/zacharski/ml-class/master/labs/pics/clothes-sprite.png)

![](https://raw.githubusercontent.com/zacharski/ml-class/master/labs/pics/clothing.gif)

Each image has an associated label from a list of 10:


| Label | Description |
| ----- | ----------- |
| 0     | T-shirt/top |
| 1     | Trouser     |
| 2     | Pullover    |
| 3     | Dress       |
| 4     | Coat        |
| 5     | Sandal      |
| 6     | Shirt       |
| 7     | Sneaker     |
| 8     | Bag         |
| 9     | Ankle boot  |

If the alogrithm randomly guessed, it would only be 10% accurate. 


#### The features

28x28 = 784 features -- pixels - gray scale.

60,000 rows x 784 features

#### The files

* Training set: [clothes_train.csv](http://zacharski.org/files/courses/cs419/clothes_train.csv)
* Test set: [clothing_test.csv](http://zacharski.org/files/courses/cs419/clothing_test.csv) Note: 



```
import pandas as pd
clothes = pd.read_csv('http://zacharski.org/files/courses/cs419/clothes_train.csv')
clothesTest = pd.read_csv('http://zacharski.org/files/courses/cs419/clothing_test.csv')
clothesY = clothes['label']
clothesX = clothes.drop('label', axis=1)
clothesX
```


And now some code to display a few sample images





```
from matplotlib import pyplot as plt
import numpy as np

def viewImage(x):
    x1 = np.array(x)
    x2 = x1.reshape([28,28]).astype(np.uint8)
    plt.figure(figsize=(2,2))
    plt.imshow(x2, interpolation='nearest', cmap='gray')
    plt.show()
    
viewImage(clothesX.iloc[1])
viewImage(clothesX.iloc[1001])
viewImage(clothesX.iloc[599])


```


![png](/machine-learning/img/baggingDemo_2_0.png)



![png](/machine-learning/img/baggingDemo_2_1.png)



![png](/machine-learning/img/baggingDemo_2_2.png)


### Converting the 0-255 integer values to floats between 0 and 1.



```
clothesXF = clothesX / 255
```

### Divide into training and testing


```
from sklearn.model_selection import train_test_split
clothes_train_features, clothes_test_features, clothes_train_labels, clothes_test_labels = train_test_split(clothesXF, clothesY, test_size = 0.2, random_state=40)
clothes_train_features
```

### Test a simple Decision Tree Classifier




```
from sklearn import tree
clf = tree.DecisionTreeClassifier(criterion='entropy')
clf.fit(clothes_train_features, clothes_train_labels)
```




    DecisionTreeClassifier(ccp_alpha=0.0, class_weight=None, criterion='entropy',
                           max_depth=None, max_features=None, max_leaf_nodes=None,
                           min_impurity_decrease=0.0, min_impurity_split=None,
                           min_samples_leaf=1, min_samples_split=2,
                           min_weight_fraction_leaf=0.0, presort='deprecated',
                           random_state=None, splitter='best')




```
from sklearn.metrics import accuracy_score

basePredictions = clf.predict(clothes_test_features)
accuracy_score(clothes_test_labels, basePredictions)
```




    0.7979166666666667



### Bagging Classifier


```
clf  = tree.DecisionTreeClassifier(criterion='entropy', max_depth=20)
from sklearn.ensemble import BaggingClassifier

bagging_clf = BaggingClassifier(clf, n_estimators=50, max_samples=.75, 
                                bootstrap=True, n_jobs=-1)

```


```
bagging_clf.fit(clothes_train_features, clothes_train_labels)
```




    BaggingClassifier(base_estimator=DecisionTreeClassifier(ccp_alpha=0.0,
                                                            class_weight=None,
                                                            criterion='entropy',
                                                            max_depth=20,
                                                            max_features=None,
                                                            max_leaf_nodes=None,
                                                            min_impurity_decrease=0.0,
                                                            min_impurity_split=None,
                                                            min_samples_leaf=1,
                                                            min_samples_split=2,
                                                            min_weight_fraction_leaf=0.0,
                                                            presort='deprecated',
                                                            random_state=None,
                                                            splitter='best'),
                      bootstrap=True, bootstrap_features=False, max_features=1.0,
                      max_samples=0.75, n_estimators=50, n_jobs=-1, oob_score=False,
                      random_state=None, verbose=0, warm_start=False)




```
bagPredictions = bagging_clf.predict(clothes_test_features)
accuracy_score(clothes_test_labels, bagPredictions)
```




    0.8739166666666667

That is a significant improvement over just a single decision tree classifier



#### Pasting 




```
pasting_clf = BaggingClassifier(clf, n_estimators=25, max_samples=.04, 
                                bootstrap=False, n_jobs=-1)

```


```
pasting_clf.fit(clothes_train_features, clothes_train_labels)
```




    BaggingClassifier(base_estimator=DecisionTreeClassifier(ccp_alpha=0.0,
                                                            class_weight=None,
                                                            criterion='entropy',
                                                            max_depth=20,
                                                            max_features=None,
                                                            max_leaf_nodes=None,
                                                            min_impurity_decrease=0.0,
                                                            min_impurity_split=None,
                                                            min_samples_leaf=1,
                                                            min_samples_split=2,
                                                            min_weight_fraction_leaf=0.0,
                                                            presort='deprecated',
                                                            random_state=None,
                                                            splitter='best'),
                      bootstrap=False, bootstrap_features=False, max_features=1.0,
                      max_samples=0.04, n_estimators=25, n_jobs=-1, oob_score=False,
                      random_state=None, verbose=0, warm_start=False)




```
pastingPredictions = pasting_clf.predict(clothes_test_features)
accuracy_score(clothes_test_labels, pastingPredictions)
```




    0.83408333333



#### Random patches




```
patch_clf = BaggingClassifier(clf, n_estimators=25, max_samples=.6, 
                                max_features=0.6, bootstrap_features=True
                                bootstrap=True, n_jobs=-1)

```


```
%time patch_clf.fit(clothes_train_features, clothes_train_labels)
```




    CPU times: user 147 ms, sys: 346 ms, total: 493 ms
    Wall time: 3min 39s
    BaggingClassifier(base_estimator=DecisionTreeClassifier(ccp_alpha=0.0,
                                                            class_weight=None,
                                                            criterion='entropy',
                                                            max_depth=20,
                                                            max_features=None,
                                                            max_leaf_nodes=None,
                                                            min_impurity_decrease=0.0,
                                                            min_impurity_split=None,
                                                            min_samples_leaf=1,
                                                            min_samples_split=2,
                                                            min_weight_fraction_leaf=0.0,
                                                            presort='deprecated',
                                                            random_state=None,
                                                            splitter='best'),
                      bootstrap=True, bootstrap_features=True, max_features=0.6,
                      max_samples=0.6, n_estimators=25, n_jobs=-1, oob_score=False,
                      random_state=None, verbose=0, warm_start=False)




```
patchPredictions = patch_clf.predict(clothes_test_features)
accuracy_score(clothes_test_labels, patchPredictions)
```




    0.87033333


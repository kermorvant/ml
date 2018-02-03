# k-Nearest Neighbors

One of the most simple yet effective methods to automaticlally classify examples is the K-Nearest Neighbors	algorithm (KNN). This algorithm is instance-based and non parametric : it does not try to model the examples to take a decision and, at test time, it only compares the new examples to the ones already seen during the training phase.



## Exercise


You need to classify objects on which you have measured length and width. The following objetcs have already been measured : 
 
A(1,1), B(3,1), C(2,2), D(6,2), E(2,3), F(5,3), G(7,3), H(6,4), I(3.5,3), J(5,6)

The classes of the points are the following : 

* classe 1 : A,B,C and E
* classe 2 : F, G, H, and J


![](images/exo_knn_points.png)
 
The distance between all the objects has been computed : 
![](images/exo_knn_distances.png)


**Question:**
> What is the predicted class for the points D and I using a kNN classifier with k=1 and for k= 2 ?
> 

## Code

The following code uses scikit-learn kNN to make the prediction : 

```
from sklearn.neighbors import KNeighborsClassifier
df = pd.DataFrame([[1,1.0,'1'],[3,1,'1'],[2,2,'1'],[2,3,'1'],[5,3,'2'],[7,3,'2'],[6,4,'2'],[5,6,'2'],[6,2,'?'],[3.5,3,'?']],
                  columns=['length','width','class'],
                  index=['A','B','C','E','F','G','H','J','D','I']
                 )
X_train = df[#INSERT DATA RANGE][[#INSERT COLUMN NAME]]
y_train = df[#INSERT DATA RANGE][[#INSERT COLUMN NAME]]
X_test = df[#INSERT DATA RANGE][[#INSERT COLUMN NAME]]
y_test = df[#INSERT DATA RANGE][[#INSERT COLUMN NAME]]
clf = KNeighborsClassifier(n_neighbors=2)
clf.fit(X_train,y_train)
clf.predict(X_test)

```
See the [KNN scikit-learn documentation](http://scikit-learn.org/stable/modules/generated/sklearn.neighbors.KNeighborsClassifier.html) for more details.

**Question :**

>  Replace `#INSERT DATA RANGE` and `#INSERT COLUMN NAME` and check that the predictions are the same as yours.
> 
>
>

## Experiment

The goal of the exercise is to train  kNN classifiers on the classical MNIST database. This database is very popular in the machine learning community as a first test for new algorithms. This dataset is quite simple and artificial : having good results on MNIST does not mean that your algorithm is good, but having bad results surely means that you have to improve your algorithm. You can find reference results [here](http://yann.lecun.com/exdb/mnist/).

### Feature extraction

The first step is to extract features from the images to convert the image into a feature vector. The images all have the same size, 32x32 pixels. We reduce them to 8x8 pixels and use the 64 pixels values vector as the features. The function `extract_features_subresolution` process a given image and return the feature vector. The list of all the images is read from the file `Data/MNIST_all.csv`. The features are stored in a matrix `X` and the target class in a vector `y`.


### Train/dev/test split

When training a classifier, the data **must** be separated into different sets : at least one training set and one test set. The split must be random and uniform, which means that the class distribution must be identical in the training and test sets.

**Question** :

> * Use [`train_test_split`](http://scikit-learn.org/stable/modules/generated/sklearn.model_selection.train_test_split.html) to create `X_train/y_train` and `X_test/y_test`. Use 80% of the data for training and 20% for testing.
> * Train a [k-nearest neighbors](http://scikit-learn.org/stable/modules/generated/sklearn.neighbors.KNeighborsClassifier.html) classififier with k=1
> * Test the k-NN with k= 1 on both the training and the test set. Print the score produced by `metrics.accuracy_score`

When evaluating a classifier, it is important to report the error rate both on the training and the classification set. These values are needed to understand what is wrong with the classifier and how to improve it.


**Question** :


The main parameter of the kNN algorithm is the number of neighbors (k). The best value for this parameter depends on the classification task and has to be found by trying different values and selecting the one with the best accuracy. However, this search for the best value **must not** be done on the set used to evaluate the classifier (the test set) but on a validation set. See [wikipedia on training, test and validation sets](https://en.wikipedia.org/wiki/Training,_test,_and_validation_sets).

**Question** : 


>  * Create three sets : train set (60%), validation set (20%) and test set (20%), using twice `train_test_split`
>  * Train a kNN classifier with different values of k and report the train/valid/test accuracy. 
>  * Select the is best value for k according to the accuracy on the dev set. Report the performance performance of the classifier on the test set for this value of k. 



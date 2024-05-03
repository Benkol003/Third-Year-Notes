
#### Gini impurity

is a measure of amount of variation or entropy in a dataset.

calculated as $1-\sum\limits_i^Jp_i^2$ given $J$ classes, with a datapoint of a class occurring with probability $p_i$.

![](misc/Pasted%20image%2020240503010958.png)

for split datasets we can combine Gini impurity via weighted average:

![](misc/Pasted%20image%2020240503011042.png)

![](misc/Pasted%20image%2020240503011102.png)


- the CART algorithm (Classification and Regression Trees) is an algorithm that uses greedy search to find the best splits for a decision tree, using gini impurity (or other entropy measures).

the algorithm tries to minimise gini impurity. from the equation, if for one of our nodes in our decision tree, if its gini impurity is zero, it means that datapoints only occur in one class with a $p_i=1$. why? if we have a node that is all of one class in a decision tree then we can label that as a leaf node - when making a prediction for a new sample when it reaches a leaf node in the decision tree, the final prediction is the class label of a leaf node. note that we can have multiple leaf nodes of the same class i.e. the subset of data for a class can be split across different nodes.


anyway the CART algorithm will create a new split at the input dimension and value that minimises the gini impurity.


#### building decision trees: CART algorithm

![](misc/Pasted%20image%2020240503013802.png)

(https://www.youtube.com/watch?v=fO2aS375WY0)

#### Split point with gini impurity for numerical values

![](misc/Pasted%20image%2020240503015919.png)

given our data sorted down the feature dimension, calculate intermediate weights (e.g. halfway) between the values, use as thresholds for L/R branch. This gives us **all possible ways to split the data down that dimension**.
Then calculate gini impurity for each of these splits, choose one with lowest.

![](misc/Pasted%20image%2020240503020122.png)

### Random forests

(we've covered this in much better detail in COMP34112 [Week 5 Ensemble methods](../COMP34112%20Mathematical%20topics%20in%20machine%20learning/Week%205%20Ensemble%20methods.md))

on their own decision trees aren't too accurate. to recap:

we use an ensemble method:

we create multiple bootstrapped datasets (random so different) to train multiple different decision trees - also use random subset of features to perform split on. The decision trees perform an ensemble vote on the final class i.e. choose the class with the highest number of votes (or over a threshold for multiclass/regression?). will perform better than just a single decision tree.


## AdaBoost

again we've covered this in [Week 5 Ensemble methods](../COMP34112%20Mathematical%20topics%20in%20machine%20learning/Week%205%20Ensemble%20methods.md)
basically boosting increases the likelihood of selecting incorrectly classified samples (train on them more) when sampling with replacement, therefore subsqeuent decision tree's are trained to correct the errors of previous ones.

weights for each trained model depends on its error.

![](misc/Pasted%20image%2020240503021621.png)


adaboost also uses *weak learners* or stumps and not decision trees - basically boosting is prone to overfitting:
https://stats.stackexchange.com/questions/520667/adaboost-why-decision-stumps-instead-of-trees

a **stump** is just a tree with a single decision

![](misc/Pasted%20image%2020240503021751.png)


## Face Detection

this is different from face recognition i.e. tagging or identifying face images. Face detection is just finding faces in an image.

After face detection we can do stuff like

- fitting active shape models to faces
- auto adjustment of camera parameters e.g. exposure, focus, red-eye removal
- then do face recognition

face detection also generalises to object detection, similar to previous object recognition

### Viola Jones algorithm

(we can also use this algorithm for detection of any other object class)

[paper](https://www.cs.cmu.edu/~efros/courses/LBMV07/Papers/viola-cvpr-01.pdf)

the algorithm works off (facial) rectangular features of different intensity, known as **Haar** features.

![](misc/Pasted%20image%2020240503172942.png)

the value of *white *

These correspond to significant facial features:

![](misc/Pasted%20image%2020240503173033.png)

how to calculate these?

#### Integral images

![](misc/Pasted%20image%2020240503173531.png)

point $(x,y)$ gives the area (sum intensity in this area) formed by $(0,0)$ and $(x,y)$. notably the image retains all original information i.e. the operation is reversible

![](misc/Pasted%20image%2020240503173639.png)

what if we want to find the sum of intensity in an area in the middle of the image?

![](misc/Pasted%20image%2020240503173732.png)

we can then use this technique to calculate intensity area values for the haar features

![](misc/Pasted%20image%2020240503174730.png)

the viola jones algorithm uses 24x24 images. even with the small image size there are still 180,000 different possible features (of the haar features in different position,scale and parity)

#### image normalisation

we can also use the integral images to do this - 

![](misc/Pasted%20image%2020240503174904.png)

(and we normalise to get mean 0, variance 1 as per usual)

### Classifier

Using all 180,000 features is unreasonable. instead the algorithm uses adaboost. for the weak learners , we use a classifier that simply thresholds a single feature to make a prediction.

![](misc/Pasted%20image%2020240503175043.png)

with this classifier they get good accuracy but time performance is still too slow

![](misc/Pasted%20image%2020240503175255.png)

### cascade classifiers

this is used to filter out images that arent faces i.e. only keep good potential matches.

also known as a *degenerate decision tree*

![](misc/Pasted%20image%2020240503175453.png)

![](misc/Pasted%20image%2020240503180800.png)

we want to minimise false negatives especially at early stages. There is a tradeoff between model size and threshold's against performance; at earlier stages more simple models are used to filter the majority of the data; at later stages can use larger models; the time performance increase is negated by the fact the most non-faces are filtered out.

![](misc/Pasted%20image%2020240503181109.png)


![](misc/Pasted%20image%2020240503181037.png)

![](misc/Pasted%20image%2020240503181043.png)

![](misc/Pasted%20image%2020240503181131.png)

applications to other objects:

![](misc/Pasted%20image%2020240503181145.png)
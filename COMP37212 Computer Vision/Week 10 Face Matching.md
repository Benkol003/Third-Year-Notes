
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



### Viola Jones algorithm


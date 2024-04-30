## What is an ensemble model?

WE have multiple versions (e.g. different parameters or weights) of the same model; each model is slightly different, but also makes errors in different ways or on different things. The idea is that say a classification error should only appear in one model \& the majority vote/contribute the correct answer.

### Combining continuous values

#### Jensen's inequality

![](misc/Pasted%20image%2020240409000027.png)
For M predictions in the ensemble:
- $x_i$ - i'th prediction
- $\lambda_i$ - weighting of i'th prediction
- **Convex function**: for any line segment drawn between two points on the line, the function lies below that segment.

![](misc/Pasted%20image%2020240408235956.png)

### Ambiguity Decomposition
Summarising this in machine learning terms, for an ensemble of regression estimators, the squared error of the ensemble is guaranteed to be less than or equal to the average squared error of the individual estimators. In the ensemble academic research community, this is known as the Ambiguity decomposition [(Krogh et al., 1995)](https://papers.nips.cc/paper_files/paper/1995/file/1019c8091693ef5c5f55970346633f92-Paper.pdf)

![](misc/Pasted%20image%2020240409000303.png)

($(\bar{x}-y)^2$ is a convex function)

### Combining votes / discrete values
[Dietterich, 2000](https://web.engr.oregonstate.edu/~tgd/publications/mcs-ensembles.pdf)
If we have an ensemble voting on e.g. class label, where we use class with max votes:
if the probability of a class voting incorrectly is $\epsilon<0.5$ then can guarantee that an ensemble vote will always perform better, if the votes are **statistically independent / independent errors**; will see later this is not always the case.

![](misc/Pasted%20image%2020240409001756.png)


![](misc/Pasted%20image%2020240409001343.png)

### Training ensembles

If we train each of our models in the ensemble on the same training data, the only difference between models is from differences from their learning algorithms; & if these are the same, we get identical models, rendering our ensemble useless.

We could split the dataset into N sets for N models to make the models independent; however this doesn't work well in practice as the models become less accurate on test data due to the reduced training dataset size.

![](misc/Pasted%20image%2020240409105700.png)
(decision trees remain the same accuracy  - naïve bayes gets worse)

However there are algorithms that mitigate this by using *bootstrapping*.

## Parallel algorithms

- **bootstrapping** - given our original dataset, sample $n$ samples, but **with replacement** i.e. can select same sample multiple times.
here where we have a dataset, size $n$, we will bootstrap samples of the same size $n$.

![](misc/Pasted%20image%2020240409110216.png)

### Bagging (Bootstrap Aggregating)

![](misc/Pasted%20image%2020240409110524.png)

- Helps add inherent randomness to the datasets and therefore the models.

![](misc/Pasted%20image%2020240409110548.png)


With the following performance:

![](misc/Pasted%20image%2020240409110625.png)

![](misc/Pasted%20image%2020240409110844.png)

(ensemble error should be under 1%)

#### Analysis

- We have a probability of $\frac{1}{n}$ of selecting when doing a single selection (i.e. for dataset size 1)
- The probability of selecting it *at least once* for dataset size n is $p=1-(1-\frac{1}{n})^n$ i.e. 1 - probability of never selecting it at all.

![](misc/Pasted%20image%2020240409111210.png)

(proof of convergence to $e$ - https://youtu.be/hpCenDJgy7w)

Therefore for large enough n we end up discarding around $36.8\%$ of the original dataset. However, this is only for **one model**.

For the entire ensemble, the probability of a single sample not being included across the entire ensemble drops off rapidly:

![](misc/Pasted%20image%2020240409114841.png)

(this is just $y=e^{-x}$)

Therefore for a large ensemble we will be using all training data - we still don't have an explanation for why the performance is above what is expected from jensen's inequality / ensemble error graph.

![](misc/Pasted%20image%2020240409115205.png)

- White squares represent statistical dependency under $\mathcal{X}^2$ test. Naive bayes classifier has more correlations due to a smaller function class space - more likely to have near duplicate or correlated models.

Decision trees have less correlation due to larger function class space - but are will also overfit more:

![](misc/Pasted%20image%2020240409115429.png)

But this is fine, as the errors caused by overfitting will cancel out as long as they are different from each other.

### Random forests

![](misc/Pasted%20image%2020240409120009.png)

- When building a normal decision tree we would pick the best feature out of **all features**, not some random subset.
- commonly $k=\lceil \sqrt{d}\rceil$    

![](misc/Pasted%20image%2020240409120206.png)

- Random forests performs better than bagging for decision trees

![](misc/Pasted%20image%2020240409120230.png)

## Boosting algorithma

This is a **sequential algorithm**. Each model tries to correct the errors of it's predecessors, \& emphasizing misclassified examples (and opposite for correctly classified):

![](misc/Pasted%20image%2020240409120417.png)

- We start off with a uniform distribution for probability of selecting samples, which gets adjusted to emphasize more wrongly classified ones.


### Adaboost

- Assume we only have two classes taking values $\{-1,1\}$

![](misc/Pasted%20image%2020240409123618.png)
- n samples
- $\epsilon$ denotes **weighted error rate** in this context (and NOT the meaning of epsilon from generalisation bounds notes!)
- $h_t()$ - t'th model in sequential ensemble 
- $H(x')$ the ensemble model; each model weighted by $\alpha_t$; the model votes either 
- sign function:

![](misc/Pasted%20image%2020240409130046.png)


#### Derivation

The average classification error is

![](misc/Pasted%20image%2020240409125709.png)

- $\delta()$ - the dirac delta function

It is hard to use this formula directly due to dirac function being discontinuous.

Instead we use a continuous function that is an upper bound to the average classification error, namely:

![](misc/Pasted%20image%2020240409130453.png)


![](misc/Pasted%20image%2020240409130509.png)

- $y\sum_j\alpha_j h_j(x)$ is positive for correct classification (i.e. model sign and true label sign the same), negative when wrong.
- In relation to this $\delta(x)=0$ for $y\sum_j\alpha_j h_j(x)>0$ on the graph, and $\delta(x)=1$ when $y\sum_j\alpha_j h_j(x)<0$ - in both cases we can see that $y\sum_j\alpha_j h_j(x) \ge \delta(x)$. 
- The exponential loss is **not zero** when we correctly classify - but it is less; important as we use this loss as a model weight later.


Our first model's loss is :

![](misc/Pasted%20image%2020240409131553.png)

Note each sample is weighted $\frac{1}{n}$ i.e. our initial uniform distribution.

Second model loss:

![](misc/Pasted%20image%2020240409131601.png)

now weighting models with $\alpha$ parameter.
this model decomposes to include first model loss, (54): 

![](misc/Pasted%20image%2020240409131625.png)

which is constant as marked, as we have already trained parameters for $h_1$ in the adaboost algorithm.

renaming this term to 

![](misc/Pasted%20image%2020240409131727.png)

to get:

![](misc/Pasted%20image%2020240409131736.png)

i.e. $w_2(i)$ weights each sample $i$, by its loss incurred by model $h_1$ i.e. focusing on incorrectly classified samples. Correctly classified samples still have a loss $>0$, so that it can still be used for the second model to learn.

We can do this for the third model's loss:

![](misc/Pasted%20image%2020240409132145.png)

The importance of a datapoint at each new model step is updated thus:

![](misc/Pasted%20image%2020240409132214.png)

, starting from $\sum_i^nw_1(i)=\sum_i^n\frac{1}{n}=1$.

however after an update $W$ does not have to sum to zero, so normalise:

![](misc/Pasted%20image%2020240409132350.png)

What about finding $\alpha$?

![](misc/Pasted%20image%2020240409132525.png)

($\epsilon_t$ is the ensemble error rate, weighted by the datapoint distribution)

#### Derivation of $\alpha$ (optional)


Most proofs & derivations split up $$\sum_i = \sum_{y_i=h_t(x_i)} + \sum_{y_i\ne h_t(x_i)}$$ i.e. seperate sums when model does/doesnt match true class label $y_i$

![](misc/Pasted%20image%2020240409142637.png)

![](misc/Pasted%20image%2020240409142650.png)

see [here](https://mbernste.github.io/files/notes/AdaBoost.pdf)


### Sequential vs Parralel boosting

- Sequential boosting reduces bias
- parralel boosting reduces variance

![](misc/Pasted%20image%2020240409143542.png)

![](misc/Pasted%20image%2020240409143552.png)


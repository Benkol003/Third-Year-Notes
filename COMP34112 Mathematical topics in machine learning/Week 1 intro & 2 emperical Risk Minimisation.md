# Recommended Reading
[Deep Learning and Computational Physics](https://arxiv.org/abs/2301.00942)
[Ensemble learning for Physics Informed Neural Networks: a Gradient Boosting approach](https://arxiv.org/abs/2302.13143)

- (Probability) Simplex: Is a probability distribution involving $k$ classes; probability of all classes sum to 1. (It is also a generalisation of a triangle to n dimensions.)
![](misc/Pasted%20image%2020240304144247.png)

If we have a $k$-class classifier, it forms a $k$-simplex and has an output space of $[0,1]^k$ (though we are restricted on combinations of each variable to sum to 1.)

A 3-class classifier can output values of the face of this triangle:

![](misc/Pasted%20image%2020240304144616.png)

### Loss functions

- Loss functions: $l(y,f(x))$. y - true data labels. $f(x)$ - predicated labels from model given inputs x. also seen $f(x;w)$ where w is the weights.
	- Squared loss: $\frac{1}{n}\sum\limits_{i=1}^n{ \frac{1}{2}(y_i - f(x_i;w))^2 }$ - the $\frac{1}{2}$ is sometimes emitted for simplicity. (the lecture notes are inconsistent with this)
	- Cross entropy loss:

	![](misc/Pasted%20image%2020240404103754.png)
	y is a **one-hot vector**, setting 1 for the correct label. i.e. cross entropy loss  = $-ln(f^d(x))$ for the output probability on c

Given our training set $D_{train}=\{x_i,y_i\}_{i=1}^n$ our training error will be $l_{train}$.   

Our objective is to find $w^* = argmin_w(l_{train}(y,f(x;w)))$ i.e. weights that minimise the loss on the training set.  

![](misc/Pasted%20image%2020240404105840.png)

We have *local & global* minima; want to find global but training algorithm can get stuck in local minima e.g. SGD.

### gradient descent

we use the 1st derivative of the loss function.

for squared loss:

![](misc/Pasted%20image%2020240404102752.png)

we have the SGD algorithm:

![](misc/Pasted%20image%2020240404103310.png)

$\nabla l(y,f(x))$ is a vector of size $T$ of the gradient at each time step $t$.   

- mini batch SGD - batch size m:

![](misc/Pasted%20image%2020240404105720.png)
- Or stochastic gradient descent - m=1. SGD can escape local minima more effectively than batch descent (as its more stochastic!)


### Decision trees

![](misc/Pasted%20image%2020240404105019.png)

Haven't been taught, but widely used

Decision tree splits off into different dicrections based on input data features; each split is performed at a specific feature dimension and value; basically this partitions n-dimension space to fit the data. tree/recursion depth is used to control under/overfitting (increase/decrease).

![](misc/Pasted%20image%2020240404105931.png)

### Terminology
- Dataset: maps input features to labels - $(x,y)\in \mathbb{R}^d \times \mathbb{R}^k$, e.g. MNIST is $[0,255]^{28\times 28}\times[0,1]^{10}$. 

A dataset $S_n$ is assumed to be an **independent sample of n samples**, $P(x,y)^n$, **from the distribution over** $D=P(x,y)$. (each data point is a sample $(x,y)\in \mathbb{R}^d \times \mathbb{R}^k$). This is known as the **independent and identically distributed (iid) assumption**.

- loss functions are functions $l : \mathbb{R}^k \times \mathbb{R}^k \rightarrow [0,\infty]$
- Models: A function $f: $ with a mapping $\mathbb{R}^d \rightarrow \mathbb{R}^k$ i.e. from d to k - dimensional space.   
- Family Models: Neural network, SVM, decision tree etc, aswell as their hyperparameters e.g. layer + layer size distinguish them as different families of models. $\mathcal{F}$ dentoes a family of models e.g. neural nets of a particular size. The set of all possible models for this mapping $\mathcal{X}\rightarrow\mathcal{Y}$ is denoted $\Omega$ - all possible functions  $\forall f \in \{\mathcal{X}\rightarrow\mathcal{Y}\}$. also called **function class**.

### Risk
(aka error)
- Emperical risk (of a model $f(x)$): normalised sum of loss over our training dataset of size n:

![](misc/Pasted%20image%2020240404110511.png)

- Population risk: emperical risk as $n\rightarrow \infty$:

![](misc/Pasted%20image%2020240404112058.png)

i.e. the loss over the entire data distribution. $E_{(x,y)~D}$ means the expected/mean value of loss over all points in distribution $D$. This is the **generalisation error**.

#### Risk Minimiser

- An emperical risk minimiser is a model in $\mathcal{F}$:

![](misc/Pasted%20image%2020240404112411.png)

$f_{erm}$ is the best model for our training dataset, for model family $\mathcal{F}$.

- A global risk minimiser does the same over the infinite distribution - is the 'best in family' model.

![](misc/Pasted%20image%2020240404112746.png)

- Global risk minimiser in $\Omega$ - known as the **Bayes Model**, with **Bayes Risk** $R(f)$:

![](misc/Pasted%20image%2020240404112823.png)

No restrictions on the model or dataset size we use. This is the **optimal model**, but hypothetical and not obtainable in any real scenario.
The bayes risk is **not zero!** - even with a perfect model, for given $x$ we may see different random $y$ from the random distribution.

The bayes model defines predictions w.r.t. a loss function:

- ![](misc/Pasted%20image%2020240404114020.png)

- (cross entropy loss)

	![](misc/Pasted%20image%2020240404114035.png)

General notation:
$R$ - population risk, $\hat{R}$ - emperical risk
$f$ - our model, $f^*$ - best in family, $y^*$ - bayes model. 

### Approximation-estimation decomposition

Risk for different models and finite vs infinite dataset:

![](misc/Pasted%20image%2020240404114310.png)


- Excess risk i.e. how much we could improve our model:
	![](misc/Pasted%20image%2020240404113706.png)


#### Approximation-estimation decomposition

![](misc/Pasted%20image%2020240404114848.png)

- Aprroximation error: due to restricting ourselves to a  specific model family
- Estimation error: due to having a limited sample size to find $R(f_{erm})$ (though don't confuse with using population risk here); don't have the best parameters/weights.

![](misc/Pasted%20image%2020240404115028.png)


#### Family size trade-off

denoted $|\mathcal{F}|$,

![](misc/Pasted%20image%2020240404121653.png)

This is underfitting vs overfitting! effectively have the *Multiple comparisons problem*; more functions to evaluate which one is best with larger family, and with fixed data amount, makes things harder.

- If bayes model $\mathcal{F}$ approximation error is zero as family model doesn't have loss of generality.


#### The Approximation/Estimation/Optimisation decomposition

Usually its impossible to find the empirical risk minimiser $F_{erm}$ on a finite dataset; this introduces **optimisation error** $R(f)-R(f_{erm})$.

![](misc/Pasted%20image%2020240404122502.png)


![](misc/Pasted%20image%2020240404122644.png)

####  The estimation-approximation decomposition is uncalculable

Why? Becuase its impossible to find a) the perfect/bayes model, b) train the bayes model on the population dataset of infinite size
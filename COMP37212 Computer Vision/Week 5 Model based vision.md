# Principal Component Analysis (PCA)

(go revise if you dont know what it is.)

is a **dimensionality reduction** technique.

Algorithm:

![](misc/Pasted%20image%2020240430222129.png)

i.e. our data points are expressed in a basis of eigenvectors.

Take a look at this data:

![](misc/Pasted%20image%2020240430222203.png)

$v_1,v_2$ are eigenvectors, $v_1$ has a larger eigenvalue. We could choose to remove the $v_2$ and express data in terms of $v_1$ i.e. a model assuming lies on the line of best fit $v_1$.
$v_2$ can also be thought as the variance w.r.t. $v_1$ of our data.


- **covariance** - the linear relationship between two dimensions. 0 if no relation.


![](misc/Pasted%20image%2020240430222410.png)


#### Constructing a covariance matrix.

Given $d$ dimensions we have a $d\times d$ matrix, with element $ij$ being covariance(i,j). Note this is a symmetric matrix as element $ij$=$ji$ as covariance(i,j) = covariance(j,i).

![](misc/Pasted%20image%2020240430222548.png)

equation for covariance:

![](misc/Pasted%20image%2020240430222602.png)


for the data in the previous graph:

![](misc/Pasted%20image%2020240430222655.png)

![](misc/Pasted%20image%2020240430222703.png)

what happens if we remove a dimension? we can calculate the amount of variance retained by:

(sum of covariance eigenvalues of reduced dimensionality model) / (sum of covariance eigenvalues for the full dimensionality model)

![](misc/Pasted%20image%2020240430222824.png)


PCA is very similar to truncated SVD (if you did ML/NLP!) however SVD performs decomposition on the full matrix of data. also in PCA the eigenvectors are **orthonormal** (they may not be in SVD).
You'll see later if we want to adjust features in active shape models, we want these features to be independent.

https://math.stackexchange.com/questions/3869/what-is-the-intuitive-relationship-between-svd-and-pca

![](misc/Pasted%20image%2020240430223833.png)

moreover SVD is better suited for features where having a small values means it is insignificant. PCA we 'normalise' our data importance or SVD operation around the mean of the dimensions.


## Active shape models

[paper](https://personalpages.manchester.ac.uk/staff/timothy.f.cootes/Papers/cviu95.pdf)

![](misc/Pasted%20image%2020240430230235.png)

for our 'active shape' we have 20 points so 40 variables total.

and we're gonna annotate our data with this shape e.g. six images here to get a $40\times 6$ matrix.

![](misc/Pasted%20image%2020240430230317.png)

Then we apply PCA to this matrix to get the strongest 'features' of our model.

![](misc/Pasted%20image%2020240430230440.png)

We also compute the mean (across the dimension of length 6) of this matrix to get the mean shape model. We may only choose to use say top n features or enough features to retain say $90\%$ covariance (so we dont deal with features that dont change the shape in any meaningful way.)

![](misc/Pasted%20image%2020240430230514.png)

we can now **generate** new active shape models by varying the features given  by the eigenvectors.

![](misc/Pasted%20image%2020240430230654.png)

![](misc/Pasted%20image%2020240430230716.png)

### Shape fitting

what if we have an active shape model that we want to fit to a new image?

![](misc/Pasted%20image%2020240430230906.png)

![](misc/Pasted%20image%2020240430231011.png)

when making your active shape model, you wanna design it so that the **points and lines lie on distinctive edges** for this to work.

Choose edges closest to your points that are strong enough (i.e. over a threshold?).

First find best general image transformations to fit the current shape to the new points. Then adjust the features in shape parameter vector $b$ to fit the model to the edges.

![](misc/Pasted%20image%2020240430231123.png)

active shape model fitting demos:

https://www.youtube.com/watch?v=HvtxnlvlSDU
https://www.youtube.com/watch?v=I3YsqHCQB4k

![](misc/Pasted%20image%2020240430231445.png)

#### Active appearance models
https://www.cs.cmu.edu/~efros/courses/AP06/Papers/cootes-pami-01.pdf
similar to active shape models.
https://en.wikipedia.org/wiki/Active_appearance_model
works with a texture instead - we fit our model so that it's texture appearance matches the image we are targeting?

![](misc/Pasted%20image%2020240430231605.png)
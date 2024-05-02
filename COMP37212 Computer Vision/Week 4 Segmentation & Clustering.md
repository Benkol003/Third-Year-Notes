- Segmentation: idea of being able to group pixels - typically by object.
- 
![](misc/Pasted%20image%2020240429231213.png)

- Gestalt pyschology: "the processing of entire patterns and configurations, and not merely individual components".

- Reification - construction of extra information from the scene i.e. we may see a white triangle or sphere.

![](misc/Pasted%20image%2020240429231306.png)

- Multistability - ambiguity in the perception of an image
- 
![](misc/Pasted%20image%2020240429231410.png)

E.g. the rubin vase may also appear to be two faces.

#### Gestalt Factors

![](misc/Pasted%20image%2020240429232435.png)

Identifying features that help us group stuff together.

These factors are used in computer vision, e.g. proximity, common fate (e.g. gradient direction)

### Achieving segmentation
First require a ground truth to compare against. Use human labeled data:
- [Berkeley segmentation dataset](https://github.com/BIDS/BSDS500)


However humans may segment inconsistently. To get around this the dataset uses the summation of human feedback so more labeled areas have stronger edges.

![](misc/Pasted%20image%2020240429232916.png)

#### Superpixels
[X. Ren and J. Malik. Learning a classification model for segmentation](https://home.ttic.edu/~xren/publication/xren_iccv03_discrim.pdf)

Create 'superpixels' or large cluster of pixels i.e. over-segment an image. Can then group superpixels into larger objects. Allows an intermediate representation which we can choose how much detail or segmentation desired in the image. E.g. group by similar colour or texture.

![](misc/Pasted%20image%2020240429233332.png)

## Segmentation as clustering
Very similar to machine learning e.g. SVD

### k-means

can use image intensity to group pixels:

![](misc/Pasted%20image%2020240429233817.png)

However what if we have say image noise or blurring?

![](misc/Pasted%20image%2020240429233852.png)

Segment the data into k-means subsets.

We want to solve to minimise the SSD (sum of squares distance) between cluster centers and pixels that have been assigned to that cluster.

The algorithm:

![](misc/Pasted%20image%2020240429234223.png)


#### feature spaces

atm we have talked about greyscale or intensity images, which is a 1D feature space. Using a colour image gives us a 3D feature space with more information to work with. We could even have a 5D feature space by encoding x and y space to enforce spatial coherence.

![](misc/Pasted%20image%2020240429234604.png)

here we see segmentation via colour but not by object with the 3D feature space.



![](misc/Pasted%20image%2020240429234717.png)

## Probabilistic clustering


### Gaussian Mixture Model (GMM)

![](misc/Pasted%20image%2020240430141441.png)

#### Covariance

- **covariance** - the linear relationship between two dimensions. 0 if no relation.


![](misc/Pasted%20image%2020240430222410.png)


![](misc/Pasted%20image%2020240430141505.png)

Basically:

- Spherical covariance - variance is same in all dimensions, i.e. spherically distributed
- Diagonal covariance - variance differs in dimensions, but dimensions are still 'independent'? covariance is oval but still aligned with axis
- Full covariance - similar to diagonal covariance but now have some rotation of the oval - indicates dependence between dimensions

A guassian mixture model (GMM) is generative i.e. can use to generate new data.

![](misc/Pasted%20image%2020240430141822.png)

![](misc/Pasted%20image%2020240430141846.png)

Have $k$ gaussian models, weighted by $k$ size parameters $\forall \pi_c | \pi_c \in \Pi$   

![](misc/Pasted%20image%2020240430142005.png)

#### Training Mixture of Gaussian model

![](misc/Pasted%20image%2020240430142110.png)

size parameter is $\alpha$ here. Works similarly to k means.


Applications:

![](misc/Pasted%20image%2020240430142300.png)

used to detect a change in the image - the above is a bad example bc the image is static (and you could just subtract the background image). Lets say you've got a CCTV feed outside that continuously changes e.g. due to weather, lighting, maybe tree movement. But you can still train a GMM model on this data. If someone breaks in you can tell theres a bunch of pixels that are outliers to the model.


Use it for segmentation!

![](misc/Pasted%20image%2020240430142454.png)

In this case they are using $(r,g,b,x,y)$ as the 5 dimensions.

![](misc/Pasted%20image%2020240430142527.png)


## Mean-shift clustering

[paper](https://courses.csail.mit.edu/6.869/handouts/PAMIMeanshift.pdf)

![](misc/Pasted%20image%2020240430152344.png)

![](misc/Pasted%20image%2020240430152353.png)

Typically use a gaussian kernel for mean shift to calculate the gradient (as in edge detection).

Given a starting point we calculate the gradient from the window kernel to calculate the **mean shift vector** that tells us which way to iteratively move the mean value to a local maxima.

We do this for all points (not always, may use points at regular intervals i.e. tesselate, as its very computationally expensive).

![](misc/Pasted%20image%2020240430152708.png)

![](misc/Pasted%20image%2020240430152721.png)

Typically windows will converge to the same local maxima - we merge these togther. The trajectories of these windows give the contours of the image surface.


![](misc/Pasted%20image%2020240430152816.png)

The no. of maxima found depends **strongly** on the size and type of kernel window used. However we don't need to tell the algorithm how many segments we have in our image, unlike k-means/GMM.

![](misc/Pasted%20image%2020240430152920.png)

performs very well at image segmentation


![](misc/Pasted%20image%2020240430152938.png)


![](misc/Pasted%20image%2020240430152952.png)

## Graph theoretic segmentation

based off this [paper](https://people.eecs.berkeley.edu/~malik/papers/SM-ncut.pdf)

Can form a fully connected graph between all pixels in an image:

![](misc/Pasted%20image%2020240430161945.png)

use affinity measures to calculate edge weights:

![](misc/Pasted%20image%2020240430162003.png)

$\sigma$ value controls affinity / dropoff in weight with larger distance

![](misc/Pasted%20image%2020240430162051.png)

graph can be represented as a matrix - weight of connecting pixel $i$ to pixel $j$ is at the $ij$'th element. Typically use a symmetric matrix where half the weight in $ij$ and $ji$ respectively.

We can segment our image by taking cuts of the graph - get rid of weak connections.

![](misc/Pasted%20image%2020240430162105.png)

$\sigma$ value - too small and have bad clustering. too high and there is not enough segmentation between clusters.

![](misc/Pasted%20image%2020240430162215.png)


### Optimal cuts

#### minimum cut

given two clusters A and B calculate the cost of the cut between them as 

![](misc/Pasted%20image%2020240430164908.png)

to find the best minimum cut can use algorithms such as https://en.wikipedia.org/wiki/Stoer-Wagner_algorithm.

However the current cost estimate is biased to make small cuts - i.e. not proportional to the size of the clusters. can fix this with *normalised cut*.

#### normalised cut
![](misc/Pasted%20image%2020240430165200.png)

Finding the best normalised minimum cut however is NP-complete as shown in the paper (compared to regular minimum cut).

i.e. value of normalised cut is weighted by the sum of all connection weights for each cluster.

![](misc/Pasted%20image%2020240430170520.png)

works well especially for small details.

![](misc/Pasted%20image%2020240430170537.png)


## Grabcut

[paper](https://www.microsoft.com/en-us/research/wp-content/uploads/2004/08/siggraph04-grabcut.pdf)

The model uses two gaussian mixture models, for background and foregound. Starts out with base assumption all pixels are foreground i.e. $\alpha_{fg}=1, \alpha_{bg}=0$. The user also provides a border region in which the object they want is contained.

![](misc/Pasted%20image%2020240430173130.png)

Algorithm:

![](misc/Pasted%20image%2020240430173147.png)


#### Superpixels for computational speedup
https://www2.cs.sfu.ca/~mori/research/superpixels/
https://arxiv.org/abs/1612.01601

can combine superpixel oversegmentation, which are fast, with other segmentation techniques - as using these algorithms on all pixels is very expensive -e.g. a 1000x1000 image requires up to a 1TB graph matrix. Many different superpixel algorithms exist.

requirements of superpixels to be useful:

![](misc/Pasted%20image%2020240430173606.png)


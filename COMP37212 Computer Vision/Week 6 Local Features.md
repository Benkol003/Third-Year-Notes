local features are local, identifiable, unique parts of an image. Used for Simultaneous Location and Mapping (SLAM), image matching, panorama stitching...



![](misc/Pasted%20image%2020240501010059.png)

![](misc/Pasted%20image%2020240501010106.png)

descriptor requirements:

![](misc/Pasted%20image%2020240501010116.png)

this is in two parts:

- feature detection
- building descriptors from the detected features

list of feature detectors and descriptors - note that *finding* a local feature and building a descriptor for it are two different things.
for example ORB finds descriptors with FAST and computes descriptors with BRIEF, but with modifications to these algorithms.
SIFT and SURF are patented feature descriptors.
HARRIS, Laplacian, DoG are some feature detectors.

![](misc/Pasted%20image%2020240501010127.png)


# Feature detection

## Harris edge detector

- edges aren't useful for local features as only unique in one direction - local feature could occur anywhere along the edge - there is not much change *along* the edge. However corners are ideal for local features as there is significant change in all directions from the corner.

![](misc/Pasted%20image%2020240501011051.png)

The **harris detector** can be used to find corners in an image.

Its important that, at a corner, there is significant **change in intensity** within the window if we move in **any** direction.

We can calculate the sum-of-squares distance change as:

![](misc/Pasted%20image%2020240501023254.png)

for some change in any direction $[u,v]$.

We can approximate this with a 2d taylor function:

![](misc/Pasted%20image%2020240501023332.png)

we use the first order approximation.

- $f_x$ and $f_y$ denote **partial derivatives w.r.t. x and y**.
- ok now lets just swap these out and name em $I_x$ and $I_y$ as the partial derivatives. We use image gradient images in x and y from e.g. canny for these when it comes to implementation. you REALLY need to use gaussian blur before edge detection - image noise looks a lot like corners and WILL create false positives.

this can be rewritten as:

![](misc/Pasted%20image%2020240501023807.png)

![](misc/Pasted%20image%2020240501023843.png)

(reintroduced the window function that was part of the equation earlier. may be flat or gaussian.)

![](misc/Pasted%20image%2020240501023953.png)

Now our matrix M we can actually interpret as a covariance matrix. The SSD value $E(u,v)$ gives the strength or density of points for the 2D gaussian.

![](misc/Pasted%20image%2020240501024330.png)

The PCA of this gives us eigenvectors, which we dont care about, and eigenvalues $\lambda_1, \lambda_2$:

![](misc/Pasted%20image%2020240501024648.png)

Corresponding to the 2 axis along which we expect to see the intensity change; at an angle depending on the angle of the edge or corner in the window.
A large eigenvalue means that there is a lot of variance in intensity i.e. a change in intensity along the direction of the corresponding eigenvector

The values of eigenvalues $\lambda_1, \lambda_2$ tell us if we have a flat, edge or corner region:

![](misc/Pasted%20image%2020240501024606.png)

We can calculate this metric R to tell us when we have a corner:

![](misc/Pasted%20image%2020240501024854.png)

In particular we only expect to see a corner when there is intensity change along both eigenvectors i.e. in all directions.

as for a small 2x2 matrix we have

![](misc/Pasted%20image%2020240501024946.png)

(unlike most cases Eigendecomposition here isn't computationally expensive for a 2x2 matrix.)

You can also use a gaussian window filter which modifies the equations thus:

![](misc/Pasted%20image%2020240501034220.png)

- Non-maxima suppression - this is the final step of harris, basically just threshold / filter out local features that are weak / have a low $R$ value.

Properties of harris edge detector:

- **Invariant** to image rotation
- **NOT INVARIANT** to image scale, especially for large scale

![](misc/Pasted%20image%2020240501034407.png)

# Scale Invariant Region Selection

## Laplacian of Gaussian methods

(papers go over in more detail...)

For a feature of interest we could compute descriptors at different scales. however this isnt computationally feasible as drastically increases the number of features we have to match.

For our detection function we will have a peak value or *maxima* at a certain scale:

![](misc/Pasted%20image%2020240501172623.png)



Laplacian of Gaussian is well suited here as it's less sensitive to the size of edges i.e. edge scale, and also **localises** the edge very precisely!.
Will see this is useful so we can precisely match the same feature at the same place across different image scales.


#### Recap of LoG

![](misc/Pasted%20image%2020240501172800.png)

Laplacian seperated kernel:

![](misc/Pasted%20image%2020240501172809.png)

laplacian full kernel:

![](misc/Pasted%20image%2020240501172816.png)

![](misc/Pasted%20image%2020240501172840.png)

![](misc/Pasted%20image%2020240501172910.png)

![](misc/Pasted%20image%2020240501172917.png)

#### Applying LoG

The *characteristic scale* is the ideal scale for a feature where the response (of LoG) is highest.

We scale the image, different scales $\sigma_1,\sigma_2,\dots$ and apply LoG to get distinctive features in each scale image.

![](misc/Pasted%20image%2020240501173058.png)

- In each scale image, look for points that are a local maxima in it's local region (i.e. 3x3 window) and above a threshold.
- The collection of of image at different scales is known as a **scale space**. For a certain feature detected at a certain scale, we only keep it if it's maximum across the scale dimension (but at the same position - this is why using LoG is important, as it find the position of features accurately) i.e. it's at the characteristic scale. We will have a lot of the same features detected but at different scales, so this allows us to find the optimal scale for the feature.


![](misc/Pasted%20image%2020240501173456.png)

### Harris-laplace
[paper](https://ieeexplore.ieee.org/document/937561)

LoG is actually pretty bad at finding unique features - it finds edges well...


So we apply harris corner detector at different scales, and LoG at those different scales. We only keep harris features at a specific location and scale that are also 
the maximum in the scale space for LoG.

![](misc/Pasted%20image%2020240501173731.png)

![](misc/Pasted%20image%2020240501173739.png)

### LoG approximation with DoG

Typically already have first derivative of gaussians to use. can approximate the LoG or second derivative of gaussian for a speedup as the difference between two first derivative of gaussians.


$$g''(x)\approx\frac{g'(x+\Delta x)-g'(x)}{\Delta x}$$    

![](misc/Pasted%20image%2020240501174333.png)

![](misc/Pasted%20image%2020240501174346.png)

![](misc/Pasted%20image%2020240501174356.png)

another note - when adjusting scale downwards we are performing downscaling or subsampling, when $\sigma=2^n$ i.e. resolution is exactly half along each dimension.

![](misc/Pasted%20image%2020240501174555.png)

Each 'step' down by half in resolution is called an octave.

# Feature Descriptors

## SIFT
[SIFT paper](https://www.cs.ubc.ca/~lowe/papers/ijcv04.pdf)

Using just the image patch alone is idea - very sensitive to rotation, scale, illumination  etc.


Instead we calculate intensity gradients for each pixel in the feature region, and build a histogram of gradient directions.

![](misc/Pasted%20image%2020240501190152.png)

To begin with, we assume we have already normalised scale: with SIFT we use DoG as mentioned earlier with a scale map to find scale with maximal response.

1. take a pixel region over the feature. e.g. 8x8 window.

![](misc/Pasted%20image%2020240501191705.png)

calculate the gradient histogram for this - we also **bin/quantize** the gradients into angles of $45\textdegree$ so that we only have 8 gradient directions.

![](misc/Pasted%20image%2020240501191800.png)

(their diagrams are shit, there's only 7 bars there)

now **normalise the orientation** so that the *dominant orientation* i.e. the direction with the most calculated gradients is chosen to be up - i.e. shift the histogram.


2. Divide the region into 4x4 subpatches. so we get $n/4 \times n/4$ subpatches.

this is 2x2 subpatches for a 8x8 image. Calculate the histogram for these subpatches. Note we dont need to orientation-normalise these again.

![](misc/Pasted%20image%2020240501192552.png)


Now concatenate these to form our descriptor - we will be left with a descriptor of size $n/4 \times n/4 \times 8$ i.e. $4\times 4\times 8$ or 128 dimensions in this case. (SIFT uses 16x16 region instead in their implementation.)


![](misc/Pasted%20image%2020240501192744.png)

![](misc/Pasted%20image%2020240501192754.png)

## Feature matching

- Use Sum-of-Square Distance (SSD) = $\sum\limits_i^I (x_i-y_i)^2$ (two samples x,y, i dimensions)

Note that 'distance' **doesn't relate to real distance** here! its the distance in the vector space of the descriptor.

however matches may be ambiguous:

![](misc/Pasted%20image%2020240501193732.png)

and can match to the wrong feature. use a ratio test to filter threshold out ambiguous matches - use the closest and second closest match by SSD:

![](misc/Pasted%20image%2020240501193756.png)

![](misc/Pasted%20image%2020240501193958.png)


#### ROC curve (recap)

![](misc/Pasted%20image%2020240501212705.png)

Given our model output, we choose a threshold for making a prediction. I.e. a threshold for the ratio, or threshold on neuron output probability for a multiclass classifier, to tell us when matches are too ambiguous or not.
for each threshold we plot the TP to FP. we can also do this for different models e.g. different algorithms, model architectures - get a ROC curve for each model.

![](misc/Pasted%20image%2020240501213202.png)

The best model is the one with the highest area under curve (AUC)- the curve will tend towards the ideal point of max TP and zero FP.


![](misc/Pasted%20image%2020240501213514.png)


#### Applications of SIFT

- object recognition - works even with occlusion of the object

![](misc/Pasted%20image%2020240501213616.png)

![](misc/Pasted%20image%2020240501213621.png)

- image stitching:

![](misc/Pasted%20image%2020240501213634.png)

wide stereo baseline stereo matching (next week) (paper)[https://ieeexplore.ieee.org/document/710802]

![](misc/Pasted%20image%2020240501214154.png)
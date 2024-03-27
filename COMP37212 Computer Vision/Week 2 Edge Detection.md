## Image Differentiation: definition of an edge

- An edge is a place where we have a rapid change in intensity.

![](misc/Pasted%20image%2020240314140453.png)

- We can use the maxima and minima (which one also determines the direction of the edge) of the first derivative to find where edges occur. (Sobel, Canny)
- Edges can also be found via the zero-crossing of the second derivative (Marrs-Hildreth uses this), however we don't get information about the direction of the edge, and strength of the edge.

 - Because differentiation only applies to continuous functions, edge detection of a derivative with e.g. sobel is an **approximation** due to discrete nature of images.

### Edge scale selection

This is a parameter we set when doing edge detection; it allows us to discard weaker edges that may be noise and only keep stronger ones; we might do this e.g. after applying a sobel operator thresholding values above a value to only keep edges above that strength.

## Gaussian blurring

A form of weighted blurring; use the Gaussian distribution to produce the kernel.

for a kernel of size $X \times Y$, $$G(x,y)=\frac{1}{2\pi\sigma}e^{-\frac{x^2+y^2}{2\sigma^2}}$$

A 2D gaussian look like thus:

![](misc/Pasted%20image%2020240314140739.png)


- It's common to perform gaussian blurring to remove artefacts for edge detection. If we apply gaussian blurring (function $f$) then find the first derivative, we get a function that looks like this:

![](misc/Pasted%20image%2020240314142435.png)

which when applied to an image (intensities $I$) looks like:

![](misc/Pasted%20image%2020240314141616.png)

![](misc/Pasted%20image%2020240314141533.png)

(You can note that $\frac{d}{dx}f$ looks like $\frac{d^2I}{dx^2}$ - don't get them confused.)

## Sobel operators 

Two kernels for calculating horizontal and vertical edges:

![](misc/Pasted%20image%2020240314140238.png)



## Separable Filters

(Or decomposable kernels). We are able to separate a kernel matrix into a product of simpler filters.

#### Separated Sobel Filter

For the vertical Sobel filter:

![](misc/Pasted%20image%2020240314140302.png)

& why are separable filters faster?

![](misc/Pasted%20image%2020240312164814.png)

In the case of the vertical Sobel operator, by using a separable filter; with the $3\times 3$ filter we perform multiplications on each row 3 times as the filter passes downwards; by using the separated version we calculate the row multiplies (with $\begin{bmatrix}1 & 0 & 1\end{bmatrix}$) only once and store these in a separate matrix to then multiply with the $\begin{bmatrix}1\cr2\cr1\end{bmatrix}$ matrix.

#### Separated Gaussian filter

The gaussian filter is also seperable into two functions in x and y using the gaussian function.

![](misc/Pasted%20image%2020240312172310.png)

![](misc/Pasted%20image%2020240312172250.png)


## Canny Edge Detection

[paper](https://ieeexplore.ieee.org/document/4767851)

 - Canny operator moves differentiation and gaussian smoothing into one kernel via property of association of kernels, but involves 


### Dealing with thick edges

A first derivative edge detector may have very thick edges; if we use a second derivative edge detector then we can more accurately localise the middle of an edge to where the zero crossing occurs:

![](misc/Pasted%20image%2020240321115119.png)


### Derivative-of-Gaussian Filter

A.k.a. first derivative of gaussian; 

![](misc/Pasted%20image%2020240321124844.png)


- The sobel edge detector is an approximation of the first derivative of gaussian [see here](https://theailearner.com/tag/scharr-operator/)

![](misc/Pasted%20image%2020240321125934.png)

### Laplacian-of-Gaussian Filter

A LoG or *2D laplacian kernel* may look like:

![](misc/Pasted%20image%2020240321123811.png)

Note that the kernels are normalised to sum to 90. LoG kernels are not seperable.

A.k.a. second derivative of gaussian; used in both canny and Marrs-Hildreth

![](misc/Pasted%20image%2020240321123259.png)

to produce images thus:

![](misc/Pasted%20image%2020240321123642.png)
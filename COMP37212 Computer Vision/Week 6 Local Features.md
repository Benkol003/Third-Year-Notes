local, identifiable, unique parts of an image. Used for Simultaneous Location and Mapping (SLAM), image matching, panorama stitching...



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

WE can calculate the sum-of-squares distance change as:

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

# Feature descriptors
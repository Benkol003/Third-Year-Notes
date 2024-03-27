## Hough Transform

![](misc/Pasted%20image%2020240321132038.png)

With the condition threshold that a line must pass through at least 3 points.

The equation of a line is $y=mx+c$; we can rearrange to $c=y-mx$. The range of all possible lines through a point is in the **Hough parameter space** of $m$ and $c$.

We can draw lines for parameters $m,c$ for all points:

![](misc/Pasted%20image%2020240321132354.png)

(m as horizontal axis, c as vertical axis)

There is an intersection point of three lines (above threshold) at $(m,c)=(-1,0)$.  Other lines may have two intersections (two points) but are not above our threshold.
The corresponding line in xy space:

![](misc/Pasted%20image%2020240321132533.png)

#### Use of polar coordinates

We can represent a line in **Hesse normal form** parameters radius $r$, angle $\theta$:
- $x = r\cos(\theta)$
- $y = r\sin(\theta)$ 

The red line described is perpendicular to the line between the origin and the point at $(r,\theta)$.

![](misc/Pasted%20image%2020240321134018.png)

It follows that we define polar Hough space as $r = x \cos(\theta) + y\sin(\theta)$.

We use polar coordinates for Hough space as with cartesian coordinates it's hard to use programmatically e.g. we cant represent vertical lines (where $m=\infty$) accurately.

In this form the hough transform is almost identical to the [radon transform](https://en.wikipedia.org/wiki/Radon_transform)
#### Programmatical Hough edge detection

We form a zero-initialised array with varying range and step of the parameters. We then fill in our hough lines in the array, incrementing elemnents it passes over:

![](misc/Pasted%20image%2020240321133302.png)

We then perform thresholding on the array to find $(r,\theta)$ pairs for lines we can draw.

We can already determine the range of $r,\theta$:
- r takes the range of values such that the radius from the image centre can reach any corner
- $\theta$ is from $0\textdegree-360\textdegree$  or $0^c-2\pi^c$ radians.

We only need to set resolution of parameters to determine the array size, + threshold value.

Note that if we used cartesian coordinates then m could be of range $[-\infty,\infty]$ (cant have infinite array!), and we would have better resolution for horizontal edges unless we used an exponential step.

## Circle Hough Transform

Allows us to find candidates for circle centre's where our edge 

#### Equation of a circle

$(x-a)^2+(y-b)^2 = r^2$

we have three parameters: circle centre $(a,b)$ & radius $r$ as our parameter space to search if points lie on circles in this parameter space. Again the range of $a,b,r$ can be inferred from image dimensions, so we just specify resolution.

Therefore in this instance we have to construct a 3D accumulative array to threshold.

Side-by-side comparison of edge-detected vs CHT at different radii:

![](misc/Pasted%20image%2020240321153555.png)

We have 1 circle of r=25, 3 of r=50.

## Generalised Hough Transform

We can generalise detection to an arbitrary shape, or *template/template matching*. 


### Base case: fixed scale and rotation

Given our template shape, which is typically a black and white image (black for edge pixels) (could be generated through edge detection on an image):

![](misc/Pasted%20image%2020240321154207.png)

We choose a reference point inside the shape e.g. centre of gravity, (centre of image).

For each edge pixel:
- Calculate relative polar coordinates to centre as parameters $r,\alpha$.

![](misc/Pasted%20image%2020240321154758.png)

- Calculate the edge direction from the edge normal as $\phi$. (All angles are from the horizontal axis.)    

![](misc/Pasted%20image%2020240321154919.png)

#### R-tables

With the above information we build an *R-table* - a list of possible $(r,\alpha)$ pairs to calculate possible image centres. Again we build our table to a resolution size of $\phi$.

![](misc/Pasted%20image%2020240321155541.png)

For the image we want to apply the generalised Hough transform to:

we build an accumulator array with same size of image:

![](misc/Pasted%20image%2020240321155648.png)


Our image needs to have edge detection applied, with **information about edge direction preserved**: we do this by using the separate edge images from horizontal and vertical sobel kernels to from the edge direction vector. We cant use Laplacian-of-Gaussian edge detection here.

- For every edge pixel in our image, we take it's edge normal direction $\phi_{x,y}$ then lookup in the R-table to calculate all possible image centres from that point: for every $(r_i,\alpha_i)$ pair in the row we use that with the edge pixel coordinate $(x,y)$ to increment all cells at locations $x_c = x + r_i\cos(\alpha_i)$,$y_c = y + r_i\sin(\alpha_i)$.
- Finally perform thresholding on cell value to get most likely image centre's.

Image examples:

![](misc/Pasted%20image%2020240321160447.png)

![](misc/Pasted%20image%2020240321160454.png)

![](misc/Pasted%20image%2020240321160500.png)

### Generalise to any scale & rotation

We have additional parameters $(s,\theta)$ which affect the equation to the image centre:

![](misc/Pasted%20image%2020240321160600.png)

Which requires a 4D accumulator array of $(x_c,y_c,s,\theta)$ votes. When creating the R-table we can assume $s=1, \theta=0$ from the source image; when we perform lookups we need to find $(r,\alpha,s,\theta)$ tuples that match edge normal $\phi$; these can be easily calculated for all values of $s,\theta$ by applying transforms to our $(s=1,\theta=0)$ R-table (for rotations, $\phi + \theta\mod 2\pi,\alpha+\theta\mod 2\pi$; for scaling, $s\times r$ ).  

Unfortunately this is extremely memory & performance intensive at larger scales/resolutions.


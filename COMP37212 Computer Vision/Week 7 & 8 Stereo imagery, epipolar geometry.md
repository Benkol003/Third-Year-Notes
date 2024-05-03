# Stereo Vision

Given a scene we have two cameras e.g. the eyes or a set of calibrated cameras. We can infer depth from the images at the two cameras.

![](misc/Pasted%20image%2020240501215837.png)

### Perspective transform

i.e. as of a pinhole camera

![](misc/Pasted%20image%2020240501215931.png)

we can move the image plane forwards without loss of generality:

![](misc/Pasted%20image%2020240501215951.png)

now we have *similar triangles* from the focal point to the image plane or the scene:


![](misc/Pasted%20image%2020240501220015.png)

$f$ - focal length. $x$ - deviation from center of camera, the **principal axis**. x is also known as **principal offset**.

![](misc/Pasted%20image%2020240501220022.png)


### Simple Parralel Stereo System

![](misc/Pasted%20image%2020240501220237.png)
distance between the two cameras is $b$.
Both cameras have a parralel view. The scene origin, denoted $X$, is halfway between the two cameras - so $|\vec{O_lX}|=|\vec{O_rX}|=b/2$. Both cameras also have the same height in y axis, and same focal length.



![](misc/Pasted%20image%2020240501220849.png)
(the notation is shit here)
- $X$ is the x component/position of the point $P$, Z is depth of $P$. $x_l,x_r$ are the x components of the cameras.
- have $X+b/2$ as there is extra distance (past the origin) of b/2 to the principal axis of camera $O_l$. For $O_r$ this distance is $X-b/2$

- **disparity** the distance between location (i.e. pixel coordinate) between the two camera images for the same point of interest.

therefore we calculate the depth of a 'point' by matching the point/feature in both images, and then using the position/disparity from both to calculate depth.

![](misc/Pasted%20image%2020240501221153.png)

![](misc/Pasted%20image%2020240501221202.png)

we can find corresponding points via local feature matching.

Say we have some local features that we match to calculate distance.

![](misc/Pasted%20image%2020240501221330.png)


![](misc/Pasted%20image%2020240501221320.png)

what if we match them wrong?

![](misc/Pasted%20image%2020240501221340.png)

changes the depth perception/calculation a lot. This is hard to do, especially for every point in the image.
https://en.wikipedia.org/wiki/Correspondence_problem

![](misc/Pasted%20image%2020240501221538.png)


#### Epipolar constraint for a parralel stereo system

given a point at a certain (x,y) in one camera the corresponding point in the other camera has to lie on what is called an **epipolar line**.

for the parallel camera case the epipolar lines are parralel. If depth/z of the point of interest increases, so does y for it to pass through the same point in the camera image to the focal point. Imagine a ray from the camera focal point, through the point on the camera image, into space - the actual point lies on this line somewhere. However bc. principal axii of both cameras are the same height then image will appear at the same height in both of them. The only thing that varies between the two camera images is x (cameras lie on different places on x axis, so the x distance to the point is different.)

![](misc/Pasted%20image%2020240501222832.png)

### Depth Operators

#### depth matching edges
 can use these to match features for depth. 
 
![](misc/Pasted%20image%2020240501224456.png)

however not ideal - different intensity / edge strengths.

Another problem - cant calculate depth accurately for horizontal edges - matching point in the other camera could be ambiguously anywhere on the horizontal edge in the parallel camera system case same applies to the general camera case - the epipolar lines could be in any orientation so the problem is worse!

![](misc/Pasted%20image%2020240501224555.png)

#### Moravec operator

using corners is better for matching features. We can use harris (as in last week).

The moravec operator is an earlier corner detector (i dont know why this is in the material, no one uses this lol. harris was literally designed as it's replacement.)

![](misc/Pasted%20image%2020240501225127.png)

anyway **using corner detectors is better**.
one question - this doesn't calculate depth the entire image? like we are only gonna get a depth map for the edges?

(seriously wtf was the point of this section)


## General stereo camera system

sometimes cameras arent gonna have parralel principal axes.

![](misc/Pasted%20image%2020240501225717.png)

### Epipolar geometry

lets cover this in the general case.

- We can assume, *without loss of generality*, that:
	- the baseline between the two cameras is horizontal. If not then we just rotate the entire scene or our final depth map.
	- the image planes for both cameras have the same rotation. If not the case then we can rotate the image plane/s to fix this, as done here:

![](misc/Pasted%20image%2020240501230349.png)

However we **cant fix relative rotation** between the two cameras.


Given a point $m_1$ in the image plane of one camera, the point in 3D space lies along $c_1$. The corresponding point in the 2nd image plane lies on the epipolar line $e_2$. Vice versa we get line $e_1$. This restricts where we  when matching features between images.

https://homepages.inf.ed.ac.uk/rbf/CVonline/LOCAL_COPIES/FUSIELLO/node4.html

![](misc/Pasted%20image%2020240501233336.png)

https://homepages.inf.ed.ac.uk/rbf/CVonline/LOCAL_COPIES/OWENS/LECT10/node3.html

All points on the epipolar line $e_1$ match with a point somewhere on the epipolar line $e_2$, and the real point is space is constrained to the epipolar plane. This is known as the **epipolar constraint**. i.e. we only need to match points in a 2D plane and not 3D space.

note that the epipolar line doesn't cross the entire width of the image; instead it stops at the baseline; these points are called **epipoles**.

![](misc/Pasted%20image%2020240501234429.png)

example of points in one image with corresponding epipolar lines in other:

![](misc/Pasted%20image%2020240501234708.png)

### Stereo reconstruction

points $p$ ( $p_l, p_r$ ), world space point $P$, translation and rotation matricies for each camera $T,R$, local origins $O$ and coordinate systems (/basis vectors) $x,y,z$, focal lengths $f$.

![](misc/Pasted%20image%2020240501235125.png)

denote converting points to scene coordinates with $P'$.

![](misc/Pasted%20image%2020240501235114.png)


![](misc/Pasted%20image%2020240501235519.png)

#### Essential matrix

relates image plane in camera 1 to image plane in camera 2.

![](misc/Pasted%20image%2020240502001116.png)

we can use to calculate epipolar lines for a point: $Ey=l'$ for $l'$ as the epipolar line in image 2.

If we want to map a point in one image point to another plane we can use the **homography matrix**.

![](misc/Pasted%20image%2020240502003112.png)


(bruh the math notation is so fucked up and diagram is wrong in the lecture slides)

looking at our geometry:

![](misc/Pasted%20image%2020240502010013.png)

https://homepages.inf.ed.ac.uk/rbf/CVonline/LOCAL_COPIES/EPSRC_SSAZ/node21.html

also note:

- $t\times t=\vec{0}$ - cross product of a vector with itself is the zero vector
- $R$ is rotation matrix, rotates axis of $O$ into alignment with $O'$
- T,X,X' are all coplanar

We have:

$X'=RX+T$

$X'\cdot(RX\times T)$ = 0 - $(RX\times T)$ produces a vector perpendicular to the plane, therefore then the dot product is 0.

- the matrix representation of $A\times B$ is $[A]_{\times}B$ 

![](misc/Pasted%20image%2020240502010511.png)

- The dot product of two vectors can be written in matrix form as $a \cdot b=a^T b$ 

now let the essential matrix be $E=[T]_{\times}R$
now we can prove that the essential matrix is

$$X'\cdot(RX\times T) = X'^\top[T]_\times RX = X'^\top EX$$


#### Disparity map

for a parallel camera setup we have the special case where we just add a value to x to get to the second image plane - as the epipolar lines are horizontal.

![](misc/Pasted%20image%2020240502011359.png)

### Rectification

For an angled stereo camera setup, we basically project the image planes so we get the above special case i.e. appears as if the cameras are in a parallel setup. Makes easier as now our epipolar lines are horizontal and parallel, thus feature matching easier.

![](misc/Pasted%20image%2020240502011832.png)

![](misc/Pasted%20image%2020240502011839.png)

![](misc/Pasted%20image%2020240502011847.png)

We have a homography matrix for each image plane. To calculate this we need data on the camera setup.
https://en.wikipedia.org/wiki/Image_rectification#Algorithms

## Correspondence problem

currently with epipolar geometry there is still a wide range of possible points.

![](misc/Pasted%20image%2020240502012203.png)

however we can have some soft constraints - not mathematically forced but can be reasonably assumed.

![](misc/Pasted%20image%2020240502012231.png)

but soft constraints can still be broken: for example ordering might be broken if we have two objects in same x and y but different depths:

![](misc/Pasted%20image%2020240502012528.png)

#### Dense correspondence search

![](misc/Pasted%20image%2020240502012909.png)

![](misc/Pasted%20image%2020240502012920.png)

(window is for pixel descriptor)

![](misc/Pasted%20image%2020240502012934.png)

#### Sparse correspondence truth

using only local features e.g. corners

![](misc/Pasted%20image%2020240502013034.png)

sparse usually performs better, but we have far less data to work with. an issue if we have a lot of occlusion, or also if we pick our features badly (or hard to get descriptors depending on the object).

![](misc/Pasted%20image%2020240502013231.png)

dense is usually very noisy, but get more data and can use for 3d surface reconstruction.

#### orderin

![](misc/Pasted%20image%2020240502013302.png)

![](misc/Pasted%20image%2020240502013319.png)

### applications

![](misc/Pasted%20image%2020240502013410.png)

![](misc/Pasted%20image%2020240502013418.png)

## Camera Parameters


![](misc/Pasted%20image%2020240502013549.png)

pixel dimensions is just the size of a pixel in real units e.g. millimetres

![](misc/Pasted%20image%2020240502013611.png)

we can also calculate these parameters if we have a reference image - this is a form of *rectification*.

![](misc/Pasted%20image%2020240502013658.png)

(idk how tho - check these out:
https://temugeb.github.io/opencv/python/2021/02/02/stereo-camera-calibration-and-triangulation.html
https://docs.opencv.org/4.x/dc/dbb/tutorial_py_calibration.html
)

### Uncalibrated views

if we dont have camera parameters and a calibration image then we can use an estimate or *relative depth* calculation.  $\text{depth} \propto \frac{1}{\text{disparity}}$ 


## Image stitching


need to do two things to stitch images:

- apply appropriate 2D transform to match them spatially i.e. alignment
- adjust image intensity to match if different lighting conditions (not covered...)

![](misc/Pasted%20image%2020240502014748.png)

so minimise the error when matching local features.

we have the following transforms to do alignment:

![](misc/Pasted%20image%2020240502014843.png)

or translation, rotation, scaling, shear, projection, warp/distortion, e.g. :

![](misc/Pasted%20image%2020240502015011.png)

our feature matching algorithms i.e. sift can handle scale, rotation (and probably translation) easily.

alright, i'm skipping all the shit about different transformations, the lecture basically just reiterates what they do.
more importantly,

a **homography** - a general mapping of one image plane to another. this only does affine transformations. Translation, scaling, rotation, and skewing are all classified as affine transforms. we use a homography matrix to transform and stitch our images - but this wont work for warped images (not affine).


generally we have the homography or sum of all these transforms as this matrix:

![](misc/Pasted%20image%2020240502015552.png)

the extra w component is for Homogeneous Coordinates from projection transforms. this is 1 for our points. how do we find this homography matrix?

![](misc/Pasted%20image%2020240502015748.png)

you may think about solving a system of linear equations with e.g. gaussian elimination, LU decomposition...
however there probably **wont be a perfect solution!** i.e. cant find a perfect projection to match the two images exactly. and those algorithms will fail.

we can also rearrange this as

![](misc/Pasted%20image%2020240502020545.png)

we minimise $||A\hat{h}||^2$ i.e. to get the least amount of residuals

after expanding this its in the form of an Eigendecomposition (hello darkness my old friend) problem .
see https://en.wikipedia.org/wiki/Linear_least_squares (idk why its eigendecomposition)

how do we do this for multiple sets of paired features? **direct linear transform**?.
https://en.wikipedia.org/wiki/Direct_linear_transformation


### RANSAC
[paper](https://dl.acm.org/doi/10.1145/358669.358692)
so far to solve the homography matrix we need to match features correctly. However wont happen for all features - will be mismatches.


![](misc/Pasted%20image%2020240502021553.png)

![](misc/Pasted%20image%2020240502024632.png)

![](misc/Pasted%20image%2020240502024024.png)

there is variation between the definition of the algorithm...

we basically iteratively train our model, and iteratively update our set of inliers, only keeping the model with the lowest error, and with enough inliers to say the model fits the data.

here's the wiki version:

![](misc/Pasted%20image%2020240502030414.png)


### Combining images via warping

now we computed the homography matrix. we need to transform an image to match another. There is an issue:

![](misc/Pasted%20image%2020240502024925.png)

pixel 'lands' inbetween pixels. we can use 'splatting' or alpha blending for this but can still have holes - so we use the reverse of this:

![](misc/Pasted%20image%2020240502025004.png)

now we wont have holes. Use e.g. bilinear interpolation to blend surrounding pixels.


images may need edge blending:

![](misc/Pasted%20image%2020240502025209.png)

![](misc/Pasted%20image%2020240502025217.png)

![](misc/Pasted%20image%2020240502025228.png)

![](misc/Pasted%20image%2020240502025239.png)

![](misc/Pasted%20image%2020240502025254.png)

bruh this book aint on the reading list
[3D Slicer software](https://www.slicer.org/)
- Volume rendering is concerned with producing models or renders for objects that doesn't have a defined surface (or this has been lost in the data).
- "Volume rendering is a set of techniques used to display a 2D projection of a 3D discretely sampled data set, typically a 3D scalar field."

A CT scanner performs volume rendering by producing images of a set of points/**voxels** in a 2D plane/slice through the patients body, which are then stacked to produce a 3D model.

CT scan of a human brain from the base to top of the skull:

![](misc/Pasted%20image%2020231205201912.png)


A CT scanner produces radiodensity values for each point in the 2D image. The **Hounsfield scale** transforms these radiodensity values onto the Hounsfield unit (HU) scale; Different types of body tissue then correspond to different ranges on this.

![](misc/Pasted%20image%2020231205202403.png)

We can then classify the voxels in our image to different materials; this allows us to assign density values (based off real material properties), & an arbitrary colour which we can use in *additive projection*.

But why could we not just use the radiodensity values directly as the voxel density? - For a medical image we would want to perform classification into materials, and then depending on what we want to view, e.g. we want to see bone structure only, we would make the bone material opaque and the rest fully transparent.

Our voxel data may be distributed like this:

![](misc/Pasted%20image%2020231205212354.png)

Our materials may fit voxel values with a degree of overlap, but this is reflected in our colour/opacity mappings.

![](misc/Pasted%20image%2020231205212457.png)

We could map to values of colour and transparency thus:

![](misc/Pasted%20image%2020231205212513.png)

![](misc/Pasted%20image%2020231205212524.png)

### Additive Projection
aka. projection of our 3D model onto a 2D view plane.
[Useful resource](https://web.cs.wpi.edu/~matt/courses/cs563/talks/powwie/p1/ray-cast.htm).

![](misc/Pasted%20image%2020231205211232.png)

We perform ray casting; as the ray proceeds through the 3D model, as it passes through a voxel it will accumulate it's colour and opacity value; We stop marching the pixel once the opacity budget for our ray is consumed. This is typically paired with using trilinear interpolation for each voxel for a smoother image.
(It is more complicated than this, see above resource).

![](misc/Pasted%20image%2020231205210412.png)

An X-ray will produce an image formed via additive projection in the real world; in this case we use actual x-rays for our ray tracing.


### Creating surfaces from volume data

We can create fake surface normals for a voxels given the value change for surrounding voxels; which can then be used in a standard rendering lighting model.

![](misc/Pasted%20image%2020231205212756.png)

Will produce images like these:

![](misc/Pasted%20image%2020231205211608.png)


## Indirect Volume Rendering

### Isosurfaces

An *isoline* is a line drawn on a 2D plane of values that always sits at a certain value interpolated from surrounding points; you will see them on terrain maps:

![](misc/Pasted%20image%2020231206015138.png)

Where we generate isolines at specific heights.

![](misc/Pasted%20image%2020231206015402.png)

An isosurface is this but in 3D. However actually creating an isosurface is much more complex.

#### Marching Cubes algorithm
See this [video](https://www.youtube.com/watch?v=M3iI2l0ltbE).
Assuming our data points are consistently arranged in a grid like manner; We can reduce the problem to a cube of 8 points; and we have a limited set of possible triangles we can draw to separate the data points into two classes. Although there are $2^8=256$ different possible combinations of point corner values we can reduce this to a set of 14, with all other cases being reflections/rotations of these.

![](misc/Pasted%20image%2020231206015851.png)

WE then march a cube along the 3 axes to evaluate the triangles to draw in each cube location to generate the final mesh.

![](misc/Pasted%20image%2020231206020053.png)

Typically we will also use interpolation to draw triangle vertices closer to the point whose value is closer to the threshold.

#### Gaussian Splatting
Each voxel is projected onto the viewing plane & plotted in a back-to-front order onto the pixels of the view buffer (front voxels are splatted over back ones). The voxels are rendered as disks whose properties of colour & transparency decrease in a gaussian manner from the center.
It has also been used to produce [very realistic renders](https://repo-sam.inria.fr/fungraph/3d-gaussian-splatting/)
### Proxy Geometry

We use multiple 'panes' layered upon each other upon which we paint our voxels. Note for e.g. CT scans our data will already be recorded in 2D slices.

![](misc/Pasted%20image%2020231206020458.png)

Typically we want to be looking directly onto the pane faces; if we look along the pane edges then we will see nothing.

Once voxels have been assigned to a pane (and pane position) they need to be painted:


- **Shear Warp**: (TODO is this accurate) The viewing transformation of a rotation is transformed into a shear warp operation; The planes are sheared with the amount of distance calculated from the principal axis of the camera, then rendered onto an image buffer that corresponds to the pane closest to the camera; this is then warped into the camera orientation.

It is fast but has a high memory overhead.

(see these [notes](https://www3.cs.stonybrook.edu/~qin/courses/visualization/visualization-shear-warping.pdf).)

![](misc/Pasted%20image%2020231206021642.png)

- **Texture-based volume rendering**: Basically utilising a GPU to have our panes in 3D space and render them to the viewing plane/image buffer.
"The voxels are rendered onto the proxy geometry/slice panes in 3D using a graphics card; These slices can either be aligned with the volume and rendered at an angle to the viewer, or aligned with the viewing plane and sampled from unaligned slices through the volume. Graphics hardware support for 3D textures is needed for the second technique." (A 3D texture is just a 3D matrix on the GPU, it is trivial to load the dataset of 3D points.)
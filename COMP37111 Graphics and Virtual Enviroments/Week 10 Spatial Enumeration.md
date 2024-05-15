Spatial enumeration involves splitting up our 2D/3D space into elements, where each element indicates which objects exist in it's spatial region; spatial enumeration is used to be more performant (compared to just naively iterating over all objects) for testing collisions between moving objects, or sampling colours when performing ray casting - it allows fast indexing to find what objects exist at a certain position.

Spatial enumeration comes at the cost of being memory intensive, and requiring cleaning up of pointers from spatial elements if an object is moved or deleted.

We will list commonly used **space partitioning** [data structures](https://en.wikipedia.org/wiki/Space_partitioning#In_computer_graphics).

# Regular Spatial Enumeration Techniques

## Grid Cell
We uniformly split our space into squares/cubes (in a **grid**); This is the native data structure for voxels and volume rendering models;

![](misc/Pasted%20image%2020231206191522.png)

Grid cells have  fast lookup but waste a lot of memory & lots of pointers to the same object;

![](misc/Pasted%20image%2020231206191743.png)

We can reduce the cell size for less memory usage but lookups will be slower.

## Quadtrees & Octrees

Similar to grid cell, but we can more efficiently divide up our space depending on where objects are located; WE split each axis in half for 4 cells in 2D (quadtree) or 8 cells in 3D (octree).

![](misc/Pasted%20image%2020231206191957.png)

To formulate a tree we keep dividing cells (or add more nodes to the tree) that contain objects over the maximum capacity - e.g. a maximum of two object (pointers) are allowed in tree cell.

# Irregular spatial enumeration techniques

- Spatial cohesion: the idea that objects in space will be close to each other, or will contain other objects, & there tends to be a lot of space between groups of objects

## Bounding Volume Hierarchies

We Contain our objects hierarchically in the bounding volume of our choice 

![](misc/Pasted%20image%2020231206201306.png)

(typically every object will have its own bounding volume).

### Constructing BVH's

**For a static scene:**
I.e. we know all of our objects & positions in advance.
These methods give us a hierarchal tree; we can then use this to construct our bounding volumes.

- Top-down: Take our set of objects and split into two (or more) subsets, repeat until we have sets containing only one object each.
- Down-up: Start with each object as it's own set/leaf node and merge them up to the top of the tree.

For a dynamic scene where we add objects, we make the tree grow as little as possible according to a cost metric (?TODO elaboration needed).
Object removal simply destroys it's bounding volume.

If we have a scene where the scene is very dynamic, e.g. lots of objects being created/deleted, or moving around a lot, an octree may be more efficient (at updating its tree).

### Choice of bounding volume

In order of computational cost:

- Circle/Sphere: Is easiest computationally to check if a point lies inside it - distance between point and centre point < radius. However will contain a lot of wasted/empty space meaning the chance of a false positive (in circle but not in object) is higher. Constructed by calculating the centre point and finding radius as the max distance between all points and the centre.
- Axis aligned bounding boxes (AABB): Smallest bounding box, but the faces and edges are aligned with the xy(z) axes. The box is defined as a range in each axis; checking for a point inside the volume requires only checking the position is within all 2/3(D) of the ranges. Globally if we want to check all objects against each other for intersection we can have a sorted list of points in each axis and check for overlapping ranges. We use the minimum/maximum xyz values of the set of points to create the AABB. **AABB is commonly used**.
- Minimum Bounding Box: The minimum box (rectangle or cuboid) in any orientation that can contain the object. Intersection is required against each of the 6 faces (more expensive). This is constructed by using algorithms such as [rotating calipers](https://en.wikipedia.org/wiki/Rotating_calipers) (TODO elaboration needed). Both searching the hierarchy & constructing the bounding volume are expensive (& do not save much space over AABB).

### Binary Space partitioning / K-d Trees

![](misc/Pasted%20image%2020231206210000.png)

We split our space into two smaller spaces, but the position of the division along the axis is arbitrary; It maintains that children nodes competely cover the area of their parent.

BSP trees may be *axis aligned*, i.e. our splits are performed along axes; these are also known as *K-d trees* (in k dimensions). These are used in nearest neighbour search.

![](misc/Pasted%20image%2020231206224806.png)

Or *polygon aligned* (BSP) - At each cell we may create the partition line along any arbitrary axis - usually in the same plane as a polygon. This is used in the *painter's algorithm* or again for collision detections.

#### Painter's algorithm with BSP trees
This paints per polygon (compared to per-pixel). (Note we usually use per-pixel/fragment depth testing with a z-buffer, as in OpenGL).

Generally the algorithm paints polygons from the furthest first & then paints closer polygons on top of this.
To be able to have a sorted list of polygons in the first place we build a BSP tree from all the polygons in the scene thus:


(The example is in 2D but you can obvs. scale it to 3D with polygons)

Given our polygons/lines we want to paint:

![](misc/Pasted%20image%2020231206235203.png)

1. Pick any line. We will perform the binary partition of space along this. The two partitions represent anything behind or in front of the chosen line.
For All other line in the scene we have 2 cases:
	1. Line is in front or behind the partition. Place it in the corresponding partition.
	2. Line lies across the divide. Divide the line into two halves and place those halves in the correct partitions.

You can see line B is split into two halves.
(Arrow represents side facing front.)

![](misc/Pasted%20image%2020231206235331.png)

We now recursively do this on our partitions:

E.g. for the right subtree

![](misc/Pasted%20image%2020231206235507.png)

The partition line is misleading; the right subtree is still bounding by the axis extended from A; so the subtree containing D2 represents the area bounded by B2 & A.

Our final tree may look like this:

![](misc/Pasted%20image%2020231206235645.png)

Now to render:
Basically a depth first search that renders the furthest polygons first.
[Stolen from wikipedia](https://en.wikipedia.org/wiki/Binary_space_partitioning#Application)

1. If the current node is a leaf node, render the polygons at the current node.
2. Otherwise, if the viewing location _V_ is in front of the current node:
    1. Render the child BSP tree containing polygons behind the current node
    2. Render the polygons at the current node
    3. Render the child BSP tree containing polygons in front of the current node
3. Otherwise, if the viewing location _V_ is behind the current node:
    1. Render the child BSP tree containing polygons in front of the current node
    2. Render the polygons at the current node
    3. Render the child BSP tree containing polygons behind the current node
4. Otherwise, the viewing location _V_ must be exactly on the plane associated with the current node. Then:
    1. Render the child BSP tree containing polygons in front of the current node
    2. Render the child BSP tree containing polygons behind the current node

## Culling
In any 3D scene there is a lot of stuff we wont see based on the camera viewpoint; there is not point rendering this stuff as it wastes processing time.

#### Backface Culling
If a polygon is facing away from the viewer, we wont see it & don't want to render it. This **only applies to** polygons that we would consider to be one sided e.g. object meshes, typically ones with surface normals (which is used in the culling algorithm).]

We can also use projected winding - if all front facing polygons vertices are given in a consistent direction e.g. clockwise in the model data, we know if say the vertex winding is anticlockwise once projected into viewing space that it is back-facing. 

If say we have a 2D image in 3D space, it presumably wont have a surface normal, & we might want to rotate it and view it flipped.

### Frustum Culling / Clipping
If polygons extend past the field of view we don't need to render the parts outside of this that we wont see; we perform *clipping* to 'cut off' the parts of the polygon we wont see.

![](misc/Pasted%20image%2020231207001413.png)

### Occlusion culling

The idea is to remove polygons/objects from the rendering pipeline early on that wont be seen; With either the Z-buffer or our depth-based BSP tree, we still depth test all objects on whether to draw them or not - this doesn't really save performance on the GPU. Occlusion culling saves GPU rendering time, however is CPU intensive as we are basically doing depth testing on the CPU instead to see what is occluded.

We could
- I possible precompute which objects are occluded, from certain viewpoints (e.g. if the player walks a set route), known as *possible viewable sets (PVS)* .

This [paper](https://dl.acm.org/doi/pdf/10.1145/199404.199422) sets out one method for doing it dynamically:

![](misc/Pasted%20image%2020231207002042.png)

See also https://www.gamedeveloper.com/programming/occlusion-culling-algorithms.
Or in godot: https://docs.godotengine.org/en/stable/tutorials/3d/occlusion_culling.html
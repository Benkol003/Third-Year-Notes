# 3D matrix transforms
## Computing camera axii
to do this calculate the cameras axis from the cross product of a general up vector $u$ (0,1,0), the direction vector $d$ i.e. where the camera is looking at.
- The up vector doesn't have to be perpendicular to the direction axis - what matters is its orientation around the direction axis

using cross products:
- camera right axis (local x axis)$r$ = $u\times d$ 
- camera local y axis = $r\times d$

## Transform matrices

given an object model in local space - cantered around origin, scale 1 - We have Model - View - Projection transform matrices.

Move object to world space - transform into camera space (rotation, scale, translation) - apply type of camera projection.

#### Interaction of rotation, translation, scaling

Invariancy:
- rotation is invariant to later scaling
- scaling is invariant to later translation

For moving a model from local space to world space, you wanna do translate X rotation X scaling.

- To iteratively update a model in world space you need to use quaternions, otherwise you will get [gimbal lock](https://en.wikipedia.org/wiki/Gimbal_lock). You may / not use this for camera rotation aswell as gimbal lock is intuitive for a camera as in fps games.

Cameras don't have scaling, instead a projection matrix.

Camera transform is projection X translation X rotation.

Scaling, which is typically applied to a local space model, should typically be applied before rotation - in the case where theres different scaling on different axis, applying the rotation afterwards also rotates the scaling axes and will stretch along the wrong axis.

### Camera Transforms / caveats
There is a caveat for updating camera transforms - you want to do them with the local axis reference, not the xyz axis. apply the current orientation to translates/rotations you want to apply. to rotate xyz axis to local axis.

To apply the camera transformation we apply the inverse camera matrix/ inverse transform to get to camera.
We can avoid directly computing the inverse matrix:
- Apply reverse rotations and transforms  i.e. -angle,-direction
- apply in order *translation X rotation* to world models
- Camera zoom e.g. 8X is achieved by dividing the FOV. (you could also do this via scaling)

## Surface normals
- used in lighting calculations
Calculated using the cross product rule:

![](misc/Pasted%20image%2020240402185432.png)

How do we know if the calculated normal points up or down?
From the above diagram we calculate the up normal vector when $\vec{b}$ is anticlockwise to $\vec{a}$.
If your vertexes have (& should!) consistent anticlockwise winding for front facing, for vertexes in order $v_!,v_2,v_3$,
$$N = \vec{v_1v_2} \times \vec{v_1v_3}$$

#### Caveats of non-uniform scaling

For a non-uniform scale, e.g. we stretch the x-axis, the gradient of edges changes (decreases) while the normal vector direction changes in the opposite scale (increases).

- Normal vector is invariant to translation, so don't take into consideration
- For a rotation matrix $M$, $M^{-1}==M^T$
- for a scaling matrix $M, M=M^T$
So we apply the inverse scale to the normal vector, $M^{-1}$, and then the transpose $M^{-1T}$ so that the rotation is invariant (by inverting it twice - $R=R^{-1}R^{-1}=R^{-1}R^T$).

- This approach is only useful when dynamically scaling at runtime - calculating the inverse of a matrix, & applying it every frame is expensive, its better to recompute normals for a mesh if you only scale infrequently.


#### Mesh non-uniform scaled copies
is it better to have different meshes under different non-uniform scales, or treat the non-uniform scale as instance data? e.g. stretching cylinder height or cube axii into a cuboid.
IMO just copy your meshes.

Per instance scaling:
- less data in VBO
- possibly less draw calls - though does this effect performance? especially with glMultiDraw

- The non-uniform scaling doesnt change per-instance - the stretched mesh can be used for multiple objects. SO how to render this? 
	- gl_DrawID - only in gl>4.6, also have to draw meshes in specific order.
	- Multiple draw calls and change a uniform inbetween calls 

Mesh copying
- More data in VBO
- More draw calls - different draw call for each type of mesh (if not using glMultiDraw)
- easier to implement

If you're scaling meshes its only likely to be with simple meshes e.g. cylinders, cubes e.t.c. 
copying meshes wont take up much more data, textures will take most of this up.

# Procedural mesh generation

## Regular polyhedra & icospheres
An icosahedron is a polygon of 12 equilateral triangle faces.

![](misc/Pasted%20image%2020240327181904.png)

from [wolfram](https://mathworld.wolfram.com/RegularIcosahedron.html);
An icosahedron can be constructed by placing a top and bottom point e.g. (0,+-1,0); then constructing the remaining verticies by:
1. Placing two circles at heights $\pm\frac{\sqrt{5}}{5}$ 
2. Construct 5 equally spaced vertices ($72\textdegree$) on each circle - the points on each circle differ by phase $36\textdegree$.

![](misc/Pasted%20image%2020240327182025.png)

#### Icosphere

Each vertex on the icosahedron already lies on the surface of a sphere around the centre, with radius 1.

An icosphere is produced by subdividing these faces and lifting the new triangles to sphere surface / triangle centre to object centre equals radius.

![](misc/Pasted%20image%2020240327182621.png)

We can generate triangles all of the same size for a consistent surface and minimal triangle count.

![](misc/Pasted%20image%2020240327183015.png)

This is actually part of the family of [geodesic polyhedrons](https://en.wikipedia.org/wiki/Geodesic_polyhedron); we can also get a sphere from subdivision of a regular tetrahedron/pyramid (but requires more subdivision) - this is easier to generate.

# OpenGL
[john carmack,openGL optimisation for quake 3]https://www.gamers.org/dEngine/quake3/johnc_glopt.html

## Vertex Array Objects

A VAO is just a specification on how to send data as input arguements to your shader program.


A VAO stores:
 - A set of bound buffers
 - Access patterns set via `glVertexAttribPointer` to those buffers to map data to you input variables in you shader program,. e.g. `layout (location = 0) in vec3 pos;`

A VAO is used when you have different vertex formats (& accompanying buffers) (may/may not be for different shader programs.)

### Buffer changes without restating vertex format
- fore core openGL, you can only bind your shader program and its attributes to a single VBO. the attribute bindings specified by `glVertexAttribFormat` also get destroyed on switching VBO.

If you have buffers in the same format, don't use different VAOs. Instead

(This is only available in >GL 4.3 - graphics cards from 2013 >GTX 7 series)
1. create your shader attributes, bound to intermediate *bind points* with `glVertexAttribBinding`
3. You can then swap out buffers without costly changes to the vertex format with `glBindVertexBuffer` to bind your buffer to the bind point.
- Put all your meshes into one VBO if in same format (VNC). Meshes can still differ in draw procedure e.g. instanced or indexed drawing. What matters is that we dont have to redefine glVertexAttribFormat.

https://registry.khronos.org/OpenGL/extensions/ARB/ARB_vertex_attrib_binding.txt

### Updating buffer data

- glBufferSubData can be used to update your VBO, either
	- update existing mesh data  - only need to do this for instance data
	- add more meshes for more objects. You will have to allocate a chunk of unused gpu memory at startup, or etc. create additional VBO memory chunks.

#### Persistent-mapped buffers
TODO

- Normally glBufferData is used to generate buffers. WE can resize when we want by calling again.
- glBufferStorage allocated *immutable* data storage - cant change size of buffer (you can destroy and create a new buffer though).

- GlMapBuffer allows you to map your buffer to cpu memory - is useful for more random data writes. With a mutable data storage, you have to call glUnmapBuffer before draw calls, causes worse performance than glSubBufferData?.

- However if buffer is immutablly allocated via glBufferStorage with correct storage bits (TODO) then don't need to unmap at draw call???

https://registry.khronos.org/OpenGL-Refpages/gl4/html/glBufferStorage.xhtml

### Other GL Primitives
Typically we use GL_TRIANGLES as its the most common format for our data, & most performant/efficient when we use indexed.

*adjacency primitives* Gl_TRIANGLE_STRIP or GL_TRIANGLE_FAN allows for less indexes/vertexes i.e. memory+bandwidth to be used, potentially better performance.

However these primitives take an arbritrary number of vertexes. To start a new primitive (at a new place) use *primitive restart*.

#### Primitive restart
(This isnt much use unless you really need triangle strips/fans)
https://www.khronos.org/opengl/wiki/Vertex_Rendering#Primitive_Restart
Basically have a magic value in our index buffer. When GL sees this is skips it, and starts a new primitive .e.g triangle strip, starting from the next index.
(Without this you would end up drawing a triangle (strip) across unrelated locations/strips, or make multiple draw calls.)

#### Indexed GL_TRIANGLE vs GL_TRIANGLE_STRIP/FAN
TLDR probably not worth it!
http://hacksoflife.blogspot.com/2010/01/to-strip-or-not-to-strip.html
They only save memory when we aren't using indexed triangles (which is never).


### Instanced rendering
https://www.khronos.org/opengl/wiki/Vertex_Rendering#Instancing
- Instance data e.g. position & rotation data goes into the VBO.
- use `glvertexAttribDivisor(attribute,1)` so that the attribute only advances once per the next instance
- **gl_Instance**$\in[0,\text{instanceCount}]$; can also access data through uniform buffer objects or textures & indexing e.g. `UBOname[gl_Instance]`.

#### Instance offsets & gl_Instance
You can access instance data at an offset (i.e. set of instance data for a new mesh) with `glDraw*BaseIndex` (see below). However **gl_Instance still starts from 0**. From GL 4.6 (2017) / ARB_shader_draw_parameters can get the offset in shader via **gl_BaseInstance**.

- Imo you should be using instanced arrays with base offsets. 

####  methods for storing & updating instance data
https://stackoverflow.com/questions/66490340/instanced-rendering-using-vertex-attribute-buffer-or-uniform-buffer-for-the-inst

- Vertex Buffer - limited by shader input attributes - 16 garunteed.
- UBO's are limited to 16kb size; SSBO's are usually unlimited in GPU size (even if spec is 128mb) however need new GPU for gl 4.6 or ARB_shader_draw_parameters support.

### OpenGL draw calls

- glDrawVertexArray - use to draw triangles (or other primitives) in sets of 3 verticies
- glDrawElements - draw using indexed verticies, stored in an EBO
- glDraw\*Instanced - instanced drawing

- glDraw\*BaseVertex - adds an offset value to your element indexes - this is useful when adding meshes to VBO; a loaded mesh will assume its vertexes start from $[0,n]$; however adding them into VBO infront of existing meshes requires an offset of $[v,n+v]$ for $v$ existing vertexes in the VBO. this draw call adds the value for us, instead of having to do it in a for loop cpu-side.
- glDraw\*baseInstance - similar to BaseVertex; you specify an instance offset / starting value for gl_Instance. Useful if instance data for vertexes is side-by-side (not as practical, still ideally need instance data for a mesh to be sequential).

#### Multi draw commands

- [`glMultiDrawElementsIndirect`](https://registry.khronos.org/OpenGL-Refpages/gl4/html/glMultiDrawElementsIndirect.xhtml) (GL 4.3), [`glDrawElementsInstancedBaseVertexBaseInstance`](https://registry.khronos.org/OpenGL-Refpages/gl4/html/glDrawElementsInstancedBaseVertexBaseInstance.xhtml) (GL 4.2), `glMultiDrawArrays`: These allow you to do your rendering with one draw call instead of a for loop for each unique mesh.
https://stackoverflow.com/a/71809842

### UBO's, SSBO'
TODO
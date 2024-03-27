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

This is actually part of the family of [geodesic polyhedrons](https://en.wikipedia.org/wiki/Geodesic_polyhedron); we can also get a sphere from subdivision of a regular tetrahedron/pyramid (but requires more subdivision) - this is easier to ghen




## Vertex Array Objects

A VAO is just a specification on how to send data as input arguements to your shader program.


A VAO stores:
 - A set of bound buffers
 - Access patterns set via `glVertexAttribPointer` to those buffers to map data to you input variables in you shader program,. e.g. `layout (location = 0) in vec3 pos;`

A VAO is used when you have different vertex formats (& accompanying buffers) (may/may not be for different shader programs.)

### Buffer changes without restating vertex format

If you have buffers in the same format, don't use different VAOs. Instead

(This is only available in GL 4.3 - graphics cards from 2013 >GTX 7 series)

1. specify the buffer format with `glVertexAttribFormat`
2. create intermediate *bind points* with `glVertexAttribBinding`
3. You can then swap out buffers without costly changes to the vertex format with `glBindVertexBuffer` to bind your buffer to the bind point.
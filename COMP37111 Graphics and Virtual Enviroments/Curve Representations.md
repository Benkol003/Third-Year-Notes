 **Definition of a curve**: A *continuous* set of points (a function generates those points) that are drawn in a 2D or 3D plane.

#### Polyline
Sequence of vertices joined via line segments. Any kind of curve will eventually be rendered as a polyline by the GPU. They are not used directly as they are not smooth or would space-intensive.

#### Spline
Defines a curve as a *piecewise polynomial function*: The curve is split up into intervals, and we use polynomials to define them. In graphics, we will divide a curve into points and the polynomials will be drawn to go through pairs of points.

**Knot**: Points where the curve passes through, and joins the piecewise polynomials.

**Control Points**: These do not lie directly on the curve but help control it's shape. Knots may also be *referred to as* control points.

![](misc/Pasted%20image%2020231006183155.png)

Here $P_0,P_3$ are knots, and $P_1,P_3$ are control points.


**Tessellation**: Breaks down our curve down in intervals to produce knots on the line.

Tessellated curve:

![](misc/Pasted%20image%2020231006182946.png)

More generally, tessellation is the process of breaking a complex geometrical object into simpler ones that can be joined together to form it as a whole. This is usually an approximation that causes a loss of accuracy in shape.

Sphere tessellated to different levels of accuracy:

![](misc/Pasted%20image%2020231006183037.png)


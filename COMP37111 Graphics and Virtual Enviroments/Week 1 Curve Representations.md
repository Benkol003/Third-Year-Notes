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

#### Interpolation vs. Approximation

An *interpolated* curve will pass through all control points.

An *approximated* curve will only pass through some control points; other control points control the *shape* of the curve, and follows a similar shape to the points.

![](misc/Pasted%20image%2020231012133719.png)

### Bezier Curves & Bernstein Polynomials

#### Cubic Bezier curve
(As a basic example)

An interpolated cubic curve that has 4 control points, passing through two of them.

![](misc/Pasted%20image%2020231012133914.png)

Cubic Bezier curves are bounded by the *convex hull* formed by it's control points:

![](misc/Pasted%20image%2020231012133956.png)

The curve has tangents that are formed by the first and last two control points - (p1,p2) & (p3,p4).

**Equation of cubic Bezier curve**: $$p(t) = (1-t)^3P_1 + 3t(1-t)^2P_2 + 3t^2(1-t)P_3 + t^3P_4$$
for parameter t of range $[0,1]$, which we use to move along the curve.

#### Bernstein Polynomials

- These are *interpolated* curves

Is a  linear sum of **Bernstein basis polynomials**. The co-efficient of each basis polynomial is known as Bezier/Bernstein coefficients, or Bezier control points.

- A *Bezier curve* and a *Bernstein polynomial* are effectively **the same thing**, just defined in different ways.

For a curve of degree n, we have n+1 basis polynomials defined:
$$b_{v,n}(t)=\binom{n}v x^v(1-t)^{n-v} , t \in [0,1]$$
($\dbinom{n}v$ - binomial coefficient.)

A Bezier curve uses it's *control points* as the *coefficients* for the sum of the Bernstein basis functions.




The Bezier curve/Bernstein polynomial is defined (for degree n):

$$ B_n(t) = \sum\limits^n_{v=0}\beta_vb_{v,n}(t) , t\in[0,1]$$

- $\beta_v$ - Bernstein coefficient, or Bezier control point (usually denoted $P_v$) 
- $b_{v,n}(t)$ - Bernstein basis polynomial, e.g. $b_{1,2}(t)=2t(1-t)$ 


Graphical representation of sum of basis polynomials for a cubic Bezier curve :
![](misc/Pasted%20image%2020231012150448.png)

Bernstein Polynomials have the following properties:

- The basis polynomials are always **non-negative** in value.
- **Unity sum**: The sum of all basis polynomials / area of Bernstein polynomial is **always 1** at any value of t.
- **Convex Hull Property**: The convex shape formed by connecting the control points will always contain the entire curve. 


#### Matrix notation of Bernstein Polynomials

P(t) = Geometry (control points) X spline basis X Power Basis (powers of parameter t)

Example: Matrix form for 3rd degree Bernstein polynomial

![](misc/Pasted%20image%2020231012175202.png)

Each row of the spline basis relates to the *coefficients* (but not the Bezier coefficient!) of the terms of a basis polynomial, once expanded **(monomial form)**.

Example:
$b_{0,3}(t)=\dbinom{3}0 t^0(1-t)^3 =1\cdot1\cdot(1-t)^3$ 
$\implies 1-3t+3t^2-t^3$
aka (1,-3,3,-1)

#### De Casteljau's Algorithm

See https://youtu.be/YATikPP2q70?t=214

Calculates a point on the Bezier curve for a given t.

For our given control points and t:
1. Connect the points in series to form a polygon
2. Place a point on each side of the polygon / lines between the control points such that it's divided in the ratio *t:1-t*.
3. Take these new points and go back to step 1. to form a polygon with one less side, until only have 1 point.
4. The final point obtained is the point on the Bezier curve.

![](misc/Pasted%20image%2020231012195923.png)

The curve will also be split in ratio t:1-t.

We can also use the new points calculated to split the curve into two separate curves; 
This diagram shows which control points are used for two new Bezier curves, circled in white.

![](misc/Pasted%20image%2020231012204523.png)

We use all the new points generated to the left/right side of the point, bar the middle ones, except for the point on the line.


### Curve Continuity

Mathematical definition of how smooth the connectivity of two curves is.

**C0 continuity**: The two curves have a common endpoint and appears as one solid line.

**C1 continuity**: At the join point, the first derivative (tangent) of both curves are equal (in direction and magnitude).

**G1 continuity**: At the joint point, the curves share a first derivative that is in the same direction, but *not necessarily* the same magnitude.

**C2 continuity**: Aswell as being C1 continuous the second derivatives (acceleration, centre of curvature) for the curves at the join are also equal.

![](misc/Pasted%20image%2020231012210752.png)

In general:

**$C_n$ Continuity** denotes that the curves share derivatives of orders 0-n, of equal value, at the join point.

**$G_n$ continuity** denotes that the curves have derivatives of orders 0-n that share the same **direction** at the join point; i.e.
the derivatives are multiples of each other:

for curves $P_1,P_2$, $$\forall i \in[0,n], \frac{d^iP_1}{dt^i} = A_i\frac{d^iP_2}{dt^i}$$
for some scalar multiple A, at the endpoints - i.e. t=0 for one and t=1 for the other curve.

### B-splines
(or basis splines)

Similar to Bezier curves; has control points that act as a convex hull for the line; however the curve *doesn't* pass through *any* of the control points.

A B-spline curve is composed of B-splines joined together at common control points known as **knots** - (the curve does not pass through these).

B-splines allow better *local control* of a curve, but not the overall shape.


**B-spline basis functions**: For splines of order *k*:

![](misc/Pasted%20image%2020231012212043.png)

**Converting between Bezier and B-spline**: Use their basis matrices.

Example: To convert from Bezier to B-spline: multiply control points by Bezier basis matrix, then by inverse of B-spline basis matrix.
(You can figure out vice versa.)

### Non-Uniform Rational Basis Splines (NURBS)

Adds two more components - *homogeneous component* & the *knot vector*, to allow for more local control.

Homogeneous component: (may know this as 'w' component in graphics) for adding weight to control points - allows fine control over shape of a curve towards a certain point.

knot vector: Changes the affect of control points on a curve, and defines which control points are active between knots.
(TODO more explanation)



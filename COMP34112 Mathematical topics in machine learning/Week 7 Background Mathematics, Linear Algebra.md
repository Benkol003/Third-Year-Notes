### Nabla operator

given a function of n variables is just the vector of first-order partial derivatives

![](misc/Pasted%20image%2020240504001254.png)


### Hessian matrix

the hessian matrix is a matrix of second order partial derivatives for a multivariate function.

![](misc/Pasted%20image%2020240503232759.png)

i.e. the hessian is symmetric because $$\frac{\partial^2f}{\partial x_i\partial x_j} = \frac{\partial^2f}{\partial x_j\partial x_i}$$   

The hessian is equal to $\nabla \nabla^\top$, or $\nabla \otimes \nabla$, and is sometimes denoted $\nabla^2$ - **dont use this!** - this is ambiguous with the *laplacian*, which uses dot product instead:

![](misc/Pasted%20image%2020240504001604.png)

the Laplacian just squares the gradient element wise. the Laplacian  = trace(hessian).

### Convexity

For a function to be convex: any two points on the graph with a line between them, the graph is always under this segment line.
also known as *strict convexity*.

why is convexity useful? *a strictly convex function can either have no global minima (e.g. $y=e^x$) or have only one global minima but not multiple.* - we can guarantee in ML training we don't have local minima to get stuck on!

also note that if $f(x),g(x)$ are convex then $f(x)+g(x)$ is convex.

![](misc/Pasted%20image%2020240409175133.png)

- $\theta$ interpolates between x & y

- We have **strict convexity** if the function never touches the line i.e. no equality:
$$\forall x,y \in \mathbb{R}, \forall\theta\in(0,1) | F(\theta x+(1-\theta)y)<\theta F(x)+(1-\theta)F(y) $$


Second definition of convexity:

![](misc/Pasted%20image%2020240409180323.png)

At any point the tangent to the function is always below the function.

![](misc/Pasted%20image%2020240409180404.png)

- Convex functions cannot have a maxima or inflection point

### Strict convexity

just when $f(y)\ge f(x)+\nabla f(x)^\top(y-x)$ (rather than $\ge$) - we can garuntee **always** have a global minima. 

### $\alpha$-strong convexity

this produces a lower bound on the functions curvature. i.e. curvature of at least $\alpha||x||^2$. e.g. $y=2x^2$ has 2-strong convexity.

![](misc/Pasted%20image%2020240503232235.png)


![](misc/Pasted%20image%2020240409183609.png)

assuming differentiability:

![](misc/Pasted%20image%2020240409183626.png)

i.e. where we have a flat part that would touch a line segment / cause equality with $L$ to happen, we have now added some $\frac{\alpha}{2}||x^2||$.

![](misc/Pasted%20image%2020240503232352.png)


### Hessian and convexity

a function is strongly convex if the hessian matrix is positive-definite.

#### symmetric positive-definiteness

the hessian matrix is symmetric; for a symmetric positive-definite matrix all of its eigenvalues are positive.

#### proof

the **multivariate form of the taylor expansion** for a twice differentiable function is:

$$f(x+h)=f(x)+\nabla f(x)^\top h+\frac{1}{2}h^\top H(x)h$$

($H(x)$ - hessian at point x)

convexity is defined as: $f(y)\ge f(x) + \nabla f(x)^\top (y-x)$ 

if $y=x+h$,

$f(x+h)\ge f(x) + \nabla f(x)^\top h$ 

substitute the taylor expansion on the L.H.S.:

$$f(x)+\nabla f(x)^\top h+\frac{1}{2}h^\top H(x)h\ge f(x) + \nabla f(x)^\top h$$

after some canceling we get:

$\frac{1}{2}h^\top H(x)h \rightarrow h^\top H(x)h\ge0$.  this is the [definition](https://en.wikipedia.org/wiki/Definite_matrix) of semi-positive-definiteness

- the eigenvalues of a semi-positive definite symmetric (like hessian) matrix are positive.

(real symmetric matrix has real orthonormal eigenvectors)

lets say $h$ is an eigenvector, and eigenvalue $\lambda$. the definition of eigenvectors is $H(x)h=\lambda h$. $h^\top H(x)h = \lambda h^\top h\ge 0$. $h^\top h$ is the dot product i.e. $\sum\limits_i^I h_ih_i=\sum\limits_i^I h_i^2$, therefore $h^\top h\ge 0$ (positive).
then $\lambda\ge 0$ for $\lambda h^\top h\ge 0$ to hold.

for the leap to $\alpha$-strong convexity, $f(x)-\frac{\alpha}{2}||h||^2$ is convex; second derivative of $\frac{\alpha}{2}||h||^2$ for taylor series (you will see for taylor $D$ is used to denote multivariate derivatives) - $DD\frac{\alpha}{2}||h||^2=D\alpha||h||=\alpha$. plug in to the above equation to get $h^\top H(x)h\ge\alpha$. similarly eigenvalues $\forall \lambda | \lambda\ge\alpha$.

![](misc/Pasted%20image%2020240504011457.png)


referenced resources - they are confusing lol

- https://math.stackexchange.com/questions/2083629/why-does-positive-semi-definiteness-imply-convexity
- https://math.stackexchange.com/questions/1353761/does-the-symbol-nabla2-has-the-same-meaning-in-laplace-equation-and-hessian
- https://leimao.github.io/blog/Hessian-Matrix-Convex-Functions/

### $\beta$-Lipschitz smoothness

i.e. the gradient is lipschitz

![](misc/Pasted%20image%2020240409193828.png)

or if differentiable, exists a maximum value $\beta$ for the second derivative of the function

![](misc/Pasted%20image%2020240409194023.png)

### Lipschitzness

![](misc/Pasted%20image%2020240409174521.png)

- note $||x-y||$ denotes a vector norm

A function is L-Lipschitz if for *all possible pairs of values* on a graph $(x_1,f(x_1)),(x_2,f(x_2))$ The **absolute gradient** of the line (i.e. for all possible lines touching two points on the graph) between these is no greater than $L$ i.e. $|f(x_1)-f(x_2)|\le L|x_1-x_2|$. i.e. if differentiable a bounded range for the first derivative.

- $F(x)=x$ is 1-Lipschitz
- $F(x)=x^2$ is not Lipschitz - gradient always increases for greater values of $|x|$

![](misc/Pasted%20image%2020240409172128.png)


As for loss functions:

- squared loss is not Lipschitz
- Huber loss is lipschitz: defined as

![](misc/Pasted%20image%2020240409171417.png)

![](misc/Pasted%20image%2020240409172003.png)

has a max gradient of $\delta$ 7 therefore is 1-lipschitz.

### Smoothness

also called Lipschitz-smooth, but dont get them confused.

$||\nabla f(x)-\nabla f(y)||\le \beta ||x-y||$ - we use $\nabla f(x)$ instead of $f(x)$ here.

(difference is there is a limit on the absolute value of the second derivative)

![](misc/Pasted%20image%2020240504070932.png)

polynomial functions similar to $f(x)=Ax^n, n\ge3, |A|\gt 0$ i.e. from $Ax^3$ are not Lipschitz-smooth.  

![](misc/Pasted%20image%2020240504070948.png)


### Convergence

convergence is just an infinite series of values, that tends towards some limit value.

for example gradient descent here has tended towards some non-trivial local minima (and gotten stuck):

![](misc/Pasted%20image%2020240504074957.png)

![](misc/Pasted%20image%2020240504075107.png)

$||a||_2$ denotes the Euclidean norm, $=\sqrt{\sum\limits_n^N a^2}$




### Supremums and Infinums

- upper and lower bounds: basically its a $\ge/\le$ bound value that is in the bounded set.

![](misc/Pasted%20image%2020240504075410.png)

however we may have infinite sets, for example $y=\frac{1}{x}$. the lower bound is 0, however 0 is not in the range of y.

the difference for supremum/infimum is that its the closest value bounding the set, however it doesn't have to be in the bounded set.
we only have a maximum or minimum if its in the set.

![](misc/Pasted%20image%2020240504075451.png)


### Matrix spectral norms

https://youtu.be/G2RKg1pHApc

remember a norm just measure the size of something - its maps something to the set of of reals $\mathbb{R}$. The spectral norm is just the definition of norm formulated for matrices.

![](misc/Pasted%20image%2020240504190234.png)

intuitively the norm is given the the maximum amount of 'stretch' the matrix can perform on a vector.

given a unit circle of different vectors the operator stretches this. 

![](misc/Pasted%20image%2020240504190903.png)

The SVD can be thought of as decomposing the operator into a rotation, scaling and second rotation of this unit circle.

![](misc/Pasted%20image%2020240504191128.png)


intuitively from this the *spectral norm of an operator A*  is equal to the largest **singular value** of A. (note that SVD is still different from eigendecomposition, though very similar.) 

actual proof of this is the **Min-Max theorem** (dont confuse with minimax!). TODO go over the actual proof.
https://en.wikipedia.org/wiki/Singular_value
https://loss.math.gatech.edu/16FALLTEA/NOTES/minmax.pdf
https://math.stackexchange.com/questions/586663/why-does-the-spectral-norm-equal-the-largest-singular-value

![](misc/Pasted%20image%2020240504190535.png)

### Matrix rank


#### vector span

![](misc/Pasted%20image%2020240504192252.png)

i.e. the vector subspace covered by the arbitrary set of vectors called the **spanning set** and all of their possible linear combinations.

#### vector basis

(also see Quantum Computing [Week 1 & 2 linear algebra](../COMP39112%20Quantum%20Computing/Week%201%20&%202%20linear%20algebra.md))

a set of vectors $B$ forms a basis in vector space $V$ if we can reach every vector in $V$ by a *single* linear combination of the basis vectors. The vectors also have to be linearly independent.

![](misc/Pasted%20image%2020240504192716.png)


- the spanning set of vectors $Span(\{v_1,\dots v_k\})$ is a basis of the span iff. the spanning set is linearly independent. If it isn't, then we can have multiple possible linear combinations of those spanning vectors to reach the same single vector in the span vector subspace - breaking the definition of a basis. 

#### dimension of a span

for **all** basis in a span, they have the same number of vectors (i.e. same number of free variables when forming a linear combination to any vector in the span),  -the **dimension** of the span.

The number of vectors in any basis of a span $V$ is always the same, called the **dimension** of V, or $\text{dim} V$.

![](misc/Pasted%20image%2020240504193821.png)

#### Image & Rank of a matrix

(hopefully you know that a linear function or operator can be expressed as a matrix, hence similar terminolgy)

given any vector $v\in \mathbb{R}^n, v=\{v_1,\dots v_n\}$ the **image** (or column space) of a matrix $A$ is the vector subspace of $A[v]$ - with this notation we just expand the general multiplication.

take a look at these examples

![](misc/Pasted%20image%2020240504194833.png)

the **rank** is just the dimensionality of the image of a matrix. a **full rank** matrix has the highest rank for a matrix of it's size (i.e. the operation doesn't discard any free variables). A full rank matrix also has linearly independent rows and columns (if we grab them out as vectors).

![](misc/Pasted%20image%2020240504194922.png)

#### matrix null space

the **null space** or **kernel** of a matrix is the set of all input vectors for that $Av=\vec{0}$.

![](misc/Pasted%20image%2020240504195944.png)


### Lipschitzness, Convexity and Smoothness

#### Lipschitzness

![](misc/Pasted%20image%2020240409174521.png)

- note $||x-y||$ denotes a vector norm

A function is L-Lipschitz if for all possible pairs of values on a graph $(x_1,f(x_1)),(x_2,f(x_2))$ The **absolute gradient** of the line (i.e. for all possible lines touching two points on the graph) between these is no greater than $L$ i.e. $|f(x_1)-f(x_2)|\le L|x_1-x_2|$. i.e. if differentiable a bounded range for the first derivative.

- $F(x)=x$ is 1-Lipschitz
- $F(x)=x^2$ is not Lipschitz - gradient always increases for greater values of $|x|$

![](misc/Pasted%20image%2020240409172128.png)


As for loss functions:

- squared loss is not Lipschitz
- Huber loss is lipschitz: defined as

![](misc/Pasted%20image%2020240409171417.png)

![](misc/Pasted%20image%2020240409172003.png)

has a max gradient of $\delta$ 7 therefore is $\delta$-lipschitz?

#### Convexity

For a function to be convex: any two points on the graph with a line between them, the graph is always under this segment line.

![](misc/Pasted%20image%2020240409175133.png)

- $\theta$ interpolates between x & y

- We have **strict convexity** if the function never touches the line i.e. no equality:
$$\forall x,y \in \mathbb{R}, \forall\theta\in(0,1) | F(\theta x+(1-\theta)y)<\theta F(x)+(1-\theta)F(y) $$


Second definition of convexity:

![](misc/Pasted%20image%2020240409180323.png)

At any point the tangent to the function is always below the function.

![](misc/Pasted%20image%2020240409180404.png)

- Convex functions cannot have a maxima or inflection point

#### $\alpha$-strong convexity

![](misc/Pasted%20image%2020240409183609.png)

assuming differentiability:

![](misc/Pasted%20image%2020240409183626.png)

i.e. where we have a flat part that would touch a line segment / cause equality with $L$ to happen, we have now added some $\frac{\alpha}{2}||x^2||$.

#### $\beta$-Lipschitz smoothness

i.e. the gradient is lipschitz

![](misc/Pasted%20image%2020240409193828.png)

or if differentiable, exists a maximum value $\beta$ for the second derivative of the function

![](misc/Pasted%20image%2020240409194023.png)


will need [Week 7 Background Mathematics, Linear Algebra](Week%207%20Background%20Mathematics,%20Linear%20Algebra.md)

we use a linear regression, i.e. $Y=X\beta$ - $\beta$ is a vector of weights.

$y_i=X_{i0}\beta_0+\dots X_{ij}\beta_j$    

and minimise the emperical risk as follows:

![](misc/Pasted%20image%2020240507135322.png)


These two assumptions are important for this weeks content:

![](misc/Pasted%20image%2020240507142443.png)

(X being full rank seems like a big assumption...)

### Moore-Penrose pseudoinverse

For the following claim:

![](misc/Pasted%20image%2020240507135743.png)

by itself $X$ isn't invertible because it needs to be a square matrix.

note the definition of the rank of $X$ means it is full rank.
note the norm $||x||^2=x^Tx$, therefore $||X^Tv||^2 = (X^Tv)^T(X^Tv)$. note for matrices $(AB)^T=(B^TA^T)$ and $X^{TT}=X$ therefore $(X^Tv)^T(X^Tv) = v^TXX^Tv$.

$X^T$ is also full rank as is $X$. 

due to the [rank nullity theorem](https://www.ucl.ac.uk/~ucahmto/0005_2021/Ch4.S16.html) - basically for a full rank matrix the null space of vectors given by $Xv=0$ happens only when $v=0$.
then if $v>0$, $v^TXX^Tv>0$, i.e. the null space/kernel of kernel($XX^T$)=0, which is by the definition of an inverse matrix. (why though? thats a whole seperate tangent https://math.stackexchange.com/questions/222240/proving-that-a-square-matrix-whose-kernel-is-0-is-invertible) 
therefore $XX^T$ has an inverse.

- The **psuedoinverse** of $X$ is $X^\dagger=X^T(XX^T)^{-1}$. note that $XX^\dagger=XX^T(XX^T)^{-1}=I$.

now lets put the linear regressor in the form $\beta=X^\dagger y$.


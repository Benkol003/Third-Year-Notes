## Transition Systems

I.e. a state diagram:

![](misc/Pasted%20image%2020240131164342.png)

- Nondeterministic TS: Given a state $s$ and an action $a$ we may transition to multiple states, e.g. $(s,a,s')$ & $(s,a,s'')$ are valid. However this is different to stochastic; e.g. there may be some hidden state that we are unaware of that affects the state transitioned to, or may adhere to a pattern to which of $s',s''$ is transitioned to.
- A Stochastic TS has a factor of randomness when choosing which of the multiple states to transition to. (stochastic also implies nondeterministic). Each transition has a associated probability $p(s,s')$; & $\sum_{s'}p(s,s')=1$.

### Complex Numbers
- Complex numbers denoted $z = a + bi$
- Complex conjugate: Given $z$, its complex conjugate $z^*=a-bi$. 
- $zz^*=|z|^2$

## Probability Amplitudes / Quantum TS

A Probability amplitude between two states is a complex number $\Psi(s,s') \in \mathbb{C}$, such that $\Psi(s,s')^2=p(s,s')$ - square of a probability amplitude gives a probability.
A Quantum TS employs probability amplitudes on the between-state edges; again probabilities sum to 1 - $\sum_{s'}|\Psi(s,s')|^2 = 1$.

### Destructive Interference

![](misc/Pasted%20image%2020240130144910.png)

Using the conjugate rule - convert to form $zz^*$, expand & simplify terms back to $|z|^2$.

Here's the working but using $a,b,c,d$ as terms instead:

![](misc/Pasted%20image%2020240130145647.png)

With our final answer as:

![](misc/Pasted%20image%2020240130145854.png)

The last two terms may not be positive - this would be *destructive interference*.

![](misc/Pasted%20image%2020240130151712.png)

The Probability is still $1/2$ in the quantum case but causes destructive interference that changes the probabilities.

## Vector Space

- A n-dimensional vector is an array of $n$ scalar elements or *field*, e.g. real ($\mathbb{R}$) or complex ($\mathbb{C}$) elements
- A *vector space* is akin to a number space like $\mathbb{N}$ - space of all possible vector values that can exist. 
- N-dimensional (e.g. real) vector space is denoted $\mathbb{R}^n$.

- Vector space maths obeys the following axioms:

![](misc/Pasted%20image%2020240131171158.png)

A you see yet we do not have an axiom to allow for multiplication:

## Inner Product Space

- An inner product operator for a vector space takes two vectors & returns a scalar - i.e. it is a generalisation :

![](misc/Pasted%20image%2020240131171421.png)

- Dot product is a inner product operator for real vector space - for n-dimensional real vectors $a,b \in \mathbb{R}^n$, $a\cdot b = \sum\limits_{i=1}^n a_ib_i$ 
- However for *complex vector space* $\mathbb{C}^n$ our inner product operator that is in the *Hermitian form* (such that it works with complex numbers): $\braket{x|y} = \sum\limits_{i=1}^n x_i^*y_i$. (We use the term Hermitian form to differentiate from normal inner product due to the use of the conjugate for obeying symmetry rules)

#### Complex Vector Spaces, Dirac Notation

*Bra-ket* or *Dirac* notation uses symbols $<,|,>$ to notate complex vector spaces: 
- A ket $\ket{a}$ denotes that $a \in \mathbb{C}^n$ for some n. 
- The norm of a vector $\ket{a}$ is as follows $\|a\|=\sqrt{\braket{a|a}}$
- A bra-ket $\braket{a|b}$ denotes the inner product of vectors $a$ & $b$. 

Bra's have multiple interpretations:
- as we know $\braket{a|b}$ is the inner product of two vectors - $\bra{a}$ represents the conjugate transpose of $\ket{a}$; i.e. $\bra{a}=[a_1*,a_2*,\dots]$ & $\ket{a}=\begin{bmatrix} a_1 \cr a_2 \cr \dots \end{bmatrix}$. This also performs our dot product of the the two vectors $\braket{a|b}$ as we treat it as matrix multiplication.
- $\bra{f}$ can also denote a function that is a map $f: \mathbb{C}^n \rightarrow \mathbb{C}$, also called a *linear form, linear functional, covector*.

- $\ket{a}\bra{b}$ denotes a *dyad*, which produces a matrix. 
## Complex Inner Product Space

We will denote this $H$. The normal vector space axioms apply (provided in dirac notation):

![](misc/Pasted%20image%2020240131173523.png)

Now with additional axioms for the behaviour of the inner product operator:

![](misc/Pasted%20image%2020240131173534.png)

[Inner product axioms](https://en.wikipedia.org/wiki/Inner_product_space#Definition)
[Definition of hermetian form](https://en.wikipedia.org/wiki/Sesquilinear_form#)


#### Example

- A function is said to be _integrable_ if its integral over its domain is finite

We will use the following $L_2(K)$ function again in the course.

![](misc/Pasted%20image%2020240202164620.png)


## Basis

![](misc/Pasted%20image%2020240202164818.png)

i.e. if we can find a set of non-zero co-efficients to produce the zero vector, then the collection of vectors is linearly dependent.

![](misc/Pasted%20image%2020240202170953.png)

i.e. we can use our basis to reach any vector in vector space $V$.

- A linearly independent finite collection of vectors that spans $V$ is a *basis* in $V$.
- Our coefficients $a_1,\dots a_n \in F$ are co-ordinates

#### Orthogonality
We have an orthogonal basis $B$ iff $\forall(\ket{x_i}\in B)$ $\forall (\ket{x_j}\in B)$ $\braket{x_i|x_j} =\delta_{ij}= \left\{ \begin{array}{} ||x_i|| & i=j \\ 0 & i\ne j \end{array}\right.$
- $\delta_{ij}$ is also known as the **Kronecker delta function**.
i.e. all combinations of two distinct basis vectors have a dot product of 0 i.e. are at 'right angles' to each other.

#### Orthonormality
We have an orthonormal basis $B$ iff it is  orthonormal & $\forall(\ket{x_i}\in B)\ \|x_i\|=1$. We can easily normalise an orthogonal basis by multiplying each basis vector by $\frac{1}{\|x_i\|}$.

#### Standard Basis
Where we have a 1 value in different dimensions for each basis vector:
![](misc/Pasted%20image%2020240202185657.png)

## Linear Operators

A linear operator maps between vector spaces / maps a vector to a vector

![](misc/Pasted%20image%2020240202173550.png)

Note that for the associative rule to work  $H_1$ & $H_2$ have to be vector spaces over the same scalar space i.e. we can't mix real & complex vector space; otherwise $\lambda,\mu$ would only exist in one of the scalar spaces and only for one side of the equation would scalar multiplication be valid.

Example Operators:

![](misc/Pasted%20image%2020240202174422.png)

- The identity operator is the same as the identity matrix

Note for $L_2(\mathbb{R}$):

Our operations on functions $L_2(\mathbb{R})$ may break the condition of it being integrable - integral may become infinite; (vice versa) may also no longer be differentiable:

For example 4:

![](misc/Pasted%20image%2020240202174739.png)

Example 5:

![](misc/Pasted%20image%2020240202174938.png)

## Pauli Matrices

Can also define operators as matrices:

![](misc/Pasted%20image%2020240202175805.png)

**Pauli Matrices: Require memorisation!**
Define the interaction of spin-1/2 particles (e.g. electrons) in the Pauli equation; also called *Pauli operators*: 

- $Q$ denotes $\mathbb{C}^2$ here and later on, as we will use this vector space a lot

![](misc/Pasted%20image%2020240202175159.png)

They follow these rules:

![](misc/Pasted%20image%2020240202175427.png)

- Adjoint operator: $A^\dagger = (A^T)^*$  
- Pauli matrices are *hermitian* & *unitary*.

(You can use this to help memorise the general repeated form of the rules)

![](misc/Pasted%20image%2020240202175347.png)

## Operator composition

![](misc/Pasted%20image%2020240202184424.png)

![](misc/Pasted%20image%2020240202184444.png)

By using taylor series we can compose a linear operator $A$ with another operator e.g. $e^x$:

The following taylor series all converge for $|x|<1$.

![](misc/Pasted%20image%2020240202184256.png)

![](misc/Pasted%20image%2020240202184459.png)

Using the Pauli matrix identities to simplify:

![](misc/Pasted%20image%2020240202184840.png)

## Matrix elements

![](misc/Pasted%20image%2020240202193331.png)

Don't manipulate the numbers inside a ket, they are labels. Later on we will denote qubits similarly e.g. $\ket{01}$.

Given a operator matrix $A$, then we can retrieve an element of $A$, written $(a_{jk})$: $\braket{j|A|k} = a_{jk}$

![](misc/Pasted%20image%2020240202194413.png)

$A$ can be written as

![](misc/Pasted%20image%2020240202193926.png)

$\ket{j}\bra{k}$ produces a matrix as we multiply a row vector by column - $n\times 1, 1\times n\rightarrow n\times n$

![](misc/Pasted%20image%2020240202194440.png)

- The identity operator is defined $I=\Sigma_k\ket{k}\bra{k}$
- Projection operator: Given a subspace $\Lambda$ spanned by basis $\{\ket{l}\}_{l=0\dots t}$, $P_\Lambda=\sum_l\ket{l}\bra{l}$  TODO what is this     

## Matrix/Operator properties

We previously defined the adjoint of an operator $A^\dagger = (A^T)^*$. All linear operators on a inner product space will always have an existing adjoint operator.

So it follows that $\braket{y|Ax}=\braket{A^\dagger y|x}$. i.e.  if for $A$ we have $\ket{j}\bra{k}$, $A^\dagger$ is $\ket{k}\bra{j}$.
- 
Then $(\lambda A + \mu B)^\dagger=\lambda^*A^\dagger+\mu^*B^\dagger$.

- An operator is **normal** iff $A^\dagger A=AA^\dagger$ & therefore $U^{-1}=U^\dagger$.
- A matrix is **symmetric** iff $A=A^T$.
- An operator is **unitary** iff $A^\dagger A=I=AA^\dagger$, & preserves the inner product.
- An operator is **hermitian** or **self-adjoint** iff $A=A^\dagger$. i.e. the operator is it's own complex conjugate - they form a real vector space. These operators serve as real-world observables in quantum mechanics e.g. position, energy, momentum.

Then for a hermitian operator A & real value $\lambda$, operator $e^{i\lambda A}$ is unitary.

![](misc/Pasted%20image%2020240202210607.png)

Conversely for a unitary operator $U$ there exists hermitian $A$, called *generator* of $U$, & real value $\lambda$ s.t. $U=e^{i\lambda A}$.

- Unitary operators are **invertible** i.e. $UU^{-1}=I=U^{-1}U$, with the inverse of $U$, $U^{-1}=U^\dagger$.
- $(U_1U_2)^{-1}=U_2^{-1}U_1^{-1}$. It's defined with the order reversed such that we can cancel to get the identity operator when substitute:

![](misc/Pasted%20image%2020240202212409.png)

## Diagonalisation

Given an operator $A$, a **nonzero** vector $\ket{v}$ is an **eigenvector** of $A$ with **eigenvalue** $\lambda$ iff $A\ket{v}=\lambda\ket{v}$. 

**Warning** - the notes use $v$ to denote eigenvalues but also $\ket{v}$ for eigenvectors which I think is confusing

Visual example of eigenvectors:

![](misc/Pasted%20image%2020240202214330.png)

This can be rewritten as $(A-\lambda I)\ket{v}=0$. (If v is nonzero then $det(A-\lambda I)=0$ (TODO why).)

#### Matrix Determinant
function of a *square* matrix to produce a scalar value; obeys rules:

- Determinant is non-zero if matrix is invertible

 ![](misc/Pasted%20image%2020240202223552.png)

and rules 2-4 also apply to columns

Can be calculated for small matrices thus:

![](misc/Pasted%20image%2020240202223725.png)

Calculating the determinant is computationally hard with [*Leibniz formula*](https://en.wikipedia.org/wiki/Leibniz_formula_for_determinants), we use a speedup called [*LU decomposition*](https://en.wikipedia.org/wiki/LU_decomposition). [notes](https://www.math.ucdavis.edu/~linear/old/notes11.pdf).

### Eigendecomposition
The *Characteristic Equation* is $det(A-\lambda I)=0$, we solve to find eigenvalues $\lambda$. We then solve $(A-\lambda I)\ket{v}=0$ to find the eigenvectors. We can then say 
$A=Q\Lambda Q^{-1}$, with
- $Q$ - matrix where i'th column corresponds to eigenvector $\ket{v_i}$.
- $\Lambda$ diagonal matrix with i'th diagonal value as eigenvalue $\lambda_i$.

#### Diagonalisation / SVD

Iff matrix A is a normal matrix, then the eigendecomposition of A can be written as $A=\sum_iv_i\ket{v_i}\bra{v_i}$, and the eigenvectors $\forall i$, $\ket{v_i}$ form an orthonormal basis across this space.

This is basically a restatement of [*Singular Value Decomposition*](https://en.wikipedia.org/wiki/Singular_value_decomposition) or *diagonalisation*; TODO (optional) cover the proof.

- For unitary $U$, also $\forall i$, $|v_i|=1$. ($v_i$ - eigenvalue)
- For hermitian $A$ also $\forall i$ $v_i$ all eigenvalues are real.
(Both unitary and hermitian imply normal)

Proof of the hermitian statement:

![](misc/Pasted%20image%2020240205200632.png)

In the proof we substitute $\frac{1}{2!}(i\lambda)^2(\sum_iv_i\ket{v_i}\bra{v_i})^2=(\sum_i\frac{1}{2!}(i\lambda v_i)^2\ket{v_i}\bra{v_i})$. We don't have squaring of the dyads due to the properties of our eigenvectors being orthonormal i.e. $\braket{v_1|v_1}=1$, $\braket{v_1|v_2}=0$.

![](misc/Pasted%20image%2020240205201750.png)

### Change of orthonormal basis

An operator we wish to use will usually have a different orthonormal basis we are working with; so lettuce change to our basis.
Given two orthonormal bases ${\ket{u_l}}_{l=0\dots n-1}$, ${\ket{v_i}}_{i=0\dots n-1}$. Each basis vector $\ket{v_i}$ can be represented in the other base as $\ket{v_i}=\sum_l\braket{u_l|v_i}\ket{u_l}=\sum_l\ket{u_l}\braket{u_l|v_i}$.

We can prove that a change of basis is a unitary operator;

![](misc/Pasted%20image%2020240213141201.png)

A change of basis operator also acts as a **rotation**.

- $\ket{x} \rightarrow U\ket{x}$ & $A \rightarrow UAU^\dagger$
- therefore we can produce the following identity $(A\ket{x})\rightarrow UAU^\dagger U\ket{x} = U(A\ket{x})$

#### Example with pauli matricies

![](misc/Pasted%20image%2020240213142558.png)

#### Hadamard Rotation

**Need to memorise the hadamard operator**

![](misc/Pasted%20image%2020240213142621.png)

![](misc/Pasted%20image%2020240213142632.png)


## Constructions on spaces

### Direct sum

Given two vector spaces $H_1, H_2$ the **direct sum** $H_1 \bigoplus H_2$ is the space whose vectors are pairs $(\ket{u},\ket{v})$. $H_1$ & $H_2$ can be seen as orthogonal components of $H_1 \bigoplus H_2$.

Linear operators on direct sum spaces:

![](misc/Pasted%20image%2020240213143048.png)

![](misc/Pasted%20image%2020240213143330.png)


#### Eigenvectors of direct sum

![](misc/Pasted%20image%2020240213143751.png)

### Tensor Product

The tensor product of two spaces $H_1, H_2$ is represented $H_1 \bigotimes H_2$.
May also be notated:

![](misc/Pasted%20image%2020240214000026.png)

Tensor products have special properties in relation to basis & eigenvectors:

- The basis vectors in $H_1 \bigotimes H_2$ are tensor products of all combinations of basis vectors from individual $H_1$ & $H_2$, denoted $\ket{u_i} \bigotimes \ket{v_k}$:

![](misc/Pasted%20image%2020240213154909.png)

Given two vectors expressed in general form of basis vectors we can express the tensor product thus:

![](misc/Pasted%20image%2020240213155001.png)


Tensor product linear operators:

![](misc/Pasted%20image%2020240213155042.png)

Note that a tensor product **is** different from a dyad or outer product. In a tensor product the numerical values of the resulting vector may be the same as for a outer product?, however they are in a different basis - the tensor product basis?

Inner product - note $\overline{v}$ & $\underline{v}$ denote two different vectors:

![](misc/Pasted%20image%2020240213161321.png)

The tensor product of two operators is defined:
$f: H_1 \rightarrow H_1, g: H_2 \rightarrow H_2$:
$(f\otimes g)(u\otimes w)=f(u)\otimes g(w)$, i.e.

![](misc/Pasted%20image%2020240213162044.png)


![](misc/Pasted%20image%2020240213162331.png)

We can also easily find the eigenvectors of the tensor product of two operators if we know them individually:

![](misc/Pasted%20image%2020240213162844.png)

Note:

![](misc/Pasted%20image%2020240214001618.png)

I think what he is trying to say here is that we can't necessarily say that the reverse statement of deconstructing a vector in tensor product space into a direct representation of components of $H_1$ & $H_2$; the vector is intrinsically linked to the tensor product space.

(again the maths has been skipped here and the explanation is vague).

Though sometimes is true e.g. for hadamard operator:

![](misc/Pasted%20image%2020240214002054.png)

#### Tensor powers and representation 

A tensor power of a space $Q$ is written $Q^{\otimes n}$, e.g. $Q \otimes Q \rightarrow Q^{\otimes 2}$.
(Remember we use $\mathbb{C}^2 \rightarrow Q$) 

Given a standard basis $\ket{0},\ket{1}$ we can write bases have tensor cube $\ket{00}, \ket{01}, \ket{10}$, ...


Applying the hadamard rotation to each basis vector:

![](misc/Pasted%20image%2020240214013645.png)

For an arbitrary rotation:

![](misc/Pasted%20image%2020240214013713.png)
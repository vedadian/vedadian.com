*NOTE*: This article has been translated from Farsi using `llama3-70b-8192` and `groq`

I stated in my previous post that when we consider derivatives with respect to matrices as derivatives with respect to vectorized matrices (using the same operator $\text{vec}$), we no longer need to deal with tensors, covectors and contravectors. Meanwhile, we need to bring the Kronecker product from that world to this world. The Kronecker product in tensor calculations creates a tensor with dimensions equal to the sum of the input tensors' dimensions. For example, the Kronecker product of two vectors results in a matrix.

If we have two matrices $A$ and $B$, their Kronecker product is defined as:

$$$
A\otimes B=\left[\begin{array}{cccc}
A_{11}B &A_{12}B &\cdots &A_{1m}B\\
A_{21}B &A_{22}B &\cdots &A_{2m}B\\
\vdots &\ddots &\vdots\\
A_{n1}B &A_{n2}B &\cdots &A_{nm}B
\end{array}\right]
$$$

For example:

$$$
\left[\begin{array}{cc}
3 &4\\
5 &6
\end{array}\right]
\otimes
\left[\begin{array}{cc}
1 &0\\
0 &1
\end{array}\right]
=
\left[\begin{array}{cccc}
3 &0 &4 &0\\
0 &3 &0 &4\\
5 &0 &6 &0\\
0 &5 &0 &6
\end{array}\right]
$$$

This product has a beautiful property that is essential for matrix derivative calculations:

$$$
\text{vec}(AXB)=(B^T\otimes A)\text{vec}(X)
$$$

For example, let's calculate the derivative of $AXB$ with respect to $X$:

$$$
\frac{\partial AXB}{\partial X}=\frac{\partial \text{vec}(AXB)}{\partial \text{vec}(X)}=\frac{\partial (B^T\otimes A)\text{vec}(X)}{\partial \text{vec}(X)}=(B^T\otimes A)
$$$

## Chain Rule and Product Rule
In the world of scalars, there was a set of general rule for derivatives that worked as a starting point for most of our calculations. These rules also apply to the world of matrices. One of these rules is the scalar product and another the sum rule, which I mentioned in the previous post.

Let's see why these rules hold:

$$$
\frac{\partial (\alpha A+\beta B)}{\partial X}_{ij}=\frac{\partial \text{vec}(\alpha A+\beta B)_i}{\partial \text{vec}(X)_j}=\frac{\partial \alpha\text{vec}(A)_i}{\partial \text{vec}(X)_j}+\frac{\partial \beta\text{vec}(B)_i}{\partial \text{vec}(X)_j}
$$$

And so on.

Similarly, we can calculate the derivative of composite functions:

$$$
\frac{\partial f(g(X))}{\partial X}_{ij}=\sum_k \frac{\partial \text{vec}(f(g(X)))_i}{\partial \text{vec}(g(X))_k}\frac{\partial \text{vec}(g(X))_k}{\partial \text{vec}(X)_j}
$$$

And in general:

$$$
\frac{\partial h(f(X),g(X))}{\partial X}_{ij}=\left(\frac{\partial h(f(X),g(X))}{\partial f(X)}\frac{\partial f(X)}{\partial X}+\frac{\partial h(f(X),g(X))}{\partial g(X)}\frac{\partial g(X)}{\partial X}\right)_{ij}
$$$

It seems that those who defined the matrix product rule have laid a solid foundation. Everything fits together nicely.

But what about the product rule for two functions?

$$$
\frac{\partial f(X)g(X)}{\partial X}=?
$$$

If you're tired of element-wise derivatives, you're rightfully so; I'm tired too. So, let's seek help from Kronecker. First, consider the function $h(X,Y)=XY$. The derivative of this function with respect to $X$ and $Y$, using Kronecker's relation, is:

$$$
\begin{aligned}
\text{vec}(XYI)=(I\otimes X)\text{vec}(Y)\Rightarrow\frac{\partial XY}{\partial Y}=(I\otimes X)\\
\text{vec}(IXY)=(Y^T\otimes I)\text{vec}(X)\Rightarrow\frac{\partial XY}{\partial X}=(Y^T\otimes I)
\end{aligned}
$$$

Now, the derivative of $h(f(X),g(X))=f(X)g(X)$ is:

$$$
\frac{\partial f(X)g(X)}{\partial X}=(I\otimes f(X))\frac{\partial g(X)}{\partial X}+(g(X)^T\otimes I)\frac{\partial f(X)}{\partial X}
$$$

Note that the identity matrices related to $f$ and $g$ should not be assumed to have the same dimensions. The identity matrix $I$ in the Kronecker product has dimensions equal to $g(X)$, and vice versa.

There's a small issue left to discuss, which is about the effect of transposition on the $\text{vec}$ operator. For example, when calculating the derivative of $A^T$ with respect to $A$:

$$$
\frac{\partial A^T}{\partial A}=\frac{\partial \text{vec}(A^T)}{\partial \text{vec}(A)}
$$$

What is the value of $\text{vec}(A^T)$? Does it have a relation with $\text{vec}(A)$? Of course, it does. Let's consider a 3x3 matrix:

$$$
\begin{aligned}
&\text{vec}\left(\begin{bmatrix}
1 &2 &3\\
4 &5 &6\\
7 &8 &9
\end{bmatrix}\right)=\begin{bmatrix}
1\\
4\\
7\\
2\\
5\\
8\\
3\\
6\\
9
\end{bmatrix}
&\text{vec}\left(\begin{bmatrix}
1 &2 &3\\
4 &5 &6\\
7 &8 &9
\end{bmatrix}^T\right)=\begin{bmatrix}
1\\
2\\
3\\
4\\
5\\
6\\
7\\
8\\
9
\end{bmatrix}
\end{aligned}
$$$

The relation between these two matrices can be written as:

$$$
\begin{bmatrix}
1 &0 &0 &0 &0 &0 &0 &0 &0\\
0 &0 &0 &1 &0 &0 &0 &0 &0\\
0 &0 &0 &0 &0 &0 &1 &0 &0\\
0 &1 &0 &0 &0 &0 &0 &0 &0\\
0 &0 &0 &0 &1 &0 &0 &0 &0\\
0 &0 &0 &0 &0 &0 &0 &1 &0\\
0 &0 &1 &0 &0 &0 &0 &0 &0\\
0 &0 &0 &0 &0 &1 &0 &0 &0\\
0 &0 &0 &0 &0 &0 &0 &0 &1
\end{bmatrix}
\begin{bmatrix}
1\\
2\\
3\\
4\\
5\\
6\\
7\\
8\\
9
\end{bmatrix}
=
\begin{bmatrix}
1\\
4\\
7\\
2\\
5\\
8\\
3\\
6\\
9
\end{bmatrix}
$$$

The matrices consisting of a 1 in each row, the permutation matrices, are used to reorder the rows of the vector (or matrix) that they multiply with. We call the matrix that transforms $\text{vec}(A)$ to $\text{vec}(A^T)$ the permutation matrix $T$. But what about the derivative of the Kronecker product? Let's forget about it for now. Just remember that the derivative of the Kronecker product results in something like:

$$$
\frac{\partial A\otimes B}{\partial A}=(I_n\otimes T_{qm} \otimes I_p)(I_{mn}\otimes \text{vec}(B))
$$$

This time, I wrote the dimensions of the identity matrices and $T$ explicitly, as it's crucial not to get them mixed up.

Well, that's it for today. We'll discuss functions like inverse and determinant of matrices in the next post and solve some problems using these tools in the final post.
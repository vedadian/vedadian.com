*NOTE*: This article has been translated from Farsi using `llama3-70b-8192` and `groq`

Now that we have reviewed the initial problems on matrix derivatives, we can talk about derivatives of general functions defined in terms of matrices, such as the inverse matrix and determinant.

Let's start with the derivative of the inverse matrix. When the inverse of a matrix is multiplied by itself, the result is an identity matrix with the same dimensions as the original matrix. We can start from here:

$$$
\frac{\partial A^{-1}A}{\partial A}=\frac{\partial I}{\partial A}=0
$$$

On the other hand, we also had a relationship for taking the derivative of the product of two matrix functions:

$$$
\frac{\partial f(A)g(A)}{\partial A}=(I\otimes f(A))\frac{\partial g(A)}{\partial A}+(g(A)^T\otimes I)\frac{\partial f(A)}{\partial A}
$$$

Substituting into the first equation, we have:

$$$
\frac{\partial A^{-1}A}{\partial A}=(I\otimes A^{-1})+(A^T\otimes I)\frac{\partial A^{-1}}{\partial A}=0
$$$

Et voila!

$$$
\frac{\partial A^{-1}}{\partial A}=-(A^{-T}\otimes A^{-1})=-(A^{-T}\otimes A^{-1})
$$$

To calculate this last part, I used two identities involving Kronecker products:

$$$
\begin{aligned}
&(A\otimes B)^{-1}=(A^{-1}\otimes B^{-1})\\
&(A\otimes B)(C\otimes D)=(AC\otimes BD)
\end{aligned}
$$$

Of course, these only hold under certain conditions. That is, in the first equation, both matrices $A$ and $B$ must be invertible, otherwise, the Kronecker product will not be invertible either. In the second equation, matrices $A$ and $C$ can be multiplied, and also $B$ and $D$, but this multiplication may not be possible in general, however, we can still multiply the output of the two Kronecker products as usual matrices.

The next function is the determinant. Calculating its derivative is a bit more involved than the previous one. To obtain the derivative of the determinant, we need to pay attention to the adjugate matrix. The relationship between the determinant and the adjugate matrix is quite interesting. It suffices to select any row of the adjugate matrix and multiply it by the corresponding column in the original matrix. The result is the determinant of the original matrix. This means that for any $i$ (in the range of 1 to the number of columns of the original matrix):

$$$
|A|=A^*_{i,.}A_{.,i}
$$$

On the other hand, the inverse of a matrix can also be calculated using the adjugate matrix:

$$$
A^{-1} = \frac{1}{|A|}A^*
$$$

With these, we can finish the task:

$$$
\begin{aligned}
&\frac{\partial |A|}{\partial A_{.,i}}=A^*_{i,.}\\
&\frac{\partial |A|}{\partial A}=\left[A^*_{1,.}, A^*_{2,.},\ldots A^*_{n,.}\right]=\text{vec}^T\left((A^*)^T\right)
\end{aligned}
$$$

Finally, we can replace $A^*$ with $|A|A^{-1}$ and we're done:

$$$
\frac{\partial |A|}{\partial A}=\text{vec}^T\left((|A|A^{-1})^T\right)=|A|\text{vec}^T(A^{-T})
$$$

If we consider $\ln|A|$, the result becomes even more beautiful:

$$$
\frac{\partial \ln|A|}{\partial A}=\text{vec}^T(A^{-T})
$$$

Can you tell why?

Finally, the last function we'll discuss in this section is the inner product of two matrices. The inner product of two matrices is equal to the product of their vectorized forms:

$$$
<A,B>=\text{vec}^T(A)\text{vec}(B)=\text{trace}(A^TB)
$$$

which is often introduced as the inner product in most texts. The reason is easy to see.

$$$
\begin{aligned}
&\text{trace}(A^TB)=\sum _i (A^TB)_{i,i}=\sum_i A_{.,i}^TB_{.,i}\\
&\text{vec}(A)=\left[\begin{array}{c}
A_{.,1}\\
A_{.,2}\\
\vdots\\
A_{.,m}
\end{array}\right],\ \text{vec}(B)=\left[\begin{array}{c}
B_{.,1}\\
B_{.,2}\\
\vdots\\
B_{.,m}
\end{array}\right]\\
&\text{vec}^T(A)\text{vec}(B)=\sum_i A_{.,i}^TB_{.,i}
\end{aligned}
$$$

If you've been careful, you'll notice that the number of rows of matrix $A$ and the number of columns of $B$ are equal. This is why this inner product is defined for such matrices. Typically, $\text{trace}$ is defined on square matrices, and the output of $A^TB$ must be a square matrix so that we can apply the $\text{trace}$ to it.

The derivative of the inner product of two matrices with respect to one of them is, as expected, the transposed vectorized form of the other:

$$$
\begin{aligned}
&\frac{\partial \text{trace}(A^TB)}{\partial B}=\text{vec}^T(A)\\
&\frac{\partial \text{trace}(A^TB)}{\partial A}=\text{vec}^T(B)\\
\end{aligned}
$$$

This is the same as what we saw in "Linear Algebra II" course in university regarding vectors:

$$$
\begin{aligned}
&\frac{\partial a^Tb}{\partial b}=a^T\\
&\frac{\partial a^Tb}{\partial a}=b^T\\
\end{aligned}
$$$

After three posts on derivatives with respect to matrices, we have reached a point where we can solve problems like estimating the covariance matrix of a Gaussian distribution from its samples. We'll solve two examples of these problems in the next post, which will conclude our discussion on matrix derivatives.
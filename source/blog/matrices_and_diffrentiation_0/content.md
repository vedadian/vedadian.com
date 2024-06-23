*NOTE*: This article has been translated from Farsi using `llama3-70b-8192` and `groq`

It is obvious from everything about me that I love mathematics; yet it was my second year in graduate school when I was forced to seriously pursue matrix calculus. It all started with a small 14-page booklet by an agricultural economics professor that set me on my path.

Now, over 12 years later, differentiating these functions is not a challenging task anymore. With tools like `PyTorch` and `TensorFlow`, you can automatically differentiate these functions and do much more. However, having knowledge of matrix calculus can still be beneficial for those who want to optimize their models (at least, it's a good excuse for me to write about it).

## Functions with matrix inputs

Examples of functions with matrix inputs are plentiful. One of the most well-known ones is the multivariate Gaussian distribution function:

$$$
p(x|\Sigma, \mu)=(2\pi\text{det}\Sigma)^{-\frac{1}{2}}\text{exp}\left(\frac{1}{2}(x-\mu)^T\Sigma^{-1}(x-\mu)\right)
$$$

If we have a series of samples from this distribution and want to estimate the $\Sigma$ matrix, we'll be dealing with functions that take matrices as input.

Another example is neural network models. In most neural network models, there are weight matrices that need to be optimized, and we'll encounter functions of these matrices.

Let's go back to estimating the $\Sigma$ matrix. We have $n$ samples of $x$, denoted by $x_1, x_2, \ldots, x_n$. We want to find the $\Sigma$ that maximizes the likelihood of these samples. Assuming that the $x_i$s are independent and identically distributed (i.i.d.), we have:

$$$
J(\Sigma)=p(x_1,x_2,\ldots x_n\mid \Sigma,\mu )=\prod (2\pi\text{det}\Sigma)^{-\frac{1}{2}}\text{exp}\left(\frac{1}{2}(x_i-\mu)^T\Sigma^{-1}(x_i-\mu)\right)
$$$

This function needs to be maximized with respect to its input, $\Sigma$. This is a function with a matrix input that we need to differentiate and set equal to zero to find the optimal value.

## Matrix Calculus Without Tears and Bloodshed

If we want to differentiate these functions in a principled way, we'll encounter tensors and tensor operations. But there's a simpler way. This method is based on a clever perspective: "When computing derivatives, the layout of matrix elements is of secondary importance."

With this perspective, we can ignore the layout of matrix elements when taking derivatives. Each matrix becomes a large vector accompanied by two numbers that specify how the elements are laid out. For example:

$$$
\left[\begin{array}{cc} 5&4\\ 3&2\\ 1&0 \end{array}\right] \sim \left(\left[\begin{array}{c} 5\\ 3\\ 1\\ 4\\ 2\\ 0 \end{array}\right], (3,2)\right)
$$$

If you're careful, you'll notice that nothing is lost in this new representation. We can reconstruct the original matrix from this vector and its dimensions.

You might ask, what's the point of this? But I won't answer that yet! First, I need to explain that concatenating the columns of a matrix is a mathematical operation called the `vec` operator.

Let's get back to the question: when we represent matrices as vectors, differentiating with respect to matrices becomes differentiating with respect to vectors, which we're familiar with from our second-year university mathematics.

## Calculus of Matrix-Valued Functions

Let's define a function $f(X) = (f_1(X), f_2(X), \ldots, f_n(X))$. According to the definition, we compute the derivative as:

$$$
\frac{\partial f}{\partial X} = \frac{\partial \text{vec}(f)}{\partial \text{vec}(X)}
$$$

Now, let's consider a simple case where we want to compute the derivative of a matrix with respect to itself:

$$$
\frac{\partial A}{\partial A} = \frac{\partial \text{vec}(A)}{\partial \text{vec}(A)} = I_{mn}
$$$

where $I_{mn}$ is the identity matrix with dimension $mn$.

Let's consider a more complex case. Assume we have a single-variable function $g$ that we can apply to each element of matrix $A$:

$$$
g(A) = \left[\begin{array}{cccc} g(A_{11}) &g(A_{12}) &\cdots &g(A_{1m})\\ g(A_{21}) &g(A_{22}) &\cdots &g(A_{2m})\\ \vdots &\ddots &\vdots\\ g(A_{n1}) &g(A_{n2}) &\cdots &g(A_{nm}) \end{array}\right]
$$$

What is $\frac{\partial g(A)}{\partial A}$? With similar calculations, we get:

$$$
J_{ij} = \frac{\partial \text{vec}\left(g(A)\right)_i}{\partial \text{vec}(A)_j} = \delta_{ij}g'(A_{\lfloor \frac{i}{n}\rfloor,i-n\lfloor \frac{i}{n}\rfloor})=\text{diag}(\text{vec}(g'(A)))
$$$

This means that the resulting matrix has only non-zero elements on the diagonal, and those elements are the derivatives of $g$ with respect to the elements of $A$.

Finally, note that differentiation with respect to matrices is a linear operator. This means that scalar multiplication and addition with respect to this operator have the usual properties:

$$$
\frac{\partial (\alpha A+\beta B)}{\partial X}=\alpha\frac{\partial \text{vec}(A)}{\partial \text{vec}(X)}+\beta\frac{\partial \text{vec}(B)}{\partial \text{vec}(X)}
$$$

In this post, we saw that if we temporarily ignore the layout of matrix elements, we can develop a convenient definition for matrix differentiation. We used the `vec` operator to concatenate matrix columns into a long vector. In the next post, I'll delve deeper into matrix calculus.
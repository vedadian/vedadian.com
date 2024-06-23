*NOTE*: This article has been translated from Farsi using `llama3-70b-8192` and `groq`

## Derivative of Correlation Matrix Estimation and Neural Network Weights

The primary task of taking derivatives is to find the extremum points; however, the method of using derivatives to reach the extremum can be either through iterative optimization methods or solving formal equations. To illustrate each of these methods, we will first estimate the correlation matrix for a Gaussian distribution and then compute the gradient of an MLP[>Multi-Layer Perceptron] with respect to its weights.

## Estimating Correlation Matrix

The multivariate Gaussian distribution is formulated as follows:

$$$
p(x|\mu,\Sigma)=\det(2\pi\Sigma)^{-\frac{1}{2}}\exp\left(-\frac{1}{2}(x-\mu)^T\Sigma^{-1}(x-\mu)\right)
$$$

Now, if we have samples $x_1$ to $x_n$ from this distribution, which values of $\mu$ and $\Sigma$ maximize the likelihood of these samples?

Assuming independence among samples, the likelihood of all samples becomes:

$$$
\begin{aligned}
p(x_1,x_2,\ldots x_n|\mu,\Sigma)&=\prod p(x_i|\mu,\Sigma)\\
\log(p(x_1,x_2,\ldots x_n|\mu,\Sigma))&=\sum \log(p(x_i|\mu,\Sigma))
\end{aligned}
$$$

Simplifying the equation, we get:

$$$
\log(p(x_1,x_2,\ldots x_n|\mu,\Sigma))=-\frac{n}{2}\log(2^m\pi^m)-\frac{n}{2}\log(\det(\Sigma))-\frac{1}{2}\sum (x_i-\mu)^T\Sigma^{-1}(x_i-\mu)
$$$

To simplify further, we take derivatives with respect to $\mu$ and $\Sigma$.

For the first derivative:

$$$
\frac{\partial y}{\partial \mu}=-\frac{1}{2}(\Sigma^{-1}+\Sigma^{-T})((\sum x_i)-n\mu)=0 \Rightarrow \mu=\frac{1}{n}\sum x_i
$$$

The second derivative:

$$$
\frac{\partial y}{\partial \Sigma}=\frac{\partial y}{\partial \Sigma^{-1}}\frac{\partial \Sigma^{-1}}{\partial \Sigma}=-\frac{n}{2}\text{vec}^T(\Sigma^T)-\frac{1}{2}\sum ((x_i-\mu)^T\otimes(x_i-\mu)^T)(\Sigma^{-T}\otimes \Sigma^{-1})=0
$$$

Simplifying, we get:

$$$
\text{vec}^T(\Sigma^T)=\frac{1}{n}\sum ((x_i-\mu)\otimes(x_i-\mu))^T
$$$

Using the relationship $\text{vec}(AXB)=(B^T\otimes A)\text{vec}(X)$, we obtain:

$$$
\text{vec}^T(\Sigma^T)=\frac{1}{n}\sum \text{vec}^T((x_i-\mu)(x_i-\mu)^T)\Rightarrow \Sigma=\Sigma^T=\frac{1}{n}\sum ((x_i-\mu)(x_i-\mu)^T)
$$$

## Derivative of MLP Weights

The layers of an MLP can be represented as:

$$$
y_{n+1}=\phi(W_ny_n+b_n)
$$$

Assuming we have an MLP with multiple layers, when we want to train it, we compare the output of the $m$th layer with the desired results and calculate the error. Let's assume we use the MSE[>Mean Squared Error] criterion:

$$$
j(W_0,b_0,W_1,b_1,\ldots W_{m-1},b_{m-1})=\frac{1}{2}\|y_m-y\|^2=\frac{1}{2}(y_m-y)^T(y_m-y)
$$$

Using the machinery we built for derivatives, we calculate $\frac{\partial j}{\partial W_i}$:

$$$
\frac{\partial j}{\partial W_i}=\frac{\partial j}{\partial y_m}\left(\prod_{m-1}^i \frac{\partial y_{j+1}}{\partial y_j}\right)\frac{\partial y_i}{\partial W_i}
$$$

Breaking down each component:

$$$
\begin{aligned}
\frac{\partial j}{\partial y_m}&=(y_m-y)^T\\
\frac{\partial y_{j+1}}{\partial y_j}&=\text{diag}\left(\phi'(W_jy_j+b_j)\right)W_j\\
\frac{\partial y_i}{\partial W_i}&=(y_i^T\otimes I)
\end{aligned}
$$$

Simplifying the expression:

$$$
\frac{\partial j}{\partial W_i}=(y_m-y)^T\left(\prod \text{diag}\left(\phi'(W_jy_j+b_j)\right)W_j\right)(y_i^T\otimes I)
$$$

Using the Hadamard product (element-wise multiplication), we get:

$$$
\frac{\partial j}{\partial W_i}=(y_m-y)^T\left(\prod \text{diag}\left(\phi'(W_jy_j+b_j)\right)W_j\right)(y_i^T\otimes I)=\text{vec}^T(e_iy_i^T)
$$$

In conclusion, the derivative of the correlation matrix estimation and MLP weights are computable using the derivatives of the logarithmic likelihood function and the error criterion, respectively.

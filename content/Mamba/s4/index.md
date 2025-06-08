+++
title = "Structured Space Space for Sequence Modeling"
slug = "s4"
+++

## What makes S4 special?
1. Addressing long-range dependencies using HIPPO framework
2. Instead of directly calculating power of $\bar{A}$, S4 use truncated generating function and IFFT(Inverse Fast Fourier Transform) to generate convolution kernel $\bar{K}$.
3. When $A$ matrix is DPLR, then by using Woodbury Identity, we can compute generating function $\hat{K}_L$ using efficient Cauchy dot-product.

I will go through all the mathematical parts.

## Applying HIPPO framework
Previous SSMs struggled because hidden state  couldn't store the past history. So they made a HIPPO framework, SSM that models past history using orthonormal basis.

In HIPPO framework, there are 4 matrix $A, B, C, D$. But the most important matrix is $A$(which is called HIPPO matrix). So they bring the HIPPO matrix to S4.

## Using truncated generating function and IFFT to generate $\bar{K}$
Computing $\bar{K}$ takes huge computation resource.
$$\bar{K}=(\bar{C} ^ *\bar{A}^0\bar{B}, \bar{C} ^ *\bar{A}^1\bar{B}, ..., \bar{C} ^ * \bar{A}^{L-1}\bar{B})$$

From $\bar{K}$, let's create a generating function $\hat{K}_L$

$$\hat{K}_L(z;\bar{A},\bar{B},\bar{C}) = \sum _{ k = 0} ^{L-1} \bar{C} ^ *\bar{A} ^ k \bar{B} z^k $$

Let's think of `Roots of Unity Filter`.
$$\omega_j=exp(-i 2\pi\frac{jk}{L}), \ \  j=0, 1, 2, ..., L-1$$

If we subsitute $z$ to $\omega_j$, then we get the following equation.
$$\begin{aligned}
&\hat{K} _L(\omega _j) \\\\
& =\sum _{k=0} ^{L-1} (\bar{C} ^* \bar{A} ^k \bar{B}) \cdot \omega _j ^k \\\\
& =\sum _{k=0} ^{L-1} \bar{C} ^* \bar{A} ^k \bar{B} \cdot exp( -i2 \pi \frac{jk}{L})
\end{aligned}$$

This is exactly the same as DFT(Discrete Fourier Transform). Think $j$ as frequency, and $k$ as time.
This means that if we can get the generating function $\hat{K}_L(z)$, we can easily calculate the convolution kernel $\bar{K}$ using IFFT.

## How to get generating function $\hat{K}_L(z)$
### Converting into inverse-form
You may think that we need all $\bar{C}\bar{A}^k\bar{B}$ terms to get generating function $\hat{K}_L(z)$.

Well, actually we don't. Let's look at some tricks to get $\hat{K}_L$ with low computation cost.

$$\begin{aligned}
&\hat{K} _L (z;\bar{A}, \bar{B}, \bar{C}) \\\\
&=\sum _{k=0} ^{L-1} \bar{C} ^* m\bar{A} ^k \bar{B}z ^k \\\\
&= \bar{C} ^* (I-\bar{A} ^Lz ^L )(I-\bar{A}z) ^{-1} \bar{B} \\\\
&= \tilde{C} ^* (I-\bar{A}z) ^{-1} \bar{B} \end{aligned}$$

We are going to use $z$ from $exp(-i2\pi\frac{k}{L}):k\in[L]$.
So $z^L$ is always 1.

### Convert $\bar{A}, \bar{B}$ into $A, B$
Previously, we discretized $A, B$ into $\bar{A}, \bar{B}$. But we are doing the reverse.

Since
$$\begin{aligned}
\bar{A}=(I-\frac{\Delta}{2}A)^{-1}(I+\frac{\Delta}{2}A) \\\\
\bar{B}=(I-\frac{\Delta}{2}A)^{-1}\Delta B
\end{aligned}$$

We can convert $\hat{K}_L(z)$ as following:
$$\begin{aligned}
&\hat{K} _L =\tilde{C} ^* (I-\bar{A}z) ^{-1} \bar{B} \\\\
&=\tilde{C} ^* (I-(I-\frac{\Delta}{2}A) ^{-1} (I+\frac{\Delta}{2}A)z) ^{-1} (I-\frac{\Delta}{2}A) ^{-1} )\Delta B \\\\
&=\tilde{C} ^* ((I-\frac{\Delta}{2}A) ^{-1} (I-\frac{\Delta}{2}A)-(I-\frac{\Delta}{2}A) ^{-1} (I+\frac{\Delta}{2}A)z) ^{-1} (I-\frac{\Delta}{2}A) ^{-1} \Delta B \\\\
&=\tilde{C} ^* [I(1-z)-\frac{\Delta}{2}A(1+z)] ^{-1} \Delta B \\\\
&=\frac{2}{1+z}\tilde{C} ^* [\frac{2(1-z)}{\Delta(1+z)}I-A] ^{-1} B
\end{aligned}$$

### Assume $A$ is DPLR(Diagonal Plus Low-Rank), and apply Woodbury identity
If we assume $A$ is DPLR, we can write $A$ as following: $\Lambda$ is a diagonal matrix $\Lambda \in \mathbb{C}^{N\times N}$.

For $P, Q \in \mathbb{C}^{N \times 1}$,
$$A = \Lambda +PQ^*$$

Woodbury identity converts inverse of DPLR into simpler form:
$$(\Lambda+PQ ^* ) ^{-1} =\Lambda ^{-1} -\Lambda ^{-1} P(1+Q ^* \Lambda ^{-1} P) ^{-1} Q ^* \Lambda ^{-1}$$

By applying it, we get the following equation for $\hat{K}_L(z)$:

$R(z)$ is also a diagonal matrix!

$$\begin{aligned}\hat{K}_L=\frac{2}{1+z}[\tilde{C}^*R(z)^{-1}B-\tilde{C}^*R(z)^{-1}P(1+Q^*R(z)^{-1}P)^{-1}Q^*R(z)^{-1}B] \\\\
where \ R(z)=\frac{2(1-z)}{\Delta(1+z)}I-\Lambda\end{aligned}$$

### Background: Cauchy Matrix
With elements $\Omega=(w_i)\in\mathbb{C}^M$ and $\Lambda=(\lambda_j)\in \mathbb{C}^N$,

Cauchy matrix is defined as follows:
$$M\in\mathbb{C} ^{M\times N} =M(\Omega, \Lambda)=(M_{ij}) _{i\in[M],\ j\in[N]} \\\\
M _{ij} =\frac{1}{\omega_i - \lambda _j}$$

### Background: Cauchy kernel(Cauchy dot-product)
when $A \in \mathbb{C}^{M\times 1}, \ B \in \mathbb{C}^{M\times N}, \ C \in \mathbb{C}^{N \times 1}$ and $B$ is Cauchy matrix, then $A^TBC$ can be efficiently computed

You don't have to understand why this form can be computed efficiently.
For simplicity, We are going to write Cauchy dot-product as
$$A^TBC=k_{\Omega, \Lambda}(A, C)$$

### Applying Cauchy dot-product to the $\hat{K}_L$ calculation
Since $R(z)^{-1}$ is a Cauchy-Matrix, we can apply Cauchy dot-product!

$$\begin{aligned}
&\hat{K} _L \\\\
&=\frac{2}{1+z}[\tilde{C} ^* R(z) ^{-1} B-\tilde{C} ^* R(z) ^{-1} P(1+Q ^* R(z)^{-1}P) ^{-1} Q ^* R(z) ^{-1} B] \\\\
&= c(z)[k _{z, \Lambda} (\tilde{C}, B)-k _{z, \Lambda} (\tilde{C}, P)(1+k _{z, \Lambda} (Q, P)) ^{-1} k _{z, \Lambda}(Q, B)]\end{aligned}$$

Since we can calculate the generating function $\hat{K}_L$ with low computation cost, we don't need huge computation to generate the convolution kernel $\bar{K}$.

## Wrapup
This math-journey is everthing for S4. It was exciting to study FFT, Generating function. I hope this post helps you guide to the ultimate goal, Mamba.

## References
[1] [https://srush.github.io/annotated-s4/](https://srush.github.io/annotated-s4/)

[2] [https://arxiv.org/abs/2111.00396](https://arxiv.org/abs/2111.00396)

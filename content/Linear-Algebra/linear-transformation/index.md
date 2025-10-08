+++
title = "Linear Transformation"
slug = "linear-transformation"
+++

# Summary

As you deal with vectors, you may heard of the term `Linear Transformation`.
Linear Transformation is a process of changing the input vector. Actually, we can view it in two kinds: Changing the input Vector, Changing the Basis

## Linear Transformation

Linear Transformation $L$ has following rules:

$$\begin{aligned}
&L(\vec{v}+\vec{w})=L(\vec{v})+L(\vec{w}) \ ...(1) \\\\
&L(c \ \vec{v}) = c \ L(\vec{v}) \ ...(2) \end{aligned}$$

In 2D space, linear transformation can be expressed in $2\times2 $ matrix.
Let's say $\vec{b}^T$ is basis vectors, $L$ is $2\times 2 $ linear transformation Matrix, and $\vec{v}$ as input vector.

Then we can express linear transformation as following:

$$\vec{b}^T \cdot \vec{v} \Rightarrow \vec{b}^T \cdot L \cdot \vec{v}  \ ...(3)$$

By applying the linear transformation matrix in the middle of basis vectors and input vector, it results linear transformation!
We expressed linear transformation in the equation (3). Since there are three terms $\(\vec{b}^T, L, \vec{v}\)$, there are two kinds of order-of-calculation.

1. Calculating $L \cdot \vec{v}$ first, and then dot-product with $\vec{b}^T$: viewing as changing the input vector
2. Calculating $\vec{b}^T \cdot L$ first, and then dot-product with $\vec{v}$: viewing as changing the basis vector

Let's take a look at two different perspectives.

## Viewing as changing input vector

If we calculate $L \cdot \vec{v}$ first, we are viewing linear transformation as changing the input vector without changing the basis vector.

## Viewing as changing basis vector

If we calculate $\vec{b}^T \cdot L$ first, we are viewing linear transformation as changing the basis vector without changing the input vector.

I highly recommend this video. It explains linear transformation with good graphical materials.

{{ youtube(id="kYB8IZa5AuE", w=500) }}

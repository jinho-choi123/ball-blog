+++
title = "Singular Value Decomposition"
slug = "singular-value-decomposition"
+++

## Singular-Value Decomposition
The definition of SVD is as follows:

For rectangular matrix $A \in \mathbb{R}^{m \times n}$, it can be decomposed to following form:
1. Orthogonal Matrix $U \in \mathbb{R}^{m \times m}$
2. Orthogonal Matrix $V \in \mathbb{R}^{n \times n}$
3. Diagonal Matrix $\Sigma \in \mathbb{R}^{m\times n}$

$$A=U\Sigma V^T \ ...(1)$$

## Orthogonal Matrix
Orthogonal matrix is a square matrix $X \in \mathbb{R}^{n \times n}$ that has following feature:

$$XX^T=I$$

## The meaning $U, \Sigma, V$

To understand the meaning of $U, \Sigma, V$ we should multiply $A^T$ to equation (1).

$$\begin{aligned}
&AA^T=U\Sigma V^T (U\Sigma V^T)^T \\\\
&= U\Sigma V^T V \Sigma^T U^T \\\\
&= U(\Sigma \Sigma^T) U^T  \\\\
&= U(\Sigma \Sigma^T)U^{-1}
\end{aligned}$$

This is the exact form of EVD(Eigen-Value Decomposition).

$U$ is the matrix of eigen-vectors of $AA^T$.
Vice-versa, $V$ is the matrix of eigen-vectors of $A^TA$.
$\Sigma \Sigma^T$ is a diagonal matrix that each diagonal elements are eigen-value of $AA^T$.

## How the calculation is done
Since we know the meaning of $U, \Sigma, V$, we can easily decompose $A$.
1. Get eigen-vectors of $AA^T$ and build-up $U$
2. Get eigen-values of $AA^T$ and build-up $\Sigma$
3. Calculate the remaining $V$ using inverse.

## References
[1] [https://youtu.be/mBcLRGuAFUk?si=Ld1YyxMeRy2el9-a](https://youtu.be/mBcLRGuAFUk?si=Ld1YyxMeRy2el9-a)

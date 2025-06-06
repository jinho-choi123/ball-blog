+++
title = "KL-Divergence"
slug = "kl-divergence"
+++

## What is Kullback-Leibler Divergence?
KL-Divergance is defined as the entropy difference between two probability distribution. In other words, it tells if two distributions are similar or not.

Cross-entropy gets smaller as two distribution gets similar. Let's take a look at the definition of cross-entropy.

$$H(P, Q) = - \sum p \cdot log_2(q)$$

## Definition of KL-Divergence
Using cross-entropy, we can express KL-Divergence:

$$KL(P||Q) = H(P, Q) - H(P) = - \sum p \cdot log_2(\frac{p}{q})$$

In every cases in ML, distribution P is ground-truth distribution. Which means that  is constant. It is fine to subtract it from cross-entropy.

## Characteristics of KL-Divergence

Since cross-entropy  is always bigger than entropy , KL-divergence should always be non-negative.

$$KL(P||Q) \ge 0$$

Also, following obvious because of the definition of KL-divergence.

$$KL(P||Q) \ne KL(Q||P)$$

## Jensen-Shannon Divergence
JS-divergence is also used to express the entropy difference between two probability distribution. It is defined as followings:

$$JSD(P||Q) = \frac{1}{2} KL(P||M) | \frac{1}{2} KL(Q||M) \\ Where \ M=\frac{1}{2}(P+Q)$$

JS-divergence is expressed using KL-divergence, but not used as much as KL-divergence.

## References
[1] [https://hyunw.kim/blog/2017/10/27/KL_divergence.html](https://hyunw.kim/blog/2017/10/27/KL_divergence.html)

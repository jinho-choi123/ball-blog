+++
title = "Attention is All you need"
slug = "attention-is-all-you-need"
+++
[Attention Is All You Need](https://arxiv.org/abs/1706.03762)

## Summary
If somebody request me to explain Attention method to non-CS personnel, then I would explain as follows:

> The purpose of Attention is converting sentence into mathematical format that embeds relationship between words.

## Attention method
Attention method is the core of transformer. At this section, we are going to take a closer look at what attention method is.

### Example 1
We have 6 students in the class, and we know the test score of 5 student. How can we guess the last student's score?
```
88, 67, 90, 100, 98, __
```

We can simply assume the last student's score will be the mean of 5 student's score.
$$score = \frac{1}{5}88 + \frac{1}{5}67 + \frac{1}{5}90 + \frac{1}{5}100 + \frac{1}{5}98$$

We got the last student's score by weighting $\frac{1}{5}$ to all other student's score and sum it.

### Example 2
Cont. by example 1, let's say student6 always studied with student4, student5. Then we can say that student6's score will be more likely to student4, student5's score. We can change the equation as follows:

$$score = \frac{1}{10}88 + \frac{1}{10}67 + \frac{1}{10}90 + \frac{7}{20}100 + \frac{7}{20}98$$

We weighted more for student4 and student5's score.

### Why do we need Attention method?
Let's say we are trying to convert the following sentence into a matrix:
"Jake had a walk with his cute dog"

Assume that we can convert each words into embedding vector. Would just concatenating all the vectors and making a matrix would be enough? No.
"Jake had a walk with his cute dog" vs "Jake didn't had a walk with his dog"

In both sentence, there is a word dog. But it has a different meaning: At first sentence, dog is cute, and had a walk with Jake. In second sentence, we don't know if the dog is cute, and dog didn't had a walk with Jake.
Even for same word, it can have a different meaning. So just concatenating isn't enough.

### How Attention method works?
Examples above is closely related to Attention Method.
"Jake had a walk with his cute dog"

Let's say we are trying to convert the word "dog" into a vector. Dog is closely related to some words: "dog"(of course, it is closely related to itself), "cute", "walk", and "Jake".

Attention method is that give some weights to closely related words, and sum it! We can express the final vector of "dog" as following equation.

$$A(vector \ of \ "Jake") + B(vector \ of \ "walk") + C(vector \ of \ "cute") + D(vector \ of \ "dog") \ ...(1)$$

## How to get the attention weights?
We discussed that attention method is summing other word's vector multiplied by weight. Then how can we get the weights?

### Key, Query, Value
Before discussing how to get the weights, we should look at some terms. Let's say we want to calculate the $A$ at equation (1). We have to calculate how much "Jake" and "dog" is related.

Assume there is a function $relationship(key, query)$ that calculates the relationship between two words.
At this point, "dog" is the Key, and "Jake" is the Query. If we get the weights, we have to multiply with $vector \ of \ "Jake"$, which will be the Value.

### relationship(key, query)
In the paper https://arxiv.org/abs/1706.03762, it introduce dot product with softmax. It just simply do the dot product of two vectors(key and query), and apply softmax to make the sum of weights to 1.
(In equation (1), sum of $A, B, C, D$ should be 1)

## References
[1] [https://www.youtube.com/watch?v=wjZofJX0v4M](https://www.youtube.com/watch?v=wjZofJX0v4M)

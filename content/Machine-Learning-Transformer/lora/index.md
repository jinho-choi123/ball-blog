+++
title = "LoRA: Low-Rank Adaption"
slug = "lora"
+++

[LoRA: Low-Rank Adaptation of Large Language Models](https://arxiv.org/abs/2106.09685)

## Fine-tuning LLM is hard
Nowadays, we can easily get pre-trained models from `huggingface`. But still we have to fine-tune the model to accomplish our custom tasks.
In autoregressive language model $P_\Phi(y|x)$ with weights $\Phi$ and dataset $Z= \{ (x_i, y_i) \}_{i=0...N}$, the training objective is to minimize the log-likelihood.

$$\underset{\Phi}{max} \sum_{(x, y) \in Z} \sum_{t=1}^{|y|}log(P_\Phi(y_t|x, y_{<t}))$$

When full fine-tuning the model by updating the weights to $\Phi_0 \rightarrow \Phi_0+\Delta\Phi$,
$\Delta \Phi$'s dimension is same as dimension of $\Phi_0$. As model gets large, calculation and memory usage to get $\Delta \Phi$ explodes!

For example, GPT-3 has 175B parameters. Which means we need 1.2TB VRAM GPU to full fine-tune the model. Do you personally have GPU that has 1.2TB VRAM?

In the LoRA paper, it suggest a new paradigm to fine-tune a large model with fewer parameters.
Please read the paper for more information.

## What is LoRA
Inspired by [Aghajanyan et al.](https://arxiv.org/abs/2012.13255), author of the paper hypothesized following statement:

> The updates to the weights have a low "intrinsic rank" during adaption

LoRA use reparametrization to make the dimension of gradient update smaller, and make the training memory-efficient and computation-efficient.

LoRA parameterize $\Delta \Phi$ into function of $\Theta$, where $rank(\Theta) \ll rank(\Phi_0)$.
Then we can express gradient update as $\Phi_0 \rightarrow \Phi_0+\Delta \Phi(\Theta)$

In fine-tuning, LoRA freeze the pre-trained weights $\Phi_0$, and adjust $\Theta$.

## LoRA in more details
In the previous section, we roughly went through key concept of LoRA. Let's take a closer look at how LoRA "actually" works.

Let's say we have a linear regression model.

$$\begin{aligned}
&h=W_0x \\\\
&W_0 \in \mathbb{R}^{d \times k}, x \in \mathbb{R}^{k \times o}
\end{aligned}$$

We want to fine-tune this model by updating the weights: $W_0 \rightarrow W_0 + \Delta W$

But the problem is that this linear regression model is too big: $d \gg 0, k \gg 0$

LoRA reparameterize $\Delta W$ into two small matrices
$$\begin{aligned}
&\Delta W = AB \\\\
&A \in \mathbb{R}^{d \times r}, B \in \mathbb{R}^{r \times k}, r \ll min(d, k)
\end{aligned}$$

Using LoRA, we just have to train much smaller parameters compared to full fine-tuning.

## Benefit of LoRA
### Generalization of full fine-tuning
As  increase for reparameterization, it roughly converges to training the original model(full-finetuning)

### No Additional Inference Latency
Adapter weight calculation part($AB$) can be parallelized with the inferencing procedure. So it doesn't require addional inference latency.
Moreover, changing weights per task has very little memory overhead because we don't have to swap the whole model, but just the small weights of the adapters.

## References
[1] [https://arxiv.org/abs/2106.09685](https://arxiv.org/abs/2106.09685)
[1] [https://arxiv.org/abs/2012.13255](https://arxiv.org/abs/2012.13255)

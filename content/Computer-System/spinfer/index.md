+++
title = "SPInfer"
slug = "spinfer"
+++

[SpInfer: Leveraging Low-Level Sparsity for Efficient Large Language Model Inference on GPUs](https://dl.acm.org/doi/10.1145/3689031.3717481)

# Summary

Nowadays, LLM(Large Language Model) use technique called "unstructured pruning" to reduce the model size. It is done by removing the unimportant portions of the model weights.

As weight matrix is becoming sparse, it is important to use sparse matrix multiplication to reduce the computation time. "SpMM" is a special kernel for sparse matrix multiplication, which is used alongside with unstructured pruning.

However, "SpMM" is not efficient for low sparsity level(around 50%) due to storage overhead. This paper proposes "SpInfer", an efficient SpMM kernel that can be applied to even low sparsity level.

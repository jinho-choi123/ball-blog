+++
title = "SPInfer"
slug = "spinfer"
date = 2025-09-12
+++

[SpInfer: Leveraging Low-Level Sparsity for Efficient Large Language Model Inference on GPUs](https://dl.acm.org/doi/10.1145/3689031.3717481)

# Summary

Nowadays, LLM(Large Language Model) use technique called "unstructured pruning" to reduce the model size. It is done by removing the unimportant portions of the model weights.

As weight matrix is becoming sparse, it is important to use sparse matrix multiplication to reduce the computation time. "SpMM" is a special kernel for sparse matrix multiplication, which is used alongside with unstructured pruning.

However, "SpMM" is not efficient for low sparsity level(around 50%) due to storage overhead. This paper proposes "SpInfer", an efficient SpMM kernel that can be applied to even low sparsity level. It uses new encoding format for sparse matrix that aims the tensor core format.

## Presentation Slides

[Presentation Slides](/SpInfer-Seminar/SpInfer_Seminar.pdf)

## Key Points

1. TCA-BME(Tensor-Core-Aware Bit Map Encoding): New encoding format for sparse matrix that aims the tensor core format.
2. SMBD(Shared Memory Bitmap Decoding): Decode the sparse matrix into dense matrix when loading the data from shared memory to register.

+++
title = "HW-Compatible Quantization Scheme"
slug = "hw-compatible-quantization-scheme"
date = 2026-02-20
+++

# Summary

There are many quantization schemes: per-Tensor, per-Token, per-Channel, group-wise, etc.

Can we apply any quantization scheme to Activation and Weight to achieve better performance?
The answer is no. Quantization scheme should be selected carefully, and need to be compatible with hardware.

**In this post, I will only focus on symmetric quantization scheme.**

## Preliminaries

## per-Tensor Quantization

In per-tensor quantization, we quantize the entire tensor using a single scale factor.
![Figure of per-Tensor Quantization](per-tensor-quantization.png)

## per-Token Quantization

In per-token quantization, we quantize each token of the tensor using a different scale factor.(We usually refer `token` as the input dimension of the activation matrix)
![Figure of per-Token Quantization](per-token-quantization.png)

## per-Channel Quantization

In per-channel quantization, we quantize each channel of the tensor using a different scale factor.(We usually refer `channel` as the output dimension of the weight matrix)
![Figure of per-Channel Quantization](per-channel-quantization.png)

## Checking HW Compatibility

Let's check if each of the quantization scheme combination is compatible with HW. I will assume that you can implement basic GEMM CUDA Kernel, and know the roles of Tensor Core.

## GEMM without Quantization

![GEMM without Quantization](gemm-without-quantization.png)

## per-Tensor Quantization for Activation and Weight(HW-Compatible)

In this case, we can implement GEMM in the following way:
![GEMM with per-Tensor Quantization](gemm-with-per-tensor-quantization.png)

## per-Token Quantization for Activation and per-Channel Quantization for Weight(HW-Compatible)

In this case, we can implement GEMM in the following way:
![GEMM with per-Token and per-Channel Quantization](gemm-with-per-token-and-per-channel-quantization.png)

## per-Channel Quantization for Activation and per-Token Quantization for Weight(HW-Incompatible)

In this case, using INT8 Tensor Core is not possible. We cannot dequantize the INT8 Output matrix into FP16. This is because scale information gets mixed up.

![GEMM with per-Channel and per-Token Quantization](gemm-with-per-channel-and-per-token-quantization.png)

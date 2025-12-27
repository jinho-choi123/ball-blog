+++
title = "Tree allReduce Algorithm"
slug = "tree-all-reduce"
date = 2025-12-26
+++

# Summary

Previously, we discussed about [ring allReduce algorithm](@/Computer-System/ring-all-reduce/index.md)

Ring allReduce benefits by utilizing the full bandwidth of the links. However, as device number grows, the latency increases **linearly**.

In this post, we will discuss about tree allReduce algorithm, which reduces the latency to **logarithmic** time.

## Assumption

Assume we have 7 devices, and we want to perform allReduce operation. Each device is connected to PCIe switch, and the link is full-duplex with 60GB/s bandwidth.

![basic-structure](basic-structure.png)

We assume each device has 60GB of data.

## Naive Tree Algorithm

Tree allReduce algorithm starts by making a binary-tree structure with the devices.
![binary-tree](binary-tree-structure.png)

Starting from the leaf nodes, each device sends its data to the parent node. And the parent node performs the reduce operation. After the final reduce operation at the root node, the result is broadcasted to all the devices.

Assume $N$ is the number of devices, and data is split into $N$ chunks.

**Black-contour data chunks** are the original data, **red-contour data chunks** are the intermediate results, and **blue-contour data chunks** are the final allReduce result.

Note that this algorithm works in pipeline fashion.

![tree-allreduce-algorithm](naive-tree-algorithm.png)

> The timestamp jumps $2/N$ or $3/N$ when two or three devices try to send $1/N$ size data to single device.
> This is because there is only a single link between PCIe switch and device.

## Time Complexity of Naive Tree Algorithm

In general, for $N$ devices and $B$bytes/s bandwidth, tree allReduce algorithm takes $3+2*3* \log_2(N)/N$ time to complete the allReduce operation for $B$ bytes of data.

The pipeline tail latency takes $2*3*\log_2(N)/N$ time.

After the pipeline is filled, it takes $N*3/N=3$ time to complete the allReduce operation.

> Rewind: Ring allReduce algorithm takes $2 * (N-1) / N$ time to complete the allReduce operation for $B$ bytes of data.

As you can see, naive tree algorithm takes longer time than ring algorithm. Let's refine it!

## Downside of Naive Tree Algorithm

The downside of the naive tree algorithm is the discrepancy between leaf nodes and non-leaf nodes.

Assume the pipeline is filled.

![downside-of-naive-tree-algorithm](downside-of-naive-tree-algorithm.png)

The leaf node has $1/N$ inbound data and $1/N$ outbound data.

The non-leaf node has $3/N$ inbound data and $3/N$ outbound data.

![compare-leaf-nonleaf-data](compare-leaf-nonleaf.png)

As a result, the leaf node has to wait for the non-leaf node to finish the transmission. This waiting occurs because the data transmission pattern is not balanced between leaf nodes and non-leaf nodes.

Let's look at the refine tree algorithm, double-tree algorithm.

## Double-Tree Algorithm

Double-tree algorithm is a refinement of the naive tree algorithm. It aims to balance the data transmission pattern between leaf nodes and non-leaf nodes.

Double tree algorithm makes two trees(Tree 1 and Tree 2). The leaf nodes of Tree 1 are non-leaf nodes in Tree 2. And the non-leaf nodes of Tree 1 are leaf nodes in Tree 2.

The data is evenly divided into two chunks.(Chunk 1 and Chunk 2)

Tree 1 processes the allReduce of Chunk 1, and Tree 2 processes the allReduce of Chunk 2.

![double-tree-algorithm](double-tree-algorithm.png)

## Time Complexity of Double-Tree Algorithm

In general, for $N$ devices and $B$bytes/s bandwidth, tree allReduce algorithm takes $2+2*4* \log_2(N)/N$ time to complete the allReduce operation for $B$ bytes of data.

The pipeline tail latency takes $2*4*\log_2(N)/N$ time.

After the pipeline is filled, it takes $(N/2)*4/N=2$ time to complete the allReduce operation.

> You may notice that the time complexity of double-tree algorithm is not better than ring algorithm. This is absolutely correct. But in small data size(where pipeline is barely filled), double-tree algorithm has much better latency than ring algorithm.

Figure below shows the comparison between ring algorithm and double-tree algorithm. It is evaluated with 8 bytes of data.(which is extremely small, and double-tree algorithm's pipeline will not be filled in this case). Given small datasize, the time complexity of ring algorithm is $O(N)$, and the time complexity of double-tree algorithm is $O(\log_2(N))$.

![compare-ring-tree](compare-ring-tree.png)

## References

[1] [https://developer.nvidia.com/blog/massively-scale-deep-learning-training-nccl-2-4/](https://developer.nvidia.com/blog/massively-scale-deep-learning-training-nccl-2-4/)

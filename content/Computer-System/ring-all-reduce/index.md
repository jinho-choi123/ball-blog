+++
title = "Ring allReduce Algorithm"
slug = "ring-all-reduce"
date = 2025-12-22
+++

# Summary

AllReduce is a collective communication operation that performs the reduce function for all the data.
![allReduce](all-reduce.png)

Let's talk about the most efficient algorithm for allReduce. How should the data move?

## Assumption

Assume we have 3 devices, and we want to perform allReduce operation. Each device is connected to PCIe switch, and the link is full-duplex with 60GB/s bandwidth.
![basic-structure](basic-structure.png)

We assume each device has 180GB of data.

## Naive Approach

The naive approach is to collect all the data to single device, and then perform the reduce operation. After that, we send the result to all the other devices.

![naive-algorithm](naive-algorithm.png)

Naive approach takes 12 seconds to complete the allReduce operation.

Big problem with naive approach is that it doesn't utilize the full bandwidth of the links.

## Ring Approach

The ring approach is to let each device send its data to the next device in the ring.

![ring-algorithm](ring-algorithm.png)

Ring approach takes 4 seconds to complete the allReduce operation.

As you can see, ring approach utilizes the full bandwidth of the links.

## Time Complexity of Ring AllReduce Algorithm

In general, for $N$ devices and $B$bytes/s bandwidth, ring approach takes $2 * (N-1) / N$ time to complete the allReduce operation for $B$ bytes of data.

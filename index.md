---
layout: page
title: OmpCloud
permalink: /
---

OmpCloud is a toolset that allows you to use the cloud as an OpenMP offloading device.

## How it works

The version 4.0 of the OpenMP standard introduces new directives that enable the transfer of computation to heterogeneous computing devices (e.g. GPUs or DSP). We use this programming model to transfer computation to a datacenter. Thus the cloud is considered as yet another target device available from the local computer, giving the programmer the ability to quickly expand the computational power of its own computer to a large-scale cloud cluster.

As an example, the code below presents a simple program loop describing a matrix multiplication which was annotated with OpenMP directives in order to offload the computation to the cloud. In the OpenMP abstract accelerator model, the *target* clause defines the portion of the program that will be executed by the target device. The *map* clause details the mapping of the data between the host and the target device:  inputs (A and B) are mapped *to* the target, and the output (C) is mapped *from* the target.

{% highlight C %}
int MatMul(float *A, float *B, float *C) {
  // Offload code fragment for acceleration on the cloud cluster
  #pragma omp target device(CLOUD)
  #pragma omp map(to: A[:N*N], B[:N*N]) map(from: C[:N*N])
  // Execute loop iterations in parallel on the cluster
  #pragma omp parallel for
  for(int i=0; i < N; ++i)
    for (int j = 0; j < N; ++j)
      C[i * N + j] = 0;
      for (int k = 0; k < N; ++k)
        C[i * N + j] += A[i * N + k] * B[k * N + j];
  // Resulted matrix 'C' is available locally
  return 0;
}
{% endhighlight %}

You can look at [Unibench](https://github.com/ompcloud/Unibench) repository if you want to see more examples.

## Screencast Demo

Here is a recorded video demonstrating cloud offloading from a simple laptop to a Spark cluster created on Amazon Web Service.

<div class="embed-responsive embed-responsive-16by9">
  <iframe class="embed-responsive-item" width="560" height="315"
    src="https://www.youtube.com/embed/R5UhzOVrohU" frameborder="0"
    allowfullscreen="">
  </iframe>
</div>

## Documentation, Installation, Configuration

All the information is provided [in the Wiki](https://github.com/ompcloud/ompcloud/wiki).

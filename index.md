---
layout: page
title: OmpCloud
permalink: /
---

OmpCloud is a toolset that allows you to use the cloud as an OpenMP offloading device.

## How it works

The version 4.0 of the OpenMP standard introduces new directives that enable the transfer of computation to heterogeneous computing devices (e.g. GPUs or DSP). We use this programming model to transfer computation to a datacenter. Thus the cloud is considered as yet another target device available from the local computer, giving the programmer the ability to quickly expand the computational power of its own computer to a large-scale cloud cluster.

As an example, the code below presents a simple program loop describing a matrix multiplication which was annotated with OpenMP directives in order to offload the computation to the cloud. In the OpenMP abstract accelerator model, the *target* clause defines the portion of the program that will be executed by the target device. The *map* clause details the mapping of the data between the host and the target device:  inputs (A and B) are mapped *to* the target, and the output (C) is mapped *from* the target.

```
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
```

You can look at [Unibench](https://github.com/ompcloud/Unibench) repository if you want to see more examples.

## Get Started

The fastest way to start using OpenMP cloud offloading is to download and use our Docker image in a container.
After installing [Docker](https://docs.docker.com/engine/installation/), you can download, run and access a container on your machine using:

```
docker run --name ompcloud-test -d ompcloud/ompcloud-test:latest /sbin/my_init
docker exec -i -t ompcloud-test /bin/bash
```

Finally, our benchmark test can be executed by running the following scripts inside the container shell:

```
$CLOUD_CONF_DIR/ompcloud-quicktests.sh
```

Our test are run locally within the container using Spark and HDFS servers that was already setup. To see the current configuration of our cloud offloading runtime, run the command:

```
cat $CLOUD_CONF_PATH
```

In fact, the environment variable `$CLOUD_CONF_PATH` defines the path of the configuration file to use when offloading the computation. This way, you can easily switch from one configuration to another by changing its value.

## Advanced Usage

### Compile your own application

Our toolset relies on custom versions of LLVM and Clang that are available [here](https://github.com/ompcloud). To use our cloud offloading workflow for your own program, you can compile your code by running something like:

```
clang -fopenmp -omptargets=x86_64-unknown-linux-spark myapp.c
```

### Use an AWS cluster

If you want to run an application on AWS, you need to setup your cluster. The cluster can be easily instantiate using [*cgcloud*](https://github.com/ompcloud/cgcloud), the  command is directly available in the container. Then you need to configure the credential to allow your application to offload computation to the cluster. You can find some examples of the configuration file [here](https://github.com/ompcloud/ompcloud-docker/tree/master/config-rtl-examples).

### Update to last git version

Our toolset is still very experimental, so it is probably a good idea to update it regularly. Update and recompilation within your container can be easily performed by running the following script:

```
$CLOUD_CONF_DIR/ompcloud-updatetools.sh
```

Another way is to update the docker image from the host using:

```
docker pull ompcloud/ompcloud-test:latest
```

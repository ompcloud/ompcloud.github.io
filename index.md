---
layout: page
title: OmpCloud
permalink: /
---

OmpCloud is a toolset that allows to use the cloud as an OpenMP offloading device.

## Get Started

The fastest way to start using OpenMP cloud offloading is to download and use our Docker image in a container.
After installing Docker, you can download and run a container on your machine using:

```
docker run --name ompcloud-test -it ompcloud/ompcloud-test:latest /sbin/my_init
```

Then, the container shell can be accessed using:

```
docker exec -i -t ompcloud-test /bin/bash
```

Finally, our benchmark test can be executed by running the following scripts within the container:

```
$CLOUD_CONF_DIR/ompcloud-updatetools.sh
$CLOUD_CONF_DIR/ompcloud-quicktests.sh
```

Our test are run locally within the container using Spark and HDFS servers that was already setup. To see the current configuration of our cloud offloading runtime, run the command:

```
cat $CLOUD_CONF_PATH
```

## Advanced Usage

If you want to run benchmark test on AWS, you need to setup your cluster and configure the credential.

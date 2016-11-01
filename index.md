---
layout: page
title: OmpCloud
permalink: /
---



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
/opt/scripts/update_ompcloud_tools.sh
/opt/scripts/run_ompcloud_test.sh
```

Our test are run locally in Spark and HDFS server already setup within the container. To see the current configuration of our cloud offloading runtime, run the command:

```
cat $CLOUD_CONF_PATH
```

## Advanced Usage

If you want to run benchmark test on AWS, you need to setup your cluster and configure the credential.

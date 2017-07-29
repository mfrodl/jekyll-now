---
layout: post
title: Getting started with Hadoop on Fedora 25
---

This tutorial will show you how to install Hadoop on Fedora 25, start a single-node cluster and run a simple MapReduce task on it.

First, install hadoop RPMs from repositores:

```
sudo dnf install hadoop-common hadoop-common-native hadoop-hdfs hadoop-mapreduce hadoop-mapreduce-example hadoop-yarn
```

Initialize the HDFS directories:
```
sudo hdfs-create-dirs
```

Start a single-node cluster:
```
sudo systemctl start hadoop-namenode hadoop-datanode hadoop-resourcemanager hadoop-nodemanager
```

Now, we need to create a directory in HDFS for the user . Chances are you will want to keep things simple and just use your own account, i.e. the one you are logged in to. In that case, just run the following two commands:
```
sudo runuser hdfs -s /bin/bash /bin/bash -c "hadoop fs -mkdir /user/$USER"
sudo runuser hdfs -s /bin/bash /bin/bash -c "hadoop fs -chown <name> /user/$USER"
```
If you'd rather use a separate account, make sure to replace `$USER` with an appropriate user name.

Now, run one of the prepackaged MapReduce examples, which calculates an approximate value of pi:
```
hadoop jar /usr/share/java/hadoop/hadoop-mapreduce-examples.jar pi 10 1000000
```

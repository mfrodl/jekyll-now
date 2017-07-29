---
layout: post
title: Getting started with hadoop on Fedora 25
---

First, install hadoop RPMs from repositores:

```
sudo dnf install hadoop-common hadoop-common-native hadoop-hdfs hadoop-mapreduce hadoop-mapreduce-example hadoop-yarn
```

Initialize the [HDFS](glossary#HDFS) directories:
```
sudo hdfs-create-dirs
```

Start the single-node cluster:
```
sudo systemctl start hadoop-namenode hadoop-datanode hadoop-resourcemanager hadoop-nodemanager
```

Create a directory for the user running the tests:
```
runuser hdfs -s /bin/bash /bin/bash -c "hadoop fs -mkdir /user/<name>"
runuser hdfs -s /bin/bash /bin/bash -c "hadoop fs -chown <name> /user/<name>"
```

Now, run one of the prepackaged [MapReduce](glossary#mapreduce) examples:
```
hadoop jar /usr/share/java/hadoop/hadoop-mapreduce-examples.jar pi 10 1000000
```

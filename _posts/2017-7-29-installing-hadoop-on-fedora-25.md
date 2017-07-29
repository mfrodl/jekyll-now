---
layout: post
title: Getting started with Hadoop on Fedora 25
---

This tutorial will show you how to install Hadoop on Fedora 25, start a single-node cluster and run a simple MapReduce task on it.

Our goal is to make the setup as simple as possible. While there are dozens of quickstart tutorials on this topic available on the Internet, some manual preparation is typically needed before you can start playing with Hadoop. You need to install a supported version of Java, download and extract Hadoop tarball somewhere on your filesystem, set appropriate permissions and configure environmental variables. This is fine if you like to tweak things and mould them to your own image. However, if you'd rather get straight down to business and not care too much about what lies underneath, you'll be glad to know there is a simpler way. All that stuff above is taken care of when you install Hadoop RPMs available from Fedora's official DNF repositories.

So, let's install some of the basic packages:
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

Now, we need to create a directory in HDFS for the user . For simplicity, you can just use your own account, i.e. the one you are logged in to:
```
sudo runuser hdfs -s /bin/bash /bin/bash -c "hadoop fs -mkdir /user/$USER"
sudo runuser hdfs -s /bin/bash /bin/bash -c "hadoop fs -chown <name> /user/$USER"
```
If you'd rather use a separate account, make sure to replace `$USER` with an appropriate user name.

Now, run one of the prepackaged MapReduce examples, which calculates an approximate value of pi:
```
hadoop jar /usr/share/java/hadoop/hadoop-mapreduce-examples.jar pi 10 1000000
```

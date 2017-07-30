---
layout: post
title: Getting started with Hadoop on Fedora Linux
---

This tutorial will show you how to install Hadoop on Fedora, start a single-node psuedo-distributed cluster and run a simple MapReduce task on it.

Before we start
----------------
Our goal is to make the setup as simple as possible. While there are dozens of quickstart tutorials on this topic available on the Internet, some manual preparation is typically needed before you can start playing with Hadoop. You need to install a supported version of Java, download and extract Hadoop tarball somewhere on your filesystem, set appropriate permissions and configure environmental variables. This is fine if you like to tweak things and mould them to your own image. However, if you'd rather get straight down to business and not care too much about what lies underneath for now, you'll be glad to know there is a simpler way. All that stuff above is taken care of when you install Hadoop RPMs available from Fedora's official DNF repositories.

Prerequisites
-------------
All you need is a working Fedora 25 (Workstation or Server) installation with a user account. That's it. No need to configure extra repositories or check which packages are installed on the system. If you are logged in to your Fedora user account (well, and haven't messed up the system terribly quite yet), you are good to go.

Installation
------------
In its most basic flavour, Hadoop consists of three core components: **HDFS (Hadoop Distributed File System)**, **MapReduce** and **YARN (Yet Another Resource Negotiator)**. Each of these is packaged as a separate RPM in Fedora repositories. Let's install them by issuing:
```
# dnf install hadoop-hdfs hadoop-mapreduce hadoop-yarn
# dnf install hadoop-common-native hadoop-mapreduce-examples
```

The HDFS framework provides a permanent storage for other Hadoop services. It is a distributed file system where MapReduce jobs load their input data from  are and where their outputs are saved to.

Initialize the HDFS directories:
```
# hdfs-create-dirs
```

Start a single-node cluster:
```
# systemctl start hadoop-namenode hadoop-datanode hadoop-resourcemanager hadoop-nodemanager
```

Now, we need to create a directory in HDFS for the user . For simplicity, you can just use your own account, i.e. the one you are logged in to:
```
# runuser hdfs -s /bin/bash /bin/bash -c "hadoop fs -mkdir /user/$USER"
# runuser hdfs -s /bin/bash /bin/bash -c "hadoop fs -chown <name> /user/$USER"
```
If you'd rather use a separate account, make sure to replace `$USER` with an appropriate user name.

Now, run one of the prepackaged MapReduce examples, which computes an approximate value of pi:
```
$ hadoop jar /usr/share/java/hadoop/hadoop-mapreduce-examples.jar pi 10 1000000
```

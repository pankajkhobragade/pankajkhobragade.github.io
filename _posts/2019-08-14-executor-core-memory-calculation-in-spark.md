---
title: 'Executor,core and memory calculation for spark job'
date: 2019-08-14
permalink: /posts/2019/08/blog-executor-core-memory-calculation-in-spark/
tags:
  - spark
  - executors
  - cores
  - Memory
---
How spark job runs?
======
Before we jump on the calculation of executors, cores and memory required for the spark job, lets look at the some of the basics of spark job.

Task :
------
A task is the unit work which is executed by executor.

Executor:
-------
An executor is a JVM process which is launched for the application and runs on the worker node. The executor is responsible for running the task and keeping the data in memory. Each application can have multiple executors. A single node can have multiple executors running. And single executor can span over multiple nodes.

Core:
--
A core is basic computation unit of the processor. CPU may have one or more cores to perform the tasks. The more cores that means more tasks can be run in prarallel.

Steps involved in cluster mode for a Spark Job:
---
* From the driver code, SparkContext connects to cluster manager (standalone/Mesos/YARN).
* Cluster Manager allocates resources across the other applications. Any cluster manager can be used as long as the executor processes are running and they communicate with each other.
* Spark acquires executors on nodes in cluster. Here each application will get its own executor processes.
* Application code (jar/python files/python egg files) is sent to executors
* Tasks are sent by SparkContext to the executors.
![spark-job](/images/spark-job.png)
 

How to calculate executors and memory
======

Aren't headings cool?
------

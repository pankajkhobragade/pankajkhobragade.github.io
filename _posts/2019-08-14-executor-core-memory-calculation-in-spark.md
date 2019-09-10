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
Before we jump on the calculation of executors, cores and memory reqired for the spark job, lets look at the some of the basics of spark job.

Task :
------
A tasks is the unit work which is executed by executor.

Executor:
-------
An executor is a JVM process which is launched for the application and runs on the worker node. The executor is responsible for running the task and keeping the data in memory. Each application can have multiple executors. A single node can have multiple executors running. And single executor can span over multiple nodes.

Core:
-----
A core is basic computation unit of the processor. CPU may have one or more cores to perform the tasks. The more cores that means more tasks can be run in prarallel.

![spark-job](/images/spark-job.png)
 

How to calculate executors and memory
======

Aren't headings cool?
------

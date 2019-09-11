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
 

How to calculate executors and memory :
======

No of cores ( --executor-cores):
----
No of cores assigned for any executors,means that executor can run that many tasks in parallel.
Through the research its been found that any application with more than 5 concurrent tasks leads to bad performance.
So we would take cores to 5 for the calculations.
So it gives us executor-cores = 5.

No of Executors (--num-executors):
---
Available executors = ( total cores of a node) * (no of nodes in cluster) / (5 /*--core from above calculation*/)
no of exectors for job = Available executors - 1

We are reducing one from total available executors bevause we need one executor dedicated to yarn job demon running in the clustor.
So it gives us num-executors = {( total cores of a node) * (no of nodes in cluster) / (5 /*--core from above calculation*/)} - 1

executor-memory :
---
total executor memory = (node memory - 1 GB) /(executors per node)
                = (node memory - 1 GB) /(num-executors /(no of nodes))

We need to also consider the garbage collection, memory overhead. So 0.07 of memory should be kept for it with that the formula becomes like below. And 1 GB is kept for the hadoop demons running in the node.

executor-memory = total executor memory - (0.07 * (total executor memory))

Example :
====
clustor details :

* Total nodes - 10
* Cores on each node = 32
* Ram on each node = 128 GB

So our param values calculation is like below :
executor-cores= 5
num-executors = {(32 * 10)/ 5}-1 = 63
executor-memory = {(64 -1 )/(63/10)} * 0.93 it comes to nearly 9 GB


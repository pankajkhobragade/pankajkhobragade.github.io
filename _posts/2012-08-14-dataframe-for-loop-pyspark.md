---
title: 'easy way to loop through the dataframe in spark'
date: 2019-08-14
permalink: /posts/2019/08/dataframe-loop/
tags:
  - dataframe
  - pyspark
  - loop in dataframe
  - spark dataframe
---
There are many ways to loop through the dataframe, but one of the easiest way to loop through the dataframe is like below.

<pre>
 df = spark.sql("Select id,name from emp")
 for row in df.collect():
     print ( row.id)
</pre>


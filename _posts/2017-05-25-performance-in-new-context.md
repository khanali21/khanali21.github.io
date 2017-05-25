---
layout: single
title: "Code Performance and Resource Consumption Testing "
excerpt: "With Serverless/Function as a Service (FaaS) becoming mainstream the Performance & Resource Consumption testing has become ever more important"
tags: [docker, devops, initializr, automation-that-works]
image:
  feature: performance-chart.png
  credit: 
  creditlink: 
comments: true
---

In those old days of bloated Java EE applications, the performance testing objectives were clear and straightforward. 
One of the key objective for a Java EE web application was the transaction response time when running under load, where load was defined as the number of concurrent users accessing the system.
Another of those key objective was to ensure that the memory usage by the application does not cause dreaded OOM on the Web Application Server when running under load. And here the load could be defined as anything from number of concurrent users to the number of threads executing in the JVM when performing some scheduled task.
Similarly another key objective of the performance testing was to do a simple benchmark in order to help devise capacity plan for the application.
These objectives were clear and the results were not only quantifiable but also assertable during the performance test execution.

µServices Architecture however changed the whole performance testing approach. µService application have very small footprint in terms of memory by default. And by definition they perform usually a fine grained single functionality.        
  
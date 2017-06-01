---
layout: single
title: "Code Performance and Resource Consumption Testing "
excerpt: "With Serverless/Function as a Service (FaaS) becoming mainstream the Performance & Resource Consumption testing has become ever more important"
tags: [aws-lambda, devops, automation-that-works]
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

µServices Architecture however changed the whole performance testing approach. µService application has very small footprint in terms of memory by default. And by definition they are fine grained enough to optimally use the system resources (cpu, disk and network).
This is the reason that when devising the performance testing plan for testing a single µService, usually test engineers focus only on REST API benchmarking or struggle with UI performance testing.
With Serverless/Function-as-a-Service (FaaS) becoming mainstream, performance testing has taken a whole new perspective. The code performance now not only contributes to overall user experience particularly when running under stress but now you have to pay the real dollars in terms of performance penalty to your cloud provider.
The objective of the performance testing, take a new perspective i.e. Optimization. There is however a delicate balancing required when optimizing the code to run as serverless, over-optimisation and premature optimisation is declared as root of all evils by DonaldKnuth.   
When developing code that is designed to execute in a serverless environment, the optimization starts right from the developer's machine and/or local test environment running as docker container or VM. 

Profiling becomes an important aspect of Quality Assurance Criteria as part of the CI pipeline. Developer may run the profiling tools as part of the local pipeline to identify the spikes and regression in the code performance covering CPU and Memory usage. For the critical functions you may want to look at the System Interrupt Calls, I/O calls.
As part of the pipeline, the tools like AppDynamics, NewRelic etc. are also very useful to profile application under load and identify defects and/or bottlenecks. 
Similarly your cloud provider may provide the profiling tools when the code is running on the cloud. AWS Lambda dashboard provides profiling single lambda function CPU performance & CloudWatch filter for monitoring memory consumption.
 
 
  
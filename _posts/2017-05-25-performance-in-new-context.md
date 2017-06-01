---
layout: single
title: "FaaS/Serverless: Code Performance and Resource Consumption Testing "
excerpt: "With Serverless/Function as a Service (FaaS) becoming mainstream the Performance & Resource Consumption testing has become ever more important"
tags: [aws-lambda, devops, faas, serverless, performance-testing, automation-that-works]
image:
  feature: performance-chart.png
  credit: 
  creditlink: 
comments: true
---

## Monolith Java/J2EE Applications

### Transaction Response Time

In those old days of bloated Java EE applications, the performance testing objectives were clear and straightforward. 
One of the key objective for a Java/J2EE web application was the **transaction response time** when running under load, where load is defined as the number of concurrent users accessing the system at any given time.
Tools like LoadRunner, Rational Performance Tester, JMeter provide excellent graph analysis for the transaction response times correlating them with the load and other performance indicators.

### Memory Footprint

Another of those key objective was **memory utilization** analysis to ensure that the memory usage by the application does not cause dreaded OOM on the Web Application Server when running under load. Here the load could be defined as anything from number of concurrent users to the number of threads executing in the JVM when performing some scheduled task.
In case of J2EE applications, memory utilization is the source of majority of the bugs that are hard to reproduce and regressed. Monolith J2EE applications running inside the Web Application Servers are prone to such issues in production. The dreaded Out-Of-Memory (OOM) error would require all kinds of heap analysis during testing and even in production.
Since monolith applications had very large memory footprint, producing the gigabytes of heap dumps and then analysing them required a lot of effort and resources.

### Benchmarking

Similarly another key objective of the performance testing was to do a **simple benchmark** in order to help devise capacity plan for the application. In case of monolith application benchmarking required a careful setup for the initial resources. Benchmarking on an inappropriate hardware and software configuration would lead to conflicting and meaningless results.

Despite all the complexity and effort involved in performing performance and reliability testing, these **objectives were clear and attainable**. The results were not only **quantifiable but also assertable** during the performance test execution.

## µServices

µServices Architecture however changed the whole performance testing approach. µService application has very small footprint in terms of memory by default, which makes it easier for the performance engineers to do memory consumption analysis. 
By definition µServices are fine grained enough to use the system resources (cpu, disk and network) in a consistent manner. The resource utilisation analysis with the help of appropriate monitoring tools makes it possible to integrate the performance testing as part of **Continuous Integration pipeline** by **shifting** the performance testing as **left** as possible.
As the bulk of functional logic moved towards the UI (browser), the JavaScript frameworks like AngularJS allowed the user to implement MVC on the client side, a new challenge emerged for the performance engineers. 
The **UI performance testing** e.g. rendering of the large list with infinite scrolling, number of concurrent ajax calls, page size etc. become important aspects of application performance.  
This is the reason that when devising the performance testing plan for testing a single µService, usually performance test engineers focus on REST API benchmarking or struggle with UI performance testing.

## Serverless/Function-as-a-Service

With Serverless/Function-as-a-Service (FaaS) becoming mainstream, performance testing has taken a whole new perspective. The code performance now not only contributes to overall user experience but now you have to pay the real dollars in terms of performance penalty to your cloud provider. FaaS providers like AWS Lambda charge based on how your code performs on their cloud, the difference between an optimally performing code and non-optimally performing code could be thousands of dollars (may be more).
 

### Optimization

The objective of the performance testing, take a new perspective i.e. Optimization. There is however a delicate balancing required when optimizing the code to run as serverless, over-optimisation and premature optimisation is declared as root of all evils by DonaldKnuth.   
When developing code that is designed to execute in a serverless environment, the optimization starts right from the developer's machine and/or local test environment running as docker container or VM. 

### Profiling

Profiling becomes an important aspect of Quality Assurance Criteria as part of the **CI pipeline**. Developer may run the profiling tools as part of the l**ocal pipeline** to identify the spikes and regression in the code performance covering CPU and Memory usage. For the critical functions you may even want to look at the System Interrupt Calls, I/O calls.
As part of the CI pipeline, the tools like AppDynamics, NewRelic etc. are also very useful to profile application under load and identify defects and/or bottlenecks. 
Similarly your cloud provider may provide the profiling tools when the code is running on the cloud. **AWS Lambda dashboard** provides profiling single lambda function CPU performance & **CloudWatch filter** for monitoring memory consumption.

_Performance (including Reliability) engineering is evolving as the application architecture is changing, in the coming years, new tools and approach will be needed for PRE (Performance & Reliability Engineering). I am keenly following the developments in this area. Get in touch if you share the interest._

## References:

[Load Testing your API](https://www.3scale.net/2015/04/how-to-load-test-and-tune-performance-on-your-api-part-i/)

[AWS Lambda Pricing](https://aws.amazon.com/lambda/pricing/)

[AWS Lambda Load Testing Harness](https://aws.amazon.com/blogs/compute/serverless-testing-with-aws-lambda/)

 
 
  
---
layout: single
title: "Performance Testing AWS lambda functions "
excerpt: "Performance Testing lambda functions is a significant challenge. Testing it locally is even bigger challenge. And interpreting the results from the tests that can be associated with the actual cost makes performance testing λ non-trivial. I discuss here how to overcome these challenges"
tags: [aws-lambda, devops, faas, serverless, performance-testing, automation-that-works, ruby-jmeter, docker-lambda, serverless-plugin-simulate]
image:
  feature: cloudwatch.png
  credit: 
  creditlink: 
comments: true
---
* [Introduction](#introduction)
* [Understanding AWS lambda cost](#understanding_lambda_cost)
* [Performance Test Approaches](#testapproaches)
* [Testing on AWS Lambda](#testingonaws)
* [Testing Locally (Simulating AWS Lambda locally)](#testinglocally)
* [Nodejs Sample Application to Test](#sampleapp)
* [Writing Performance Benchmark Test](#performancetests)
* [Executing Performance Benchmark](#executingbenchmark)
* [Performance Test Results](#testresults)
* [Estimating Costs ](#estimatingcosts)
* [Performance Tuning](#performancetuning)

<a name="introduction"></a>
## Introduction
For the first time in software development performance penalty is directly associated with the actual dollars. Performance of the application is no more a "Non-Functional Requirement" which if not met will only result in some disgruntled customers complaining about slow response time.
Unfortunately now a developer can not just recommend upgrading the server to compensate for a non performant piece of code. 
FaaS has brought us back to the days when you would optimize your code to run on a 512MB RAM. You would profile your code to look for excessive CPU utilization which may make your application unable to run on 486 machine.
Writing a non performance piece of code has a price, the difference could be paying $10 or $5000.
To understand this let us understand as an example AWS Lambda pricing model.

<a name="understanding_lambda_cost"></a>
## Understanding AWS lambda cost

### Factors impacting AWS Lambda Cost
There three main factor impacting AWS Lambda cost.

**a. Number of Executions:**
Number of times your λ functions executed during a month. I will discuss further how the design of your serverless services impact the number of executions of λ function.   

**b. Memory Usage:**
Memory required by your λ functions. It is important to note that AWS charges the memory in chunks of 128, 256, 512 upto 1536.
This makes it very hard to optimize your λ function for memory utilization. I will discuss that in detail later in the article, to give you an early hint, consider a λ function that usually requires <100MB however every now and then (say one every 3 minutes (or one every 300 requests)) for a particular request the memory usage jumps to 1024MB due to certain data being processed by this particular request. Obviously you would need to allocate 1024MB memory to your Lambda function. 
However you can design two functions one needing <128M for every invocation and one needing ~1024M. You just doubled the number of executions required.

**c. CPU time:**
Third factor is the time it took for single execution of your function from the invocation to completion. The way CPU time is inter-connected with the memory allocation (due to Garbage Collection) in java and javascript (nodejs) based applications, makes it even harder to optimize your application. More on this later in the article.

### AWS lambda cost components
There are two components to the AWS lambda costs.

1. **Invocations:**
Invoking your lambda functions has an associated cost. Currently the first one million requests per month are free. Thereafter you pay $0.20 per million request.
This has direct impact on the architecture and size of your λ functions. You may either have multiple λ functions chained to execute a single request or a single large function serving all the requests.
A fat (large) λ function is not necessarily wrong, the memory footprint of your λ function more often then not is dependent on the data loaded from external sources (e.g. DynmoDB table).
Having multiple chained functions requiring the same data can increase the cost dramatically.
On the other hand, a fat λ function may be an anti-pattern for your application architecture. In case of a µServices based architecture, the granularity of the µServices in the system is one of the key architecture decision as this increases the complexity of your architecture.
It is very tempting to create a one-to-one mapping between your µService and λ function. However this may not be the right design. I will discuss this in a separate paper. 

2. **GB-Seconds:**
The biggest component of the AWS lambda cost is GB-seconds. This is dependent on all three factors number of execution, memory allocation and CPU time. 
Calculating GB-seconds with the number of invocations for your λ function is trivial in itself. The data reported by the AWS lambda monitoring tells you the number of millisecond each invocation of your lambda function took. 
Formula for calculating the GB-Seconds is 
GB-s = (Number of Execution * Memory allocated for the function)/1024

### Cost Prediction Model
Developing a cost prediction for application consisting of several serverless functions requires careful estimates based on proper benchmarking techniques. 
I would discuss the basis for a such a cost prediction model in a separate paper. For the purpose of this document we use a very basic cost prediction model based on the data collected from the benchmark test against single REST endpoint of a sample nodejs λ function.


<a name="testapproaches"></a>
## Performance Test Approaches
I will discuss here two approaches for AWS lambda testing. 

* **Benchmarking with Profiling**

Performance Benchmarking with profiling is the approach to test your λ functions with a reasonable (non-enterprise) dataset locally. 
In the context of RESTapi endpoint benchmarking such test can be written in JMeter. I discuss in detail below the approach write RESTapi benchmark tests using ruby-jmeter and integrating these tests as part of Continuous integration.
Later on based on the benchmark results we do the CPU and memory profiling using standard nodejs profiling techniques and tune the code for optimal performance. And run the benchmark tests again to see the impact on AWS lambda costs.

* **Performance Testing**

A proper performance test of application consisting of several lambda functions has three main pre-requisites.

a. **Appropriate Dataset:** A properly scaled version of the enterprise dataset.

b. **Test Scenario:** Carefully selected test scenario representing the real life user interactions.

c. **Test Infrastructure:**  Running performance test against a real AWS Lambda test environment can be very very expensive, therefore it is imperative to run such test against a properly simulated environment. Which may require local infrastructure set up.   

**_Note: In this article I discuss only benchmarking λ functions._**

<a name="testingonaws"></a>
## Testing on AWS Lambda
AWS Lambda provide a very basic load test harness blueprint, which can be used to create a custom load test harness capable of running a load test which is essentially a unit test with number of iteration to be executed.
Instead of using the AWS lambda test harness, you can also create a test harness consisting of your custom Jmeter tests and run them against real AWS lambda environment.
Performance Testing against AWS lambda would however mean you pay for the test too. One of the desirable consequence of testing against real AWS Lambda environment is that it can be helpful in your cost estimation model for the application.

<a name="testinglocally"></a>
## Testing Locally (Simulating AWS Lambda locally)
Running benchmarks against a simulated AWS Lambda environment on a local infrastructure has several benefits. I will discuss here how to simulate AWS lambda environment using [serverless framework](https://serverless.com/) and [docker-lamda](https://github.com/lambci/docker-lambda).

### serverless framework
[Serverless Framework](https://github.com/serverless/serverless) is a very handy framework for developing serverless functions for AWS Lambda and Google cloud. It also provides a command line tool for workflow automation including testing and deployment.
Here I discuss how use serverless framework to bring up a simulated AWS lambda test environment using docker and local S3, Dynmodb services.
 
### docker-lambda
[docker-lambda](https://github.com/lambci/docker-lambda) is a docker image that replicates a local sandboxed AWS Lambda environment almost identically. It includes all the installed software and libraries, file structure, permissions, environment variables and behaviour of real AWS lambda container.
I will be using nodejs image of docker-lambda to simulate AWS lambda environment.

### serverless-plugin-simulate
[serverless-plugin-simulate](https://github.com/serverless-community-labs/serverless-plugin-simulate) is serverless plugin to replicate Amazon api-gateway to invoke docker-lambda runtime.
The plugin also supports spinning up other Amazon Services like S3, DynmoDB etc. running inside docker container.

### serverless-s3-local
[serverless-s3-local](https://www.npmjs.com/package/serverless-s3-local) is the serverless framework plugin to simuate S3 locally. 

### Local DynmoDB Docker Container
[docker_aws_dynamodb_local](https://github.com/deangiberson/docker_aws_dynamodb_local) is a docker image for running the [DynamoDBLocal.jar](http://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Tools.DynamoDBLocal.html) inside docker container.

### ruby-jmeter
[ruby-jmeter](https://github.com/flood-io/ruby-jmeter) provides an very useful DSL for writing performance test cases which are then translated to Jmeter test plan before executing via Jmeter. 
It is an essential tool for any developer to write simple but powerful Jmeter tests without being exposed to nitty-gritty of [JMeter](http://jmeter.apache.org/).

<a name="sampleapp"></a>
## Nodejs Sample Application to Test
For the purpose of this article I shall be using [an example](https://github.com/serverless/examples/tree/master/aws-node-rest-api-with-dynamodb-and-offline) from serverless framework. I have forked [here](https://github.com/ali-himindz/examples/tree/master/aws-node-rest-api-with-dynamodb-and-offline) and added the tests and updated the configuration to run simulated AWS lambda.
In the subsequent article, I will be using a more concrete application consisting of several AWS lambda functions with a web UI based on a real life application.

<a name="performancetests"></a>
## Writing Performance Tests
stay tuned...

<a name="executingbenchmark"></a>
## Executing Performance Benchmark 
stay tuned...

<a name="testresults"></a>
## Performance Test Results
stay tuned...

<a name="estimatingcosts"></a>
## Estimating Costs 
stay tuned...

<a name="performancetuning"></a>
## Performance Tuning
stay tuned...


## References:

[Load Testing your API](https://www.3scale.net/2015/04/how-to-load-test-and-tune-performance-on-your-api-part-i/)

[AWS Lambda Pricing](https://aws.amazon.com/lambda/pricing/)

[AWS Lambda Load Testing Harness](https://aws.amazon.com/blogs/compute/serverless-testing-with-aws-lambda/)

 
 
  
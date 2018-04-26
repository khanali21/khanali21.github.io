---
layout: single
title: "Performance Testing AWS lambda (λ) functions - (Continued.) "
excerpt: "Performance Testing λ functions is a significant challenge. Testing it locally is even bigger challenge. And interpreting the results from the tests that can be associated with the actual cost makes performance testing λ non-trivial. I discuss here how to overcome these challenges"
tags: [aws-lambda, devops, faas, serverless, performance-testing, automation-that-works, ruby-jmeter, docker-lambda, serverless-plugin-simulate]
image:
  feature: lambda-benchmark.png
  credit: 
  creditlink: 
comments: true
---
* [Executing Performance Benchmark](#executingbenchmark)
* [Performance Test Results](#testresults)
* [Estimating Costs ](#estimatingcosts)
* [Some Questions](#questions)

Read the [Part 1](https://ali-himindz.github.io/performance-testing-%CE%BB-functions/) of this article.

<a name="executingbenchmark"></a>
## Executing Performance Benchmark 


<a name="testresults"></a>
## Performance Test Results
### Ruby-JMeter

Ruby-Jmeter is the **"gem"** , those who have experienced the pain of running the performance tests via LoadRunner and JMeter GUI or have
tried to automate them to include in there CI/CD pipelines will know what a gigantic challenge it is. This gem brings what is lacking in 
JMeter a truly simplified DSL to define your performance/benchmark test. I am not going to discuss what you can do with this DSL. But I 
would use this DSL to define the benchmark tests.

Here are two simple benchmarks for our todo application.

```ruby
require 'ruby-jmeter'
test do
  header [
    { name: 'Content-Type', value: 'application/json' }
  ]
  threads count: 1, loops: 10, scheduler: false do
    get name: 'Get /todos', url: 'http://localhost:3000/todos'
  end
  
end.run( jtl: 'todos_list.jtl')

```

```ruby
require 'ruby-jmeter'
test do
  header [
    { name: 'Content-Type', value: 'application/json' }
  ]
  threads count: 1, loops: 10, scheduler: false do
    post name: 'Post /todos', url: 'http://localhost:3000/todos',
         raw_body: '{ "text": "Learn Serverless" }'
  end
  
end.run( jtl: 'todos_create.jtl')

```

So neat and easy to understand.

### Simulator
```bash
# sls simulate lambda -p 4000
Serverless: Starting mock services.
Serverless: awsnoderestapiwithdynamodbandoffline_dynamodb_1 is up-to-date
awsnoderestapiwithdynamodbandoffline_s3_1 is up-to-date
Serverless: Starting registry with db at /Users/ali/git/aws-node-rest-api-with-dynamodb-and-offline/.sls-simulate-registry
Serverless: Starting registry at: http://192.168.1.232:4000
PUT /functions 200 15.228 ms - -
```


```bash
# sls simulate apigateway -p 3000 --lambda-port 4000

Serverless: Registering 5 functions with http://localhost:4000/functions
Serverless: Starting mock services.
Serverless: awsnoderestapiwithdynamodbandoffline_dynamodb_1 is up-to-date
awsnoderestapiwithdynamodbandoffline_s3_1 is up-to-date
Serverless: [POST /todos] => λ:create
Serverless: [OPTIONS /todos] => CORS origin: * methods: OPTIONS,POST,GET
Serverless: [GET /todos] => λ:list
Serverless: [OPTIONS /todos] => CORS origin: * methods: OPTIONS,POST,GET
Serverless: [GET /todos/{id}] => λ:get
Serverless: [OPTIONS /todos/{id}] => CORS origin: * methods: OPTIONS,GET,PUT,DELETE
Serverless: [PUT /todos/{id}] => λ:update
Serverless: [OPTIONS /todos/{id}] => CORS origin: * methods: OPTIONS,GET,PUT,DELETE
Serverless: [DELETE /todos/{id}] => λ:delete
Serverless: [OPTIONS /todos/{id}] => CORS origin: * methods: OPTIONS,GET,PUT,DELETE
Serverless: Invoke URL: http://localhost:3000
```

### Executing Benchmark Tests
```bash
for f in test/todo*.rb; do ruby $f; done
```

#### Output from Lambda Simulator.

```bash
Serverless: Invoking function todos/create.create
START RequestId: baf88cba-28c8-125c-a5f0-76cedd3fae5a Version: $LATEST
END RequestId: baf88cba-28c8-125c-a5f0-76cedd3fae5a
REPORT RequestId: baf88cba-28c8-125c-a5f0-76cedd3fae5a  Duration: 2356.39 ms    Billed Duration: 2400 ms        Memory Size: 1024 MB    Max Memory Used: 38 MB

{"statusCode":200,"body":"{\"id\":\"ff036980-495d-11e8-9a27-7bb8d3845c02\",\"text\":\"Learn Serverless\",\"checked\":false,\"createdAt\":1524752869399,\"updatedAt\":1524752869399}"}
POST /2015-03-31/functions/serverless-rest-api-with-dynamodb-dev-create/invocations 200 3881.138 ms - 181
Serverless: Looking up serverless-rest-api-with-dynamodb-dev-create in registry
Serverless: Invoking serverless-rest-api-with-dynamodb-dev-create with RequestResponse
Serverless: {"key":"serverless-rest-api-with-dynamodb-dev-create","serviceName":"serverless-rest-api-with-dynamodb","servicePath":"/Users/ali/git/aws-node-rest-api-with-dynamodb-and-offline","region":"us-east-1","stage":"dev","functionName":"create","handler":"todos/create.create","runtime":"nodejs4.3","environment":{"DYNAMODB_TABLE":"serverless-rest-api-with-dynamodb-dev"}}
Serverless: Invoking function todos/create.create
START RequestId: 09bed184-8c01-106a-9da8-8e7ca955a960 Version: $LATEST
END RequestId: 09bed184-8c01-106a-9da8-8e7ca955a960
REPORT RequestId: 09bed184-8c01-106a-9da8-8e7ca955a960  Duration: 2075.48 ms    Billed Duration: 2100 ms        Memory Size: 1024 MB    Max Memory Used: 38 MB
```

### Process Output
I have put together a simple script to process the output from lambda emulator to convert into csv.
The script is located at scripts/parse.rb.
```sh
ruby test/parse.rb lambda.out >report/lambda.csv
```
### Generate Report 
```sh
ruby ../test/generate_report.rb ./lambda.csv ../serverless.yml
```
![Report](/assets/images/lambda-benchmark.png)




<a name="estimatingcosts"></a>
## Estimating Costs 

```ruby
    aws_prices = {
      "128" =>	{ "free"=>3200000,"price"=>0.000000208},
      "192" =>	{ "free"=>2133333,"price"=>0.000000313},
      "256" =>	{ "free"=>1600000,"price"=>0.000000417},
      "320" =>	{ "free"=>1280000,"price"=>0.000000521},
      "384" =>	{ "free"=>1066667,"price"=>0.000000625},
      "448" =>	{ "free"=>914286,"price"=>0.000000729},
      "512" =>	{ "free"=>800000,"price"=>0.000000834},
      "576" =>	{ "free"=>711111,"price"=>0.000000938},
      "640" =>	{"free"=>640000,"price"=>0.000001042},
      "704" =>	{"free"=>581818,"price"=>0.000001146},
      "768" =>	{"free"=>533333,"price"=>0.000001250},
      "832" =>	{"free"=>492308,"price"=>0.000001354},
      "896" =>	{"free"=>457143,"price"=>0.000001459},
      "960" =>	{"free"=>426667,"price"=>0.000001563},
      "1024" =>	{"free"=>400000,"price"=>0.000001667},
      "1088" =>	{"free"=>376471,"price"=>0.000001771},
      "1152" =>	{"free"=>355556,"price"=>0.000001875},
      "1216" =>	{"free"=>336842,"price"=>0.000001980},
      "1280" =>	{"free"=>320000,"price"=>0.000002084},
      "1344" =>	{"free"=>304762,"price"=>0.000002188},
      "1408" =>	{"free"=>290909,"price"=>0.000002292},
      "1472" =>	{"free"=>278261,"price"=>0.000002396},
      "1536" =>	{"free"=>266667,"price"=>0.000002501}
    }
    
    @compute_ms100 = (@average_execution_time/100)*30000000
        
    @compute_charges = aws_prices[@function_memory_size.to_s]['price']*@compute_ms100

```
It sometimes appears as a black art to calculate or even estimate the real cost of an Amazon service.
The above formula is verified from with the real cost of todos lambda function.

The two key calculations are 

- compute_ms100
This is the multiplier of 100ms interval based on 30 millions requests per month.
- compute_charges
This is the total expected charges based on the average execution time.


<a name="questions"></a>
## Question & Answers

A reader has asked some pertinent questions about performance testing the lambda functions. I would try to first answer those questions before continuing with the rest of the article. 

**1. What type of tests we can do to get the proper metrics to establish the benchmark**

Benchmarking Lambda/FaaS is inherentally different from benchmark in-permise application running on a dedicated physical/virtual 
infrastructure. What you don't want to do is to test the AWS infrastructure. Or hit by the AWS tricks like throttling due to traffic 
coming from a single source, artificial limits imposed or even AWS's own performance issues. Remember AWS is __eventually__ good and not 
~~consistently~~ good.

It is for that reason I recommend doing the benchmark on an _emulated lambda environment_ running on local infrastructure.
When benchmarking lambda function, it is important to identify the key components that need to be benchmarked. Typically if your lambda 
function is invoked via a REST api call you would like to perform the traditional benchmarking using the standard REST API tests. 

Representing a real life workload in a benchmark test plan is the key.  The tests you want to model should be focused on the actual functionality your lambda function is implementing. And whereever possible or required use the mocked services to keep the benchmark focus only on the real tests.
For the nodejs based lambda it is important that you benchmark the performance of your code under several different external conditions. For example if your code is consuming external rest api service or reading and writing large amount of data asyncronously to database, then it is a good idea to have reasonable data in your database.
For the lambda functions invoked via other events like S3, Dynamodb streams, step functions etc. It is important to design your test to simulate the external events behaviour correctly.

**2. What are the critical parameters we need to consider that give us a good information in terms of Performance ( __for example, we have considered duration, memory and invocations of Lambda but it looks if we increase the load only difference happening in the number of invocations per a certain interval. We are not sure how we can to interpret this to understand the impact of the load on Lambda__).**

This is key question, first thing to understand is that you are not testing the scalability of lambda under certain load. As each lambda function runs in its own container you can be sure that the increase in load will not make any difference to individual execution. However it does not mean that your lambda will always have the same execution time and memory consumption in the production as well. The memory consumption more often than not depends upon the data being processed by your lambda function, be it the json received from the external request/response or from the database. Therefore during the benchmarking if you see the same memory usage than it either means that your code is very well written, or you are not testing with the real life data scenario. 
The execution time is one of the key metric to collect, not only AWS charges are governed by the execution time but it is the key factor in your application performance. Majority of the performance issues I have observed in a lambda based implementation of RESTApi invoked from a Web or Mobile application relates to the slower response time. During the benchmark or laod testing these issues usually remain undiscovered for the following reasons:
- The execution time is dependent on use of different coding patterns, e.g. when processing data what patterns developer has used simple loops, syncronous/async maps etc. Similarly when invoking other external services are they invoked via async or sync.
- The execution time also depends on the size of data and how it is read synchronously or asynchronously.

Consistent execution time in a production environment is exteremely important. Remember lambda functions are not supposed to a monolithic application with extremely large code. They are what they are "Functions" , small, simple and quick. They need to be chained via a good microservices architecture. 

However I seen several development teams trying to convert a lambda function to a bloated monolith java or even worse nodejs application.

Finally the key metric which may not be available via standard reporting from cloudwatch is network bandwidth. This is often ignored during the benchmark and load testing. It is normally good idea to put the cloudwatch minitors for all external services consumed by your lambda function. 
When running the benchmark locally it is a good idea to monitor the traffic between different docker containers involved in the test. 

**3. What are the recommended tuning and probable that could occur.**

Very recently I was involved in a discussion about what to tune in AWS lambda infrastructure. My answer was **tune the code**. Yes the 
best thing to tune is your code, FaaS brings the performance engineer task back to the developer. 

It does not however mean that there are
no tuning parameter. The obvious ones are the function memory size and the execution time. Most of the performance engineers coming from
the in-premise monolith background would like to set the tuning parameters as much higher than what is observed in the benchmark or load test. A typical scenario is when average execution time for 99 percent of the invocation is 200ms but one of invocation taking 10 
seconds. Instinctively the tuning recommendation will be to set the timeout to 10+ second to avoid a single failure. But it might be 
a good idea to return an error.
   


## References:

[Load Testing your API](https://www.3scale.net/2015/04/how-to-load-test-and-tune-performance-on-your-api-part-i/)

[AWS Lambda Pricing](https://aws.amazon.com/lambda/pricing/)

[AWS Lambda Load Testing Harness](https://aws.amazon.com/blogs/compute/serverless-testing-with-aws-lambda/)

 
 
  
---
layout: single
title: "Docker Performance Benchmark"
excerpt: "Why docker makes so much sense.."
tags: [docker, devops, initializr, automation-that-works]
image:
  feature: docker-perf-one-host.png
  credit: Alexey Ermakov
  creditlink: https://arxiv.org/pdf/1704.05592.pdf
comments: true
---

Containerization is one of those technologies that have changed the way we develop, manage and operate software applications.
In the past few years, Docker has become most widely used tool not only in production environments running µServices or even monolith applications, but also the essential tool for any software developer involved in any kind of software development.
One of the question however still asked by many who are not familiar with the internals of Docker and LxC, is the performance of applications running inside the container.
I came across this recently published research paper on the performance of Docker for HPC applications. And the graph caught my eye was:</br>
![Alt](/assets/images/docker-perf-one-host.png "Docker Performance 1-Host Benchmark") 

Yes true!!! it does show that Docker perform 285+ GFLOPS, a little more floating operations than on a bare metal machine. Well off course that may not be possible, only thing this graph highlights is that applications running inside a container may have exactly the same performance (or notice very little performance impact, particularly the applications which are CPU intensive).
This is because the containers only add a very thin layer which consists of Bins/Libs. <br/>
![Alt](/assets/images/docker-teardown-02-architecture.png "Docker Performance 1-Host Benchmark") 
<br>(Image Credit: <a href="http://www.networkcomputing.com/data-centers/docker-containers-9-fundamental-facts/1537300193">Docker Containers: 9 Fundamental Facts</a>)

You can read the paper <a href="https://arxiv.org/pdf/1704.05592.pdf">here</a>.    
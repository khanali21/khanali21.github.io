---
layout: single
title: "Cloudbees Enterprise Jenkins Alternative"
excerpt: "Alternative approach to implement an enterprise scale Jenkins implementation."
tags: [devops, jenkins, cloudbees, automation-that-works]
image:
  feature: jenkins-enterprise-1.png
  credit:
  creditlink: 
comments: true
---

Implementing an enterprise scale Jenkins infrastructure that can support multiple teams working at the sametime is challenging to say the least.
<a href="https://www.cloudbees.com/products/cloudbees-jenkins-platform/enterprise-edition">Cloudbees Jenkins Enterprise</a> provide the enterprise features including high availability, horizontal scaling and backup/restore with multiple Jenkins masters.
CBJE is commercial solution available as in-premise and cloud offering. 
I would discuss here two alternative approaches for implementing an enterprise solution based on the open source Jenkins and Docker that provides the following enterprise features:

### High Availability
High Availability is the most desired feature in enterprise Jenkins implementation particularly when Jenkins is running the CI/CD pipelines in an agile based software development team. 
As the Jenkins architecture is not designed for high availability, keeping the configuration of multiple Jenkins masters in sync becomes a significant challenge. In the Head-Master-with-multiple-masters solution I would discuss the git based configuration store for Jenkins.
In the Docker Swarm based approach with one master per team, I would discuss how to implement a folder structure to store the configuration of each Jenkins master that can be used to bring up dockerized Jenkins master on demand.

### Horizontal Scaling
Jenkins provide the multiple-slave solution for horizontal scalability to run the concurrent jobs on different machines at the same time. Usually the Jenkins Slave are created as physical machines or VMs with Jenkins slave provisioned at the time of setup.
I would describe the Docker based Jenkins slave solution where the new slaves are spinned up on demand on a docker swarm providing the maximum scalability. This solution provides the necessary isolation for each slave according to the requirements of the jobs running on this slave.
Similarly on demand provisioning of Jenkins masters on a docker swarm is described on the second solution. 
 
### Backup Restore
As the Jenkins data is stored on the disk both configuration (xml) files and data are stored inside the same folder structure, implementing an elegant backup restore strategy is very challenging. 
I describe two solutions for Jenkins backup based on the git plus docker volumes for the jenkins data. 

## Solution 1: Head-Maser-plus-Multiple-Masters Jenkins Infrastructure
The solution consists of Jenkins Master server running either inside a docker container or on a physical/virtual machine designated as *Head-Master*.
And one or more Jenkins master running inside a docker container or physical/virtual machine designated as *read-only* Jenkins master. 
The head-master Jenkins is the primary Jenkins which contains the up-to-date configuration of all the jobs. All the jobs are initially created on the head-master.
<br/>
<img src="/assets/images/jenkins-enterprise-1.png"/>
All the job and jenkins configuration are pushed to a git repository via a scheduled job on the Jenkins *head-master*.
On the other Jenkins masters the job and jenkins configuration is pulled from the git repository periodically via a scheduled jenkins job on each Jenkins master.
Each Jenkins master uses a dockerized jenkins slaves to execute the jobs. We create the Jenkins slave docker image based on the requirement of the team or the jobs running on that slave.
The Dockerfiles provided in the repository in provide the Jenkins slave for Java 8, Maven, Gradle, NodeJS and Ruby based project builds.

(Note: Git code associated with this article will be released as part of the book.)

## Solution 2: Open-Source Jenkins Enterprise

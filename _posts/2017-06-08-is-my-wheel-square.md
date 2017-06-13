---
layout: single
title: "Is my new wheel design square?"
excerpt: "Implementing a comprehensive automation strategy for µServices Architecture based application is like re-inventing the wheel, despite the fact that there are so many wheels out there. But nothing fits your exotic new car."
tags: [devops, automation, ci-cd, automaton, automation-that-works]
image:
  feature: do-not-reinvent-the-wheel.jpg
  credit: 
  creditlink: 
comments: true
---

## DevOps Automation Engine  

### Why Re-invent the wheel?

As this [awsome article](https://blog.codinghorror.com/dont-reinvent-the-wheel-unless-you-plan-on-learning-more-about-wheels/) on codinghorror blog suggests, _"So, no, you shouldn't reinvent the wheel. Unless you plan on learning more about wheels, that is."_
The truth is in the field of DevOps Automation for modern applications particularly based on µServices Architecture, each one of us wants to learn more about the wheels, that will support the car having the high-performance automation engine that we are developing. 
In past 5 years, I have been involved in implementing automation strategy for 5 projects of different scale and technology. The two key areas of the automation involved CI/CD pipeline and Test Automation. 
In this article, I will discuss the pitfalls that will make your new wheel a square or triangle.

### DevOps Team (Oxymoron) 
People like me who have worked in software development team structure where there used to be separate Development, Testing and Operations team, when exposed to DevOps approach for the first time try to retro-fit their old concepts to new ideas. 
DevOps team is an oxymoron, which goes against the basic principle of DevOps. And usually this structure results from a completely misunderstood concept of DevOps by an executive or a manager. 
Below is the example of a team structure where a "DevOps Team", is responsible for providing support to Dev teams for automation and infrastructure.

1. Three small Development Teams
2. A Test Team
3. A "DevOps Team"

The "DevOps Team", quickly form another silo, with their own bureaucratic structure. I happened to work in a similar team where 4+ "DevOps Engineers", will have a stand up in the morning discussing the support issues raised by the Developers and Architectures.
The "DevOps Team" will have its own sprint, its own stories and what not. This was a complete murder of DevOps culture. The issues will range from nightly build failing to integration test failing and like a true "enterprise team", the response time to these issues with be 24 hours.
Pretty soon "DevOps Team" was responsible for all the delays, and the developers will hate them. 
This structure is called ["Anti-Type B DevOps Team topology"](http://web.devopstopologies.com/#anti-types).  

### Jenkins Pipeline pitfalls

Jenkins pipelines have been the biggest area of trial & error in implementing a stable CI/CD pipeline. In 2012, working on a monolith application, I was so excited on implementing a pipeline consisting of 12 Jenkins jobs going all the way to deploy on staging environment after performance and integration testing.
This pipeline looked so awesome and in the beginning was so stable that I believed there can not be any better wheel then this.
For monolith application this was probably an acceptable approach, but soon the inherent instability of such pipeline started manifesting itself. With so many independent Jenkins jobs chained via a plugin, each job having its own probability failure, the whole pipeline would fail most of the time.
Implementing such a pipeline dependant on so many jobs is not only hard to set up but also very hard to maintain. For µServices this is recipe for disaster. 

#### Implicit Dependencies in Pipeline -- Don't mix jobs & code
The two key aspects of pipeline-as-a-code are 
1) Keeping the pipeline definition under source control, perferably with application code.
2) No implicit dependencies (or declarative dependencies)on the infrastructure running the pipeline.

The first aspect is very promising and understandable, I was involved in a project where the new wheel design had the Jenkinsfile consisting of 10+ stages checked into git repo along with the spring application. 
Everything look pretty sound, and the team claimed that they have source controlled pipeline definition as desired.

Implementing the second aspect is rather tricky. In the above example, the source controlled Jenkinsfile, each stage was calling another Jenkins job. And soon for only **<10 Springboot applications** there were **144 jobs** running on the Jenkins server.
Not only the pipeline was dependant on the existence of those jobs on the Jenkins server, but setting up all these jobs for every new springboot application was a significant effort. In this case an Automation Engineer was creating clone of existing jobs and then modifying the definition of each job which you can guess was highly error prone.
The second attempt to redesign the pipeline by attempting to bring in the jobs inside the pipeline as a code, also failed. As the pipeline code was still dependent on the Jenkins environment running the pipeline. From the presence of tools like gradle to the dependency on the directory structure outside the control of the Jenkinsfile there were numerous implicit dependencies that made the pipeline fragile and hard to setup.

#### Continuous Delivery to Staging Environment
The continuous integration (CI) part of the Jenkins pipeline is usually straightforward, if you are not using a comprehensive shift-left testing approach, deployment of the code on a staging environment becomes unavoidable. 
Continuous delivery to a fixed staging environment on every commit is prone to instability of the CI/CD pipeline. For the µService based applications it is essential for all the test phases (including integration, performance and acceptance testing) to be part of the CI/CD pipeline without any dependency to external system. Hence a robust **mocking strategy** is imperative.


## References:
[DevOps Topologies](http://web.devopstopologies.com)
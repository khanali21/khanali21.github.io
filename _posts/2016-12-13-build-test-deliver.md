---
layout: single
title: "Build, Test and Deliver"
excerpt: "Software development process now depends on large and complex configuration of different services and infrastructure pieces."
tags: [paas, saas, aws, cloud]
image:
  feature: jekyll-logo-light-solid.png
  credit:
  creditlink: https://www.google.ie/url?sa=i&rct=j&q=&esrc=s&source=imgres&cd=&ved=0ahUKEwj57Kre5NrQAhWEBcAKHVV3BycQjRwIBw&url=https%3A%2F%2Ftalk.jekyllrb.com%2F&psig=AFQjCNFeOHJgEzHUfZA1JqTdeW8aS8jkmA&ust=1480949445098114
comments: true
---
For the success any software development project, besides the actual development (i.e. writing the code), the three other extremely important process are Build, Test and Delivery (BTD). In my experience of working with home grown hobby projects to large-scale enterprise system, all of these three processes are usually blamed for the delay in the completion of the project and most often blamed from all the issues in production.

It is pertinent to note that the three processes are usually implemented independent of each other, despite a very strong dependency relationship and common basis. In this article I try to define the common objectives of BTD and how to achieve them.

Software development process is no more isolated efforts of individual developers working on their local machines depending only on a common source code management system. Software development process now depends on large and complex configuration of different services and infrastructure pieces including the following:
1. Cloud based PaaS services (GCE, AWS, OpenStacks and even in-permises cloud)
2. Cloud based Saas services (GIT, JIRA, Bamboo, Confluence, Jazz to name a few)
3. Local Infrastructure (Test environments, developer machines etc)

The configurational complexity of these services and systems depends upon the nature of the project. Modern software development projects are designed to run in a cloud hosted environment with the intentions of auto-scaling for large number of users. In this article I discuss the BTD process in the context of similar cloud based multi-tenant software projects.

It is not surprising to find out that the most successful and extremely large scale internet applications started as a small projects with even smaller teams. It is also not surprising that a lot of tools of trades used in BTD were by-product of such software projects. Therefore it is important to understand that the smaller teams whether working in a large company or a startup, must have the vision to make the system exponentially scalable.

In this context I define the following objectives for a useful BTD process.
1. Optimal and Cost-effective use of the PaaS and SaaS services during the software development life cycle.
2. Robust and Time-efficient Automated Build, Test and Delivery process often known as CI/CD process.

Optimal and Cost-effective use of PaaS and SaaS services:
Back in 2003, when I was working on the DLPAR's of IBM pSeries systems, explaining my boss that how the dynamic logical partitioning could provide us the optimal use of our pSeries infrastructure. As some of the LPAR's resources (particularly memory and cpu) were under utilized and while others were maxing out. My boss posed me the question how can we ensure that DLPAR are always used at their full capacity (60% as defined in on of the millions of IT standards in the bank). So in reality optimal and cost-effective are sometimes two competing objectives. This applies to Cloud services in almost all cases including Platform and Software as a service.

(P.S. This is part of my upcoming book 'Automation that works'.)
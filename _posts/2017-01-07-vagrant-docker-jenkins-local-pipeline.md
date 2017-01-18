---
layout: single
title: "Vagrant + Docker + Jenkins - Dev Environment with Local CI Pipeline"
excerpt: "Vagrant+Docker+Jenkins Developer's Environment with Local CI Pipeline"
tags: [vagrant, docker, jenkins, automation-that-works]
image:
  feature: vagrant+docker+jenkins.png
  credit:
  creditlink: 
comments: true
---

In the last few projects I have implemented a Development environment for the Monolith Java Apps, µServices and Android applications using the Vagrant with Docker.

In those good days when the software was to be released every year and the developers will stay in the company for decades, there used to be tens of wiki pages describing how to set up your local machine for developing the greatest app on the earth. There used to many more pages, describing how to push the changes to git/svn repo in order to make sure that the Continuous Integration process does not fail overnight and your release manager does not yell at you. But the yelling will never stop and you will find the pissed off colleagues telling you how inept you are, whenever there is a bad code commit that fails the CI or build. And you would be frustratingly thinking that everything passed on my local machine. Then there will be even more Wiki pages consisting of FAQ’s and even then step you are stuck will not be listed in the FAQ.

Fast forward to 2010, with multiple git repos, separate projects for each µService meaning 10’s if not 100’s of repositories, then their associated CI pipelines (say one for each µService), and the enthusiastic developers coding day and night pushing the changes to git repository triggering the CI and even CD process. And now even though your CI process runs takes only minutes to run for each service, but there are 10's of concurrently executing CI builds. Last thing you need is the scenario where developer checks in the code that keeps on failing on the CI server while being successful on his/her local machine.
Last but not the least, if the team consists of developers with notebooks/desktops running Windows, Linux and Mac, then you need to maintain the wiki pages for each environment set up and also deal with the situation where something is available on one platform and not on the other.

In this context the Vagrant+Docker setup for local environment using the local Jenkins instance which is identical to the central CI Jenkins, takes care of all of the above concerns. Let us say you join an awesome software development company and they give you a brand new laptop and ask you to just install vagrant and your preferred IDE and then only thing you need to do is git checkout the code which also contains three additional files.

a) **Vagrantfile**: Containing the complete version controlled Virtual Machine consisting of all the necessary tools developer needs including the Docker host with QA and Dev container images. 

b) **Dockerfile**: Containing the definition of the QA container consisting of the local Jenkins instance.

c) **Jenkinsfile**: The Jenkinsfile containing the CI pipeline with the pipeline stages applicable to the developer e.g. Unit Test, Contract Testing, Mocked Integration Tests, Static Code Analysis etc.

Finally all you need to do is "_**vagrant up**_" and there you go:

What you get:

1. A local instance of Jenkins which has exactly the same pipeline with relevant stages only.

2. All the necessary developer’s tools inside the docker container called dev-container, which could be managed by the team lead.


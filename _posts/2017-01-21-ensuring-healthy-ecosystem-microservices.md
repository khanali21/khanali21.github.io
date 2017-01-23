---
layout: single
title: "Ensuring Healthy EcoSystem for µServices"
excerpt: "Developing µServices that are responsible citizen of a healthy ecosystem."
tags: [microservice-testing, distribute-tracing, hystrix, polyglot-microservices, microservice-ecosystem]
image:
  feature: ecosystem-health.png
  credit:
  creditlink: http://www.wikihow.com/Tell-if-Your-Fish-Is-Dead
comments: true
---
µServices thrive in a good ecosystem, building applications based on µService Architecture requires evolving an ecosystem in which every member not only perform optimally but evolve as part of that system. The complex dependency graph among mutually dependant µServices require each of the µService to follow certain quality acceptance criteria. This criteria not only applies to the communication between the µServices, but also to the other internal and external systems interacting with that µService.
Take the example of Swagger endpoint exposed by the µService, your architecture may require the endpoint to be exposed and up during the test and pre-prod phases but disabled in production. Similarly in the example of logging and tracing, a good architect will enforce certain requirements for any µService to implement tracing to help debugging e.g. using Spring Sleuth for the tracing, logging pushed to Kafka topic etc. There could be numerous such other requirements besides the functionality, which a µService developer must implement when developing a new µService.

Enforcing a good quality gating for µService becomes a real challenge particularly when the development has been outsources to third parties. In this scenario, multiple teams with different background and skill level may write code of varying quality and also implement their own local standardization causing the ripples in ecosystem. A good CD pipeline need to ensure that the µService release acts as a responsible citizen in the complex ecosystem. This requires additional stage in the CD pipeline to test for the readiness of the µService to go in production. 
In this chapter we discuss few of such tests.

1. Distributed Tracing Validation
2. Swagger Endpoints Validation 
3. Chef cookbook (if used) Validation 
4. Docker container (if used) validation
5. Service Health Endpoint validation
6. Logging standard validation
7. Fault tolerance validation (Hystrix if used)

The above tests validation should be included as part of the CI, in order to ensure a fast feedback. 

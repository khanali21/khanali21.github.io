---
layout: single
title: "No DevOps without ChatOps"
excerpt: "ChatOps is an essential ingredient for the DevOps strategy"
tags: [chatops, devops, slack, mattermost, microservice-ecosystem]
image:
  feature: no-devops-without-chatops.png
  credit:
  creditlink: 
comments: true
---
_A decade ago, when I was working with IBM, I loved one of the nice features of IBM Sametime to paste images directly in the conversation. It made it so useful as they say a picture is worth one thousand words. (I always used animated gif which is probably worth more than ten thousand words)._

One of the core value of DevOps is Culture, it ensures that the teams work together while sharing the responsibility for the success of application. The culture evolves as the teams and team members communicate. The access to right information as soon as it becomes available is the key for adopting DevOps culture. The evolution of such culture depends on a good ChatOps strategy. I call this “**No DevOps without ChatOps**”.  The Chat system is the pivot for all the DevOps toolchain and the people. The information is fed into Chat System via integration with CI/CD pipeline, Test and Production Environments, Agile tools and the team members. The modern Chat system allow creation of automated ChatBots which can even use AI to provide the right information. Similarly the chat tool may provide API to create the Channels/Rooms which then can be integrated with the CI/CD pipeline and other systems and environments.

A good ChatOps implementation depends on the Chat system/tool. Below are some of the key characteristics that one must keep in view when choosing a good Chat system/tool.

1.	**Performance and Reliability**: One of the key requirement for a good Chat System is its performance and reliability. If you happened to use the chat system in 90’s and first decade of 21st century you would still remember how painfully slow Skype/Sametime/Messenger could be. As the Chat System gets integrated with other tools and processes, it may have to process many more messages then a simple group chat may have too. This becomes more important when you need to implement an on premise Chat System instead of a cloud hosted solution.

2.	**API for Integration**: A non-integrated Chat System is no better than an email notification system. The modern Chat Systems support API for integration which can be used to integrate with other tools and processes.

3.	**Support for ChatBots**: ChatBots are essential tool for implementing a useful ChatOps system. You can implement various ChatBots for different needs. ChatBots are better replacement for Wiki pages and even documentation. Couple of ChatBots that we implement in this chapter:

•	A ChatBot for on-boarding new developer, business analyst or any other member of the team,

•	 A ChatBot for querying key information about any process or current status of environment, pipeline and even production systems.

4.	**Search**: A key feature for a good ChatOps system is ability to index, tag and search the messages in Channels/Conversations. A Search ChatBot may make it even easier to query the information based on different tags.

5.	**Mobile UI**: A mobile native interface is another essential requirement for the Chat system.

Slack is an excellent Chat system that meets all the requirements stated above. An open source implementation of feature compatible Slack is Mattermost. In this chapter we shall use both Mattermost and Slack for integration use cases. I also touch upon HipChat and upcoming IBM product codenamed Toscana.

---
layout: single
title: "ChatOps in a DevOps Strategy"
excerpt: "ChatOps - Implementation"
tags: [devops, chatops, automation-that-works]
image:
  feature: chatops.png
  credit:
  creditlink: 
comments: true
---
Collaboration and communications is at the heart of a successful DevOps strategy. The DevOps culture evolves not only because of the open communication amongst the human stakeholders but also because of the communication and collaboration among the tools in toolchain used in the automation.
<br/>
In a successful automation helping achieve the DevOps model, the tools/processes/technologies like Jenkins, Jira, Git, Chef, µServices, API GWs, ChatBots and you name it, interact with humans via common communication channels like any other human stakeholder. 

## ChapOps Model

The modern Chat Systems including Slack, MatterMost, HipChat provide the necessary integration points and Chat Bots to implement a truly amazing communication model that serves as the pivot for the whole team including the tools and processes. I will be discussing the implementation of a ChatOps model using MatterMost and Slack in the following chapter. But let us first discuss the key characteristics of the ChatOps model.

### No Spam

The key feature of a useful ChapOps system is the No Spam filter. Spam in this context means that the information goes to only relevant stake holders. This is controlled via categorised Channels in Slack/MatterMost and AI based bots which can route and filter the messages according to their relevance and importance. You will find the examples of the ChatBots code written in ruby for the Slack/MatterMost helping filtering out spam from the channels in the following sections in this chapter. 

### Self Organising Chat

Like Spam, the dis-organised, all over the place messages, instead of contributing to better communication become hinderance in a proper collaboration. This becomes more evident when there are several information feeders to the communication channels in the shape of tools integration. Just imagine if there are 100's of Jenkins jobs feeding into the Chat system about the health and status of CICD pipelines for dozens of µServices under development. Or Git feeding the information about dozens of Pull Requests being created every day. For a successful ChatOps system the structure of the channels is critical in making the information useful for the stakeholders. The automation for creation and cleanup of the channels in slack/MatterMost and the structure of information in those channels will be our main goal in this chapter.

### Search, Search, Search

One of the key success factor in Google's success was the demonstration of how search can turn the pile of apparently useless data into useful information. The twitter made is possible to tag the information such that you can retrieve without searching.
The utility of a ChatOps systems can be measured by observing how the developers, engineers and the business analyst can find the useful information by searching the history. Indeed Slack/MatterMost have made the Confluence/Wiki somewhat irrelevant for getting the most update information about the project. I will be discussing the tagging and searching strategy for Slack/MatterMost across different channels, we also implement a search bot that collects the information not only from within the slack but also correlate it will google and other resources.

### Let's make it fun
Communication must be fun, else it becomes the biggest factor in the failure of DevOps culture evolution. Modern chat systems make it really easy to have a fun chat even in depressing circumstances. They say a picture is worth thousand of words, and an animated gif is worth hundred of thousands of words. I have seen how /giphy can take the team communication to the next level. A Jenkins job message that the CI is broken can be depressing but not when it shows this:
<br/><a href="https://media.giphy.com/media/3o6ZsXAElDtzalJcZy/giphy.gif"><img src="https://media.giphy.com/media/3o6ZsXAElDtzalJcZy/giphy.gif"/>

### Chat Bots

Chat Bots play the key role in ChatOps implementation. Search is useful but asking is even better. Back in the old days, one used to create long wiki pages for the new developer to start in a team. From how to setup the laptop with essential tools to the information about the architecture and the dependencies everything is buried in those long wiki pages. But any new developer instead of reading those wiki pages will come to you and ask "How do you get vagrant installed on my windows?", "Do I need homebrew on my Mac?". We implement a #newstarterbot at the end of this chapter.










  

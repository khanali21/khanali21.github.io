---
layout: single
title: "Short Story Long - User Story Automation"
excerpt: "The pivot for automation in an agile based software development team is the user story. "
tags: [agile, userstory, automation-that-works, user-story-automation]
image:
  feature: we-believed-your-story.png
  credit:  Image from Public Lecture by Dr. Greg Salyer 
  creditlink: https://www.youtube.com/watch?v=W5Yz3n8Or8E&t=2793s
comments: true
---
User stories are the basic building block in Agile approach for software development. User story is probably the most talked about subject in any software development team using agile development model as User Stories concern all the stakeholders. Besides being:
> the simple descriptions of a feature told from the perspective of the person who desires the new capability, usually a user or customer of the system.
<br/>--Mike Cohen <a href='https://www.mountaingoatsoftware.com/agile/user-stories'>User Stories</a>

User stories serve several different purposes including:
1. They are the basic constituent of Sprint planning. 
2. They also depict the velocity of the team if appropriately sized.
3. Interdependencies in user stories help in resource scheduling
4. **User stories are basis for the automation**, the whole automation revolves around the user story from inception to completion and then in production, a user story serves as the pivot for any good automation. 
5. And last but not the least user stories are the basis of completion of the project for client and developer alike.

### Structure and Content of User Story
The structure and content of the user story depends on several factors, in the past 5 years I have worked on 6 unique projects in three continents of the world with six different teams varying not only in size and skills but coming from different cultural backgrounds. In this journey, I have worked with teams in very large enterprises to start-up developing the next generation technologies.

In this experience only common part in the user stories I have come across is the start of the story with "As a ... ", I have seen user stories that consists of:
* The algorithmic style if-then-else. 
* The user story that is concise enough to capture the functionality that is deliverable in a single day.
* To the user story that is big enough to capture the whole functionality of the project.
* The user story that serves as the pivot for a fully automated Continuous delivery pipeline.
* To user story that have no test or automation aspects, not even any acceptance criteria or conditions of satisfaction.

### Automation 
A user story should act as a pivot for robust automation. From the inception of the user story as it gets added in the agile tool (like JIRA), to the production as it is delivered and deployed on different environments, the automation should revolve around the user story.
The user story workflow would be our primary target for automation. I take two deifferent tools Redmine and Jira, to automate the complete workflow of a user story. This automation include:
1. Automated status change of the story from one state to another based on the events.
2. Acceptance Test Automation
3. Automated notification related to one or group of user stories via ChatOps system. 
4. Automation for the release notes based on the user stories including in the release.
5. Automated Bug/Defect association with User Story.
6. Automated Reporting and Analytics.




` To believe in the story you need automation that proves it.`
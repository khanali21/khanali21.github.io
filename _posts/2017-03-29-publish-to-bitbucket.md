---
layout: single
title: "Publish to Bitbucket Jenkins Plugin"
excerpt: "The Jenkins plugin to create a new repository (and associated project), and initialize the repository with current code in the workspace, in Bitbucket server."
tags: [bitbucket, devops, jenkins, automation-that-works]
image:
  feature: publish-to-bitbucket.png
  credit:
  creditlink: 
comments: true
---
*This is one of the essential plugin in helping µServices development process by automating creating project, repo and Jenkins CI/CD helper jobs  *

*How it Works*

Let's say you have been working day in, day out on a new spike/code/project locally on your laptop with a local git repo. And now ready to publish this to the Bitbucket server. This would require following steps:

1. Create a project on Bitbucket server.
2. Create the repository under the new project.
3. Assign users/group with write permissions to the project and repository.
4. Finally import your code into the remote repository by adding remote "origin" and pushing the code.
5. As an add-on, if you have Jenkins CI server, you may want to run the one time [external async post receive hook](https://marketplace.atlassian.com/plugins/com.ngs.stash.externalhooks.external-hooks/server/overview) e.g. to create associated jobs in Jenkins for this µService. 

*"Publish To Bitbucket" plugin does all of the above for you.*  
All you need is to add a build step **publish to bitbucket**.

*For more information go to [plugin page](https://plugins.jenkins.io/publish-to-bitbucket)*.
![Alt](/assets/images/publish-to-bitbucket-example.png "Publish to Bitbucket")

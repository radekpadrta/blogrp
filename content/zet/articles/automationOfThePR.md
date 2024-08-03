---
author:
  - Radek Padrta
title: Automation of Pull Requests
date: 2024-08-03
description: 
tags:
  - Azure
  - DevOps
  - Automation
---

### Automation Process
One of my projects was to create an automation process to build a Web App when a PR is created.

Anyone with access to the Repo can easily build the WebApp for their Use Case, for example to quickly build a webpage with something for an individual client. No need to come to a Developer and say we need this and the developer will spend time to prepare the thing. The Web App should be like a template for users.

I have prepared an environment which needs to be built if the solution is needed. So imagine Franta Flinta needs a web app which can be shown in a presentation.

He will come to GitHub and he will create a PR request. He puts a label "Create Environment" there and the magic starts.

### Pipeline Workflow
Choosing the label "Create Environment", triggers the pipeline. The Pipeline is creating the whole process.

- Building the ACR image
- Creating the App Service Plan
- Creating the Web App
- Assigning Managed Identity
- Assigning AcrPull Role
- Setting env variables
- Restarting the Web App

Web App In the repo we have the code and Dockerfile prepared.

It's the same with closing PR. If you unlabeled the label or close the PR, there is another Pipeline which destroys everything that was built.

You're now thinking how the user will get the address back to the GitHub repo?
We are using just webhooks and the webhook payload can be accessed in the Pipeline. So we are checking the number.

The number references the issue/PR and then we know where we need to send the comments. 
So after correct steps we always inform the user about the process of the solution.

### Solution

![](/Autoamtion.png)

### Improvements?
Yes, there are, and these are everywhere.

What I want to do is to create an action to check how long the Web App has been up, so we can remove it after a certain time period and save money.
Also, there can be some field in the PR which can be extracted in the Pipeline, and the Pipeline will assign the variable into the Code.
Lastly, what I'm thinking is to get info to the user if something bad happened during the creation of the solution.



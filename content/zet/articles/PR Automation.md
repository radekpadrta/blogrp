---
author:
  - Radek Padrta
title: Public Speaking at Mambo Jambo
date: 2024-08-19
description: 
tags:
  - Azure
  - DevOps
---

# PRAutomation

We have a couple of codes in one repository, each creates a web application. We need to create the web app with an environment that will be short-lived, allowing someone to test the app within short time period.

We want to automate the process, and one way to do that is to build the solution through a Pull Request.

## Why through Pull Request?

Imagine you created an application or just some feature in your app. You don’t want to push it to the production environment, but rather to a short-lived environment, to ensure that everything is okay and to decide if you can merge the PR request, for example, to the main branch. The short-lived environment provides us with a space to test the application.

## How it works?

A user on GitHub will open a Pull Request. They will then choose which application they want to build (remember, we have multiple applications/features in one repository). The user selects the option they want to build, for example, Variant2.

#### How can the user choose which app?

In the PR, there are labels. We have created a label named Variant2. If the user selects this label and opens the PR, it will trigger the pipeline, and many things will be triggered. We will discuss this in more detail later, but first, let’s talk about our possibilities.

### What are our options?

There are three types I'm working on. There may be more options, and I would be really glad to hear your thoughts about it.

Our case: We have code in GitHub.
My testing includes the following:
- Web App - Azure DevOps
- Static Web App - Azure DevOps
- Static Web App - GitHub Actions

Let's discuss each, how it works at a high level, and the pros and cons.

## Web App - Azure DevOps

We opened the PR. We didn't select a label. Should the pipeline be triggered? No.
Why is it not triggered? Because we are using a webhook, and we defined what will trigger it. In our case, it is when a label is added.

![](/PRAUT_WebhookTrigger.png)

You might be wondering, what if we select a different label?
For this, we have a pipeline with many conditions that check what is needed and what is already set.

Here is a visualization of the flow.
![](/PRAUT_WA.png)


I will not dive into the solution, or I would be writing about it for hours. The solution is in [azure-pipelines-pr-open.yml](https://github.com/radekpadrta/PRAutomation/blob/main/azure-pipelines-pr-open.yml). You can check it there and you will find other pipeline there.

##### What is cool about this solution?
You can manage everything and have full control over your build. You can trigger comments for your developers, letting them know what is happening in the pipeline while they wait for the short-lived environment. It’s a flexible solution and, in my opinion, this approach is best for larger applications because it allows you to simulate everything needed in Azure. We are not just creating the Web App, we are also creating the container image, setting policies, and managing RBAC for each Web App. No need to use any tokens...everything is done through CLI and managed identity.

##### What is bad?
You need to pay for the service plan, create additional components, and the pipeline can become larger and harder to maintain with changes. You also need to implement lifecycle management. If you want to use a web app, you need to create a container for your code and wait for boot-up until your app is ready. Typically, this takes about 6-9 minutes, even when using very basic code.


## Static Web App - Azure DevOps
The trigger is the same as above. We are using a Static Web App, which means we don’t need as much in the pipeline as for a Web App. All we need is the Static Web App.


![](/PRAUT_SWA.png)

##### What is cool?
You can create preview environments and configure them as needed. For example, you can create two or three apps in one PR by selecting two or three labels, and it is cheaper than using a Web App.

##### What is bad?
It is similarly challenging to set up as a Web App and hard to maintain if there are changes.



## Static Web App - GitHub Actions

You need to set up the connection between the Static Web App and GitHub. You can follow this guide to do that: [How to conenct SWA with GitHub](https://github.com/radekpadrta/PRAutomation/tree/testPR)

Once you set the connection, a pipeline for GitHub Actions will be automatically created in your repository.

This automatically created pipeline is not ideal for our use case, but if we make it prettier, it can be very useful!

The udpated pipeline can be found [here](https://github.com/radekpadrta/PRAutomation/blob/main/.github/workflows/azure-static-web-apps-black-hill-01e38be0f.yml)

We don’t need to worry about webhooks or payloads, and we don’t use any conditions. Everything is handled in a different way, which we don't see.

The only things we need to configure is how to get the URL of the preview environment and how to choose which app we want to build.

##### What is cool?
If the code is in GitHub, it’s easy to set up Actions, understand, and maintain. One PR = One App, though setting up Actions for multiple apps might be challenging.

##### What is bad?
We don’t work with the payload, making it difficult to create flexible tasks. Additionally, we don’t see what is happening in GitHub Actions, for example when it sends information to users.


## Conclusion

If you have a lot of time, want to be sure everything is set up according to your needs, and you have money, go for the Web App.

If you have some time, and already Azure set for your Web App environments, and want to test the solution and discard it without incurring high costs, choose the Static Web App.

If you want an easy solution, don’t want to spend much time on it, and don’t need a highly flexible setup, go with the Static Web App using GitHub Actions.




















#### LINKS:




16-08-2024-08:49

---

[[]]
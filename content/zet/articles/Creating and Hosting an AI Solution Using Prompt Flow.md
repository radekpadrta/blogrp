---
author:
  - Radek Padrta
title: Creating and Hosting an AI Solution Using Prompt Flow
date: 2024-07-12
description: 
tags:
  - Azure
  - AI
  - Prompt flow
  - Machine Learning
  - LLMOps
---

## Creating and Hosting an AI Solution Using Prompt Flow

Let's talk about why we chose the Prompt Flow. 
In the AI world, we wanted to have one place where we could build our AI solutions and  we wanted to have platform where other developers could easily collaborate. And this is a great use case for Prompt Flow, which works as an orchestration tool for our builds.

We built two solutions. Our vision was to get metadata from some article and alternative text from pictures, and then each solution needed a final step: a translator to translate the results into different languages.

And these two solutions were created through the Prompt Flow. [Source code here](https://github.com/radekpadrta/MamboJambo_PromptFlow)

When we built the solution through Prompt Flow, our next step was to create an endpoint where the solution could be hosted and wait for travelers (requests).

### How Did We Do That?

When we create builds on Prompt flow, we used the Machine Learning Workspace, which also has an option to create the endpoint. Creating the endpoint was a simple task, but how could we assign the Prompt Flow solution to the endpoint, right?

### Where the Fun Begins

The solution needs to be packaged first, and that the need for us to create a **model** in MLW. It means: Prompt Flow build become a Model. Once we have the model, we can create an empty endpoint, which will wait for our model. However, to complete this, we need to create an Environment.

The Environment for building our model needs to be somehow added to the endpoint, and the environment is something that will handle the deployment. If you remember from the start, I mentioned that our use case also needed a translator, right? This is a small problem for the environment because we can't use a pre-built environment and we need to create our own.

### Why?

The translator isn't included by default in Prompt Flow, but you can install packages in Prompt Flow. That is why you need to have the package also in your environment. We used a predefined image and we added the RUN command for the translator package. That is how we built the environment through Dockerfile and ACR. We hosted the Docker image in Azure Container Registry.

Ok, we have Prompt Flow (Builds) - Model - Endpoint (Empty) - Environment. So what is the next and the last step to push travelers(request) to the castle(endpoint) ?

### Deployment: Facing the Final Boss

Deployment 
This is last boss who is waiting for us and need to be finished. Here we need to gather all the information about our previous road - model, environment, and the endpoint need to be entered to the deployment, but we also need to provide SKU (VM) and assign the traffic. 

Then if we get green light, the boss is finished and our castle can welcome travelers.

More about the project will be added soon.

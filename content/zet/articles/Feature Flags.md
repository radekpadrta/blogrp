---
author:
  - Radek Padrta
title: Feature flags - App Configuration and Web App
date: 2024-07-16
description: 
tags:
  - Azure
  - Feature Flags
---


This article is about finding a way how to create feature flags on a Web app or App configuration. This is the result of my notes, where I tried to find a solution. 

### Environment Variables
On Azure web app, there is an option to use **environment variables**, but it is not a good approach if you don't want to get alerts. The alerts can be created because your app will be restarted if you assign the variable. The app will be offline for some time. This means if you need to rewrite the variable in the future, you always need to restart the app. So this option was out because of the bigger web app, and we did not want to have it offline for some time.

### Feature Flags
Another option was to use **Feature flags** in the App Configuration service. Feature flags are in the **Feature manager** in App Configuration. The feature flags can be called only through SDK; there is no REST API, and that was the problem. Because if you want to use SDK, you need to install some library, and in our case, it was nugets, and that was a no-go.

So how we can make the Feature flags work?

### Key Value
The approach was to use **Key Value** which can be found in **Configuration explorer**.

![](/configExplorer.png)
But I found the authorization through REST API a little bit more complicated than usual. You need to get a Bearer token first and then reach the endpoint.

And this is how you can get the token:
![](/GetToken.png)

```RestAPI
POST https://login.microsoftonline.com/ad9ab3ca-16d5-496d-8591-b7f91a515135/oauth2/v2.0/token
Body - x-www-form-urlencoded
Values:
client_id:value
client_secret:value
grant_type:client_credentials
scope:https://management.azure.com/.default
```

Unfortunately, the Bearer token is a little bit complicated to get, and you need to use ClientID and Client Secret from your App Registration, and this is something that needs to be changed. But it is ok if you want this approach, you just need to store these values in Key Vault and reference the Key Vault in your code.

---

And this is the URL how to get the endpoint for KeyValue (GET)

```Rest Api
https://management.azure.com/subscriptions/SubscriptionID/resourceGroups/RGname/providers/Microsoft.AppConfiguration/configurationStores/AppConfigurationName/keyValues/NameOfKeyValue$NameOfLabel?api-version=2023-03-01
```

![](/GetKeyValue.png)

---


Our approach is upgrade the solution to used managed identity to communicate with the App Configuration, but how we do that. This will be added later.



---
### Pipeline
This can be used in Pipeline if we need to use feature flags.

In the pipeline approach, this should work to get info about a Feature flag from Feature manager.

```Azure CLI
az appconfig feature show -n NameOfTheAppConfiguration --key .appconfig.featureflag/NameOfTheFeatureFlag --subscription xxx
```

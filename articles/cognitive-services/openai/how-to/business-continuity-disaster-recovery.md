---
title: 'Cross-region disaster recovery with Azure OpenAI'
titleSuffix: Azure OpenAI
description: Cross-region disaster recovery with Azure OpenAI 
services: cognitive-services
manager: nitinme
ms.service: cognitive-services
ms.subservice: openai
ms.topic: how-to
ms.date: 6/24/2022
author: ChrisHMSFT
ms.author: chrhoder
recommendations: false
keywords: 

---

# Cross-region disaster recovery with Azure OpenAI

When you create an Azure OpenAI resource, you specify a region. From then on, your resource and all of its operations stay associated with that particular Azure server region. It's rare, but not impossible, to encounter a network issue that hits an entire region. If your service needs to always be available, then you should design it to either fail-over into another region or split the workload between two or more regions. Both approaches require at least two resources in different regions. This article provides general recommendations for how to implement cross-region disaster recovery for your Azure OpenAI applications.

## Business Scenario

If your app or business depends on the use of an Azure OpenAI model, we recommend that you create a replica of your resource in an additional supported region. If a regional outage occurs, you can then access your model in the other fail-over region where you replicated your resource. Replicating a resource means that you create another resource or finetune your model in the failover region with the same set of data.

## Prerequisites

1. Two Azure OpenAI resources in different Azure regions. 
2. The key, endpoint URL, and subscription ID for your Azure OpenAI resources.

### How to monitor service availability

You should configure your client code to monitor errors, and if the errors persist, be prepared to redirect to another region of your choice where you have an Azure OpenAI subscription.

Follow these steps to configure your client to monitor errors:

1. Use this page (https://azure.microsoft.com/en-us/explore/global-infrastructure/geographies/?cdn=disable#overview) to identify the list of available regions for the Azure OpenAI service.

2. Select a primary and secondary/backup regions from the list.

3. Create Azure OpenAI Service resources for each region selected

4. For the primary region and any backup regions your code will need to know:

      a. Base URI for the resource

      b. Regional access key or Azure Active Directory access

5. Configure your code so that you monitor connectivity errors (typically connection timeouts and service unavailability errors).  

      a. Given that networks yield transient errors, for single connectivity issue occurrences, the suggestion is to retry.  

      b. For persistence redirect traffic to the backup resource in the region you've created.

## Cross-region disaster recovery requires custom code

The recovery from regional failures for this usage type can be performed instantaneously and at a very low cost. This does however, require custom development of this functionality on the client side of your application.

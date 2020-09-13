---
layout: post
title: Azure CI timed deploy
date: 2020-02-23 14:56:10
author: Peter Stevenson
summary: How to defer Azure CI deployments
categories: sysadmin
thumbnail:
tags:
 - Azure
 - CI
 - DevOps
 - Deployments
 - Pipelines
---

# Azure CI timed deploy

Azure DevOps aka VSTS Continuous Integration Pipelines.

This post has been sat around in my drafts for a long time. Anyway I thought I best post it before it becomes totally irrelevant due to Microsoft using more [Infrastructure as code (IaC)](https://en.wikipedia.org/wiki/Infrastructure_as_code). 

TODO: Find the other term I cannot think of right now. Config Driven Orchestration?

## Deferred deployments

This is one way to make a deployment happen at a given time. However it relies on you ensuring you check that box every time. Which is very easy to miss one day. Let alone if you have a team of people who are expected to do the same. Sooner or later someone will slip up as humans are human.

![screenshot-1](/blog/assets/2020-02-23/vsts-screenshot-1.png)

## A better way of doing timed deploys

### Step 1 approval

![screenshot-2](/blog/assets/2020-02-23/vsts-screenshot-2.png)

### Step 2 deployment

This is the actual deployment step which isn't triggered until the scheduled time is hit.

![screenshot-3](/blog/assets/2020-02-23/vsts-screenshot-3.png)

## Configurations

### Approval task

![screenshot-4](/blog/assets/2020-02-23/vsts-screenshot-4.png)

### Approval task triggers

![screenshot-5](/blog/assets/2020-02-23/vsts-screenshot-5.png)

### Approval task job

![screenshot-6](/blog/assets/2020-02-23/vsts-screenshot-6.png)

### Deployment task

You will want to set a different scheduled time here. Normally later in the evening out of hours.

![screenshot-7](/blog/assets/2020-02-23/vsts-screenshot-7.png)
---
layout: post
title: Cloud provider comparison notes
date: 2021-07-29 12:10:10
author: Peter Stevenson
summary: AWS vs Azure notes
categories: sysadmin
thumbnail:
tags:
 - AWS
 - Azure
---

# Cloud provider comparison notes

Mostly based around my workflows and use cases so it may miss out some products along with being time sensitive. 

## AWS

* [Calculator](https://calculator.aws/#/createCalculator)
* [Free Tier](https://aws.amazon.com/free/)
* [AWS pricing as MSP](https://www.reddit.com/r/msp/comments/99zjxs/aws_pricing_as_msp/e4saqqj/?utm_source=share&utm_medium=web2x&context=3)

### EC2

* [Estimate for a MC server](https://calculator.aws/#/estimate?id=79c40fb3d8634a44c35f9083db08d127b2fc7195) (61.30 USD monthly)
* [Estimate for a Windows server](https://calculator.aws/#/estimate?id=0190475dcf932284a083ec6cd6b094adc4640c72) (81.44 USD monthly)
* Mac dedicated host.
	* [How to access Amazon EC2 MacOS instance GUI](https://www.youtube.com/watch?v=c46ifRWVLyc)
* Doesn't clean up disks when you kill a VM unless you set "Delete on termination".
* AWS seems to have smaller OS disks by default but you pay for all of it (thick provisioned).

### Lightsail

* Kinda like a simplified GUI bolted onto normal AWS console. Makes AWS simplified like a old school VPS provider.
* [run-your-own-minecraft-server](https://aws.amazon.com/getting-started/hands-on/run-your-own-minecraft-server/)
* [Amazon Lightsail: The Easiest Way to Get Started on AWS](https://www.youtube.com/watch?v=taMlabDBO58)

### S3

* TODO: See blog post draft.
* [Introduction to Amazon Simple Storage Service (S3) - Cloud Storage on AWS](https://www.youtube.com/watch?v=77lMCiiMilo)
* FTP
	* [https://aws.amazon.com/blogs/aws/new-aws-transfer-for-sftp-fully-managed-sftp-service-for-amazon-s3/](https://aws.amazon.com/blogs/aws/new-aws-transfer-for-sftp-fully-managed-sftp-service-for-amazon-s3/)
	* [Simple and seamless file transfer to Amazon S3 using SFTP, FTPS, and FTP](https://aws.amazon.com/aws-transfer-family/?whats-new-cards.sort-by=item.additionalFields.postDateTime&whats-new-cards.sort-order=desc)
	* See my blog post: SFTP/SSH jail

### Glacier

* Long term archival storage. Cheaper than S3 but costs to take the data out.
* [AWS Glacier Tutorial \| Introduction to Amazon Glacier \| AWS Training \| Edureka](https://www.youtube.com/watch?v=HcYutDorr2Q)

### RDS (Relational Database Service)

* [Understanding Amazon Relational Database Service (RDS)](https://www.youtube.com/watch?v=eMzCI7S1P9M)
* [Getting Started with Amazon RDS - Relational Database Service on AWS](https://www.youtube.com/watch?v=knrNBkr5iTM)
* Amazon RDS for MYSQL vs Amazon Aurora MySQL-Compatible
* [Amazon RDS Pricing](https://aws.amazon.com/rds/pricing/)

### ELB (Elastic Load Balancer)

* [AWS Elastic Load Balancer (ELB) Tutorial How-To](https://www.youtube.com/watch?v=Sr2Mq9Gegew)

### Elastic Beanstalk

* Also covers VS plugin for AWS.
* [How to Deploy .NET Code to AWS from Within Visual Studio](https://www.youtube.com/watch?v=pgRzdZeNxD8)
* [[ AWS 10 ] Elastic Beanstalk \| Deploying your first application](https://www.youtube.com/watch?v=254OsG2ZPLA)
* [Tutorial: Deploying an ASP.NET core application with Elastic Beanstalk](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/dotnet-core-tutorial.html)

### Lambda/API Gateway

* [How to build an API with Lambdas and API Gateway](https://www.youtube.com/watch?v=4_WI8ZGIcXY)
* [Choosing between HTTP APIs and REST APIs](https://docs.aws.amazon.com/apigateway/latest/developerguide/http-api-vs-rest.html)
* [Building Lambda functions with C#](https://docs.amazonaws.cn/en_us/lambda/latest/dg/lambda-csharp.html)
* \>  Note that the console code editor supports only Node.js, Python, and Ruby.
	* [AWS Lambda: The code editor does not support the .NET Core 3.1 (C#/PowerShell) runtime](https://stackoverflow.com/questions/62588996/aws-lambda-the-code-editor-does-not-support-the-net-core-3-1-c-powershell-r)
	* The code editor does not support the Java 11 (Corretto) runtime.
	* The code editor does not support the .NET Core 2.1 (C#/PowerShell) runtime.
	* The code editor does however work for Python and Node.js
* [AWS Toolkit for Visual Studio Code](https://aws.amazon.com/visualstudiocode/)
* [Deploying serverless applications with the AWS Toolkit for Visual Studio Code](https://docs.aws.amazon.com/toolkit-for-vscode/latest/userguide/deploy-serverless-app.html) bah too hefty.
* [AWS Lambda for .NET Core](https://github.com/aws/aws-lambda-dotnet/) See my blog post
* [.NET Core CLI](https://docs.aws.amazon.com/lambda/latest/dg/csharp-package-cli.html) See my blog post
* [AWS Serverless Application Repository Examples](https://github.com/amazon-archives/serverless-app-examples/tree/master/javascript)
* [2E0PGS Blog - ASP.NET Core Web API AWS Lambda](https://2e0pgs.github.io/blog/programming/2020/10/01/aspnet-core-web-api-aws-lambda/)
* [api-lambda-send-email-ses](https://github.com/simalexan/api-lambda-send-email-ses)

### Lambda

* [Introduction to AWS Lambda & Serverless Applications](https://www.youtube.com/watch?v=EBSdyoO3goc)
* [AWS Lambda with C#](https://youtu.be/Km5G9sNUmV8)

### SES (Simple Email Service)

* A bit like Twilio SendGrid.
* Has [SMTP](https://docs.aws.amazon.com/ses/latest/DeveloperGuide/send-email-smtp.html) and [APIs](https://docs.aws.amazon.com/ses/latest/DeveloperGuide/send-email-api.html)

### ChatBot

* TODO

### WorkSpaces

* Web client works for Windows host.
* Web client doesn't work for Linux host.
* Windows client works well for both.
* [Install software packages on an Amazon Linux instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/install-software.html)
* Don't forget to enable web client access: [New – Web Access for Amazon WorkSpaces](https://aws.amazon.com/blogs/aws/new-web-access-for-amazon-workspaces/)

### ECS (Elastic Container Service)

* Deploy docker images.

### Dynamo DB

* [blueprint-microservice-http-endpoint-on-demand pricing calculator](https://calculator.aws/#/estimate?id=65ae5a6211a2e33e54bfbbefda41445b11163bf9)

### CloudShell

* \> AWS CloudShell is a browser-based shell that gives you command-line access to your AWS resources in the selected AWS region.
* Cloudshell with custom dotfiles and scripts you could put in the userspace it gives you makes it much nicer as a jump box for aws cli work.

## Azure

* [Calculator](https://azure.microsoft.com/en-gb/pricing/calculator/)
* [Azure free services](https://portal.azure.com/#blade/Microsoft_Azure_Billing/FreeServicesBlade)

### Virtual Machine

* [multiplayer-basic-game-server-hosting](https://docs.microsoft.com/en-us/gaming/azure/reference-architectures/multiplayer-basic-game-server-hosting)
* [free-tier-windows-virtual-machine-in-azure](https://www.51sec.org/2019/10/12/create-a-free-tier-windows-virtual-machine-in-azure)
* [Azure VM simple sizes](https://azure.microsoft.com/en-gb/services/virtual-machines/#pricing)
* [Estimate for a MC server](https://azure.com/e/1feff86e78f84676928460944df5267f) (US$75.08 monthly)
* [Estimate for a Windows server](https://azure.com/e/fca1979cff554524ab420150b15959bc) (US$142.24 monthly)
* Doesn't clean up disks when you kill a VM.
* Azure prefers larger disks by default, although using a 128GB disk it's thin provisioned so you only pay for what you use.

### Container Instance

* [Deploy a .NET Core API with Docker (Step-by-Step)](https://www.youtube.com/watch?v=f0lMGPB10bM)

### Blob Storage

* [Azure Blob Storage Tutorial - Setup & Explore with Azure Portal \| Part 1](https://youtu.be/-sCKnOm8G_g)
* FTP
	* [FTP to Azure Blob Storage](https://stackoverflow.com/questions/39992173/ftp-to-azure-blob-storage)
	* See my blog post: SFTP/SSH jail

### SendGrid

* [How to Send Email Using SendGrid with Azure](https://docs.microsoft.com/en-us/azure/sendgrid-dotnet-how-to-send-email)

### Functions

* [Intro to Azure Functions - What they are and how to create and deploy them](https://www.youtube.com/watch?v=zIfxkub7CLY)

## Comparison notes

More of a side by side comparison/observational notes.

> reddit, Instacart, and Lyft are some of the popular companies that use Amazon S3, whereas Azure Storage is used by Starbucks, Yammer, and Microsoft. Amazon S3 has a broader approval, being mentioned in 3194 company stacks & 1559 developers stacks; compared to Azure Storage, which is listed in 82 company stacks and 42 developer stacks.

Ref: [stackshare.io/stackups/amazon-s3-vs-azure-storage](https://stackshare.io/stackups/amazon-s3-vs-azure-storage?)

* [Linke - Comparing the cloud giants: Uptime and reliability](https://www.linkeit.com/blog/comparing-the-gigants-of-cloud-uptime-and-reliability)
* [Azure docs comparing to AWS](https://docs.microsoft.com/en-us/azure/architecture/aws-professional/services)
* Azure easy RDP access
* Azure option of Windows 10 virtual machine which is currently not possible on AWS, however: [Windows Virtual Desktop non-persistent desktops](https://docs.microsoft.com/en-us/answers/questions/192665/windows-virtual-desktop-non-persistent-desktops.html)
* Azure easy auto shutdown option. Shows estimates on pricing when provisioning. 
* TODO: Azure cost vs AWS for Windows VM.
* AWS also easy rdp access, option for Windows server only with no desktop. Nice desktop wallpaper overlay to show IP and hostname. No auto shutdown, no cost estimate on provisioning. 
* AWS free tier on EC2 micro.
* Azure free is an internal page inside their portal along with being a public website page. AWS free is only a public website page.
[Azure free services](https://portal.azure.com/#blade/Microsoft_Azure_Billing/FreeServicesBlade)
* AWS EC2 provisioning, cold start and regular start-up is faster than Azure.
* Ability to disk export in Azure virtual machine via a link.
* I think EC2 restoring snapshot breaks RDP password... yes it does so stop and start VM. don't try reboot as it doesn't update the "get pass"
* Azure makes it easier to view all resources: [Browse all resources](https://portal.azure.com/#blade/HubsExtension/BrowseAll)
* AWS: "Tag filters are required." which means you have to tag everything in order to not loose track of it.
* EC2 restore launch state on storage device makes it easier than Azure to reset a VM to it's original image.
* Azure you need a backup to be able to restore the VM: [Factory reset Windows Server 2016 on Azure](https://stackoverflow.com/questions/45457898/factory-reset-windows-server-2016-on-azure)
* Reload/refresh icon on AWS console for EC2 instances didn't work, I had to use browser refresh specifically to see a change in "Instance state"
* \> Your services were disabled because you reached your spending limit
	* Azure Cost analysis (preview) is good.
* Azure RDP you set the username and password but AWS it's created for you when you restart it and upload the pem key to decrypt it.
* AWS RDP drops if you try copying a large file.
* Azure console has a strange horizontal scrolling UI.
* OTT browser security settings on AWS and Azure Windows server images however I used wWindows 10 desktop on Azure which doesn't have such security settings.

## Other notes

Some of these will move around once I tidy up the post.

* [Comparison Of Different SQL Query Responses Of SQL Server On Windows And Ubuntu Linux](https://www.c-sharpcorner.com/article/comparison-of-different-sql-query-responses-of-sql-server-on-windows-and-ubuntu/)
* AWS Security hub
* AWS Macie
* AWS Inspector
* AWS cloudcommit is git repos
* AWS cloudwatch is logging
* AWS trusted advisor
* AWS cloudformation runs from serverless.template
* AWS CodeCommit
* AWS Cloud9 is a IDE
	* [Language Support in the AWS Cloud9 Integrated Development Environment (IDE)](https://docs.aws.amazon.com/cloud9/latest/user-guide/language-support.html)
* OS disk pricing isn't included on calculator since it doesn't know the OS Image size
	* [virtual machine pricing for operaing system disk](https://docs.microsoft.com/en-us/answers/questions/48739/virtual-machine-pricing-for-operaing-system-disk.html)
		* [ACCEPTED ANSWER](https://docs.microsoft.com/answers/answers/48630/view.html)
	* [How are charges for Amazon EBS volumes calculated on my bill?](https://aws.amazon.com/premiumsupport/knowledge-center/ebs-volume-charges/)
* Azure ephemeral disks
* [How to enable SSH into an Azure Windows VM?](https://serverfault.com/questions/1016653/how-to-enable-ssh-into-an-azure-windows-vm)
	*  \> SSM Agent isn't installed on the instance. You can install the agent on both Windows instances and Linux instances.
* Careful on azure pricing estimates
* [AWS EC2 Instance Comparison: T3 vs T3a vs T4g](https://www.learnaws.org/2020/12/19/t3-t3a-t4g/)
	* T3 good for most things
	* T4 ARM and only good for Linux atm 
* [Configure Windows Server and SQL Server on Amazon EC2](https://calculator.aws/#/createCalculator/EC2WinSQL)
	* SQL Developer is local dev stuff only.
	* SQL Web some cheaper thing for public services.
	* SQL Standard is what you want in most cases.
	* SQL Enterprise offers extra features.
	* Linux is cheaper
* AWS EFS (File system)
* AWS FSx (SMB)
* GP2 is simpler, GP3 if u need refined IOps and throughput
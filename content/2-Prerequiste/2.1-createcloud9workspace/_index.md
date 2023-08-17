---
title: "Create a Cloud9 Workspace"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 2.1 </b> "
---

#### Create a Cloud9 Workspace

1. Type **Cloud9** in the service search bar on the AWS Console then select **Cloud9**.
2. Click **Create environment**
3. Name the Cloud9 Workspace **serverless-workshop.**
4. In the **Description** field, enter the purpose you want to use in this workspace. Enter **deploy "Todos app"**
5. **Environment type,** here we will create a server to run this Cloud9 workspace. I will choose **New EC2 instance** option to create a new server.

![Name for Cloud9 Workspace và chọn Environment type](/images/2.prerequisite/2.1-createcloud9workspace/2.1-1.png)

6. Go to settings for **New EC2 instance,** select **Additional instance types** then we choose **t3.medium**

![Setting cho new EC2 instance](/images/2.prerequisite/2.1-createcloud9workspace/2.1-2.png)

7. Leave defaults for other settings. Click the **Create** button

![Keep default](/images/2.prerequisite/2.1-createcloud9workspace/2.1-3.png)

8. Wait about 10 minutes for Cloud9 Workspace to be created. Once the Cloud9 Workspace is created, we will have an environment to work with the AWS CLI and other tools. In the list of **Environments** created, find the **serverless-workshop** environment and click the **Open** button to open the Cloud9 environment.

![VPC](/images/2.prerequisite/2.1-createcloud9workspace/2.1-4.png)

10. After the environment opens, let's close the sections below that were initialized at the start and create a new **terminal page**.

![VPC](/images/2.prerequisite/2.1-createcloud9workspace/2.1-5.png)

Our workspace will look like this.

![VPC](/images/2.prerequisite/2.1-createcloud9workspace/createcloud9-6.png)

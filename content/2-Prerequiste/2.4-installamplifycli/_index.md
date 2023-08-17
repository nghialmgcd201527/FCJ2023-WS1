---
title: "Install Amplify CLI"
date: "`r Sys.Date()`"
weight: 4
chapter: false
pre: " <b> 2.4 </b> "
---

According to AWS, Amplify is a service designed to help developers build web and mobile applications faster. Amplify provides a set of tools and services for building web and mobile applications. Amplify CLI is part of the Amplify Framework, which provides commands to create and manage AWS resources such as Amazon Cognito, Amazon API Gateway, Amazon Lambda function, Amazon DynamoDB table, and Amazon S3 bucket.

![VPC](/images/2.prerequisite/2.4-amplifycli/amplifycli-1.gif)

In this workshop, we will only configure [AWS Amplify Hosting](https://aws.amazon.com/amplify/hosting/), which provides an easily manageable service for deploying applications global static web application. It will use Amazon S3 and Amazon CloudFornt to store and deliver the static resources of that application.

1. Install the latest version of Amplify CLI by running the following command:

```
npm i -g @aws-amplify/cli

```

Complete the installation of Amplify CLI.

![VPC](/images/2.prerequisite/2.4-amplifycli/2.4-1.png)

2. Once the download is complete, configure the Amplify CLI:

```
amplify configure
```

We will get a link **https://console.aws.amazon.com/** to login to the AWS console.

![VPC](/images/2.prerequisite/2.4-amplifycli/2.4-2.png)

After logging in, press **Enter** and select **Region** that you are doing the workshop, my name is **ap-southeast-1.** Then press **Enter** to continue, we we will get a path **https://console.aws.amazon.com/iamv2/home#/users/create** to create a new IAM user.

![VPC](/images/2.prerequisite/2.4-amplifycli/2.4-3new.png)

Access the above link, name **amplify-user** for that user, select **Next.**

![VPC](/images/2.prerequisite/2.4-amplifycli/2.4-4new.png)

{{% notice note  %}}
By default, we will assign **AdministratorAccess** permission to the user configured for Amplify CLI
{{% /notice %}}

In the **Permmissions options** section, select **Attach policies directly.** Find and select **AdministratorAccess**. Then click **Next.** On the **Review and create page,** select **Create user.**

![VPC](/images/2.prerequisite/2.4-amplifycli/2.4-5new.png)

Next, select the newly created user **amplify-user,** select the **Security credentials tab,** under **Access keys,** click on **Create access key.**

![VPC](/images/2.prerequisite/2.4-amplifycli/2.4-6new.png)

In step **Access key best practices & alternatives,** select **Command Line Interface (CLI)** and tick **Confirm** then select **Next.**

![VPC](/images/2.prerequisite/2.4-amplifycli/2.4-7.png)

On the page **Set description tag,** enter a description for **Access key** depending on your preference, leave the default and select **Create access key.** So you have successfully created ** Access key** for user **amplify-user,** save **Access key** and **Secret access key.** Go back to terminal in Cloud9, press **Enter,** it will ask you enter **Access key** and **Secret access key** just created. After entering, press **Enter,** in **Profile name,** I will leave default, press **Enter,** so I have successfully configured Amplify CLI.

![VPC](/images/2.prerequisite/2.4-amplifycli/2.4-8new.png)

Now we are ready to use Amplify!

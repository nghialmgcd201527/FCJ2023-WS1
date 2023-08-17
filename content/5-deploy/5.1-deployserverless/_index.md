---
title: "Deploy the Serverless Application"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 5.1 </b> "
---

In this step, we will use the SAM CLI to build and deploy the serverless application stack.

First, we need to ensure we're using the latest version of the SAM CLI:

```
pip install --user --upgrade aws-sam-cli

```

We will see the output as shown.

![VPC](/images/5.deploy/5.1-serverless/5.1-1.png)

Next, go to the **sam:** folder

```
cd ~/environment/serverless-tasks-webapp/sam

```

Run the following command to build the application:

```
sam build
```

When the build is successful, we will have as shown.

![VPC](/images/5.deploy/5.1-serverless/5.1-2.png)

The sam build command processes your AWS SAM template file, application code, and any applicable language-specific files and dependencies. The command also copies build artifacts in the format and location expected for subsequent steps in your workflow.

Now we can deploy the application:

```
sam deploy --guided

```

This command packages and deploys the application to your AWS account. It provides a series of prompts:

First, it will ask us for the stack name, we can name it any, here we set it as **tasks-app.** Then it will choose the region we want to deploy the application to, here we choose **ap-southeast-1**.

![VPC](/images/5.deploy/5.1-serverless/5.1-3.png)

Next, we will type **y** and press **Enter** to confirm deploy.

To allow SAM to create roles to connect to resources in our template, let's allow SAM CLI to create IAM roles, we type **Y** and press **Enter.**

To disable rollback to protect our resources when the declaration fails, we type **y** and press **Enter.**

To allow arguments to be saved to the configuration file, we enter **Y** and press **Enter.**

Since the SAM configuration file does not exist yet, we create it, we press **Enter** to default, similar to the environment of SAM configuration.

Check the information before deploying.

![VPC](/images/5.deploy/5.1-serverless/5.1-4.png)

If the deploy is successful, we will see the output as shown.

![VPC](/images/5.deploy/5.1-serverless/5.1-5new.png)

{{% notice note %}}
Please make a note of the **TasksApi** endpoint URL and **S3BucketName** values; you will need these in subsequent steps.
{{% /notice %}}
